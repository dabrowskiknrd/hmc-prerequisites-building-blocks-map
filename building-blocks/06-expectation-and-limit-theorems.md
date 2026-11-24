# 06. Expectation, variance and limit theorems

- **Layer:** Probability
- **Depends on:** [04. Probability densities](04-probability-densities.md)
- **Used by:** [07. Multivariate Gaussian distribution](07-multivariate-gaussian-distribution.md), [08. Bayesian posterior as the target density](08-bayesian-posterior-as-target.md), [09. Typical set and high-dimensional geometry](09-typical-set-and-high-dimensional-geometry.md), [10. Monte Carlo integration](10-monte-carlo-integration.md), [13. Convergence and ergodicity of Markov chains](13-convergence-and-ergodicity.md), [15. Random-walk Metropolis and its scaling](15-random-walk-metropolis-and-its-scaling.md), [26. Energy error and the modified (shadow) Hamiltonian](26-energy-error-and-shadow-hamiltonian.md)

## Role in HMC

The purpose of MCMC is the numerical evaluation of expectations with respect to the target density. The law of large numbers and the central limit theorem state that an average over draws converges to the expectation and quantify the error. The square-root growth of a sum of independent steps is the quantitative reason why random-walk samplers are slow and why HMC was introduced.

## Required knowledge

- **Expectation.** $\mathbb{E}_\pi[f] = \int f(q)\,\pi(q)\,dq$, and expectation is linear. Posterior means, variances, and probabilities of events (expectations of indicator functions) are all of this form.
- **Variance and covariance.** $\operatorname{Var}[X] = \mathbb{E}[(X-\mathbb{E}X)^2]$, $\operatorname{Cov}[X,Y] = \mathbb{E}[(X-\mathbb{E}X)(Y-\mathbb{E}Y)]$, and correlation is covariance divided by the product of standard deviations. The covariance matrix of a random vector collects the pairwise covariances.
- **Variance of a sum.** $\operatorname{Var}\big[\sum_i X_i\big] = \sum_i\sum_j \operatorname{Cov}[X_i,X_j]$. For independent variables the covariances vanish and the variances add. For correlated draws the covariance terms remain; they are the origin of the effective sample size (block [18](18-mcmc-diagnostics.md)).
- **Law of large numbers.** For independent, identically distributed $X_1,\dots,X_N$ with mean $\mu$, the sample mean $\bar X_N$ converges to $\mu$.
- **Central limit theorem.** If in addition the variance $\sigma^2$ is finite, $\bar X_N$ is approximately distributed as $N(\mu,\sigma^2/N)$ for large $N$. The standard error is $\sigma/\sqrt N$.
- **Growth of a random walk.** A sum of $n$ independent zero-mean steps, each of standard deviation $s$, has standard deviation $s\sqrt n$. The distance covered therefore grows as $\sqrt n$, whereas motion in a fixed direction covers a distance proportional to $n$.
- **Jensen's inequality (tuning theory only).** For a convex function $g$, $\mathbb{E}[g(X)]\ge g(\mathbb{E}[X])$. Applied to $\mathbb{E}[e^{-\Delta}] = 1$, the inequality gives $\mathbb{E}[\Delta]\ge 0$ for the energy error $\Delta$ (block [26](26-energy-error-and-shadow-hamiltonian.md); the result is used in block [31](31-step-size-tuning.md)).

## Not required

- Proofs of the limit theorems and the distinctions between modes of convergence.
- Characteristic functions and moment generating functions.

## Sources

- Betancourt (2017): §1.1 (expectation as an integral over parameter space); §2.2 (central limit theorem for estimators).
- MacKay (2003): eqs. (29.3)–(29.7) (expectation and estimator variance).
- Murphy (2025), *Probabilistic Machine Learning: Advanced Topics*: eqs. (12.102)–(12.105) (variance of a sum of correlated draws).
- Neal (2011): §3.3 (distance covered by a random walk); eqs. (4.17)–(4.20) (Jensen's inequality and the central limit theorem in the scaling analysis).
- BDA3: §10.5 (standard error of a simulation estimate).
