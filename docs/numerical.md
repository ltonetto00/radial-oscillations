# Numerical methods

## Pipeline

For each user input (central density ρ\_c, mode number n):

1. **Solve TOV** outward from r → 0 → r = R (where p = 0).
2. **Resample** the background onto a uniform grid in r ∈ [0, R] (N = 800
   points).
3. **Integrate ν(r)** inward from the surface using the boundary condition
   e<sup>ν(R)</sup> = 1 − 2M/R.
4. **Scan ω-space**: integrate the GHZ system for ~2500 trial values of ω on
   a logarithmic grid in [10⁻⁴, 0.6] (geometrized, km⁻¹) and record the
   surface value Δp(R, ω).
5. **Bisect** on each sign change of Δp(R, ω) to refine each eigenvalue
   to relative precision ~10⁻¹².
6. **Re-integrate** the GHZ system with the n-th eigenvalue and store the
   full eigenfunctions ξ(r), Δp(r) for plotting and animation.

## TOV integration

A classical RK4 with adaptive step is used. The step size is reduced near the
surface to handle the rapid pressure drop:

| p / p\_c | step h (km) |
|---|---|
| > 0.5 | 5 × 10⁻⁴ |
| 0.1 – 0.5 | 3 × 10⁻⁴ |
| 0.01 – 0.1 | 1 × 10⁻⁴ |
| < 0.01 | 3 × 10⁻⁵ |

The integration starts at r\_min = 10⁻⁶ km (effectively zero) with seed values
p(r\_min) = p\_c and m(r\_min) = (4π/3) ρ\_c r\_min³. The final step is taken
as a fractional Euler step to land exactly on p = 0.

## GHZ integration

The system is integrated with a classical RK4. To handle the singular term
1/r in eq. (11) at the origin, we start at the first nonzero grid point r₁
with initial values:

- ξ(0) = 1 (normalization)
- Δp(0) = −3 ξ(0) Γ(0) p(0)   (regularity at center)

The choice of normalization is arbitrary — the eigenfunctions are scaled to
max|ξ| = 1 and max|Δp| = 1 for display *after* the integration.

## Eigenvalue search

The strategy is:

1. **Coarse scan** over a logarithmic grid of ω. For each ω, integrate the GHZ
   system and record the surface boundary condition residual Δp(R).
2. **Detect sign changes** in the residual. Each sign change is one
   eigenvalue.
3. **Bisect** the bracketing interval [ω<sub>−</sub>, ω<sub>+</sub>] until
   |ω<sub>+</sub>/ω<sub>−</sub> − 1| < 10⁻¹².

For each new ρ\_c, the eigenvalues for all modes (n = 0, 1, 2, 3) are
computed and cached, so switching modes is instant.

## Why a coarse scan is needed

The boundary condition Δp(R, ω) is *not* a monotonic function of ω. Between
each pair of consecutive eigenvalues, it has at least one extremum. A naive
bisection from a single initial bracket would converge to whichever eigenvalue
happens to bracket the sign change, missing the others or returning them in
the wrong order. The coarse scan locates *all* sign changes and identifies
which is the n-th.

## Sources of error

1. **TOV interpolation** — the GHZ coefficients are evaluated on the uniform
   N = 800 grid, which involves linear interpolation of the adaptively-spaced
   TOV solution. Switching to higher-order interpolation (cubic spline) would
   help at very high ρ\_c where the EOS curvature is largest.
2. **Step size at the surface** — pressure approaches zero, and several
   coefficients in eq. (12) (notably 1/(p+ρ)) become numerically delicate.
   The bisection converges to a slightly density-dependent precision.
3. **Polytropic Γ approximation** — for a true relativistic polytrope, the
   "frozen" adiabatic index Γ\_frozen and the EOS index Γ\_EOS coincide (as
   discussed in GHZ §6), so this is not an issue here. For a tabulated
   realistic EOS, the distinction matters and must be computed separately.

## Benchmarking

The result at ρ\_c = 1 × 10¹⁵ g/cm³ (n = 1 polytrope, K = 100 km²) is
compared against Kokkotas & Ruoff (2001), Table A.18:

| Quantity | This code | K&R 2001 | Δ |
|---|---:|---:|---:|
| R | 10.81 km | 10.81 km | 0.0% |
| M | 0.802 M☉ | 0.802 M☉ | 0.0% |
| f₀ | 2.15 kHz | 2.150 kHz | ~0% |
| f₁ | 5.01 kHz | 5.007 kHz | ~0% |
| f₂ | 7.39 kHz | 7.394 kHz | ~0% |

Equivalent agreement is observed at other rows of the table.
