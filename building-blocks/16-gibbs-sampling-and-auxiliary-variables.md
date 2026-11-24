# 16. Gibbs sampling and auxiliary variables

- **Layer:** MCMC algorithms
- **Depends on:** [04. Probability densities](04-probability-densities.md), [07. Multivariate Gaussian distribution](07-multivariate-gaussian-distribution.md), [12. Invariant distribution, detailed balance and reversibility](12-invariant-distribution-and-detailed-balance.md), [14. Metropolis–Hastings algorithm](14-metropolis-hastings-algorithm.md)
- **Used by:** [17. Slice sampling](17-slice-sampling.md), [27. Phase-space target and momentum resampling](27-phase-space-target-and-momentum-resampling.md), [36. The No-U-Turn Sampler (NUTS)](36-nuts-algorithm.md)

## Role in HMC

HMC samples a joint distribution of positions and momenta in which the momenta are auxiliary variables introduced by the method. The momentum resampling step of HMC is a Gibbs update. The validity argument of NUTS treats the momentum, the slice variable and the trajectory as auxiliary variables that are updated by Gibbs steps.

## Required knowledge

- **Gibbs update.** For a joint density $\pi(x_1,\dots,x_k)$, a Gibbs update replaces one component or block $x_i$ by a draw from its full conditional $\pi(x_i\mid x_{-i})$, where $x_{-i}$ denotes the remaining components.
- **Invariance of a Gibbs update.** The joint density factorizes as $\pi(x_i\mid x_{-i})\,\pi(x_{-i})$. The update leaves $x_{-i}$ unchanged, so the marginal $\pi(x_{-i})$ is preserved. The new $x_i$ is an exact draw from $\pi(x_i\mid x_{-i})$. Hence the updated state is distributed according to the joint density whenever the previous state was.
- **Relation to Metropolis–Hastings.** A Gibbs update is a Metropolis–Hastings update whose proposal is the full conditional; its acceptance probability equals 1.
- **Combination of updates.** A Gibbs update of a single block changes that block only and is not irreducible alone. It is combined with other invariant kernels by composition (block [12](12-invariant-distribution-and-detailed-balance.md)).
- **Limitation of Gibbs sampling as a complete sampler.** Each update is restricted to the conditional distribution of one block. For strongly correlated components the conditional scale is much smaller than the marginal scale (block [07](07-multivariate-gaussian-distribution.md)), and the chain moves diffusively.
- **Auxiliary-variable method.** To sample $\pi(q)$, choose any conditional density $\pi(v\mid q)$ for an additional variable $v$, and define the joint density $\pi(q,v) = \pi(q)\,\pi(v\mid q)$. A kernel that leaves the joint density invariant produces states whose $q$-components have the invariant density $\pi(q)$; the variable $v$ is discarded. The conditional $\pi(v\mid q)$ is chosen so that the joint density is easier to explore than $\pi(q)$.
- **Instances used later.** The momentum $p$ of HMC, with $\pi(q,p) = \pi(q)\,N(p\mid 0,M)$; drawing $p\sim N(0,M)$ is an exact Gibbs update (block [27](27-phase-space-target-and-momentum-resampling.md)). The slice variable (block [17](17-slice-sampling.md)). The direction choices and the set of trajectory states in NUTS (block [36](36-nuts-algorithm.md)).

## Not required

- Collapsed and blocked Gibbs samplers, parameter expansion, and model-specific data augmentation.
- Conjugacy and the derivation of full conditionals for particular models.

## Sources

- BDA3: §11.1 (Gibbs sampler); §11.3 (Gibbs as a special case of Metropolis–Hastings); §12.1 (auxiliary variables).
- MacKay (2003): §29.5, eqs. (29.35)–(29.37); §30.1 (momentum update as a Gibbs step).
- Murphy (2025): §12.3.1 (basic idea); §12.3.2, eqs. (12.20)–(12.22) (Gibbs sampling as a special case of Metropolis–Hastings); §12.4 (auxiliary-variable MCMC).
- Neal (2011): §3.2 (momentum drawn from its conditional distribution); §4.3 (combination of HMC with other updates).
- Hoffman and Gelman (2014): §3.1.1 (resampling of momentum, slice variable and trajectory sets as a Gibbs update).
