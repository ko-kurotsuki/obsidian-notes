<%*
// Templater template: monthly Hentai Heroine Rescue Program issue.
// Every lady is free, comes with 1 complimentary perk and 1 voucher.
const clean = s => (s ?? "").replace(/[\\/:*?"<>|#^\[\]]/g, "").trim();
const split = s => { s = s ?? ""; const i = s.indexOf(":"); return i < 0 ? [s.trim(), ""] : [s.slice(0, i).trim(), s.slice(i + 1).trim()]; };

const series = clean(await tp.system.prompt("Issue name (e.g. HHRP 3)"));
const week = await tp.system.prompt("Week number");
const count = parseInt(await tp.system.prompt("Number of ladies", "6")) || 6;

for (let i = 1; i <= count; i++) {
  const name = clean(await tp.system.prompt(`Lady ${i}: name`));
  const origin = (await tp.system.prompt(`Lady ${i}: series/game`, "")) ?? "";
  const art = (await tp.system.prompt(`Lady ${i}: artist(s)`, "")) ?? "";
  const [pn, pd] = split(await tp.system.prompt(`${name}: free perk (paste "Name: description")`));
  const pname = clean(pn);

  const perkText = "---\ntype: perk\nname: \"" + pname + "\"\nwaifu: \"[[" + name + "]]\"\nseries: \"[[" + series + "]]\"\nowned: false\nplanned: false\n---\n" + pd + "\n";
  await tp.file.create_new(perkText, pname, false, "Perks");

  const waifuText = [
    "---", "type: waifu", "name: \"" + name + "\"", "series: \"[[" + series + "]]\"",
    "games: \"" + origin + "\"", "artists: \"" + art + "\"",
    "catalogue: true", "perk_count: 1", "owned: false", "planned: false", "---",
    "# " + name, "",
    "```dataview", "TABLE WITHOUT ID file.link AS Perk, owned AS Owned, planned AS Planned",
    "FROM \"Perks\"", "WHERE waifu = this.file.link", "```", ""
  ].join("\n");
  await tp.file.create_new(waifuText, name, false, "Waifus");
}

await tp.file.rename(series);
await tp.file.move("Series/" + series);
-%>
---
type: series
week: <% week %>
vouchers: 0
bonus_used: 0
bonus_earned: 0
hhrp: true
---
# <% series %> (Week <% week %>)

Hentai Heroine Rescue Program: every lady is free, comes with her complimentary perk, and brings 1 voucher usable on any of this month's weekly issues.

Catalogue image: ![[]]

## Rescue tracker

```dataviewjs
const me = dv.current();
const inSeries = p => p.series && p.series.path === me.file.path;
const waifus = dv.pages('"Waifus"').where(inSeries).values;
const perks = dv.pages('"Perks"').where(inSeries).values;

// Rescue program rules: every lady is free, comes with her complimentary perk,
// and brings 1 voucher usable on any of this month's weekly issues.
// Nothing is spent here; each lady taken in EARNS a voucher.
const run = isOn => {
  const rows = waifus.map(w => ({
    w, on: isOn(w),
    perks: perks.filter(p => p.waifu && p.waifu.path === w.file.path)
  }));
  return { rows, got: rows.filter(r => r.on).length };
};
const o = run(w => w.owned === true);
const pl = run(w => w.owned === true || w.planned === true);

dv.paragraph(
  `**Ladies taken in:** ${o.got} / ${waifus.length}  \n` +
  `**Vouchers earned:** ${o.got} (1 per lady, good for any issue this month)  \n` +
  `**If you also take your planned picks:** ${pl.got} ladies, ${pl.got} vouchers`
);

dv.table(
  ["Lady", "Free perk", "Voucher", "Status"],
  pl.rows.map((r, i) => [
    r.w.file.link,
    r.perks.length ? r.perks.map(p => p.file.link).join(", ") : "",
    r.on ? "+1" : "",
    o.rows[i].on ? "✅ taken in" : r.on ? "📝 planned" : "—"
  ])
);
```
