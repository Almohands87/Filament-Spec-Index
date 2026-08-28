# Contributing

The most valuable thing you can contribute is **measured data**. Every number in this database is either a manufacturer datasheet figure or an engineering judgement call. A single tensile coupon you printed and pulled yourself is worth more than any of it.

## What is most wanted

**1. Z-axis tensile measurements.** The layer-adhesion derate (`zRatio`) is the weakest data in the project. It is estimated for most materials. If you print XY and Z coupons of the same material on the same machine and pull both, that pair of numbers is directly useful.

**2. PPA-CF Core versus ordinary PPA-CF.** Core-shell filaments claim better Z-strength because the unfilled outer shell puts polymer, not chopped fibre, in the weld zone between layers. Currently entered at `zRatio: 0.5` on a manufacturer claim, flagged low confidence. Nobody appears to have published a measured comparison.

**3. Creep, fatigue and wear.** These are 1–5 judgement ratings. Any real measurement beats them.

**4. Corrections.** If a manufacturer datasheet says something different from what is in here, that is a bug. Say which datasheet.

## Submitting test data

Open an issue using the **Material test data** template. Please include:

- Material and exact product name (brand and grade, e.g. "Polymaker Fiberon PA6-CF20", not just "nylon")
- What you measured and the standard, if any (ISO 527, ASTM D638, or a described custom rig)
- Specimen orientation — XY or Z
- Print settings: nozzle and bed temperature, chamber temperature, layer height, wall count, infill percentage and pattern, speed
- Whether the filament was dried, and whether the part was annealed
- Number of specimens and the spread, not just an average
- Machine used

Raw numbers with imperfect methodology are still welcome. Describe what you did and the data will be labelled accordingly. **Do not smooth or round your results to look tidier** — the spread is information.

## How data enters the database

Each material carries a confidence rating, and contributions are labelled by their strength of evidence:

| Rating | Meaning |
|---|---|
| **high** | Multiple independent sources publish consistent ISO 527 values |
| **medium** | Single source, or values inferred from fibre loading |
| **low** | Manufacturer comparative claim, no published ISO figure |

Measured community data with documented methodology can raise a rating. A marketing claim cannot.

## Editing the data

All material data sits in one array near the top of `index.html`, marked `const M = [`. Each entry is a plain object. Extended properties (creep, fatigue, wear, moisture, and the four chemical sub-ratings) live in the `EXT` object immediately after it.

Two rules the data must satisfy, because the code and the copy both depend on them:

- **Amorphous polymers have `tm: null` and `anneal: null`.** They have no crystal phase, so there is no melting point and annealing cannot raise heat resistance. An amorphous entry with an annealing block contradicts the explanation in the Classes tab.
- **Semi-crystalline polymers have a numeric `tm`.**

Chart axis maxima are computed from the data at runtime, so adding a material with a higher value than any existing one will not break a chart.

## Adding a material

Copy the nearest existing entry and edit every field. Required keys:

```
id, name, full, fam, struct, fiber, tg, tm, clarity,
nozzle, bed, fan, minHotend, fanNote, enc, hard, dry,
anneal, glue, glueAvoid,
tensile, tLo, tHi, zRatio, modulus, impact, elong,
hdt, density, cost, diff, uv, chem, tough, conf,
pros, cons, use, avoid, note, brands
```

Then add a matching row to `EXT` with `creep, fatigue, wear, hygro, cw, co, cs, ca`.

`minHotend` is the **minimum hotend capability needed to print the material at all**, not the top of its usable range. PC prints fine at 275 °C, so its `minHotend` is 280 rather than 300. Getting this wrong will wrongly exclude the material from the Finder for people whose printers can actually handle it.

## Code style

Plain HTML, CSS and JavaScript with no build step, and it should stay that way. If a change requires a bundler, it probably belongs in the React version under `react/` instead.

## Scope

In scope: FDM thermoplastic filaments.

Out of scope for now: photopolymer resins. Resins are thermosets — crosslinked networks with a Tg but no Tm, no crystallinity, and far less layer anisotropy. Nozzle temperature, cooling fan, enclosure and annealing-for-crystallinity are all meaningless for them, and post-curing is a different mechanism entirely. Resins deserve a parallel data model rather than being forced into this one. If you want to build that, open an issue first so it can be designed properly.
