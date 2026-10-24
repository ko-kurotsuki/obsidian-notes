---
type: series
week: 5
vouchers: 9
bonus_used: 4
bonus_earned: 0
hhrp: false
---
# Type-Moon (Week 5)

Previous: [[HHRP 1]] | Next: [[Fairy Tail]]

Catalogue image: ![[]]

## Voucher tracker

```dataviewjs
const me = dv.current();
const budget = (me.vouchers ?? 9) + (me.bonus_used ?? 0);
const inSeries = p => p.series && p.series.path === me.file.path;
const waifus = dv.pages('"Waifus"').where(inSeries).values;
const perks = dv.pages('"Perks"').where(inSeries).values;
const waifuPaths = new Set(waifus.map(w => w.file.path));

// Rules: waifu = 1 voucher, perk = 1 voucher (bought separately).
// If all of a waifu's perks (perk_count, default 3) are owned, she comes free.
// 3+ waifus from the series = free Transdimensional Transport Pass.
// Perks with no waifu link ("loose perks") still cost 1 each.
function run(isOn) {
  let spent = 0, have = 0, perksOn = 0;
  const rows = waifus.map(w => {
    const wp = perks.filter(p => p.waifu && p.waifu.path === w.file.path);
    const need = w.perk_count ?? 3;
    const pn = wp.filter(isOn).length;
    const full = need > 0 && pn >= need;
    const bought = isOn(w);
    const wCost = bought && !full ? 1 : 0;
    const hers = bought || full;
    if (hers) have++;
    perksOn += pn;
    spent += wCost + pn;
    return { w, pn, need, full, hers, cost: wCost + pn };
  });
  const loose = perks.filter(p => !(p.waifu && waifuPaths.has(p.waifu.path)) && isOn(p)).length;
  spent += loose; perksOn += loose;
  return { rows, spent, have, perksOn, loose };
}

const o = run(p => p.owned === true);
const pl = run(p => p.owned === true || p.planned === true);
const pass = n => n >= 3 ? "✅ earned" : `❌ ${3 - n} more waifu(s) needed`;
const bonus = me.bonus_used ? ` (includes ${me.bonus_used} bonus vouchers)` : "";

dv.paragraph(
  `**Spent:** ${o.spent} / ${budget}${bonus}, ${budget - o.spent} left  \n` +
  `**Waifus owned:** ${o.have} | **Perks owned:** ${o.perksOn} (${o.loose} without a waifu)  \n` +
  `**Transport Pass:** ${pass(o.have)}  \n\n` +
  `**If you buy your planned picks too:** ${pl.spent} / ${budget} | Pass: ${pass(pl.have)}`
);

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
```
