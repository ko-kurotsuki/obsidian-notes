# Waifu Dashboard

## All weeks

```dataviewjs
const seriesPages = dv.pages('"Series"').where(p => p.type === "series").sort(p => p.week).values;
const allWaifus = dv.pages('"Waifus"').values;
const allPerks = dv.pages('"Perks"').values;

let totSpent = 0, totWaifus = 0, totPerks = 0, passes = 0, earned = 0, used = 0;
const ownedNames = [], planned = [];

const rows = seriesPages.map(s => {
  const inS = p => p.series && p.series.path === s.file.path;
  const ws = allWaifus.filter(inS);
  const ps = allPerks.filter(inS);
  const wpaths = new Set(ws.map(w => w.file.path));
  const free = s.hhrp === true;
  let spent = 0, have = 0, perksOn = 0;
  for (const w of ws) {
    const wp = ps.filter(p => p.waifu && p.waifu.path === w.file.path);
    const need = w.perk_count ?? 3;
    const pn = wp.filter(p => p.owned === true).length;
    const full = need > 0 && pn >= need;
    const bought = w.owned === true;
    if (bought || full) { have++; ownedNames.push(w.file.link); }
    else if (w.planned === true) planned.push(w.file.link);
    spent += free ? 0 : (bought && !full ? 1 : 0) + pn;
    perksOn += pn;
  }
  const loose = ps.filter(p => !(p.waifu && wpaths.has(p.waifu.path)) && p.owned === true).length;
  spent += free ? 0 : loose; perksOn += loose;
  const budget = (s.vouchers ?? 9) + (s.bonus_used ?? 0);
  const pass = !free && have >= 3;
  totSpent += spent; totWaifus += have; totPerks += perksOn; if (pass) passes++;
  earned += free ? have : (s.bonus_earned ?? 0); used += s.bonus_used ?? 0;
  return [s.week, s.file.link, free ? `free (+${have} vouchers)` : `${spent} / ${budget}`, have, perksOn, pass ? "✅" : ""];
});

dv.paragraph(
  `**Total waifus:** ${totWaifus} | **Total perks:** ${totPerks} | **Vouchers spent:** ${totSpent} | **Transport Passes:** ${passes}  \n` +
  `**Bonus vouchers:** ${earned} earned, ${used} used, ${earned - used} left`
);
dv.table(["Week", "Series", "Vouchers", "Waifus", "Perks", "Pass"], rows);

dv.header(3, "Owned waifus");
dv.list(ownedNames);
dv.header(3, "Planned waifus");
dv.list(planned);
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
