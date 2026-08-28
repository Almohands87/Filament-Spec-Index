# Filament Spec Index

A material selection tool for FDM 3D printing. 33 filaments with mechanical, thermal and structural properties, plus a requirements-driven finder that ranks materials for a specific part and service environment.

**[→ Open the tool](https://almohands87.github.io/Filament-Spec-Index/)**

Single HTML file. No build step, no framework, no dependencies beyond a web font.

---

## What it does

| Tab | Purpose |
|---|---|
| **Finder** | Answer nine questions about the part and its service conditions. Materials that cannot survive are ruled out with a stated reason; survivors are ranked with the reasoning shown. |
| **Classes** | Materials grouped by polymer structure (amorphous / semi-crystalline) and fibre reinforcement (unfilled / CF / GF). |
| **Compare** | Charts up to four materials across strength, stiffness, toughness, HDT, Tg, density and price. |
| **Data** | Full datasheet per material: Tg, Tm, transparency, annealing, adhesives, cooling settings, pros and cons, and the published figures from each brand. |

## Three things that make it different from a typical filament chart

**1. Everything is normalised to XY tensile strength (ISO 527).**
Manufacturers do not publish comparable numbers. Bambu Lab's public comparison guide quotes **bending** strength, while Prusa and Polymaker quote **tensile**. Bending runs roughly 1.45× tensile for these polymers, so averaging them directly inflates one brand by about 45%. Most "average filament strength" charts online make exactly this mistake. Here, flexural figures are converted and every brand's original number is shown separately with its test method labelled.

**2. Z-axis strength is shown alongside XY.**
The strength bars are two-tone: the pale block is XY strength, the solid block inside is strength across layer lines. That gap is where printed parts actually fail. ASA retains roughly 35% across layers; PA6-CF around 33%; PETG around 75%. A part designed to the datasheet number and printed in the wrong orientation can fail at a third of the expected load.

**3. Crystallinity is treated as the organising principle.**
Whether a polymer forms crystals decides four things at once — whether it can be transparent, whether annealing raises heat resistance, how much it shrinks and warps, and how hard it is to glue. Some consequences that surprise people:

- PLA prints clear because it barely crystallises. Anneal it and it turns opaque white — that is crystal formation you can see.
- PETG-CF gains no heat resistance from fibre, but PET-CF gains a great deal. PETG is glycol-modified specifically to block crystallisation, so fibre cannot nucleate anything.
- The best chemical resistance and the worst glueability are the same property. PP, POM, PPS and PEEK resist solvents so well that no solvent can weld them.

## Materials covered

**Amorphous** — PETG, PCTG, ABS, ASA, PC, PMMA, PVB, HIPS, PEI (ULTEM), PETG-CF, ASA-CF, ABS-GF, PC-CF

**Semi-crystalline** — PLA, Tough PLA, HT-PLA, Nylon (PA6/CoPA), PP, POM, PEEK, PLA-CF, HT-PLA-GF, PET-CF, PA6-CF, PA6-GF, PAHT-CF/PA12-CF, PPA, PPA-GF, PPA-CF, PPA-CF Core, PPS-CF, PPS-GF

**Block copolymer** — TPU 95A

## Data sources

Published technical datasheets from **Prusa Research**, **Bambu Lab**, **Polymaker** and **Siraya Tech**, supplemented by polymer literature for Tg and Tm where a manufacturer does not publish DSC data.

Every material carries a confidence rating:

- **high** — multiple brands publish consistent ISO 527 tensile values
- **medium** — a single source, or values inferred from fibre loading
- **low** — manufacturer comparative claims only, not a published ISO figure

## Limitations — read this before designing a part

**These are datasheet typicals, not design allowables.** They come from solid test coupons at 100% infill. Your part, printed with three walls and 20% gyroid infill, will be substantially weaker.

The **Z-axis figure is a derate, not a measurement.** Layer adhesion depends on chamber temperature, layer height, nozzle material and print speed as much as on the polymer.

The **creep, fatigue and wear ratings are judgement calls on a 1–5 scale**, not measured values. The hard gates in the Finder (temperature, chemical, printer capability) rest on published data and are solid. The ordering *within* the surviving materials is directional.

**Print a test coupon before trusting a load path.** If you measure something that contradicts this database, please [open an issue](../../issues/new/choose) — measured data is worth more than anything here.

## Running it

Clone and open `index.html` in a browser. That is the entire process.

```bash
git clone https://github.com/YOUR-USERNAME/filament-spec-index.git
cd filament-spec-index
open index.html          # macOS
# xdg-open index.html    # Linux
# start index.html       # Windows
```

To serve it locally instead:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Publishing to GitHub Pages

1. Push this repository to GitHub.
2. Go to **Settings → Pages**.
3. Under **Source**, choose **Deploy from a branch**.
4. Select branch `main` and folder `/ (root)`, then **Save**.
5. Wait a minute or two. Your site appears at `https://YOUR-USERNAME.github.io/filament-spec-index/`.

Pages serves `index.html` from the repository root automatically, which is why the file is named that way.

Then update the demo link at the top of this README.

## Working offline

The only external request is to Google Fonts. Without a network the tool still works — it falls back to system fonts. To remove the dependency entirely, delete the three `<link>` tags in the `<head>` of `index.html`; the CSS already declares full fallback stacks.

## React version

`react/FilamentSpecIndex.jsx` contains the same tool as a single React component, for embedding in a larger application. It needs React 18+ and no other packages.

## Contributing

Measured test data is the most valuable contribution. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Licence

Code is [MIT](LICENSE). Material property values are drawn from publicly published manufacturer datasheets and remain the property of their respective manufacturers; they are reproduced here as factual measurements for comparison and reference.
