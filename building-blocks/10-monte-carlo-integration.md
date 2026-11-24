# 10. Monte Carlo integration

- **Layer:** Monte Carlo
- **Depends on:** [06. Expectation, variance and limit theorems](06-expectation-and-limit-theorems.md), [08. Bayesian posterior as the target density](08-bayesian-posterior-as-target.md)
- **Used by:** [13. Convergence and ergodicity of Markov chains](13-convergence-and-ergodicity.md), [18. MCMC output analysis](18-mcmc-diagnostics.md)

## Role in HMC

Monte Carlo integration replaces an integral over parameter space by an average over random draws. It reduces the computation of posterior expectations to the generation of draws from the target. The failure of direct sampling methods in high dimension is the reason for constructing Markov chains.

## Required knowledge

- **Estimator.** For independent draws $q_1,\dots,q_N$ from $\pi$, the estimator $\hat f_N = \frac1N\sum_{n=1}^N f(q_n)$ is unbiased for $\mathbb{E}_\pi[f]$.
- **Error.** Since the draws are independent, $\operatorname{Var}[\hat f_N] = \operatorname{Var}_\pi[f]/N$. The standard error is therefore $\sqrt{\operatorname{Var}_\pi[f]/N}$, which decreases as $1/\sqrt N$ and does not depend on the dimension $d$. By the central limit theorem, $\hat f_N$ is approximately Gaussian around $\mathbb{E}_\pi[f]$.
- **Remaining problem.** The dimension enters only through the difficulty of generating draws from $\pi$ when only $\tilde\pi$ can be evaluated.
- **Inverse-distribution-function and grid sampling.** These methods require the normalized distribution function and are restricted to one or a few dimensions.
- **Rejection sampling.** A draw $q$ from a proposal density $g$ with $\tilde\pi\le c\,g$ is accepted with probability $\tilde\pi(q)/(c\,g(q))$. The acceptance rate is proportional to $1/c$. For a Gaussian target and a slightly wider Gaussian proposal, $c$ grows exponentially with $d$.
- **Importance sampling.** Draws from $g$ receive weights $w_n = \tilde\pi(q_n)/g(q_n)$, and the estimator is $\sum_n w_n f(q_n)/\sum_n w_n$. In high dimension a few weights dominate the sum, and the estimator has very large variance.
- **Transition to MCMC.** Markov chain Monte Carlo abandons independence. It generates each draw by a local modification of the preceding draw, which requires only ratios of $\tilde\pi$. The draws are correlated, so the error is larger than $\sqrt{\operatorname{Var}_\pi[f]/N}$; the correction is the effective sample size (block [18](18-mcmc-diagnostics.md)).

## Not required

- Variance-reduction techniques, quasi-Monte Carlo, importance resampling and sequential Monte Carlo.

## Sources

- MacKay (2003): §29.1, eqs. (29.6)–(29.7) (estimator and its variance); §29.2, eqs. (29.21)–(29.28) (importance sampling and its failure in high dimension); §29.3, eqs. (29.29)–(29.30) (rejection sampling and its acceptance rate).
- BDA3: §10.1, eq. (10.1) (simulation estimate of an expectation); §10.3 (grid, inverse-distribution-function and rejection sampling); §10.4, eqs. (10.2)–(10.4) (importance sampling); §10.5 (accuracy of simulation estimates).
- Betancourt (2019), *The Convergence of Markov Chain Monte Carlo Methods*: §2 (exact samples versus correlated samples).
- Robert and Casella (2004), *Monte Carlo Statistical Methods*: §7.1.
