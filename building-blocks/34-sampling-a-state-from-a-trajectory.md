# 34. Sampling a state from a trajectory

- **Layer:** No-U-Turn Sampler
- **Depends on:** [04. Probability densities](04-probability-densities.md), [12. Invariant distribution, detailed balance and reversibility](12-invariant-distribution-and-detailed-balance.md), [17. Slice sampling](17-slice-sampling.md), [29. The HMC algorithm and its invariance proof](29-hmc-algorithm-and-invariance.md)
- **Used by:** [35. Trajectory doubling and termination](35-trajectory-doubling-and-termination.md), [36. The No-U-Turn Sampler (NUTS)](36-nuts-algorithm.md)

## Role in HMC

Standard HMC proposes the final state of a trajectory and accepts or rejects it. NUTS selects the next state from all states of a trajectory, with probabilities that make the selection leave the target invariant. This replacement removes the accept–reject step and is the mechanism that permits trajectories whose length is not fixed in advance.

## Required knowledge

- **Trajectory as a set.** Write $z = (q,p)$. A numerical trajectory $t$ is a set of consecutive leapfrog states $\lbrace\Phi_\varepsilon^{i}(z_0) : i = a,\dots,b\rbrace$ with $a\le 0\le b$. It contains the initial state $z_0$ and extends backward and forward in time.
- **Two-stage transition.** A trajectory is drawn given the initial state, with probability $T(t\mid z_0)$. The next state $z'$ is then drawn from the trajectory, with probability $T(z'\mid t)$.
- **Symmetry condition.** The construction is valid when $T(t\mid z) = T(t\mid z')$ for all states $z$, $z'$ of $t$. Every state of a trajectory must be equally likely to have generated that trajectory. For a trajectory of a fixed number of states this condition holds when the position of the initial state within the trajectory is drawn uniformly.
- **Multinomial sampling.** Given the trajectory, the next state is drawn with probability

  $$T(z'\mid t) = \frac{e^{-H(z')}}{\sum_{z''\in t}e^{-H(z'')}}.$$

  Because the leapfrog map preserves volume, the values $e^{-H(z)}$ on the finite set $t$ act as probability masses. Together with the symmetry condition, this selection leaves the canonical distribution invariant.
- **Slice sampling.** In the original NUTS a slice variable $u\sim\mathrm{Uniform}[0,e^{-H(z_0)}]$ is drawn first (block [17](17-slice-sampling.md)). The next state is drawn uniformly from the states of $t$ with $e^{-H(z)}\ge u$.
- **Effect of the energy error.** A state with a large energy error has a small weight $e^{-H(z)}$ and is rarely selected. A single such state does not cause the rejection of the whole trajectory.
- **Progressive sampling.** The trajectory need not be stored. When an old part with total weight $w_{\text{old}} = \sum_{z\in t_{\text{old}}}e^{-H(z)}$ and a new part with weight $w_{\text{new}}$ are merged, each part carries one selected state. Uniform progressive sampling keeps the state of the new part with probability $w_{\text{new}}/(w_{\text{old}}+w_{\text{new}})$.
- **Biased progressive sampling.** At the merge of a trajectory with its newly added half, the state of the new half is taken with probability $\min(1,\ w_{\text{new}}/w_{\text{old}})$. This rule favors states far from the initial state and leaves the target invariant. In the slice version the weights are replaced by the numbers of admissible states.

## Not required

- The measure-theoretic statement of the invariance result for general orbit-selection and index-selection kernels.
- Rao–Blackwellized estimators that average over all states of a trajectory.

## Sources

- Betancourt (2017): Appendix A.1–A.2 (trajectory sets, two-stage transition, multinomial and slice sampling, invariance proof); Appendix A.3 (uniform and biased progressive sampling).
- Betancourt (2016), *Identifying the Optimal Integration Time in HMC*: §2.1, eqs. (3)–(4).
- Hoffman and Gelman (2014): §3.1.1 (candidate set and uniform selection); eq. 10 (biased kernel toward the new half-tree); eq. 12 and Algorithm 3 (progressive selection by counts).
- Durmus et al. (2026): eqs. (16) and (18), Proposition 6; Bou-Rabee, Carpenter and Marsden (2026): eqs. (20)–(21).
