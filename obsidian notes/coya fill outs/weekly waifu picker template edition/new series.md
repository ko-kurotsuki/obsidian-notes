<%*
// Templater template: creates a series note + all waifu and perk notes for the week.
const clean = s => (s ?? "").replace(/[\\/:*?"<>|#^\[\]]/g, "").trim();
const split = s => { s = s ?? ""; const i = s.indexOf(":"); return i < 0 ? [s.trim(), ""] : [s.slice(0, i).trim(), s.slice(i + 1).trim()]; };

const series = clean(await tp.system.prompt("Series name"));
const week = await tp.system.prompt("Week number");
const count = parseInt(await tp.system.prompt("Number of catalogue entries", "7")) || 7;

for (let i = 1; i <= count; i++) {
  const name = clean(await tp.system.prompt(`Entry ${i}: waifu name (pairs are fine, e.g. Rose & Menat)`));
  const games = (await tp.system.prompt(`Entry ${i}: game(s)`, "")) ?? "";
  const artists = (await tp.system.prompt(`Entry ${i}: artist(s)`, "")) ?? "";

  for (let j = 1; j <= 3; j++) {
    const [pn, pd] = split(await tp.system.prompt(`${name}, perk ${j} (paste "Name: description")`));
    const pname = clean(pn);
    const perkText = "---\ntype: perk\nname: \"" + pname + "\"\nwaifu: \"[[" + name + "]]\"\nseries: \"[[" + series + "]]\"\nowned: false\nplanned: false\n---\n" + pd + "\n";
    await tp.file.create_new(perkText, pname, false, "Perks");
  }

  const waifuText = [
    "---",
    "type: waifu",
    "name: \"" + name + "\"",
    "series: \"[[" + series + "]]\"",
    "games: \"" + games + "\"",
    "artists: \"" + artists + "\"",
    "catalogue: true",
    "owned: false",
    "planned: false",
    "---",
    "# " + name,
    "",
    "```dataview",
    "TABLE WITHOUT ID file.link AS Perk, owned AS Owned, planned AS Planned",
    "FROM \"Perks\"",
    "WHERE waifu = this.file.link",
    "```",
    ""
  ].join("\n");
  await tp.file.create_new(waifuText, name, false, "Waifus");
}

await tp.file.rename(series);
await tp.file.move("Series/" + series);
%>
---
type: series
week: <% week %>
vouchers: 9
---
# <% series %> (Week <% week %>)

Paste the catalogue image here: ![[]]

Off-catalogue waifus bought for 1 voucher: create them from the **Waifu** template with `catalogue: false`.

## Voucher tracker

```dataviewjs
const me = dv.current();
const budget = me.vouchers ?? 9;
const inSeries = p => p.series && p.series.path === me.file.path;
const waifus = dv.pages('"Waifus"').where(inSeries).values;
const perks = dv.pages('"Perks"').where(inSeries).values;

// Rules: waifu = 1 voucher, perk = 1 voucher (bought separately).
// If all of a waifu's perks are owned, she comes free.
// 3+ waifus from the series = free Transdimensional Transport Pass.
function run(isOn) {
  let spent = 0, have = 0, perksOn = 0;
  const rows = waifus.map(w => {
    const wp = perks.filter(p => p.waifu && p.waifu.path === w.file.path);
    const pn = wp.filter(isOn).length;
    const full = wp.length > 0 && pn === wp.length;
    const bought = isOn(w);
    const wCost = bought && !full ? 1 : 0;
    const hers = bought || full;
    if (hers) have++;
    perksOn += pn;
    spent += wCost + pn;
    return { w, pn, total: wp.length, full, hers, cost: wCost + pn };
  });
  return { rows, spent, have, perksOn };
}

const o = run(p => p.owned === true);
const pl = run(p => p.owned === true || p.planned === true);
const pass = n => n >= 3 ? "✅ earned" : `❌ ${3 - n} more waifu(s) needed`;

dv.paragraph(
  `**Spent:** ${o.spent} / ${budget} (${budget - o.spent} left)  \n` +
  `**Waifus owned:** ${o.have} | **Perks owned:** ${o.perksOn}  \n` +
  `**Transport Pass:** ${pass(o.have)}  \n\n` +
  `**If you buy your planned picks too:** ${pl.spent} / ${budget} (${budget - pl.spent} left) | Pass: ${pass(pl.have)}`
);

dv.table(
  ["Waifu", "Perks", "Cost", "Status", "Note"],
  pl.rows.map((r, i) => {
    const owned = o.rows[i];
    const planned = r.pn - owned.pn;
    return [
      r.w.file.link,
      `${owned.pn}/${r.total}` + (planned > 0 ? ` (+${planned} planned)` : ""),
      r.cost,
      owned.hers ? "✅ owned" : r.hers ? "📝 planned" : "—",
      r.full ? "🎁 full pack, waifu free" : (r.total > 0 && r.pn === r.total - 1 ? "💡 1 perk from a free waifu" : "")
    ];
  })
);
```
