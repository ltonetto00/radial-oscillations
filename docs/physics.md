# Physics & equations

## 1. Conventions

We use **geometrized units** internally: G = c = 1, with all lengths in
kilometers. In this system:

| Quantity | Geometrized → CGS factor |
|---|---|
| Length | 1 km ≡ 1 km = 10⁵ cm |
| Time | t (km) × (km / c) = t × 3.336 × 10⁻⁶ s |
| Mass | M (km) × (c² · km / G) = M × 1.347 × 10²⁸ g |
| Mass-energy density | ρ (km⁻²) × (c² / (G · km²)) = ρ × 1.347 × 10¹⁸ g/cm³ |
| Pressure | same factor as density |

The metric convention follows Gondek–Haensel–Zdunik (1997):

$$ds^2 = e^{\nu} c^2 dt^2 - e^{\lambda} dr^2 - r^2 (d\theta^2 + \sin^2\theta\, d\phi^2)$$

Note this differs from the Kokkotas–Ruoff (2001) convention where the metric
exponents are 2ν and 2λ. We use the GHZ convention throughout.

## 2. Equation of state

A polytrope:

$$p = K \rho^{1 + 1/n}$$

where ρ here is the mass-energy density. Defaults: n = 1, K = 100 km². The
adiabatic index Γ entering the perturbation equations is:

$$\Gamma = \frac{p + \rho c^2}{p} \frac{dp}{d\rho} = \left(1 + \frac{1}{n}\right) \frac{p + \rho c^2}{\rho c^2}$$

In geometrized units the c² factors drop, leaving Γ = (1 + 1/n)·(p + ρ)/ρ.

## 3. Static background: TOV equations

We solve the Tolman–Oppenheimer–Volkoff system:

$$\frac{dp}{dr} = -\frac{G m \rho}{r^2 \left(1 - \frac{2Gm}{rc^2}\right)} \left(1 + \frac{p}{\rho c^2}\right) \left(1 + \frac{4\pi p r^3}{m c^2}\right)$$

$$\frac{dm}{dr} = 4\pi r^2 \rho$$

$$\frac{d\nu}{dr} = -\frac{2}{p + \rho c^2} \frac{dp}{dr}$$

with boundary conditions p(0) = p\_c (set by ρ\_c through the EOS), m(0) = 0,
and ν(R) = ln(1 − 2GM/Rc²) for continuity with the Schwarzschild exterior.

In code, we integrate outward using a 4th-order Runge–Kutta scheme with
adaptive step size near the surface (where p drops rapidly). ν is integrated
*inward* from r = R after R and M are known, using the surface BC.

The metric function λ is algebraic:

$$e^{-\lambda} = 1 - \frac{2 G m(r)}{r c^2}$$

## 4. Radial perturbation equations (GHZ 1997)

The dynamical variables are the **relative radial Lagrangian displacement**

$$\xi(r) \equiv \frac{\Delta r}{r}$$

and the **Lagrangian pressure perturbation** Δp. Both have harmonic time
dependence ∝ e<sup>iωt</sup>.

### Eq. (11) of Gondek–Haensel–Zdunik (1997):

$$\frac{d\xi}{dr} = -\frac{1}{r}\left(3\xi + \frac{\Delta p}{\Gamma p}\right) - \frac{dp/dr}{p + \rho c^2}\, \xi$$

### Eq. (12) of Gondek–Haensel–Zdunik (1997):

$$\frac{d\Delta p}{dr} = \xi \left\{ \frac{\omega^2}{c^2} e^{\lambda - \nu} (p + \rho c^2) r - 4 \frac{dp}{dr} \right\}$$
$$+ \xi \left\{ \frac{(dp/dr)^2 r}{p + \rho c^2} - \frac{8\pi G}{c^4} e^{\lambda} (p + \rho c^2) p\, r \right\}$$
$$+ \Delta p \left\{ \frac{dp/dr}{p + \rho c^2} - \frac{4\pi G}{c^4} (p + \rho c^2) r\, e^{\lambda} \right\}$$

### Boundary conditions

**At the center** (r → 0), regularity of eq. (11) requires the coefficient
of 1/r to vanish:

$$(\Delta p)_{\text{center}} = -3 \,(\xi \Gamma p)_{\text{center}}$$

We normalize ξ(0) = 1.

**At the surface** (r = R, where p = 0), the Lagrangian pressure perturbation
must vanish:

$$(\Delta p)_{\text{surface}} = 0$$

This is the eigenvalue condition: only discrete ω<sub>n</sub> satisfy it.

## 5. Eigenvalue problem

The system (11)–(12) with the two boundary conditions is a Sturm–Liouville
eigenvalue problem with a discrete, ordered spectrum:

$$\omega_0^2 < \omega_1^2 < \omega_2^2 < \dots$$

The n-th eigenfunction ξ<sub>n</sub>(r) has exactly **n internal nodes** in
the interval 0 < r < R. The fundamental mode n = 0 has no nodes; the first
overtone n = 1 has one node, and so on.

The eigenfunction Δp<sub>n</sub>(r) also has n nodes.

If ω²<sub>0</sub> < 0, the star is dynamically unstable — radial perturbations
grow exponentially in time. This occurs for ρ\_c > ρ\_critical corresponding
to the maximum-mass configuration in the M(ρ\_c) curve.

## 6. Animation

For a finite (display-only) amplitude ε, the radial displacement is:

$$\Delta r(r, t) = \varepsilon \cdot \xi_n(r) \cdot r \cdot \cos(\omega_n t)$$

so a fluid element originally at radius r moves to

$$r(t) = r \left[ 1 + \varepsilon \, \xi_n(r) \cos(\omega_n t) \right]$$

The Lagrangian density perturbation tracks Δp through the EOS; for the
visualization we display δρ ∝ Δp(r) · cos(ωt) as a diverging colormap.

The linearization is only valid for ε ≪ 1; values ε ≈ 0.05–0.2 give clear
visualization while remaining in the linear regime.

## 7. References

- Gondek, D., Haensel, P., & Zdunik, J. L. 1997, A&A 325, 217.
  [arXiv:astro-ph/9705157](https://arxiv.org/abs/astro-ph/9705157).
  *Source of equations (11), (12), boundary conditions, and the (ξ, Δp)
  formulation.*
- Chandrasekhar, S. 1964, ApJ 140, 417. *Original derivation of the
  relativistic radial pulsation equation.*
- Chanmugam, G. 1977, ApJ 217, 799. *Equivalent first-order rewriting.*
- Kokkotas, K. D., & Ruoff, J. 2001, A&A 366, 565.
  [arXiv:gr-qc/0011093](https://arxiv.org/abs/gr-qc/0011093).
  *Comparison/reference tables of eigenfrequencies, including Table A.18
  used here as a benchmark.*
- Misner, C. W., Thorne, K. S., & Wheeler, J. A. 1973, *Gravitation*,
  W. H. Freeman.
