# 3D Rolling Coin (Top‑View Projection)

Deployed at: https://mzaiss.github.io/rolling_coin/

A single‑file HTML5/Canvas demo of a rigid disk rolling without slipping on a plane, simulated in 3D but rendered as an orthographic top‑view projection (the tilted coin appears as an ellipse). Orientation is integrated with a quaternion; world‑space angular momentum L is held constant (no external torques). The no‑slip constraint drives translation from spin.

## Files
- `index.html`: 3D coin simulation (this README describes this file)
- `coin.html`: 2D no‑slip reference variant (optional)

## Controls (in `index.html`)
- Mass m
- Radius R (px)
- Thickness h (px)
- Time Scale
- Angular Momentum components: Lx, Ly, Lz
- Initial Tilt θ (deg)
- Heading ψ (deg)
- Initial Spin Phase φ (deg)
- Buttons: Reset, Pause, Trails, Random L
- Keyboard: Space = Pause, T = Trails, R = Reset

## Physics model
- Rigid disk with principal inertias
  - I⊥ = 1/4 m R² + (1/12) m h²  (transverse, Ixx = Iyy)
  - I∥ = 1/2 m R²               (axial, Izz)
- World‑space angular momentum L is constant.
  - Compute ω_body = I^{-1} · L_body, then rotate to world: ω = R_body→world(ω_body)
- Orientation update via quaternion q
  - q̇ = 1/2 · q ⊗ [0, ω]
- No‑slip rolling on a horizontal plane (ẑ is plane normal)
  - Let n be the coin’s world normal (body z‑axis in world)
  - ŝ = normalize(ẑ − (n·ẑ) n)  (direction of contact on the rim)
  - v_cm = R · (ω × ŝ)
- Projection and rendering
  - The tilted circle (radius R) appears as an ellipse with major a = R and minor b = R · |cos θ|, where θ is the tilt from vertical.
  - Optional spokes, projected axes, and L arrow aid visualization.

## How to run locally
- Open `index.html` directly in a modern browser, or serve the folder with any static server.
- Mobile‑friendly controls; canvas fills the window.

## Notes
- Position wraps around the viewport to keep the coin on screen.
- A small reference panel draws example projected coins for sanity checks.
- The `coin.html` page shows a simpler 2D no‑slip model (ω = L/I with I = 1/2 m R², v = R·ω).

## License
See `LICENSE`.
