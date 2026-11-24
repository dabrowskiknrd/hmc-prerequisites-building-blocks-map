# 01. Multivariable calculus: gradient, chain rule, Taylor expansion

- **Layer:** Mathematical foundations
- **Depends on:** none (entry point)
- **Used by:** [03. Ordinary differential equations and flows](03-ordinary-differential-equations-and-flows.md), [04. Probability densities](04-probability-densities.md), [05. Change of variables and Jacobian determinants](05-change-of-variables-and-jacobians.md), [08. Bayesian posterior as the target density](08-bayesian-posterior-as-target.md), [19. Newtonian mechanics](19-newtonian-mechanics.md), [20. Hamiltonian mechanics](20-hamiltonian-mechanics.md), [21. Properties of Hamiltonian flow](21-properties-of-hamiltonian-flow.md), [33. U-turn criterion](33-u-turn-criterion.md)

## Role in HMC

Hamilton's equations are written in terms of partial derivatives of a scalar function, and every step of the numerical integrator evaluates the gradient of the log target density. The proofs that Hamiltonian dynamics conserves energy and preserves volume use the chain rule and the symmetry of second derivatives. The accuracy of an integrator is stated as a power of the step size, which is a statement about a Taylor expansion.

## Required knowledge

- **Partial derivative and gradient.** For $f:\mathbb{R}^d\to\mathbb{R}$, the gradient is $\nabla f(q) = (\partial f/\partial q_1,\dots,\partial f/\partial q_d)$. The reader must be able to compute the gradient of a log density, for example $\nabla\big(\tfrac12 q^\top A q\big) = Aq$ for a symmetric matrix $A$.
- **Multivariate chain rule.** For a curve $x(t)$ in $\mathbb{R}^n$, $\frac{d}{dt} f(x(t)) = \sum_i \frac{\partial f}{\partial x_i}\,\dot x_i(t)$. This identity yields $dH/dt = 0$ along a Hamiltonian trajectory (block [21](21-properties-of-hamiltonian-flow.md)).
- **Second derivatives.** The Hessian $\nabla^2 f$ is the matrix of second partial derivatives. For a twice continuously differentiable $f$, $\partial^2 f/\partial x_i\partial x_j = \partial^2 f/\partial x_j\partial x_i$. This symmetry produces the cancellation in the proof of volume preservation. With an identity mass matrix, the eigenvalues of the Hessian of the negative log density measure curvature and bound the stable step size (block [25](25-leapfrog-integrator.md)).
- **Jacobian matrix and divergence.** For a map $F:\mathbb{R}^n\to\mathbb{R}^n$, the Jacobian matrix has entries $\partial F_i/\partial x_j$. The divergence of a vector field is $\nabla\cdot F = \sum_i \partial F_i/\partial x_i$, which equals the trace of the Jacobian matrix, that is, the sum of its diagonal entries.
- **Taylor expansion and order notation.** For a three times continuously differentiable $f$, $f(x+\varepsilon) = f(x) + \varepsilon f'(x) + \tfrac12\varepsilon^2 f''(x) + O(\varepsilon^3)$. A quantity is $O(\varepsilon^k)$ when its absolute value is bounded by a constant times $\varepsilon^k$ for small $\varepsilon$. Local and global errors of integrators are expressed in this notation (block [24](24-numerical-integration-of-odes.md)).
- **Derivative of a squared distance.** $\frac{d}{dt}\tfrac12\lVert x(t)-x_0\rVert^2 = (x(t)-x_0)\cdot\dot x(t)$. The U-turn criterion of NUTS tests the sign of an inner product of this form at both ends of a trajectory (block [33](33-u-turn-criterion.md)).

## Not required

- Integration techniques beyond reading $\int g(q)\,dq$ as a volume-weighted sum.
- Integral theorems of vector calculus (Green, Stokes, Gauss) and differential forms.

## Sources

- Neal (2011), *MCMC using Hamiltonian dynamics*: §2.1, eqs. (2.1)–(2.3) (partial derivatives in Hamilton's equations); eq. (2.12) (chain rule); eqs. (2.13)–(2.21) (divergence, mixed partial derivatives, first-order expansion). The paper assumes this material as known.
- Vishnoi (2021), *An Introduction to HMC Method for Sampling*: §3.
- Hoffman and Gelman (2014), *The No-U-Turn Sampler*: eq. 3 (derivative of the squared distance).
