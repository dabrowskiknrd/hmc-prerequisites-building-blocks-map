# 19. Newtonian mechanics

- **Layer:** Classical mechanics
- **Depends on:** [01. Multivariable calculus](01-multivariable-calculus.md), [03. Ordinary differential equations and flows](03-ordinary-differential-equations-and-flows.md)
- **Used by:** [20. Hamiltonian mechanics](20-hamiltonian-mechanics.md)

## Role in HMC

HMC interprets the parameter vector as the position of a particle that moves without friction in a potential equal to the negative log target density. Newton's second law gives the physical content of the two operations that the integrator alternates: the force changes the momentum, and the momentum changes the position. Conservation of total energy is the reason why a trajectory remains in a region of comparable joint density.

## Required knowledge

- **State of a particle.** Position $q(t)\in\mathbb{R}^d$, velocity $\dot q(t)$, and mass $m$, or more generally a symmetric positive-definite mass matrix $M$. The momentum is $p = M\dot q$.
- **Newton's second law.** $M\ddot q = F(q)$. Together with an initial position and an initial velocity, this equation determines the trajectory (block [03](03-ordinary-differential-equations-and-flows.md)).
- **Conservative force.** A force is conservative when $F(q) = -\nabla U(q)$ for a scalar potential energy $U$. In HMC, $U(q) = -\log\tilde\pi(q)$, so the force is $\nabla\log\tilde\pi(q)$ and points toward higher density.
- **Kinetic energy.** $K = \tfrac12\dot q^\top M\dot q = \tfrac12 p^\top M^{-1}p$.
- **Conservation of total energy.** Let $E = K + U$. By the chain rule, $\frac{dE}{dt} = \dot q^\top M\ddot q + \nabla U(q)\cdot\dot q$. By Newton's law, $M\ddot q = -\nabla U(q)$. Hence $\frac{dE}{dt} = 0$. Along a trajectory, kinetic and potential energy are exchanged while their sum is constant.
- **First-order form.** Newton's law is equivalent to $\dot q = M^{-1}p$, $\dot p = -\nabla U(q)$. These are Hamilton's equations for $H = U + K$ (block [20](20-hamiltonian-mechanics.md)).
- **Harmonic oscillator.** The potential $U(q) = \tfrac12 kq^2$ with mass $m$ gives oscillation at angular frequency $\omega = \sqrt{k/m}$. A one-dimensional Gaussian target with standard deviation $\sigma$ corresponds to $U(q) = q^2/(2\sigma^2)$; with unit mass, $\omega = 1/\sigma$.
- **Qualitative behavior.** A particle with nonzero momentum does not move directly to the minimum of $U$. It passes the minimum and climbs the potential until its kinetic energy is exhausted. Reversing the velocity at any instant makes the particle retrace its path.

## Not required

- Lagrangian mechanics: generalized coordinates, the principle of least action, the Euler–Lagrange equations and the Legendre transform. Of the consulted sources, only Leimkuhler and Matthews (2015, §1.3–1.4) pass through the Lagrangian; Hanada and Matsuura (2022) pass directly from Newton's law to Hamilton's equations, and the remaining sources begin from the Hamiltonian.
- Friction, constraints, rigid bodies and many-particle interaction potentials.

## Sources

- Leimkuhler and Matthews (2015): eqs. (1.2)–(1.4) (Newton's law, conservative force, conservation of energy with derivation).
- Hanada and Matsuura (2022), *MCMC from Scratch*: Appendix C (momentum, force, Hamilton's equations from Newton's law).
- Neal (2011): §2 (frictionless puck on a surface); Thomas and Tu (2021): §3.1, Fig. 1.
- Betancourt (2017): §3.1 (orbit analogy for the role of momentum).
- Bou-Rabee and Sanz-Serna (2018): eq. (2.2) (Newtonian system in first-order form).
