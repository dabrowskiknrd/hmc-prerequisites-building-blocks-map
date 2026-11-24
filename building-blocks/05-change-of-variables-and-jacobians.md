# 05. Change of variables and Jacobian determinants

- **Layer:** Probability
- **Depends on:** [01. Multivariable calculus](01-multivariable-calculus.md), [02. Linear algebra](02-linear-algebra.md), [04. Probability densities](04-probability-densities.md)
- **Used by:** [08. Bayesian posterior as the target density](08-bayesian-posterior-as-target.md), [21. Properties of Hamiltonian flow](21-properties-of-hamiltonian-flow.md), [24. Numerical integration of ODEs](24-numerical-integration-of-odes.md), [25. Leapfrog (Störmer–Verlet) integrator](25-leapfrog-integrator.md), [28. Metropolis correction for a deterministic proposal](28-metropolis-correction-for-deterministic-proposals.md), [30. Mass matrix (Euclidean metric)](30-mass-matrix.md), [39. Hierarchical funnel and reparameterization](39-hierarchical-funnel-and-reparameterization.md)

## Role in HMC

A Metropolis–Hastings proposal that is a deterministic map requires, in general, the Jacobian determinant of the map in its acceptance probability. Hamiltonian flow and the leapfrog integrator have Jacobian determinant 1, so the factor is absent. The same formula governs two practical operations: the transformation of constrained parameters to $\mathbb{R}^d$ before sampling, and the equivalence between a mass matrix and a linear transformation of the parameters.

## Required knowledge

- **Volume factor.** For a differentiable map $F:\mathbb{R}^n\to\mathbb{R}^n$ with Jacobian matrix $J_F(x)$, a small region of volume $dV$ at $x$ is mapped to a region of volume $\lvert\det J_F(x)\rvert\,dV$.
- **Change of variables in an integral.** For a differentiable bijection $F$ with a differentiable inverse, $\int_{F(A)} g(y)\,dy = \int_A g(F(x))\,\lvert\det J_F(x)\rvert\,dx$.
- **Transformation of a density.** If $x$ has density $\pi_X$ and $y = F(x)$, then $\pi_Y(y) = \pi_X(F^{-1}(y))\,\lvert\det J_{F^{-1}}(y)\rvert$. For a linear map $y = Ax$, $\pi_Y(y) = \pi_X(A^{-1}y)/\lvert\det A\rvert$.
- **Volume-preserving map.** $F$ preserves volume when $\lvert\det J_F(x)\rvert = 1$ for all $x$. By the chain rule, $J_{F\circ G}(x) = J_F(G(x))\,J_G(x)$. Hence a composition of volume-preserving maps preserves volume.
- **Shear map.** The maps $(q,p)\mapsto(q,\;p+g(q))$ and $(q,p)\mapsto(q+h(p),\;p)$ have block-triangular Jacobian matrices with identity diagonal blocks. Their determinants are therefore 1 for any differentiable $g$ and $h$. Each sub-step of the leapfrog integrator is a shear map (block [25](25-leapfrog-integrator.md)).
- **Density is not invariant; probability is.** Under a reparameterization the density values change by the Jacobian factor, while the probability of a region does not change. Consequently the location of the mode depends on the parameterization.
- **Unconstraining transformation.** A positive parameter $\tau$ is replaced by $\lambda = \log\tau\in\mathbb{R}$. Since $d\tau/d\lambda = \tau$, the log density of $\lambda$ equals the log density of $\tau$ plus $\log\tau$. In practice HMC and NUTS are applied to the unconstrained parameters.

## Not required

- Pushforward measures and differential forms.
- Change of variables on manifolds with several coordinate charts.

## Sources

- Neal (2011): §2.2 (the Jacobian determinant as volume factor, taken as given); §2.3 (shear maps); §4.1 (density under a linear transformation).
- Betancourt (2017): §1.1 and §1.4 (density and volume transform in opposite ways under reparameterization); §3.2 (momenta transform in the opposite way to the parameters).
- BDA3: §12.5, "Transforming to log τ" (addition of $\log\tau$ to the log posterior).
- Thomas and Tu (2021), *Learning HMC in R*: §5.2.1 (log transformation of a variance parameter).
