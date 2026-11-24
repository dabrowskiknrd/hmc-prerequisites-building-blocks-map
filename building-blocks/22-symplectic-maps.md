# 22. Symplectic maps

- **Layer:** Classical mechanics
- **Depends on:** [02. Linear algebra](02-linear-algebra.md), [21. Properties of Hamiltonian flow](21-properties-of-hamiltonian-flow.md)
- **Used by:** [25. Leapfrog (Störmer–Verlet) integrator](25-leapfrog-integrator.md), [26. Energy error and the modified (shadow) Hamiltonian](26-energy-error-and-shadow-hamiltonian.md)

## Role in HMC

Symplecticness is a structural property of Hamiltonian flow that is stronger than volume preservation. The leapfrog integrator possesses it exactly. The validity of HMC requires only its consequence, volume preservation. The efficiency of HMC over long trajectories depends on symplecticness itself, because it is the hypothesis under which the numerical energy error remains bounded (block [26](26-energy-error-and-shadow-hamiltonian.md)). The sources use the term "symplectic integrator" throughout.

## Required knowledge

- **Definition.** Let $J$ be the $2d\times 2d$ block matrix with block rows $(0,\ I)$ and $(-I,\ 0)$, as in block [20](20-hamiltonian-mechanics.md). A differentiable map $\Phi$ on $\mathbb{R}^{2d}$ with Jacobian matrix $\Phi'(z)$ is symplectic when $\Phi'(z)^\top J\,\Phi'(z) = J$ for all $z$.
- **Symplectic maps preserve volume.** Taking determinants in the definition gives $\det(\Phi')^2\,\det J = \det J$. Hence $\det\Phi' = \pm1$; the value is in fact $+1$.
- **Relation to volume preservation.** For $d = 1$, a map is symplectic exactly when it preserves oriented area. For $d\gt 1$, symplecticness is strictly stronger than volume preservation.
- **Composition.** If $\Phi$ and $\Psi$ are symplectic, the chain rule gives $(\Phi\circ\Psi)' = \Phi'\,\Psi'$. Therefore $(\Phi'\Psi')^\top J\,\Phi'\Psi' = \Psi'^\top(\Phi'^\top J\,\Phi')\Psi' = \Psi'^\top J\,\Psi' = J$. Hence a composition of symplectic maps is symplectic.
- **Hamiltonian flows are symplectic.** For every Hamiltonian $H$ and every time $t$, the flow $\varphi_t$ is a symplectic map. This statement is taken from the sources without proof.
- **Symplectic integrator.** A numerical integrator is symplectic when its one-step map is a symplectic map for every step size. An integrator obtained by composing exact flows of Hamiltonians is symplectic by the two preceding items; the leapfrog integrator is of this type (block [25](25-leapfrog-integrator.md)).
- **Counterexample.** One step of the explicit Euler method on the harmonic oscillator has Jacobian determinant $1 + h^2$ for step size $h$. The method is therefore neither symplectic nor volume preserving.
- **Energy.** A symplectic integrator does not conserve $H$ exactly in general. It conserves a nearby modified Hamiltonian (block [26](26-energy-error-and-shadow-hamiltonian.md)).

## Not required

- The symplectic two-form, differential forms, Darboux coordinates and symplectic manifolds. These belong to the geometric formulation of Betancourt and Stein (2011) and Betancourt et al. (2017); the introduction of the latter paper reserves careful study of this material for readers who develop new implementations.
- Poisson brackets and Lie derivatives.

## Sources

- Neal (2011): §2.2, eq. (2.22) (symplecticness and its relation to volume preservation).
- Bou-Rabee and Sanz-Serna (2018): §2.2, eq. (2.11), Propositions 2.1–2.2 and Theorem 2.1 (definition, determinant, composition, characterization of Hamiltonian systems; stated with citations); Theorem 4.1 (splitting integrators are symplectic).
- Leimkuhler and Matthews (2015): §2.3.3, eq. (2.17); §2.3.4–2.3.5 (flow maps are symplectic; group property).
- Barbu and Zhu (2020): §9.3.3 (sketch in the language of two-forms).
- Betancourt (2017): §5.1 (symplectic integrators, informal).
