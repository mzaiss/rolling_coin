
# Deployed at
https://mzaiss.github.io/rolling_coin/

# Galaxy/Grid N‑Body Playground (Leapfrog)

A single‑file HTML5/Canvas simulator that displays a 2D grid of interacting particles (“planets”) integrated with a symplectic leapfrog method. The UI lets you tune the force law and time scaling in real time.

## UI
- **Control panel**: Fixed, minimizable panel with responsive layout.
  - **Force Law** selector: `1/r³`, `1/r²`, `1/r`, `log(r)` (default), `1` (constant), `√r`, `r`, `r²`, `r³`.
  - **Sliders**: `G` (force strength), `Time Scale`, `Initial Angular Momentum`.
  - **Buttons**: `Reset`, `Pause`, `Random All` (randomizes positions/velocities within the original grid bounds), `Trails` (toggles persistent trails by skipping canvas clear).
- **Mobile friendly**: Scales fonts/spacing; supports touch for camera controls.

## Visualization
- **Canvas**: Full‑screen `canvas` is appended to `body` and used for all rendering.
- **Particles**: 20×20 grid initialized near screen center (shifted ~20% down). Radius ≈ 3 px.
  - Initial tangential velocity set from the chosen angular momentum slider to create rotation about the shifted center.
  - **Color**: HSL hue/lightness derived from each particle’s initial radial distance to center for a radial color gradient.
- **Trails**: When enabled, the canvas is not cleared each frame and particles are drawn with partial alpha to leave faded trajectories.
- **Camera**: Pan/zoom via touch gestures (pinch to zoom with gentle sensitivity and clamped zoom; camera transforms applied around a fixed “galaxy center”).

## ODE and Integrator
- **State per particle**: position `(x, y)`, velocity `(vx, vy)`, `mass`, `radius`.
- **Forces**: Pairwise central force along the separation vector with magnitude `G * f(r)`:
  - `f(r)` determined by the selected force law; a small‑r clamp avoids singularities; a short‑range cutoff (`r > 4 * radius`) avoids extreme near‑field forces.
  - A frame‑wise **normalization factor** is computed from the net force on one reference particle and used to scale `G` for stability across laws.
- **Time stepping**: Fixed step `dt ≈ 0.016 * TimeScale` seconds.
  - **Leapfrog (kick–drift–kick)**: half‑step velocity update from forces, full position update, second half‑step velocity update. This is a symplectic scheme that better preserves qualitative behavior than simple Euler.
- **Open domain**: No boundaries or wall bounces; particles can drift off‑screen.

## Main entry points
- `initializePlanets()` sets up the grid, colors, and initial tangential velocities.
- `calculateForce(r, forceLaw)` returns the scalar force magnitude per law.
- `updatePlanets(dt)` computes pairwise forces and applies the leapfrog update.
- `drawPlanets()` applies camera transforms and renders particles (with or without trails).
- Event handlers wire UI changes and touch camera controls; `animate()` drives the frame loop via `requestAnimationFrame`.

This summarizes the current `index.html` implementation’s UI, rendering, and ODE/leapfrog integration. The next step is to replace it with the new physics problem (no planets).

