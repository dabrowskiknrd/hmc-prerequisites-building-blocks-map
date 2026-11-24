# 27. Phase-space target and momentum resampling

- **Layer:** Hamiltonian Monte Carlo
- **Depends on:** [07. Multivariate Gaussian distribution](07-multivariate-gaussian-distribution.md), [08. Bayesian posterior as the target density](08-bayesian-posterior-as-target.md), [16. Gibbs sampling and auxiliary variables](16-gibbs-sampling-and-auxiliary-variables.md), [23. Canonical distribution and energy level sets](23-canonical-distribution.md)
- **Used by:** [29. The HMC algorithm and its invariance proof](29-hmc-algorithm-and-invariance.md)

## Role in HMC

This block is the construction that converts a sampling problem into a mechanical system. The target density is extended to a joint density on phase space, and the idealized HMC transition is defined from the exact Hamiltonian flow. The idealized transition requires no accept–reject step; the practical algorithm (block [29](29-hmc-algorithm-and-invariance.md)) is its approximation by a numerical integrator.

## Required knowledge

- **Extended target.** An auxiliary momentum $p\in\mathbb{R}^d$ is added to the parameters $q$. The joint density is $\pi(q,p) = \pi(q)\,N(p\mid 0,M)\propto\exp(-H(q,p))$, with $H(q,p) = U(q) + K(p)$, $U(q) = -\log\tilde\pi(q)$ and $K(p) = \tfrac12 p^\top M^{-1}p$. This is the auxiliary-variable method of block [16](16-gibbs-sampling-and-auxiliary-variables.md) and the canonical distribution of block [23](23-canonical-distribution.md). The marginal of $q$ is the target $\pi(q)$.
- **Idealized transition.** From the current position $q$: draw $p\sim N(0,M)$; apply the Hamiltonian flow, $(q',p') = \varphi_T(q,p)$, for an integration time $T$; return $q'$ and discard $p'$.
- **Invariance of the idealized transition.** The momentum is independent of $q$ under the joint density, so the draw $p\sim N(0,M)$ is an exact Gibbs update and leaves $\pi(q,p)$ invariant. The flow $\varphi_T$ leaves the canonical distribution invariant (block [23](23-canonical-distribution.md)). Hence the composition leaves $\pi(q,p)$ invariant, and the positions have the invariant density $\pi(q)$.
- **Necessity of both operations.** The flow is deterministic and conserves $H$, so it explores one energy level set only. Momentum resampling changes the kinetic energy and thereby the level set, and it does not move $q$. Neither operation alone is a complete sampler.
- **Effect of the momentum.** The momentum preserves the direction of motion across integrator steps, so the distance traveled grows in proportion to the integration time. The force $-\nabla U(q)$ converts kinetic into potential energy and back, which confines the trajectory to states of comparable joint density.
- **Magnitude of the energy change.** The kinetic energy of a fresh momentum has mean $d/2$ and variance $d/2$ (block [07](07-multivariate-gaussian-distribution.md)). One resampling therefore changes the energy by an amount of order $\sqrt d$.
- **Choice of kinetic energy.** The kinetic energy is a design choice. An admissible $K$ has a finite integral $\int e^{-K(p)}\,dp$ and satisfies $K(-p) = K(p)$. The Gaussian choice is standard, and its matrix $M$ is a tuning parameter (block [30](30-mass-matrix.md)).

## Not required

- Partial momentum refreshment and non-Gaussian kinetic energies.
- Position-dependent kinetic energies (Riemannian manifold HMC) and the construction of the extended target as a disintegration on the cotangent bundle.

## Sources

- Neal (2011): §3.1–3.2 (joint density, momentum drawn from its Gaussian distribution).
- Betancourt (2017): §3.2–3.3 (lift, flow, projection); §4.1 (exploration within and between level sets).
- Vishnoi (2021): Algorithm 1 and Theorem 5.1 (idealized HMC preserves the target).
- Bou-Rabee and Sanz-Serna (2018): Algorithm 5.2 and Theorem 5.1 (exact HMC).
- BDA3: §12.4; MacKay (2003): §30.1, eq. (30.3); Barbu and Zhu (2020): §9.5.1–9.5.2.
- Betancourt and Stein (2011), *The Geometry of HMC*: eqs. (4)–(6) (admissible kinetic energies).
