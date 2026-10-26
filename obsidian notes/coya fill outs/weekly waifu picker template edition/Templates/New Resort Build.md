<%*
// Templater template: a pocket dimension resort build week (Summer season).
// You get that week's vouchers plus the leftovers of the preceding weeks listed in carry_from.
// Every option becomes a perk note with resort: true and its own cost.
const clean = s => (s ?? "").replace(/[\\/:*?"<>|#^\[\]]/g, "").trim();
const split = s => { s = s ?? ""; const i = s.indexOf(":"); return i < 0 ? [s.trim(), ""] : [s.slice(0, i).trim(), s.slice(i + 1).trim()]; };
const ask = async (q, d = "") => (await tp.system.prompt(q, d)) ?? "";
const fence = "`".repeat(3);
const waifuBlock = fence + "dataviewjs\n" + String.raw`const me = dv.current();
const base = l => l ? String(l.path ?? l).replace(/[\[\]]/g, "").split("|")[0].split("/").pop().replace(/\.md$/, "") : "";
const rows = dv.pages('"Perks"').where(p => base(p.waifu) === me.file.name).values;
if (rows.length) dv.table(["Perk", "Cost", "Resort", "Owned", "Planned"], rows.map(p => [p.file.link, p.cost ?? 1, p.resort ? "🏝️" : "", p.owned ? "✅" : "", p.planned ? "📝" : ""]));
else dv.paragraph("No perks.");` + "\n" + fence + "\n";
const mkPerk = async (name, desc, waifu, series, resort, cost) => {
  const text = ["---", "type: perk", "name: \"" + name + "\"", "waifu: \"" + (waifu ? "[[" + waifu + "]]" : "") + "\"",
    "series: \"[[" + series + "]]\"", "resort: " + resort, "cost: " + cost, "owned: false", "planned: false", "---", desc, ""].join("\n");
  await tp.file.create_new(text, name, false, "Perks");
};
const mkWaifu = async (name, games, artists, series, perkCount) => {
  const text = ["---", "type: waifu", "name: \"" + name + "\"", "series: \"[[" + series + "]]\"",
    "games: \"" + games + "\"", "artists: \"" + artists + "\"", "catalogue: true", "perk_count: " + perkCount,
    "owned: false", "planned: false", "---", "# " + name, "", waifuBlock].join("\n");
  await tp.file.create_new(text, name, false, "Waifus");
};

const series = clean(await ask("Series name (e.g. Beach Resort Build)"));
const week = await ask("Week number");
const season = await ask("Season number", "2");
const vouchers = parseInt(await ask("Vouchers for this week", "6")) || 0;
const carryIn = await ask("Preceding weeks whose leftover vouchers carry over (series names, comma separated)");
const carry = carryIn.split(",").map(s => clean(s)).filter(s => s).map(s => "\"[[" + s + "]]\"").join(", ");
const count = parseInt(await ask("Number of options", "12")) || 12;

for (let i = 1; i <= count; i++) {
  const name = clean(await ask(`Option ${i}: name`));
  const cost = parseInt(await ask(`Option ${i}: voucher cost`, "1")) || 1;
  const desc = await ask(`Option ${i}: description`);
  await mkPerk(name, desc, "", series, true, cost);
}

await tp.file.rename(series);
await tp.file.move("Series/" + series);
-%>
---
type: series
week: <% week %>
season: <% season %>
vouchers: <% vouchers %>
bonus_used: 0
bonus_earned: 0
hhrp: false
resort_build: true
carry_from: [<% carry %>]
---
# <% series %> (Week <% week %>)

Resort build week: any red options you selected in earlier weeks apply, and leftover vouchers from the weeks in carry_from are added to this week's vouchers.
To buy an option set owned: true on its note; use planned: true to mock up a build first.

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
