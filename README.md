# Relativistic Radial Oscillations of a Polytropic Neutron Star

An interactive, browser-based visualization of the lowest radial pulsation modes
of a relativistic polytropic neutron star, solving the Gondek–Haensel–Zdunik
(1997) system of linearized perturbation equations.

![screenshot](docs/screenshot.png)

## Live demo

Open `index.html` in any modern browser, or deploy via Vercel
(see [Deployment](#deployment)).

## What the application does

For a given central density ρ\_c and a polytropic equation of state
p = K ρ<sup>1+1/n</sup> (defaults: n = 1, K = 100 km²):

1. Integrates the **Tolman–Oppenheimer–Volkoff (TOV) equations** in geometrized
   units to obtain the static stellar background — pressure p(r), mass-energy
   density ρ(r), enclosed mass m(r), and the metric potentials ν(r), λ(r).
2. Solves the **Gondek–Haensel–Zdunik (GHZ) system** (eqs. 11–12 of Gondek,
   Haensel & Zdunik 1997) for the relative radial Lagrangian displacement
   ξ ≡ Δr/r and the Lagrangian pressure perturbation Δp.
3. Uses a **shooting + bisection** method to locate the n = 0, 1, 2, 3
   eigenfrequencies ω<sub>n</sub> satisfying the boundary conditions
   (Δp)<sub>center</sub> = −3(ξΓp)<sub>center</sub> and (Δp)<sub>surface</sub> = 0.
4. Converts ω back to physical units (s⁻¹) and animates the resulting motion
   in both **2D (equatorial slice)** and **3D (interactive WebGL sphere with
   equatorial section)** views.

The color map encodes the Lagrangian density perturbation δρ ∝ Δp · cos(ωt):
**blue = compression**, **orange = rarefaction**.

## Controls

- **View** — toggle between 2D equatorial slice and 3D rendering (click-drag to
  rotate in 3D).
- **Radial mode n** — select the fundamental (n=0) or one of the first three
  overtones. Mode n has n internal nodes in both ξ(r) and Δp(r).
- **log ρ\_c** — central density on a log scale. Changes M, R, and the
  eigenvalues. The whole TOV + eigenvalue problem is re-solved on each change.
- **Amplitude ε** — display-only multiplier on ξ. The radial displacement
  shown in the animation is Δr(r, t) = ε · ξ(r) · r · cos(ωt). The linearized
  equations are valid for ε → 0; ε is enlarged purely for visualization.
- **Speed** — playback rate multiplier (the physical period is sub-ms, so the
  animation runs ~20× faster than real time to be perceptible).

## Validation

Benchmark against Kokkotas & Ruoff (2001), Table A.18 (polytrope n = 1,
K = 100 km², zero-temperature):

| ρ\_c (g/cm³) | R (km) | M (M☉) | f₀ (kHz) | f₁ (kHz) | f₂ (kHz) |
|---:|---:|---:|---:|---:|---:|
| 1.0 × 10¹⁵ | 10.81 | 0.802 | 2.150 | 5.007 | 7.394 |
| 2.0 × 10¹⁵ | 9.673 | 1.126 | 2.323 | 6.237 | 9.295 |
| 5.0 × 10¹⁵ | 7.787 | 1.348 | 1.129 | 7.475 | 11.365 |

Reproducing these values is the convergence test for the code.

## Documentation

- [`docs/physics.md`](docs/physics.md) — equations, derivation notes,
  unit conventions
- [`docs/numerical.md`](docs/numerical.md) — integration scheme, eigenvalue
  search algorithm, boundary conditions

## References

1. **Gondek, D., Haensel, P., & Zdunik, J. L.** (1997).
   *Radial pulsations and stability of protoneutron stars.*
   Astronomy & Astrophysics, **325**, 217.
   [arXiv:astro-ph/9705157](https://arxiv.org/abs/astro-ph/9705157)
2. **Kokkotas, K. D., & Ruoff, J.** (2001).
   *Radial oscillations of relativistic stars.*
   Astronomy & Astrophysics, **366**, 565.
   [arXiv:gr-qc/0011093](https://arxiv.org/abs/gr-qc/0011093)
3. **Chandrasekhar, S.** (1964).
   *The dynamical instability of gaseous masses approaching the Schwarzschild
   limit in general relativity.* ApJ, **140**, 417.
4. **Chanmugam, G.** (1977).
   *Radial oscillations of zero-temperature white dwarfs and neutron stars
   below nuclear densities.* ApJ, **217**, 799.
5. **Tolman, R. C.** (1939), **Oppenheimer, J. R. & Volkoff, G. M.** (1939) —
   the TOV equations.

## Deployment

This is a single static HTML file with one CDN dependency (Three.js). No build
step is required.

### Local

```bash
git clone https://github.com/<your-username>/stellar-oscillations.git
cd stellar-oscillations
# just open index.html in your browser:
open index.html      # macOS
xdg-open index.html  # Linux
start index.html     # Windows
```

### Vercel

Push the repo to GitHub, then in the [Vercel dashboard](https://vercel.com/new):
import the GitHub repo and accept all defaults (no framework, no build command,
no output directory). Vercel will serve `index.html` from the root.

Or, from the CLI:

```bash
npm i -g vercel
vercel
```

The included `vercel.json` is optional — Vercel auto-detects static sites — but
it's included so you can pin caching headers and rewrites if you extend the
project.

## License

MIT — see [`LICENSE`](LICENSE).
