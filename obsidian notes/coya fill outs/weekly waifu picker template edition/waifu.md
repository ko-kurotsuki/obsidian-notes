type: waifu
name: <% tp.file.title %>
series: ""
games: ""
artists: ""
catalogue: true
owned: false
planned: false
---
# <% tp.file.title %>

<!-- Set series to a link like "[[Street Fighter]]". Off-catalogue waifus (1 voucher, no perks): set catalogue: false. -->

```dataview
TABLE WITHOUT ID file.link AS Perk, owned AS Owned, planned AS Planned
FROM "Perks"
WHERE waifu = this.file.link
```
