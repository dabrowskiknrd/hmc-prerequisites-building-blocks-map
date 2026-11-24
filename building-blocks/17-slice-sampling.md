# 17. Slice sampling

- **Layer:** MCMC algorithms
- **Depends on:** [16. Gibbs sampling and auxiliary variables](16-gibbs-sampling-and-auxiliary-variables.md)
- **Used by:** [34. Sampling a state from a trajectory](34-sampling-a-state-from-a-trajectory.md)

## Role in HMC

The original No-U-Turn Sampler of Hoffman and Gelman is formulated with a slice variable. The slice variable converts the selection of a trajectory state with probability proportional to its density into a uniform selection among the states that pass a threshold test. Current implementations replace this construction by multinomial sampling (block [34](34-sampling-a-state-from-a-trajectory.md)), so slice sampling is required mainly to read the original paper and its validity proof.

## Required knowledge

- **Region under the density.** Let $\tilde\pi$ be an unnormalized density. Consider the pair $(q,u)$ distributed uniformly on the region $\lbrace (q,u) : 0\le u\le\tilde\pi(q)\rbrace$. The joint density is $p(q,u)\propto\mathbb{1}[0\le u\le\tilde\pi(q)]$.
- **Marginal.** Integrating the joint density over $u$ gives $\int_0^{\tilde\pi(q)} du = \tilde\pi(q)$. Hence the marginal of $q$ is the target $\pi$. The variable $u$ is an auxiliary variable in the sense of block [16](16-gibbs-sampling-and-auxiliary-variables.md).
- **Conditional distributions.** Given $q$, the variable $u$ is uniform on $[0,\tilde\pi(q)]$. Given $u$, the variable $q$ is uniform on the slice $\lbrace q : \tilde\pi(q)\ge u\rbrace$.
- **Slice sampler.** Alternating the two conditional updates is a Gibbs sampler for the joint density. More generally, the update of $q$ may be any kernel that leaves the uniform distribution on the slice invariant.
- **Consequence used by NUTS.** Conditional on $u$, all admissible states are equally probable. A selection weighted by density is thereby replaced by a count of the states that satisfy $\tilde\pi(q)\ge u$ and a uniform choice among them.
- **Form in NUTS.** With the joint density of position and momentum proportional to $\exp(-H(q,p))$ (block [27](27-phase-space-target-and-momentum-resampling.md)), the slice variable is drawn as $u\sim\mathrm{Uniform}\big[0,\exp(-H(q,p))\big]$. A state $(q',p')$ of the trajectory is admissible when $\exp(-H(q',p'))\ge u$.

## Not required

- The stepping-out and shrinkage procedures for one-dimensional slice sampling.
- Multivariate and elliptical slice samplers.

## Sources

- MacKay (2003): §29.7 (slice sampling, including stepping-out and shrinkage).
- Murphy (2025): §12.4.1, eqs. (12.58)–(12.60).
- BDA3: §12.3.
- Hoffman and Gelman (2014): §3.1, eqs. 4–5 (slice variable and joint density in NUTS). The paper states the properties and refers to Neal (2003) for slice sampling itself; that reference is not in the sources folder.
- Betancourt (2017): Appendix A.2.2 (slice sampling of a state from a trajectory).
