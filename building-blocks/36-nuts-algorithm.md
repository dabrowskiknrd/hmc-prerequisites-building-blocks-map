# 36. The No-U-Turn Sampler (NUTS)

- **Layer:** No-U-Turn Sampler
- **Depends on:** [16. Gibbs sampling and auxiliary variables](16-gibbs-sampling-and-auxiliary-variables.md), [29. The HMC algorithm and its invariance proof](29-hmc-algorithm-and-invariance.md), [33. U-turn criterion](33-u-turn-criterion.md), [34. Sampling a state from a trajectory](34-sampling-a-state-from-a-trajectory.md), [35. Trajectory doubling and termination](35-trajectory-doubling-and-termination.md)
- **Used by:** [37. Step-size adaptation by dual averaging](37-step-size-adaptation-by-dual-averaging.md), [38. HMC-specific diagnostics](38-hmc-diagnostics.md)

## Role in HMC

NUTS is HMC with an integration time selected anew in every iteration by the algorithm itself. It is the default sampler of Stan, PyMC and NumPyro. This block assembles the U-turn criterion, the doubling procedure and the selection of a state from the trajectory into the algorithm, and it states the structure of the validity argument.

## Required knowledge

- **One iteration from the current position $q$.**
  1. Draw a momentum $p\sim N(0,M)$ and set $z_0 = (q,p)$.
  2. Slice version only: draw $u\sim\mathrm{Uniform}[0,\ e^{-H(z_0)}]$.
  3. Start with the trajectory $\lbrace z_0\rbrace$ and double it in random directions (block [35](35-trajectory-doubling-and-termination.md)).
  4. Stop when the U-turn criterion is met by the tree or by a sub-tree, when the energy error exceeds the threshold, or when the maximum tree depth is reached.
  5. Select the next state from the retained trajectory (block [34](34-sampling-a-state-from-a-trajectory.md)) and return its position.
- **Absence of an accept–reject step.** The selection among the states of the trajectory replaces the Metropolis test. The next state differs from the current state unless the current state is selected.
- **Validity argument of Hoffman and Gelman.** The state is extended by the momentum, the slice variable, the set $B$ of all visited states and the set $C\subseteq B$ of candidate states. Steps 1–3 resample these auxiliary variables from their conditional distribution given $q$ and are a Gibbs update (block [16](16-gibbs-sampling-and-auxiliary-variables.md)). Four conditions are imposed on the construction: the leapfrog map preserves volume; the current state belongs to $C$; all states of $C$ satisfy the slice condition; and every state of $C$ generates the same $B$ and $C$ with the same probability. Under these conditions the conditional distribution of the state given the auxiliary variables is uniform on $C$. A selection rule that leaves the uniform distribution on $C$ invariant therefore leaves the joint distribution invariant, and the positions have the invariant density $\pi(q)$.
- **General formulation.** NUTS is an instance of dynamic HMC: draw a momentum, draw an index set (orbit) with a kernel that satisfies the symmetry condition, draw an index from the orbit with a kernel that leaves the weights $e^{-H}$ invariant, and move to the corresponding state.
- **Versions.** The original algorithm uses slice sampling and the two-sided U-turn criterion. Stan uses multinomial sampling with biased progressive selection and the generalized U-turn criterion. NumPyro uses an iterative construction of the tree.
- **Remaining tuning parameters.** The step size $\varepsilon$ and the mass matrix $M$ are adapted during warm-up (blocks [37](37-step-size-adaptation-by-dual-averaging.md) and [30](30-mass-matrix.md)). The number of leapfrog steps is no longer a parameter.
- **Efficiency.** Hoffman and Gelman report an efficiency, in effective sample size per gradient evaluation, comparable to that of HMC with a well-chosen number of steps.

## Not required

- The Riemannian generalization of NUTS and the formulation as a Gibbs self-tuning sampler with its other instances.
- The proofs of irreducibility and geometric ergodicity of NUTS.

## Sources

- Hoffman and Gelman (2014): §3.1 and Algorithm 2 (naive NUTS); §3.1.1, conditions C.1–C.4 and eqs. 6–7 (validity); §3.1.2 and Algorithm 3 (efficient NUTS); Algorithm 6 (with step-size adaptation); §4 (empirical efficiency).
- Betancourt (2017): Appendix A.5 (NUTS as a composition; the version used in Stan).
- Durmus et al. (2026): Definition 1, eq. (3) (dynamic HMC); Proposition 2, Corollary 3 and Theorem 4 (invariance; the proof of Proposition 2 was not consulted).
- Bou-Rabee, Carpenter and Marsden (2026): §3.5, eqs. (24)–(25) and Corollary 5 (NUTS as a Gibbs self-tuning sampler with acceptance probability 1).
- Phan, Pradhan and Jankowiak (2019): §3.1 and Appendix A (iterative NUTS).
- BDA3: §12.4 and Murphy (2025): §12.5.4.1 (short qualitative descriptions).
