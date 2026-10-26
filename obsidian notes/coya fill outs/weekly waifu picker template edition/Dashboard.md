# Waifu Dashboard

## All weeks

```dataviewjs
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

const seriesPages = dv.pages('"Series"').where(p => p.type === "series").sort(p => p.week).values;
let totSpent = 0, totWaifus = 0, totPerks = 0, totResort = 0, passes = 0, earned = 0, used = 0;
const ownedNames = [], planned = [];

const rows = seriesPages.map(s => {
  const free = s.hhrp === true;
  const o = calc(s, p => p.owned === true);
  const pl = calc(s, p => p.owned === true || p.planned === true);
  o.rows.forEach(r => { if (r.hers) ownedNames.push(r.w.file.link); });
  pl.rows.forEach((r, i) => { if (r.hers && !o.rows[i].hers) planned.push(r.w.file.link); });
  const b = budgetOf(s);
  const spent = free ? 0 : o.spent;
  const pass = !free && o.have >= 3;
  totSpent += spent; totWaifus += o.have; totPerks += o.perksOn; totResort += o.resortOn;
  if (pass) passes++;
  earned += free ? o.have : (s.bonus_earned ?? 0);
  used += s.bonus_used ?? 0;
  return [
    s.week, s.season ?? "", s.file.link,
    free ? `free (+${o.have} vouchers)` : `${spent} / ${b.total}`,
    o.have, o.perksOn, o.resortOn || "", pass ? "✅" : ""
  ];
});

dv.paragraph(
  `**Total waifus:** ${totWaifus} | **Total perks and options:** ${totPerks} | **Resort options:** ${totResort} | **Vouchers spent:** ${totSpent} | **Transport Passes:** ${passes}  \n` +
  `**Bonus vouchers:** ${earned} earned, ${used} used, ${earned - used} left`
);
dv.table(["Week", "Season", "Series", "Vouchers", "Waifus", "Perks", "Resort", "Pass"], rows);

dv.header(3, "Owned waifus");
dv.list(ownedNames);
dv.header(3, "Planned waifus");
dv.list(planned);
```

## Beach resort options owned

```dataview
TABLE WITHOUT ID file.link AS Option, series AS Series, cost AS Cost
FROM "Perks"
WHERE owned = true AND resort = true
SORT series ASC
```

## Resort options still planned

```dataview
TABLE WITHOUT ID file.link AS Option, series AS Series, cost AS Cost
FROM "Perks"
WHERE planned = true AND owned != true AND resort = true
SORT series ASC
```

## Owned perks

```dataview
TABLE WITHOUT ID file.link AS Perk, waifu AS Waifu, series AS Series
FROM "Perks"
WHERE owned = true
SORT series ASC
```

## Planned perks

```dataview
TABLE WITHOUT ID file.link AS Perk, waifu AS Waifu, series AS Series
FROM "Perks"
WHERE planned = true AND owned != true
SORT series ASC
```
