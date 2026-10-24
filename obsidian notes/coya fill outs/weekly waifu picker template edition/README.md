# Weekly Waifu Tracker: Setup

## Plugins
- **Dataview** (enable "JavaScript queries" in its settings; the trackers use dataviewjs)
- **Templater** (set the template folder to `Templates`)

## Folders to create in your vault
```
Templates/   <- New Series.md, New HHRP.md, Waifu.md, Perk.md
Series/
Waifus/
Perks/
```
Put `Dashboard.md` anywhere (the vault root is fine).

## Every week: the regular catalogue
1. Create a new empty note, then run **Templater: Open insert template modal** and pick `New Series`.
2. Answer the prompts: series name, week number, number of entries (7), then for each entry the waifu name, game(s), artist(s), and 3 perks. Paste each perk as `Name: description`.
3. It creates the series note (moved into `Series/`), plus every waifu and perk note, all linked together.
4. Set `owned: true` on a waifu or perk to buy it. Use `planned: true` to mock up a build first.
5. Off-catalogue waifus (1 voucher, no perks): use the `Waifu` template and set `catalogue: false`, `perk_count: 0`, `series: "[[Series Name]]"`, `owned: true`.

## Every month: the Hentai Heroine Rescue Program
1. Run `New HHRP` the same way (new empty note, then the Templater insert modal).
2. Enter the issue name (like `HHRP 3`), week number, number of ladies (6), and for each lady her name, series, artist and her free perk (`Name: description`).
3. To take a lady in, set `owned: true` on her note and on her perk note.
4. The rescue tracker shows how many ladies you have taken in and how many vouchers that earned.

## Rules the math follows
Regular weeks:
- 9 vouchers per week (change `vouchers:` in the series note if it varies)
- Waifu = 1 voucher, perk = 1 voucher, bought independently
- All of a waifu's perks owned (`perk_count`, default 3) = the waifu is free and counts as owned
- A multi-waifu entry (like Rose & Menat) counts as one waifu
- 3+ waifus from a series = free Transdimensional Transport Pass
- Perks with no `waifu` link still cost 1 voucher each
- `bonus_used` on a series note adds borrowed bonus vouchers to that week's budget

Rescue program weeks (`hhrp: true` on the series note):
- Every lady is free, with her complimentary perk
- Each lady brings 1 voucher, usable on any of this month's weekly issues
- Nothing is spent in these weeks, and they do not count toward a Transport Pass
- The dashboard counts each lady taken in as 1 bonus voucher earned

## Tip
Toggle `owned` faster by installing **Meta Bind** and adding an inline toggle, or just click the property checkbox in the Properties panel.
