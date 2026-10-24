---
type: series
week: 8
vouchers: 0
bonus_used: 0
bonus_earned: 6
hhrp: true
---
# HHRP 2 (Week 8)

Previous: [[Overlord]] | Next: [[Konosuba]]

- Hentai Heroine Rescue Program: the monthly special issue.
- Every lady is free and comes with her complimentary perk and 1 voucher, usable on any of this month's weekly issues.
- Ladies are free to leave if you treat them poorly.

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
