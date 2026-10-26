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
-%>
---
type: series
week: <% week %>
vouchers: 9
---
# <% series %> (Week <% week %>)<%*
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


Paste the catalogue image here: ![[]]

Off-catalogue waifus bought for 1 voucher: create them from the **Waifu** template with `catalogue: false`.

## Voucher tracker

```dataviewjs
const me = dv.current();
const budget = (me.vouchers ?? 9) + (me.bonus_used ?? 0);
const inSeries = p => p.series && p.series.path === me.file.path;
const waifus = dv.pages('"Waifus"').where(inSeries).values;
const perks = dv.pages('"Perks"').where(inSeries).values;
const waifuPaths = new Set(waifus.map(w => w.file.path));

// Rules: waifu = 1 voucher, perk = 1 voucher (bought separately).
// If all of a waifu's perks (perk_count, default 3) are owned, she comes free.
// 3+ waifus from the series = free Transdimensional Transport Pass.
// Perks with no waifu link ("loose perks") still cost 1 each.
function run(isOn) {
  let spent = 0, have = 0, perksOn = 0;
  const rows = waifus.map(w => {
    const wp = perks.filter(p => p.waifu && p.waifu.path === w.file.path);
    const need = w.perk_count ?? 3;
    const pn = wp.filter(isOn).length;
    const full = need > 0 && pn >= need;
    const bought = isOn(w);
    const wCost = bought && !full ? 1 : 0;
    const hers = bought || full;
    if (hers) have++;
    perksOn += pn;
    spent += wCost + pn;
    return { w, pn, need, full, hers, cost: wCost + pn };
  });
  const loose = perks.filter(p => !(p.waifu && waifuPaths.has(p.waifu.path)) && isOn(p)).length;
  spent += loose; perksOn += loose;
  return { rows, spent, have, perksOn, loose };
}

const o = run(p => p.owned === true);
const pl = run(p => p.owned === true || p.planned === true);
const pass = n => n >= 3 ? "✅ earned" : `❌ ${3 - n} more waifu(s) needed`;
const bonus = me.bonus_used ? ` (includes ${me.bonus_used} bonus vouchers)` : "";

dv.paragraph(
  `**Spent:** ${o.spent} / ${budget}${bonus}, ${budget - o.spent} left  \n` +
  `**Waifus owned:** ${o.have} | **Perks owned:** ${o.perksOn} (${o.loose} without a waifu)  \n` +
  `**Transport Pass:** ${pass(o.have)}  \n\n` +
  `**If you buy your planned picks too:** ${pl.spent} / ${budget} | Pass: ${pass(pl.have)}`
);

dv.table(
  ["Waifu", "Perks", "Cost", "Status", "Note"],
  pl.rows.map((r, i) => {
    const owned = o.rows[i];
    const planned = r.pn - owned.pn;
    return [
      r.w.file.link,
      `${owned.pn}/${r.need}` + (planned > 0 ? ` (+${planned} planned)` : ""),
      r.cost,
      owned.hers ? "✅ owned" : r.hers ? "📝 planned" : "—",
      r.full ? "🎁 full pack, waifu free" : (r.need > 0 && r.pn === r.need - 1 ? "💡 1 perk from a free waifu" : "")
    ];
  })
);
```
