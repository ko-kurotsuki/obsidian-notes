---
type: waifu
name: <% tp.file.title %>
series: ""
games: ""
artists: ""
catalogue: true
perk_count: 3
owned: false
planned: false
---
# <% tp.file.title %>

<!-- Set series to a link like "[[Naruto]]". Off-catalogue waifus (1 voucher, no perks): set catalogue: false and perk_count: 0. -->

```dataviewjs
const me = dv.current();
const base = l => l ? String(l.path ?? l).replace(/[\[\]]/g, "").split("|")[0].split("/").pop().replace(/\.md$/, "") : "";
const rows = dv.pages('"Perks"').where(p => base(p.waifu) === me.file.name).values;
if (rows.length) dv.table(["Perk", "Cost", "Resort", "Owned", "Planned"], rows.map(p => [p.file.link, p.cost ?? 1, p.resort ? "🏝️" : "", p.owned ? "✅" : "", p.planned ? "📝" : ""]));
else dv.paragraph("No perks.");
```
