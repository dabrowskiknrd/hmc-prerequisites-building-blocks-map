# 07. Multivariate Gaussian distribution

- **Layer:** Probability
- **Depends on:** [02. Linear algebra](02-linear-algebra.md), [04. Probability densities](04-probability-densities.md), [06. Expectation, variance and limit theorems](06-expectation-and-limit-theorems.md)
- **Used by:** [09. Typical set and high-dimensional geometry](09-typical-set-and-high-dimensional-geometry.md), [10. Monte Carlo integration](10-monte-carlo-integration.md), [15. Random-walk Metropolis and its scaling](15-random-walk-metropolis-and-its-scaling.md), [16. Gibbs sampling and auxiliary variables](16-gibbs-sampling-and-auxiliary-variables.md), [19. Newtonian mechanics](19-newtonian-mechanics.md), [23. Canonical distribution and energy level sets](23-canonical-distribution.md), [25. Leapfrog (Störmer–Verlet) integrator](25-leapfrog-integrator.md), [27. Phase-space target and momentum resampling](27-phase-space-target-and-momentum-resampling.md), [30. Mass matrix (Euclidean metric)](30-mass-matrix.md)

## Role in HMC

The momentum in standard HMC is drawn from a multivariate Gaussian distribution, and the kinetic energy is the negative logarithm of its density up to an additive constant. Gaussian targets are the reference model on which the stability of the integrator and the choice of the mass matrix are analyzed; the scaling of the step size with dimension is analyzed on targets with independent components, of which the product Gaussian is the standard example. The random-walk Metropolis proposal is also Gaussian.

## Required knowledge

- **Density.** $N(x\mid\mu,\Sigma) = (2\pi)^{-d/2}\,\lvert\det\Sigma\rvert^{-1/2}\exp\big(-\tfrac12 (x-\mu)^\top\Sigma^{-1}(x-\mu)\big)$. The negative log density is a quadratic form plus $\tfrac12\log\det\Sigma$ plus a constant.
- **Sampling.** If $z\sim N(0,I)$ and $\Sigma = LL^\top$, then $\mu + Lz\sim N(\mu,\Sigma)$.
- **Affine transformation.** If $x\sim N(\mu,\Sigma)$, then $Ax+b\sim N(A\mu+b,\,A\Sigma A^\top)$.
- **Marginals and conditionals.** Every marginal and every conditional of a joint Gaussian is Gaussian. For strongly correlated components the conditional variance of one component given the others is much smaller than its marginal variance. This gap limits Gibbs sampling, whose step is set by conditional scales (block [16](16-gibbs-sampling-and-auxiliary-variables.md)); random-walk Metropolis is limited by the smallest principal-axis scale of the next item (block [15](15-random-walk-metropolis-and-its-scaling.md)).
- **Geometry.** The level sets of the density are ellipsoids. Their axes are the eigenvectors of $\Sigma$, and the standard deviation along the $i$-th axis is $\sqrt{\lambda_i}$. The smallest value $\sqrt{\lambda_{\min}}$ bounds the usable step size of a sampler, and the largest value $\sqrt{\lambda_{\max}}$ is the distance that must be traversed.
- **Distribution of the quadratic form.** If $p\sim N(0,M)$ in $d$ dimensions, then $p^\top M^{-1}p\sim\chi^2_d$, the distribution of a sum of $d$ squared independent $N(0,1)$ variables, which has mean $d$ and variance $2d$. Hence the kinetic energy $\tfrac12 p^\top M^{-1}p$ has mean $d/2$ and variance $d/2$. Momentum resampling (block [27](27-phase-space-target-and-momentum-resampling.md)) and the typical set (block [09](09-typical-set-and-high-dimensional-geometry.md)) use this fact, and it underlies the energy diagnostic (block [38](38-hmc-diagnostics.md)).
- **Standard normal distribution function.** $\Phi(x) = \int_{-\infty}^x N(t\mid 0,1)\,dt$; it appears in the formula for the expected acceptance rate (block [31](31-step-size-tuning.md)).

## Not required

- The Wishart distribution, Gaussian processes, and general exponential-family theory.
- Explicit formulas for the conditional mean and covariance beyond the bivariate case.

## Sources

- Neal (2011): eq. (2.6) and §3.2 (Gaussian momentum); §4.1 (Gaussian targets under linear transformation); eq. (4.20) (use of $\Phi$).
- Betancourt (2017): §4.2.1 (Euclidean–Gaussian kinetic energy).
- Betancourt (2016), *Diagnosing Suboptimal Cotangent Disintegrations in HMC*: §3.2 ($\chi^2$ distribution of the kinetic energy).
- BDA3: §11.1 (Gibbs sampling for a bivariate normal); §12.4 (momentum distribution $N(0,M)$).
