# Linear Map Visualizer

An interactive tool for seeing what a matrix does to the plane, styled to
match [hwGenie](https://github.com/tghyde/hwgenie) course sites and the
[Row Reducer](https://github.com/tghyde/rowreducer).

**Status:** in development; not yet linked from the Math 221 course page.

## Features

- Choose the matrix size m × n (2 or 3 each). The 2 × 2 case is complete;
  3-dimensional pictures are planned.
- Matrix entry follows the Row Reducer conventions: a default `0` clears
  when you click into it, arrow keys move between cells, Enter maps.
  Entries can be integers, fractions (`1/2`), decimals, or small
  expressions (`sqrt(3)/2`, `2pi`, `1 - sqrt(2)`); they display as
  typeset LaTeX after **Map**.
- Example buttons fill in a fresh rotation, dilation, reflection, shear,
  projection onto a line, or random integer matrix, with a one-line
  description of the map.
- Domain and codomain side by side. Toggles: a figure, the unit circle,
  a lattice spanned by two draggable vectors *u* and *v* (their ℤ²-span,
  with lines colored by direction), the standard basis with the unit
  square, and the background grid. Each view has zoom buttons.
- Drag the tips of *u* and *v* in the domain (snaps to half-units; hold
  Shift for free movement). Singular maps collapse the lattice to a line.
- The URL hash encodes the whole state (matrix, toggles, vectors, zoom),
  so **Copy link** reproduces a picture.

## Implementation notes

Everything is one dependency-free `index.html` (KaTeX from CDN is the only
external resource), deployed to GitHub Pages with no build step.

- Entries parse to a tiny AST (`parseExpr`) that yields both a float
  (for drawing) and LaTeX (for display).
- Figures and circles are drawn in math coordinates inside an SVG `<g>`
  whose `transform` is the matrix composed with the pixel scaling, with
  `vector-effect="non-scaling-stroke"` so outlines stay crisp. The
  lattice, arrows, and unit square are computed explicitly so they can be
  clipped to the window and keep fixed-size arrowheads.
- To change the default figure, edit the `FIGURE` string (SVG markup in
  math coordinates, y up).
- Theme, fonts, and flat card styling come from hwGenie's `slate` theme;
  the light/dark toggle shares the `hwg-theme` localStorage key with the
  course sites.

## Local preview

```bash
python3 -m http.server 8738
```

then open http://localhost:8738/.
