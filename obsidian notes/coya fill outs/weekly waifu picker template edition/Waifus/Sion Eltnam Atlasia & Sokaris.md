---
type: waifu
name: "Sion Eltnam Atlasia & Sokaris"
series: "[[Type-Moon]]"
games: "Melty Blood/Fate Grand Order"
artists: "Arufa (th bluerose)"
catalogue: true
perk_count: 3
owned: true
planned: false
---
# Sion Eltnam Atlasia & Sokaris

Origin: Melty Blood/Fate Grand Order
Art by: Arufa (th bluerose)

```dataviewjs
const me = dv.current();
const base = l => l ? String(l.path ?? l).replace(/[\[\]]/g, "").split("|")[0].split("/").pop().replace(/\.md$/, "") : "";
const rows = dv.pages('"Perks"').where(p => base(p.waifu) === me.file.name).values;
if (rows.length) dv.table(["Perk", "Cost", "Resort", "Owned", "Planned"], rows.map(p => [p.file.link, p.cost ?? 1, p.resort ? "🏝️" : "", p.owned ? "✅" : "", p.planned ? "📝" : ""]));
else dv.paragraph("No perks.");
```
