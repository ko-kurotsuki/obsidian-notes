---
type: waifu
name: "Claudia Levantine"
series: "[[HHRP 2]]"
games: "Kuroinu Kedakaki Seijo wa Hakudaku ni Somaru"
artists: "Official Artwork"
catalogue: true
perk_count: 1
owned: true
planned: false
---
# Claudia Levantine

Origin: Kuroinu Kedakaki Seijo wa Hakudaku ni Somaru
Art by: Official Artwork

```dataviewjs
const me = dv.current();
const base = l => l ? String(l.path ?? l).replace(/[\[\]]/g, "").split("|")[0].split("/").pop().replace(/\.md$/, "") : "";
const rows = dv.pages('"Perks"').where(p => base(p.waifu) === me.file.name).values;
if (rows.length) dv.table(["Perk", "Cost", "Resort", "Owned", "Planned"], rows.map(p => [p.file.link, p.cost ?? 1, p.resort ? "🏝️" : "", p.owned ? "✅" : "", p.planned ? "📝" : ""]));
else dv.paragraph("No perks.");
```
