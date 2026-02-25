# Simplex-Algorithm-Visualizer

An interactive, animated visualization of the **simplex method** for 2D linear programs — fully editable, running entirely in the browser with no dependencies.

![Simplex Visualizer](https://img.shields.io/badge/type-HTML%20%2F%20Vanilla%20JS-blue) ![License](https://img.shields.io/badge/license-GNU-green)

---

## What it does

The tool lets you define a linear program (LP) in two variables and watch the simplex algorithm solve it step by step. On each step you can see:

- The **feasible region** (shaded polygon) formed by your constraints
- **Isoprofit lines** rendered as a color gradient from purple (low z) to yellow (high z), with filled bands between them showing the objective landscape
- A **∇z arrow** indicating the direction of improvement
- The **current vertex** the algorithm is visiting (orange glow)
- The **simplex path** traced across vertices with animated arrows
- **Neighbor rejection marks** (✗) at the optimality check step
- A live **vertex panel** listing all BFS vertices and their objective values

---

## Getting started

No installation needed. Just open `simplex_animation.html` in any modern browser.

```bash
open simplex_animation.html       # macOS
start simplex_animation.html      # Windows
xdg-open simplex_animation.html   # Linux
```

---

## How to use it

### 1 — Define the objective function

At the top of the left panel, set the coefficients for the objective:

```
max z = [cx] x + [cy] y
```

Both coefficients can be any real number (positive, negative, or zero).

### 2 — Add or edit constraints

Each constraint has the form `ax + by ≤ c`. You can:

- Edit any of the three coefficients `a`, `b`, `c` directly in the input fields
- Click **+ Add Constraint** to append a new row
- Click **✕** on any row to remove it

> **Note:** Non-negativity (`x ≥ 0`, `y ≥ 0`) is no longer assumed automatically. If your problem requires it, add the constraints explicitly as `-1x + 0y ≤ 0` and `0x - 1y ≤ 0`.

### 3 — Set the plot range

Below the constraints, set the x and y axis bounds independently:

```
x ∈ [xmin, xmax]
y ∈ [ymin, ymax]
```

Both bounds can be negative. This controls what portion of the plane is drawn — it does not affect the LP itself.

### 4 — Solve

Click **▶ Solve & Animate**. The algorithm runs instantly and the animation resets to step 0.

### 5 — Step through the animation

| Button | Action |
|---|---|
| **◀ Prev** | Go back one step |
| **Next ▶** | Advance one step |
| **▶▶ Auto** | Play through automatically (click again to pause) |
| **↺ Reset** | Return to step 0 |

---

## How the solver works

The solver is a pure JavaScript implementation of **vertex-walking simplex** for 2D LPs.

1. **Vertex enumeration** — all feasible basic feasible solutions (BFS) are found by intersecting every pair of constraint boundary lines and checking feasibility against all other constraints.
2. **Starting point** — the algorithm begins at the vertex with the lowest objective value (worst BFS), simulating a cold start.
3. **Pivoting** — at each step, the algorithm finds the adjacent vertex (sharing a constraint boundary edge) with the greatest improvement in `z`. It moves there.
4. **Optimality** — when no adjacent vertex improves `z`, the current vertex is declared optimal. All neighbors are highlighted with ✗ to show that no improving direction exists.

The algorithm is guaranteed to terminate for bounded, non-degenerate feasible regions.

---

## Example programs to try

### Classic resource allocation (default)
```
max z = 3x + 2y
x  +  y ≤  8
2x +  y ≤ 12
x  + 2y ≤ 12
-x      ≤  0    (x ≥ 0)
    -y  ≤  0    (y ≥ 0)
```

### Negative variable range
```
max z = x + 2y
x  + y  ≤  4
-x + y  ≤  2
x  - y  ≤  2
Plot: x ∈ [-3, 6], y ∈ [-3, 6]
```

### Minimization (negate the objective)
To minimize `c·x`, maximize `-c·x` by flipping the sign of both coefficients.

---

## File structure

```
simplex_animation.html   — the entire application (single self-contained file)
README.md                — this file
```

All logic (LP editor, geometry, simplex solver, canvas rendering) lives in a single HTML file with no external dependencies beyond a Google Fonts import for typography.

---

## Browser compatibility

Works in any browser with Canvas 2D and ES6 support:

- Chrome / Edge 80+
- Firefox 75+
- Safari 14+

---

## Limitations

- **2D only** — the visualizer is designed specifically for LPs with two decision variables.
- **Bounded regions only** — unbounded feasible regions (where z → ∞) will not produce a solution. Add bounding constraints to cap the region.
- **Non-degenerate assumption** — degenerate cases (three or more constraints meeting at a single vertex) may cause the solver to cycle or produce unexpected paths.
- **No sensitivity analysis** — the tool shows the simplex path but does not compute dual values, ranging, or shadow prices.

---

## License

MIT — free to use, modify, and share.
