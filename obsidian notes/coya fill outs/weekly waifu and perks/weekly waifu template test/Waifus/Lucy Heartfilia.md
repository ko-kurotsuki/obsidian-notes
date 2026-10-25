---
type: waifu
name: "Lucy Heartfilia"
series: "[[Fairy Tail]]"
games: "Fairy Tail"
artists: "NeoArtCore & rotix"
catalogue: true
perk_count: 3
owned: true
planned: false
---
# Lucy Heartfilia

Origin: Fairy Tail
Art by: NeoArtCore & rotix

```dataviewjs
const me = dv.current();
const base = l => l ? String(l.path ?? l).replace(/[\[\]]/g, "").split("|")[0].split("/").pop().replace(/\.md$/, "") : "";
const rows = dv.pages('"Perks"').where(p => base(p.waifu) === me.file.name).values;
if (rows.length) dv.table(["Perk", "Cost", "Resort", "Owned", "Planned"], rows.map(p => [p.file.link, p.cost ?? 1, p.resort ? "🏝️" : "", p.owned ? "✅" : "", p.planned ? "📝" : ""]));
else dv.paragraph("No perks.");
```
