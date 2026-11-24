# 09. Typical set and high-dimensional geometry

- **Layer:** Probability
- **Depends on:** [06. Expectation, variance and limit theorems](06-expectation-and-limit-theorems.md), [07. Multivariate Gaussian distribution](07-multivariate-gaussian-distribution.md)
- **Used by:** [15. Random-walk Metropolis and its scaling](15-random-walk-metropolis-and-its-scaling.md), [18. MCMC output analysis](18-mcmc-diagnostics.md)

## Role in HMC

In high dimension the probability mass of a distribution is not located where the density is largest. It is located in a thin region called the typical set. This fact explains why grid integration and mode-based approximations fail, why random-walk proposals are rejected or short, and why HMC is designed to follow trajectories that move along the typical set and not toward the mode.

## Required knowledge

- **Mass is density times volume.** The contribution of a region to an expectation is $\pi(q)\,dq$. A region of high density contributes little when its volume is small.
- **Growth of volume.** In $d$ dimensions the volume between radius $r$ and $r+dr$ around a point is proportional to $r^{d-1}\,dr$. For large $d$ almost all volume in a neighborhood of the mode lies far from the mode.
- **Typical set.** The density decreases away from the mode while the volume increases. The product is non-negligible only in an intermediate region, the typical set. Expectations are determined by this region.
- **Gaussian case.** For $x\sim N(0,I_d)$, $\lVert x\rVert^2\sim\chi^2_d$. Hence $\lVert x\rVert$ is close to $\sqrt d$, with a standard deviation that remains of order 1 as $d$ grows. The draws lie in a thin spherical shell of radius $\sqrt d$, and the mode at the origin lies outside the shell.
- **Concentration of the log density.** On the typical set of this Gaussian, $-\log\pi(x)$ equals $\tfrac12\chi^2_d$ plus a constant; its mean is $d/2$ and its standard deviation is $\sqrt{d/2}$. A sampler must therefore reach density values far below the maximum.
- **Consequences for sampling.** A grid with $n$ points per dimension costs $n^d$ evaluations. An isotropic random perturbation of a point in the typical set leaves the typical set with high probability unless it is small (block [15](15-random-walk-metropolis-and-its-scaling.md)). The gradient of the log density points toward the mode and not along the typical set; a momentum is added in HMC so that trajectories move along the typical set (block [27](27-phase-space-target-and-momentum-resampling.md)).

## Not required

- Formal concentration inequalities.
- The information-theoretic definition of the typical set through the asymptotic equipartition property.

## Sources

- Betancourt (2017): §1.3 (growth of volume with dimension); §1.4 (typical set, concentration of measure and cost of quadrature); §3.1 (the gradient points toward the mode).
- MacKay (2003): §29.1, eqs. (29.18)–(29.19) (number of uniform samples required to reach the typical set; the passage uses the entropy-based definition of the typical set).
