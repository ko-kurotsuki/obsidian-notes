---
type: waifu
name: "Cynthia & Lusamine"
series: "[[Pokemon]]"
games: "Sinnoh & Alola Regions"
artists: "tansoku102cm"
catalogue: true
perk_count: 3
owned: true
planned: false
---
# Cynthia & Lusamine

Origin: Sinnoh & Alola Regions
Art by: tansoku102cm

```dataviewjs
const me = dv.current();
const base = l => l ? String(l.path ?? l).replace(/[\[\]]/g, "").split("|")[0].split("/").pop().replace(/\.md$/, "") : "";
const rows = dv.pages('"Perks"').where(p => base(p.waifu) === me.file.name).values;
if (rows.length) dv.table(["Perk", "Cost", "Resort", "Owned", "Planned"], rows.map(p => [p.file.link, p.cost ?? 1, p.resort ? "🏝️" : "", p.owned ? "✅" : "", p.planned ? "📝" : ""]));
else dv.paragraph("No perks.");
```
