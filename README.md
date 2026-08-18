# Battery Data Visualizer

A browser-based viewer for BioLogic EC-Lab `.mpt` files. Open one or more raw exports and get
interactive impedance plots, per-cycle trends, a correlation matrix, and an Excel export. The
whole thing is a single HTML file, so there is nothing to install.

From the [Rolston Lab](https://github.com/rolston-lab-asu) at ASU, which works across battery
and solar energy materials. This tool covers the battery side: potentiostat and cycler data.

![The Graph tab: Nyquist impedance of a Molicel 21700 cell, coloured by cycle age](docs/screenshot-graph.png)

<sub>Nyquist view of one cell over 20 cycles. Solid lines are the diffusion-limited (Warburg)
branch, dotted lines the kinetic arc, coloured early to late by cycle number.</sub>

## Quick start

1. Download `Rolston_BatteryViewer_V1.1.html` (Code → Download ZIP, or clone the repo).
2. Double-click it. It opens in your default browser.
3. Click Open, or drag `.mpt` files onto the page.

Use `Rolston_BatteryViewer_V1.1.html`. `Rolston_BatteryViewer_V1.html` is the original release,
kept for reference and for reproducing older figures.

Note that you need an internet connection. Plotly and SheetJS load from a CDN when the page
opens, so offline the page loads but charts will not render.

## What it does

You can load any number of `.mpt` files at once. Each is parsed in the browser, and every tab
works across the whole loaded set.

**Graph** gives interactive Plotly charts with zoom, pan, and hover readouts:

- Nyquist, Bode, and Black impedance plots (EIS)
- 3D discharge curve
- Within-cycle profiles, against either time in cycle or capacity
- Custom, where you pick the X and Y columns yourself

You control what is drawn with a row filter (all rows, EIS only, or DC cycling only), a cycle
selection (full range, every Nth sweep, an evenly spread set of slices, or a typed list of
cycle numbers), a colour-by option (cycle age, sweep, protocol step `Ns`, or source file), and
a layout (one battery, stacked panels, or everything overlaid on shared axes).

**Trends** plots per-cycle behaviour across a run, either as a line against cycle number or as
box plots showing the spread within each cycle. This is where capacity fade, coulombic
efficiency, and resistance growth show up.

**Correlation** draws a Pearson correlation matrix as a heatmap, so you can see which measured
columns move together. It can be filtered to EIS rows, non-EIS rows, or both.

**Export** writes the parsed and filtered data to `.xlsx`, including a column selection, so you
can hand a clean sheet to Excel, Python, or MATLAB rather than re-parsing raw `.mpt` headers.

**File info** shows the EC-Lab header metadata, and **Glossary** explains what each column
means. One point from the glossary is worth repeating here, because it catches people out:

> Rows are logged on events, not on a fixed clock. A new row is written whenever a set time
> passes, or voltage / current / charge moves by a set amount. Stable plateaus produce few
> rows; fast changes produce many. Every row carries a cycle number (one charge plus one
> discharge) and an `Ns` protocol-step index. EIS rows have `freq/Hz > 0` and DC cycling rows
> have `freq/Hz = 0`, so most impedance columns read `0` on cycling rows.

## Your data stays local

The viewer makes no network calls. Files are read with the browser's `FileReader` API and
parsed in memory, so nothing is uploaded, logged, or transmitted. The only outbound requests
the page makes are for the two CDN libraries mentioned above.

## Input format

It accepts `.mpt` and `.txt` BioLogic EC-Lab text exports, several at a time. The EC-Lab header
block (`Nb header lines : ...`) is parsed automatically, so you do not need to strip it first.

## Version history

**V1.1** added multi-battery comparison (stacked panels and overlay layouts), cycle-age colour
gradients, colour-by-protocol-step, typed cycle lists, and a choice of X-axis for within-cycle
profiles.

**V1** was the original release shared to Discord, focused on a single battery.

## Repository

```
Rolston_BatteryViewer_V1.html      original release
Rolston_BatteryViewer_V1.1.html    current version
docs/screenshot-graph.png          screenshot used above
```

Both HTML files are self-contained, with all HTML, CSS, and JavaScript in one document. To
change anything, open the file in a text editor. There is no build step or toolchain.

## Credits

Written by [Isaiah Milkey](https://github.com/Isaiah-Milkey) and originally developed at
[Isaiah-Milkey/MPT_FileViewer](https://github.com/Isaiah-Milkey/MPT_FileViewer), which remains
the upstream repository. The full commit history and authorship are preserved here.

Built on [Plotly.js](https://plotly.com/javascript/) and [SheetJS](https://sheetjs.com/).

## License

Released under the [MIT License](LICENSE): free to use, modify, and redistribute, with
attribution and without warranty.

Copyright © 2026 Rolston Lab, Arizona State University. The original code was authored by
Isaiah Milkey; please keep that attribution intact in any derivative work.
