# Weekly Waifu Tracker: Setup

## Plugins
- **Dataview** (enable "JavaScript queries" in its settings; the trackers use dataviewjs)
- **Templater** (set the template folder to `Templates`)

## Folders to create in your vault
```
Templates/   <- New Series, New HHRP, New Resort Build, Waifu, Perk
Series/
Waifus/
Perks/
```
Put `Dashboard.md` anywhere (the vault root is fine).

## Every week: the regular catalogue
1. Create a new empty note, then run **Templater: Open insert template modal** and pick `New Series`.
2. Answer the prompts: series name, week number, season, number of entries (7), perks per entry (3), then for each entry the waifu name, game(s), artist(s) and her perks. Paste each perk as `Name: description`. Leave a perk blank to skip it.
3. For a red resort option (Season 2, Summer) start the perk with `*`, for example `*Royal Library: You get a massive library...`.
4. It creates the series note (moved into `Series/`), plus every waifu and perk note, all linked together.
5. Set `owned: true` on a waifu or perk to buy it. Use `planned: true` to mock up a build first.
6. Off-catalogue waifus (1 voucher, no perks): use the `Waifu` template and set `catalogue: false`, `perk_count: 0`, `series: "[[Series Name]]"`, `owned: true`.

## Every month: the Hentai Heroine Rescue Program
Run `New HHRP`: issue name, week, season, number of ladies (6), then each lady and her free perk. To take a lady in, set `owned: true` on her note and on her perk. The rescue tracker shows how many ladies you took in and how many vouchers that earned.

## Season 2 (Summer): the pocket dimension beach resort
- Leftover vouchers at the end of the month go into your personal pocket dimension beach resort.
- Some waifus have a **red option**: a resort option in place of an ordinary perk. It still counts toward buying all of her perks to get her free. Mark it `resort: true` on the perk note (or start it with `*` in `New Series`).
- The **resort build week** (week 16, and the Season 2 finale) uses `New Resort Build`. Each option is a perk note with `resort: true` and its own `cost` (the amusement park costs 2).
- You get that week's vouchers (6 for week 16) **plus the leftovers from the 3 preceding weeks**. List those weeks in the `carry_from` property of the resort week and the tracker adds their leftovers for you. Red options you selected in earlier weeks apply to the resort.
- The dashboard lists every resort option you own across all weeks.

## Rules the math follows
Regular weeks:
- 9 vouchers per week (change `vouchers:` in the series note if it varies)
- Waifu = 1 voucher, perk = 1 voucher (or its `cost` property), bought independently
- All of a waifu's perks owned (`perk_count`, default 3) = the waifu is free and counts as owned
- A multi-waifu entry (like Rose & Menat) counts as one waifu
- 3+ waifus from a series = free Transdimensional Transport Pass
- Perks and options with no `waifu` link cost their own price
- `bonus_used` on a series note adds borrowed bonus vouchers to that week's budget
- `carry_from` (resort weeks) adds the leftover vouchers of the listed weeks

Rescue program weeks (`hhrp: true`):
- Every lady is free, with her complimentary perk
- Each lady brings 1 voucher, usable on any of this month's weekly issues
- Nothing is spent, and they do not count toward a Transport Pass

## Tip
Toggle `owned` faster by installing **Meta Bind** and adding an inline toggle, or just click the property checkbox in the Properties panel.
