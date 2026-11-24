# 18. MCMC output analysis: warm-up, R-hat, effective sample size

- **Layer:** MCMC algorithms
- **Depends on:** [09. Typical set and high-dimensional geometry](09-typical-set-and-high-dimensional-geometry.md), [10. Monte Carlo integration](10-monte-carlo-integration.md), [13. Convergence and ergodicity of Markov chains](13-convergence-and-ergodicity.md)
- **Used by:** [37. Step-size adaptation by dual averaging](37-step-size-adaptation-by-dual-averaging.md), [38. HMC-specific diagnostics](38-hmc-diagnostics.md)

## Role in HMC

The convergence theorems are asymptotic, and their conditions cannot be verified for a practical model. The quantities of this block are the empirical checks applied to the output of any MCMC method, and they define the measure of efficiency by which HMC, NUTS and their tuning choices are compared.

## Required knowledge

- **Warm-up.** The initial segment of a chain is discarded. During this phase the chain moves from its starting point to the typical set, and the tuning parameters of the sampler are adapted. Adaptation is switched off at the end of warm-up.
- **Multiple chains.** Several chains are run from dispersed starting points. Agreement between chains is evidence that each chain has reached the same distribution.
- **Potential scale reduction $\hat R$.** For $m$ chains of length $n$, let $W$ be the mean of the within-chain variances and $B$ be $n$ times the variance of the chain means. With $\widehat{\operatorname{var}}^{+} = \frac{n-1}{n}W + \frac1n B$, the statistic is $\hat R = \sqrt{\widehat{\operatorname{var}}^{+}/W}$. It is computed on chains split into halves. Values close to 1 are required; the sources state thresholds of 1.1 (BDA3) and 1.01 (Vehtari et al., as reported by Murphy).
- **Autocorrelation.** $\rho_t$ is the correlation between $f(q_n)$ and $f(q_{n+t})$ in a stationary chain.
- **Effective sample size.** $\mathrm{ESS} = N\big/\big(1+2\sum_{t\ge1}\rho_t\big)$ for $N$ draws. It is the number of independent draws that would give the same estimator variance.
- **Monte Carlo standard error.** $\mathrm{MCSE} = \sqrt{\operatorname{Var}_\pi[f]/\mathrm{ESS}}$. This expression replaces the $1/\sqrt N$ error of independent draws (block [10](10-monte-carlo-integration.md)).
- **Efficiency of a sampler.** Samplers are compared by effective sample size per unit of computation. For HMC and NUTS the unit is one gradient evaluation, since one transition costs as many gradient evaluations as it has integrator steps.
- **Limits of diagnostics.** A satisfactory value of every diagnostic is necessary for a valid estimate and is not sufficient.

## Not required

- The details of estimating $\rho_t$ (variogram estimate, truncation rules) and rank-normalized or tail variants of $\hat R$ and ESS.
- Mixing-time bounds, eigenvalue gaps and conductance.

## Sources

- BDA3: §11.4, eqs. (11.3)–(11.4) (warm-up, multiple chains, $\hat R$); §11.5, eqs. (11.5)–(11.8) (effective sample size).
- Murphy (2025): §12.6, eqs. (12.97)–(12.100) ($\hat R$) and (12.107)–(12.110) (effective sample size).
- Betancourt (2017): §2.2 (Monte Carlo standard error and effective sample size); §2.3 and §6.3 ($\hat R$; diagnostics are necessary and not sufficient).
- Hoffman and Gelman (2014): §4 and Appendix A (effective sample size per gradient evaluation as the efficiency measure).
