# 13. Convergence and ergodicity of Markov chains

- **Layer:** Markov chain theory
- **Depends on:** [06. Expectation, variance and limit theorems](06-expectation-and-limit-theorems.md), [10. Monte Carlo integration](10-monte-carlo-integration.md), [12. Invariant distribution, detailed balance and reversibility](12-invariant-distribution-and-detailed-balance.md)
- **Used by:** [14. Metropolis–Hastings algorithm](14-metropolis-hastings-algorithm.md), [18. MCMC output analysis](18-mcmc-diagnostics.md), [29. The HMC algorithm and its invariance proof](29-hmc-algorithm-and-invariance.md)

## Role in HMC

Invariance guarantees that a chain started from the target distribution remains distributed according to it. The theorems of this block guarantee the practically relevant statements: a chain started from an arbitrary point approaches the target, and averages along the chain converge to target expectations. The conditions of these theorems identify the ways in which HMC can fail.

## Required knowledge

- **Irreducibility.** A chain is irreducible when every region of positive target probability can be reached from every starting point in a finite number of steps. Irreducibility implies that the invariant distribution is unique.
- **Aperiodicity.** A chain is aperiodic when it does not visit regions of the state space in a fixed cycle. A positive probability of remaining at the current state, as produced by a rejection, is sufficient.
- **Convergence theorem (statement).** For an irreducible, aperiodic chain with invariant distribution $\pi$, the distribution of $q_n$ converges to $\pi$ as $n\to\infty$. On a continuous state space the statement for every starting point requires Harris recurrence, and convergence is measured in total variation distance, which is the largest difference between the probabilities that the two distributions assign to the same set.
- **Ergodic theorem (statement).** For an irreducible chain with invariant distribution $\pi$ and a function $f$ with $\mathbb{E}_\pi\lvert f\rvert\lt\infty$, $\frac1N\sum_{n=1}^N f(q_n)\to\mathbb{E}_\pi[f]$ with probability 1; on a continuous state space the statement holds for $\pi$-almost every starting point, and for every starting point when the chain is Harris recurrent. This theorem justifies the use of the Monte Carlo estimator of block [10](10-monte-carlo-integration.md) with the dependent draws of a Markov chain. It is the law of large numbers for Markov chains.
- **Central limit theorem for Markov chains (statement).** Under additional conditions, $\hat f_N$ is approximately distributed as $N\big(\mathbb{E}_\pi[f],\operatorname{Var}_\pi[f]/\mathrm{ESS}\big)$, where $\mathrm{ESS}$ is the effective sample size (block [18](18-mcmc-diagnostics.md)).
- **Geometric ergodicity (definition).** A chain is geometrically ergodic when the distance between the distribution of $q_n$ and $\pi$ is bounded by $C(q_0)\,\rho^n$ with $\rho\lt 1$. Geometric ergodicity, together with a moment condition on $f$ such as $\mathbb{E}_\pi\lvert f\rvert^{2+\delta}\lt\infty$ for some $\delta\gt 0$, is the standard sufficient condition for the central limit theorem.
- **Initial transient.** The early states of a chain depend on the starting point and bias the estimator. They are discarded as warm-up (block [18](18-mcmc-diagnostics.md)).
- **Failure modes relevant to HMC.** Trajectories of exactly periodic length return to their starting point, which violates irreducibility (block [32](32-integration-time.md)). Heavy tails and regions of high curvature obstruct geometric ergodicity, in which case the central limit theorem can fail and estimates from finite chains can be strongly biased (block [38](38-hmc-diagnostics.md)). Geometric ergodicity can rarely be verified theoretically for a practical model; empirical diagnostics are used instead.

## Not required

- Proofs of the theorems, including coupling arguments.
- Drift and minorization conditions, small sets, spectral gaps and mixing-time bounds.

## Sources

- Durrett (2016): §1.3 (irreducibility, recurrence); §1.6, Theorem 1.19 (convergence theorem) and Theorem 1.22 (ergodic theorem); §1.7 (proof of the ergodic theorem); §1.8 (proof of the convergence theorem).
- Robert and Casella (2004): Definitions 6.13 and 6.32, Theorems 6.50–6.51, 6.63 and 6.65 (general state space; statements only were consulted).
- Betancourt (2017): §2.1–2.3 (convergence, central limit theorem, geometric ergodicity and its obstruction by regions of high curvature).
- Livingstone et al. (2019), *On the Geometric Ergodicity of HMC*: abstract (geometric ergodicity of HMC depends on the growth of the gradient of the log density in the tails).
- Neal (2011): §3.2, "Ergodicity of HMC" (periodic trajectories).
