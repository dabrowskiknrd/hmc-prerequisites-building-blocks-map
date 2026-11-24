# 12. Invariant distribution, detailed balance and reversibility

- **Layer:** Markov chain theory
- **Depends on:** [11. Markov chains and transition kernels](11-markov-chains-and-transition-kernels.md)
- **Used by:** [13. Convergence and ergodicity of Markov chains](13-convergence-and-ergodicity.md), [14. Metropolis–Hastings algorithm](14-metropolis-hastings-algorithm.md), [16. Gibbs sampling and auxiliary variables](16-gibbs-sampling-and-auxiliary-variables.md), [29. The HMC algorithm and its invariance proof](29-hmc-algorithm-and-invariance.md), [34. Sampling a state from a trajectory](34-sampling-a-state-from-a-trajectory.md)

## Role in HMC

The defining requirement of an MCMC kernel is that it leaves the target distribution invariant. Detailed balance is the condition by which invariance is verified for Metropolis–Hastings, for the HMC accept–reject step, and for the selection of a state in NUTS. The rule that a composition of invariant kernels is invariant justifies the two-part structure of an HMC iteration.

## Required knowledge

- **Invariant distribution.** A density $\pi$ is invariant for the kernel $T$ when $\pi(q') = \int \pi(q)\,T(q'\mid q)\,dq$ for all $q'$. In the finite case the condition is $\pi P = \pi$. If $q_n\sim\pi$, then $q_{n+1}\sim\pi$.
- **Detailed balance.** $T$ satisfies detailed balance with respect to $\pi$ when $\pi(q)\,T(q'\mid q) = \pi(q')\,T(q\mid q')$ for all $q,q'$.
- **Detailed balance implies invariance.** Integrating the detailed-balance identity over $q$ gives $\int\pi(q)\,T(q'\mid q)\,dq = \pi(q')\int T(q\mid q')\,dq$. Since $\int T(q\mid q')\,dq = 1$, the right-hand side equals $\pi(q')$. Hence $\pi$ is invariant.
- **Reversibility.** A chain that satisfies detailed balance and starts from $\pi$ has the same distribution when observed in reversed time order. Such a chain is called reversible. Detailed balance is sufficient for invariance and is not necessary.
- **Composition and mixture.** If $T_1$ and $T_2$ each leave $\pi$ invariant, then the composition "apply $T_1$, then $T_2$" leaves $\pi$ invariant, since each application maps $\pi$ to $\pi$. The same holds for a mixture. The composition need not satisfy detailed balance.
- **Invariance on an extended space.** If a kernel on pairs $(q,p)$ leaves a joint density $\pi(q,p)$ invariant, and $\int\pi(q,p)\,dp = \pi(q)$, then the positions of the chain have the invariant density $\pi(q)$ (block [16](16-gibbs-sampling-and-auxiliary-variables.md)).
- **Invariance does not imply convergence.** The kernel that never moves leaves every distribution invariant. Convergence requires the additional conditions of block [13](13-convergence-and-ergodicity.md).

## Not required

- The Kolmogorov cycle condition and the explicit formula for the kernel of the time-reversed chain.

## Sources

- Durrett (2016): §1.4 (stationary distributions); §1.5, eq. (1.11) (detailed balance condition); §1.5.1, Theorem 1.15 (reversibility).
- MacKay (2003): eqs. (29.41)–(29.46) (invariance, concatenation and mixture of transitions, detailed balance).
- Neal (2011): eqs. (3.7)–(3.12) (detailed balance and its implication for the HMC update); §3.2 (sequential application of updates that each preserve the target).
- Murphy (2025): eq. (12.7).
- Robert and Casella (2004): Definitions 6.35, 6.44, 6.45 and Theorem 6.46.
- Betancourt (2017): §2.1 (preservation of the target distribution).
