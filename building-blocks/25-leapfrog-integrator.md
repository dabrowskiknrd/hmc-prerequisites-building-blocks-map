# 25. Leapfrog (Störmer–Verlet) integrator

- **Layer:** Numerical integration
- **Depends on:** [05. Change of variables and Jacobian determinants](05-change-of-variables-and-jacobians.md), [21. Properties of Hamiltonian flow](21-properties-of-hamiltonian-flow.md), [22. Symplectic maps](22-symplectic-maps.md), [24. Numerical integration of ODEs](24-numerical-integration-of-odes.md)
- **Used by:** [26. Energy error and the modified (shadow) Hamiltonian](26-energy-error-and-shadow-hamiltonian.md), [28. Metropolis correction for a deterministic proposal](28-metropolis-correction-for-deterministic-proposals.md)

## Role in HMC

The leapfrog integrator generates the proposal in every practical implementation of HMC and every leaf of the trajectory tree in NUTS. It is explicit, costs one gradient evaluation per step, and retains exactly the two properties of the Hamiltonian flow on which the acceptance probability depends: volume preservation and reversibility.

## Required knowledge

- **Update.** One step of size $\varepsilon$ maps $(q,p)$ to $(q',p')$ by

  $$p_{1/2} = p - \tfrac{\varepsilon}{2}\nabla U(q),\qquad q' = q + \varepsilon\,M^{-1}p_{1/2},\qquad p' = p_{1/2} - \tfrac{\varepsilon}{2}\nabla U(q').$$

  The map is denoted $\Phi_\varepsilon$. In $L$ consecutive steps the adjacent half-steps of the momentum combine, so $L$ steps cost $L+1$ gradient evaluations.
- **Splitting.** The Hamiltonian is the sum of $U(q)$ and $K(p)$. The flow of $U$ alone is $(q,p)\mapsto(q,\ p - t\,\nabla U(q))$. The flow of $K$ alone is $(q,p)\mapsto(q + t\,M^{-1}p,\ p)$. Both flows are available in closed form. The leapfrog step is the symmetric composition: flow of $U$ for time $\varepsilon/2$, flow of $K$ for time $\varepsilon$, flow of $U$ for time $\varepsilon/2$.
- **Volume preservation.** Each of the three sub-steps is a shear map with Jacobian determinant 1 (block [05](05-change-of-variables-and-jacobians.md)). Hence $\det\Phi_\varepsilon' = 1$ for every step size. As a composition of Hamiltonian flows, $\Phi_\varepsilon$ is also symplectic (block [22](22-symplectic-maps.md)).
- **Reversibility.** With the momentum flip $S(q,p) = (q,-p)$, the symmetric order of the sub-steps gives $S\circ\Phi_\varepsilon\circ S = \Phi_\varepsilon^{-1}$. Hence $L$ steps, a momentum flip, $L$ further steps and a second flip return the initial state.
- **Order.** The local error is $O(\varepsilon^3)$ and the global error is $O(\varepsilon^2)$.
- **Energy.** $H$ is not conserved exactly. Below the stability limit the energy error oscillates and remains bounded along the trajectory (block [26](26-energy-error-and-shadow-hamiltonian.md)).
- **Stability limit.** On the harmonic oscillator with frequency $\omega$ the leapfrog integrator is stable exactly when $\varepsilon\,\omega\lt 2$. For a one-dimensional Gaussian target with standard deviation $\sigma$ and unit mass, the condition is $\varepsilon\lt 2\sigma$. For a multivariate Gaussian target the condition is $\varepsilon\,\omega_{\max}\lt 2$; the step size is therefore limited by the direction of smallest standard deviation. Above the limit the energy error grows exponentially.
- **Fixed step size.** The step size must be constant along a trajectory. A step size that depends on the state destroys reversibility.

## Not required

- Higher-order and multi-stage symplectic integrators, and the position-first variant of the scheme.
- The implicit generalized leapfrog integrator of Riemannian manifold HMC.

## Sources

- Neal (2011): §2.3, eqs. (2.28)–(2.30) (update; volume preservation by shear maps; reversibility); eqs. (4.4)–(4.7) (stability limit $\varepsilon\lt2\sigma$); §5.1 (splitting view).
- Bou-Rabee and Sanz-Serna (2018): eqs. (3.13)–(3.15) (Strang splitting, velocity Verlet); Theorems 4.1–4.2 (symplecticness, reversibility); Examples 3.3–3.4 and Proposition 3.1 (stability, multivariate Gaussian model).
- Leimkuhler and Matthews (2015): eqs. (2.4)–(2.8); §2.4.1 and Example 2.5 (splitting); introduction of Ch. 4 (stability).
- Barbu and Zhu (2020): §9.4.3–9.4.4; Hanada and Matsuura (2022): §5.1.2, eqs. (5.11)–(5.16) (step-by-step proof of reversibility) and Exercise 5 (Jacobian).
- Hoffman and Gelman (2014): eq. 2; Betancourt (2017): §5.1; Betancourt and Girolami (2015): §III.A.1.
