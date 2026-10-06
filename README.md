# Linear Map Visualizer

An interactive tool for seeing what a matrix does to the plane, styled to
match [hwGenie](https://github.com/tghyde/hwgenie) course sites and the
[Row Reducer](https://github.com/tghyde/rowreducer).

**Status:** in development; not yet linked from the Math 221 course page.

## Features

- Choose the matrix size m × n (2 or 3 each). All four cases work: the
  domain and codomain are drawn in 2D or 3D as their dimension requires.
- Matrix entry follows the Row Reducer conventions: a default `0` clears
  when you click into it, arrow keys move between cells, Enter maps.
  Entries can be integers, fractions (`1/2`), decimals, or small
  expressions (`sqrt(3)/2`, `2pi`, `1 - sqrt(2)`); they display as
  typeset LaTeX after **Map**.
- Example buttons fill in a fresh rotation, dilation, reflection, shear,
  projection, or random integer matrix for the current size. In 3D the
  rotation is about a coordinate axis, the reflection is across a plane,
  and the projection is onto a plane or a line; the 3 × 2 and 2 × 3 sizes
  use the 3D example composed with the standard inclusion of the plane or
  the projection that drops the z-coordinate.
- Domain and codomain side by side. Toggles: a figure (a running figure
  in 2D, a small house in 3D), the unit circle or sphere, a lattice spanned
  by *u*, *v* (and *w* in 3D) with the fundamental parallelogram or
  parallelepiped shaded and lines colored by direction, the standard basis
  with the unit square or cube, and the background grid.
- In 2D, drag the tips of *u* and *v* (hold Shift to snap to half-units)
  and drag the figure around; the codomain follows live. The vectors can
  also be typed: click their typeset form, edit, press Enter. Clicking the
  typeset matrix reopens the matrix editor.
- 2D views pan by dragging empty space and zoom about the cursor with the
  scroll wheel or trackpad (the +/− buttons zoom too); 3D views share an
  orbit camera, so dragging rotates. Double-click a view to reset it.
- The unit circle is a filled disc and the sphere a solid ball, each with
  one half red and the other amber so rotations are easy to read.
- Maps that flatten space (a 3 × 2 matrix, or a singular 3 × 3 one) draw the
  figure as a flat silhouette and the sphere as the filled ellipse it maps
  onto. Singular 2 × 2 maps collapse the lattice to a line.
- The URL hash encodes the whole state (matrix, toggles, vectors, zoom),
  so **Copy link** reproduces a picture.

## Implementation notes

Everything is one dependency-free `index.html` (KaTeX from CDN is the only
external resource), deployed to GitHub Pages with no build step.

- Entries parse to a tiny AST (`parseExpr`) that yields both a float
  (for drawing) and LaTeX (for display).
- Each view builds a list of primitives in the output space (polygons,
  polylines, arrows, a planar figure, a mesh) and hands it to a renderer:
  2D draws directly, 3D projects through an orthographic orbit camera.
  The planar figure is SVG markup drawn inside a `<g>` whose `transform`
  is the whole map (matrix, and in 3D the camera too) composed with the
  pixel scaling, so curves stay exact; the 3D figure is a polygon mesh
  drawn back to front with flat shading from the theme colours.
- The 2D figure is the `FIGURE` string (SVG markup in math coordinates,
  y up), traced from `guy.png` into `guy.svg`; the 3D figure is the
  `HOUSE` mesh (vertices and faces).
- Theme, fonts, and flat card styling come from hwGenie's `slate` theme;
  the light/dark toggle shares the `hwg-theme` localStorage key with the
  course sites.

## Local preview

```bash
python3 -m http.server 8738
```

then open http://localhost:8738/.
