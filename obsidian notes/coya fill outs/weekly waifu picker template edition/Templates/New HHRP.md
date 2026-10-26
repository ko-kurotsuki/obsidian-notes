<%*
// Templater template: monthly Hentai Heroine Rescue Program issue.
// Every lady is free, comes with 1 complimentary perk and 1 voucher.
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

const series = clean(await ask("Issue name (e.g. HHRP 4)"));
const week = await ask("Week number");
const season = await ask("Season number", "2");
const count = parseInt(await ask("Number of ladies", "6")) || 6;

for (let i = 1; i <= count; i++) {
  const name = clean(await ask(`Lady ${i}: name`));
  const origin = await ask(`Lady ${i}: series/game`);
  const art = await ask(`Lady ${i}: artist(s)`);
  const [pn, pd] = split(await ask(`${name}: free perk (paste "Name: description")`));
  await mkPerk(clean(pn), pd, name, series, false, 1);
  await mkWaifu(name, origin, art, series, 1);
}

await tp.file.rename(series);
await tp.file.move("Series/" + series);
-%>
---
type: series
week: <% week %>
season: <% season %>
vouchers: 0
bonus_used: 0
bonus_earned: <% count %>
hhrp: true
resort_build: false
---
# <% series %> (Week <% week %>)

Hentai Heroine Rescue Program: every lady is free, comes with her complimentary perk, and brings 1 voucher usable on any of this month's weekly issues.

Catalogue image: ![[]]

## Rescue tracker

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

// Rescue program rules: every lady is free, comes with her complimentary perk,
// and brings 1 voucher usable on any of this month's weekly issues.
// Nothing is spent here; each lady taken in EARNS a voucher.
const o = calc(me, p => p.owned === true);
const pl = calc(me, p => p.owned === true || p.planned === true);

dv.paragraph(
  `**Ladies taken in:** ${o.have} / ${o.rows.length}  \n` +
  `**Vouchers earned:** ${o.have} (1 per lady, good for any issue this month)  \n` +
  `**If you also take your planned picks:** ${pl.have} ladies, ${pl.have} vouchers`
);
dv.table(
  ["Lady", "Free perk", "Voucher", "Status"],
  pl.rows.map((r, i) => [
    r.w.file.link,
    r.perks.map(p => p.file.link),
    r.hers ? "+1" : "",
    o.rows[i].hers ? "✅ taken in" : r.hers ? "📝 planned" : "—"
  ])
);
```
