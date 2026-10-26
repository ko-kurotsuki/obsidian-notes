---
type: waifu
name: "Olga Discordia"
series: "[[HHRP 1]]"
games: "Kuroinu Kedakaki Seijo Wa Hakudaku Ni Somaru"
artists: "Sakimichan"
catalogue: true
perk_count: 1
owned: true
planned: false
---
# Olga Discordia

Origin: Kuroinu Kedakaki Seijo Wa Hakudaku Ni Somaru
Art by: Sakimichan

```dataviewjs
const me = dv.current();
const base = l => l ? String(l.path ?? l).replace(/[\[\]]/g, "").split("|")[0].split("/").pop().replace(/\.md$/, "") : "";
const rows = dv.pages('"Perks"').where(p => base(p.waifu) === me.file.name).values;
if (rows.length) dv.table(["Perk", "Cost", "Resort", "Owned", "Planned"], rows.map(p => [p.file.link, p.cost ?? 1, p.resort ? "🏝️" : "", p.owned ? "✅" : "", p.planned ? "📝" : ""]));
else dv.paragraph("No perks.");
```
