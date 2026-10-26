---
type: waifu
name: "Rani VIII"
series: "[[Type-Moon]]"
games: "Fate/Extra"
artists: "Sanbon Hijiki"
catalogue: true
perk_count: 3
owned: true
planned: false
---
# Rani VIII

Origin: Fate/Extra
Art by: Sanbon Hijiki

```dataviewjs
const me = dv.current();
const base = l => l ? String(l.path ?? l).replace(/[\[\]]/g, "").split("|")[0].split("/").pop().replace(/\.md$/, "") : "";
const rows = dv.pages('"Perks"').where(p => base(p.waifu) === me.file.name).values;
if (rows.length) dv.table(["Perk", "Cost", "Resort", "Owned", "Planned"], rows.map(p => [p.file.link, p.cost ?? 1, p.resort ? "🏝️" : "", p.owned ? "✅" : "", p.planned ? "📝" : ""]));
else dv.paragraph("No perks.");
```
