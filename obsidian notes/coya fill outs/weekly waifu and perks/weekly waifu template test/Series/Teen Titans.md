---
type: series
week: 19
season: 2
vouchers: 9
bonus_used: 5
bonus_earned: 0
hhrp: false
resort_build: false
---
# Teen Titans (Week 19)

Previous: [[Justice League Unlimited]] | Next: [[Wanted]]

Catalogue image: ![[]]

## Voucher tracker

```dataviewjs
const me = dv.current();

const base = l => {
  if (!l) return "";
  let s = typeof l === "string" ? l : (l.path ?? "");
  s = s.replace(/^\[\[/, "").replace(/\]\]$/, "").split("|")[0].split("#")[0];
  return s.split("/").pop().replace(/\.md$/, "");
};
const cost = p => (typeof p.cost === "number" ? p.cost : 1);

// Rules: waifu = 1 voucher, perk = 1 voucher unless it has a cost property (bought separately).
// If all of a waifu's perks (perk_count, default 3) are owned, she comes free.
// Red resort options (resort: true) are ordinary perks for this rule.
// Perks/options with no waifu cost their own price.
// 3+ waifus from a series = free Transdimensional Transport Pass.
function calc(s, isOn) {
  const mine = x => base(x.series) === s.file.name;
  const waifus = dv.pages('"Waifus"').where(mine).values;
  const perks = dv.pages('"Perks"').where(mine).values;
  const names = new Set(waifus.map(w => w.file.name));
  let spent = 0, have = 0, perksOn = 0;
  const rows = waifus.map(w => {
    const wp = perks.filter(p => base(p.waifu) === w.file.name);
    const on = wp.filter(isOn);
    const need = typeof w.perk_count === "number" ? w.perk_count : 3;
    const full = need > 0 && on.length >= need;
    const bought = isOn(w);
    const wCost = bought && !full ? 1 : 0;
    const pCost = on.reduce((a, p) => a + cost(p), 0);
    const hers = bought || full;
    if (hers) have++;
    perksOn += on.length;
    spent += wCost + pCost;
    return { w, perks: wp, pn: on.length, need, full, hers, cost: wCost + pCost };
  });
  const loose = perks.filter(p => !names.has(base(p.waifu)));
  const looseOn = loose.filter(isOn);
  spent += looseOn.reduce((a, p) => a + cost(p), 0);
  perksOn += looseOn.length;
  const resortOn = perks.filter(p => p.resort === true && isOn(p)).length;
  return { rows, loose, spent, have, perksOn, resortOn };
}

const baseBudget = s => (s.vouchers ?? 9) + (s.bonus_used ?? 0);
// Resort build weeks: leftover vouchers from the weeks listed in carry_from are added.
function budgetOf(s) {
  const prev = Array.from(s.carry_from ?? []).map(base);
  let carry = 0;
  if (prev.length) {
    for (const ps of dv.pages('"Series"').where(x => prev.includes(x.file.name)).values) {
      if (ps.hhrp === true) continue;
      carry += Math.max(0, baseBudget(ps) - calc(ps, p => p.owned === true).spent);
    }
  }
  return { base: baseBudget(s), carry, total: baseBudget(s) + carry };
}

const b = budgetOf(me);
const o = calc(me, p => p.owned === true);
const pl = calc(me, p => p.owned === true || p.planned === true);
const pass = n => n >= 3 ? "✅ earned" : `❌ ${3 - n} more waifu(s) needed`;
const parts = [];
if (me.bonus_used) parts.push(`${me.bonus_used} bonus`);
if (b.carry) parts.push(`${b.carry} carried over`);
const note = parts.length ? ` (includes ${parts.join(", ")})` : "";
const hasWaifus = o.rows.length > 0;

dv.paragraph(
  `**Spent:** ${o.spent} / ${b.total}${note}, ${b.total - o.spent} left  \n` +
  (hasWaifus ? `**Waifus owned:** ${o.have} | ` : "") + `**Perks and options owned:** ${o.perksOn}  \n` +
  (hasWaifus ? `**Transport Pass:** ${pass(o.have)}  \n` : "") + `\n` +
  `**If you buy your planned picks too:** ${pl.spent} / ${b.total}` + (hasWaifus ? ` | Pass: ${pass(pl.have)}` : "")
);

if (hasWaifus) {
  dv.table(
    ["Waifu", "Perks", "Cost", "Status", "Note"],
    pl.rows.map((r, i) => {
      const owned = o.rows[i];
      const planned = r.pn - owned.pn;
      return [
        r.w.file.link,
        `${owned.pn}/${r.need}` + (planned > 0 ? ` (+${planned} planned)` : ""),
        r.cost,
        owned.hers ? "✅ owned" : r.hers ? "📝 planned" : "—",
        r.full ? "🎁 full pack, waifu free" : (r.need > 0 && r.pn === r.need - 1 ? "💡 1 perk from a free waifu" : "")
      ];
    })
  );
}
if (pl.loose.length) {
  dv.header(3, "Perks and options without a waifu");
  dv.table(
    ["Name", "Cost", "Resort", "Status"],
    pl.loose.map(p => [
      p.file.link, cost(p), p.resort ? "🏝️" : "",
      p.owned === true ? "✅ owned" : p.planned === true ? "📝 planned" : "—"
    ])
  );
}
```
