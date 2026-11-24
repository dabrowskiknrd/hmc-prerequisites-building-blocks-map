# 21. Properties of Hamiltonian flow: energy conservation, reversibility, volume preservation

- **Layer:** Classical mechanics
- **Depends on:** [01. Multivariable calculus](01-multivariable-calculus.md), [02. Linear algebra](02-linear-algebra.md), [03. Ordinary differential equations and flows](03-ordinary-differential-equations-and-flows.md), [05. Change of variables and Jacobian determinants](05-change-of-variables-and-jacobians.md), [20. Hamiltonian mechanics](20-hamiltonian-mechanics.md)
- **Used by:** [22. Symplectic maps](22-symplectic-maps.md), [23. Canonical distribution and energy level sets](23-canonical-distribution.md), [24. Numerical integration of ODEs](24-numerical-integration-of-odes.md), [25. Leapfrog (Störmer–Verlet) integrator](25-leapfrog-integrator.md)

## Role in HMC

Three properties of the exact Hamiltonian flow make it a valid and efficient proposal mechanism. Conservation of energy gives an acceptance probability of 1. Reversibility makes the proposal symmetric. Volume preservation removes the Jacobian determinant from the acceptance probability. The correctness proof of HMC uses reversibility and volume preservation, which the numerical integrator retains exactly. Conservation of energy, which the integrator retains only approximately, determines the acceptance probability and therefore the efficiency.

## Required knowledge

- **Conservation of energy.** By the chain rule, $\frac{dH}{dt} = \sum_i\big(\frac{\partial H}{\partial q_i}\dot q_i + \frac{\partial H}{\partial p_i}\dot p_i\big)$. By Hamilton's equations, $\dot q_i = \partial H/\partial p_i$ and $\dot p_i = -\partial H/\partial q_i$. Hence $\frac{dH}{dt} = \sum_i\big(\frac{\partial H}{\partial q_i}\frac{\partial H}{\partial p_i} - \frac{\partial H}{\partial p_i}\frac{\partial H}{\partial q_i}\big) = 0$, and $H(\varphi_t(z)) = H(z)$.
- **Reversibility.** Let $S(q,p) = (q,-p)$ denote the momentum flip. If $H(q,-p) = H(q,p)$, which for $H = U(q) + K(p)$ means $K(-p) = K(p)$, then $S\circ\varphi_t\circ S = \varphi_{-t} = \varphi_t^{-1}$. Equivalently, the map $S\circ\varphi_t$ applied twice is the identity. Following a trajectory, negating the momentum, and following the trajectory for the same time returns the initial position with negated momentum.
- **Volume preservation (Liouville's theorem).** The vector field of Hamilton's equations is $f = (\partial H/\partial p,\ -\partial H/\partial q)$. Its divergence is $\sum_i\big(\frac{\partial^2H}{\partial q_i\partial p_i} - \frac{\partial^2H}{\partial p_i\partial q_i}\big)$, which equals 0 by the symmetry of second derivatives. A divergence-free vector field generates a flow with $\det\varphi_t'(z) = 1$. Hence $\varphi_t$ maps every region of phase space to a region of equal volume.
- **Sketch of the last implication.** For a small time $\delta$, $\varphi_\delta(z) = z + \delta f(z) + O(\delta^2)$, so the determinant expansion of block [02](02-linear-algebra.md) gives $\det\varphi_\delta' = 1 + \delta\,\nabla\cdot f + O(\delta^2) = 1 + O(\delta^2)$. The flow over time $t$ is a composition of $t/\delta$ such maps, and the product of their determinants converges to 1 as $\delta\to0$.
- **Joint consequence.** A density of the form $g(H(z))$ is invariant under $\varphi_t$: the value of the density is unchanged along a trajectory, and volume is unchanged. The canonical density $e^{-H}$ is of this form (block [23](23-canonical-distribution.md)).
- **Limitation.** The flow cannot change the value of $H$. A sampler that used the flow alone would remain on one energy level set. HMC adds momentum resampling for this reason (block [27](27-phase-space-target-and-momentum-resampling.md)).
- **Under discretization.** The leapfrog integrator retains reversibility and volume preservation exactly and conserves energy only approximately (block [25](25-leapfrog-integrator.md)).

## Not required

- Poincaré recurrence, ergodic theory of Hamiltonian systems, and Noether's theorem.

## Sources

- Neal (2011): §2.2, eq. (2.12) (conservation); eqs. (2.13)–(2.21) (volume preservation, with the small-time argument); reversibility in the same section.
- Bou-Rabee and Sanz-Serna (2018): Theorem 2.2 (conservation); §2.2 (divergence-free fields); §2.3, eqs. (2.12)–(2.13) (reversibility with respect to the momentum flip).
- Barbu and Zhu (2020): §9.3.1–9.3.3; Leimkuhler and Matthews (2015): §1.5.1, §2.3.1, §3.6.1.
- Vishnoi (2021): §3, items 1–3; Murphy (2025): eq. (12.66); Betancourt (2017): §3.2, §4.1.
