# 35. Trajectory doubling and termination

- **Layer:** No-U-Turn Sampler
- **Depends on:** [33. U-turn criterion](33-u-turn-criterion.md), [34. Sampling a state from a trajectory](34-sampling-a-state-from-a-trajectory.md)
- **Used by:** [36. The No-U-Turn Sampler (NUTS)](36-nuts-algorithm.md)

## Role in HMC

A trajectory whose length is determined by its own states satisfies the symmetry condition of block [34](34-sampling-a-state-from-a-trajectory.md) only if it is built in a particular way. NUTS builds the trajectory by repeated doubling in a random direction of time and tests the U-turn criterion on a prescribed family of sub-trajectories. This block is the part of NUTS that makes a state-dependent trajectory length compatible with invariance of the target.

## Required knowledge

- **Doubling.** The trajectory initially consists of the state $z_0$. At iteration $j = 0,1,2,\dots$ a direction $v_j\in\lbrace -1,+1\rbrace$ is drawn uniformly, and $2^j$ leapfrog steps of size $v_j\varepsilon$ are taken from the corresponding end. After iteration $j$ the trajectory has $2^{j+1}$ states.
- **Binary tree.** The states of the trajectory are the leaves of a balanced binary tree. A tree of height $j$ is the union of two trees of height $j-1$. The recursive procedure that builds a tree returns its two end states, one selected state, the total weight or count, and a stop indicator; its base case is a single leapfrog step.
- **Purpose of the random directions.** Consider a trajectory of $2^k$ states produced by $k$ doublings. Each of its states would have produced the same trajectory by at most one sequence of $k$ direction choices, and by exactly one once the sub-tree checks of the next item are imposed. Every state therefore generates the trajectory with the same probability $2^{-k}$. This property is the symmetry condition of block [34](34-sampling-a-state-from-a-trajectory.md).
- **Termination on sub-trees.** The U-turn criterion is evaluated on the end states of the full tree and of every balanced sub-tree. The check on sub-trees is necessary because the doubling procedure started from another state of the trajectory would have encountered these sub-trees as its complete trajectory at an earlier iteration and would have stopped.
- **Treatment of the last doubling.** If the criterion is met by a sub-tree inside the newly added half, the new half is discarded, and expansion stops. If the criterion is met only by the full tree, the new half is retained, and expansion stops. In both cases the retained trajectory satisfies the symmetry condition.
- **Cost.** A tree of height $j$ has $2^j-1$ balanced sub-trees of positive height, and the checks cost $2^{j+1}-2$ inner products. The number of states that must be stored is of order $j$.
- **Termination on energy error.** Expansion also stops when a new state has an energy error above a threshold. Hoffman and Gelman stop when $-H(z)-\log u\lt-\Delta_{\max}$ with $\Delta_{\max} = 1000$, which measures the error relative to the slice variable $u$, and they discard the half-tree that contains such a state. Stan tests $H(z)-H(z_0)\gt 1000$ and records the event as a divergence (block [38](38-hmc-diagnostics.md)).
- **Maximum tree depth.** Implementations limit the number of doublings. A trajectory that reaches the limit is terminated without having met the U-turn criterion.
- **Consequence for cost.** The number of states of the retained trajectory is a power of two, and the number of leapfrog steps varies between iterations.

## Not required

- The iterative, non-recursive formulation with bit operations, which matters for implementations that are compiled just in time (Phan et al., 2019, Appendix A).
- The binary-encoding lemmas and ergodicity proofs of Durmus et al. (2026).

## Sources

- Hoffman and Gelman (2014): §3.1.1 (doubling, conditions for validity, treatment of the last doubling); Algorithms 2–3 (recursive construction); eq. 8 (energy-error stop); §3.1.2 (memory). The converted text is the arXiv version 1 and does not mention a maximum tree depth.
- Betancourt (2017): Appendix A.3.1 (doubling and binary trees); Appendix A.4.1 (termination on sub-trees).
- Betancourt (2016), *Identifying the Optimal Integration Time in HMC*: §2.2, Algorithms 1–2.
- Durmus et al. (2026): §3.1–3.2, Proposition 5, Algorithm 4 (maximum tree height $K_m$).
- Bou-Rabee, Carpenter and Marsden (2026): §3.5, eq. (23), Remarks 6–8 (maximum number of doublings; cost of doubling).
- Stan source code, `src/stan/mcmc/hmc/nuts/base_nuts.hpp` (energy test relative to the initial energy; divergence flag). This file is not in the sources folder.
