# 23. Canonical distribution and energy level sets

- **Layer:** Statistical mechanics
- **Depends on:** [04. Probability densities](04-probability-densities.md), [20. Hamiltonian mechanics](20-hamiltonian-mechanics.md), [21. Properties of Hamiltonian flow](21-properties-of-hamiltonian-flow.md)
- **Used by:** [27. Phase-space target and momentum resampling](27-phase-space-target-and-momentum-resampling.md), [32. Integration time (trajectory length)](32-integration-time.md), [38. HMC-specific diagnostics](38-hmc-diagnostics.md)

## Role in HMC

The canonical distribution of statistical mechanics relates an energy function to a probability density. Read in one direction, it assigns to the target density a potential energy. Read in the other direction, it assigns to a Hamiltonian a joint density on phase space. This correspondence is the step by which a sampling problem becomes a mechanical system.

## Required knowledge

- **Canonical distribution.** A system with energy function $E(x)$ at temperature $T$ has the density $P(x) = Z^{-1}\exp(-E(x)/T)$, where $Z$ is the normalizing constant. HMC uses $T = 1$.
- **Energy of a density.** Every positive density can be written in canonical form with $E(x) = -\log\tilde\pi(x)$. States of low energy are states of high density.
- **Canonical distribution on phase space.** For a Hamiltonian $H(q,p)$, the joint density is $\pi(q,p)\propto\exp(-H(q,p))$. For $H = U(q) + K(p)$ the density factorizes as $e^{-U(q)}\,e^{-K(p)}$. Hence $q$ and $p$ are independent, the marginal of $q$ is proportional to $e^{-U(q)}$, and for $K(p) = \tfrac12p^\top M^{-1}p$ the marginal of $p$ is $N(0,M)$.
- **Invariance under the flow.** The canonical density is a function of $H$. By block [21](21-properties-of-hamiltonian-flow.md), the Hamiltonian flow $\varphi_t$ conserves $H$ and preserves volume. Hence the canonical distribution is invariant under $\varphi_t$ for every $t$.
- **Decomposition by energy.** Phase space is the union of the level sets $H^{-1}(E)$. The canonical distribution decomposes into the distribution $\pi(E)$ of the energy and, for each $E$, a distribution on the level set, called the microcanonical distribution.
- **Two modes of exploration.** The Hamiltonian flow moves the state within one level set and is deterministic. A change of energy requires a random operation; in HMC this operation is momentum resampling (block [27](27-phase-space-target-and-momentum-resampling.md)). The efficiency of HMC depends on both parts: the exploration of each level set, which is governed by the integration time (block [32](32-integration-time.md)), and the movement between level sets, which is monitored by the energy diagnostic (block [38](38-hmc-diagnostics.md)).

## Not required

- The derivation of the canonical distribution by entropy maximization, thermodynamic quantities, and the equivalence of ensembles.
- Temperatures other than 1, which occur only in tempering methods.
- The measure-theoretic construction of the microcanonical distribution by disintegration.

## Sources

- Neal (2011): §3.1, eqs. (3.1)–(3.3) (canonical distribution, energy of a density, factorized joint density).
- Betancourt (2017): §3.2 (canonical distribution); §4.1 (microcanonical decomposition, exploration within and across level sets).
- Bou-Rabee and Sanz-Serna (2018): Theorem 2.3 and eq. (5.7) (invariance of the Boltzmann–Gibbs distribution under the flow).
- Leimkuhler and Matthews (2015): §6.1.2–6.1.3, eq. (6.3) (derivation of the canonical density).
- Barbu and Zhu (2020): eqs. (9.14)–(9.18); MacKay (2003): §30.1, eq. (30.3).
- Betancourt (2016), *Diagnosing Suboptimal Cotangent Disintegrations*: §2; Betancourt (2016), *Identifying the Optimal Integration Time*: §1.2.1, eq. (1).
