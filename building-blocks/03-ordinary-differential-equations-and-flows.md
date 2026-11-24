# 03. Ordinary differential equations and flows

- **Layer:** Mathematical foundations
- **Depends on:** [01. Multivariable calculus](01-multivariable-calculus.md), [02. Linear algebra](02-linear-algebra.md)
- **Used by:** [19. Newtonian mechanics](19-newtonian-mechanics.md), [21. Properties of Hamiltonian flow](21-properties-of-hamiltonian-flow.md), [24. Numerical integration of ODEs](24-numerical-integration-of-odes.md)

## Role in HMC

Hamilton's equations are a system of $2d$ first-order ordinary differential equations. The proposal of HMC is the solution of this system, started at the current state and followed for a fixed time. The leapfrog integrator is an approximation of the solution map, and the properties required of the proposal are stated as properties of that map.

## Required knowledge

- **Initial value problem.** A system $\dot z = f(z)$ with $z(0) = z_0$, where $f:\mathbb{R}^n\to\mathbb{R}^n$ is a vector field. For a smooth $f$ the solution $z(t)$ exists and is unique. The curve $t\mapsto z(t)$ is the trajectory through $z_0$.
- **Reduction to first order.** The second-order equation $\ddot q = a(q)$ is equivalent to the first-order system $\dot q = v$, $\dot v = a(q)$.
- **Flow map.** The flow $\varphi_t$ maps an initial state to the state at time $t$: $\varphi_t(z_0) = z(t)$. For an autonomous system, $\varphi_0$ is the identity, $\varphi_{t+s} = \varphi_t\circ\varphi_s$, and $\varphi_t^{-1} = \varphi_{-t}$.
- **Linear systems.** The solution of $\dot z = Az$ is determined by the eigenvalues of $A$: purely imaginary eigenvalues give oscillation, and eigenvalues with positive real part give exponential growth.
- **Harmonic oscillator.** The system $\dot q = p$, $\dot p = -\omega^2 q$ has the solution $q(t) = q_0\cos\omega t + (p_0/\omega)\sin\omega t$, $p(t) = -\omega q_0\sin\omega t + p_0\cos\omega t$. For $\omega = 1$ the flow is a rotation of the $(q,p)$ plane. The motion is periodic with period $2\pi/\omega$. This system is the reference example for the stability of integrators (block [24](24-numerical-integration-of-odes.md)) and for the periodicity of HMC trajectories (block [32](32-integration-time.md)).
- **Conserved quantity.** A function $I$ is conserved when $\frac{d}{dt}I(z(t)) = \nabla I(z)\cdot f(z) = 0$ along every trajectory. Trajectories then remain on level sets of $I$.
- **Divergence and volume.** A vector field with $\nabla\cdot f = 0$ generates a flow that preserves volume. The statement is used for Hamiltonian systems (block [21](21-properties-of-hamiltonian-flow.md)).

## Not required

- Analytical solution techniques beyond the linear case, and qualitative theory of nonlinear systems (Lyapunov stability, bifurcations).
- Partial differential equations and stochastic differential equations; the latter occur only in Langevin methods, which the sources treat as a comparison.

## Sources

- Bou-Rabee and Sanz-Serna (2018), *Geometric integrators and the HMC method*: §2.1, eq. (2.1) (vector field); eqs. (2.6)–(2.7) (flow and group property); Example 2.1, eqs. (2.3)–(2.5) (harmonic oscillator as a rotation).
- Neal (2011): §2.1–2.2 (solution map $T_s$ and its composition); eqs. (2.9)–(2.11) (one-dimensional example).
- Leimkuhler and Matthews (2015), *Molecular Dynamics*: eq. (1.5) and §1.5.1 (initial value problem, flow map); the book assumes a basic understanding of ordinary differential equations.
- Vishnoi (2021): §3, eqs. (1)–(4).
- Betancourt (2017): §3.1–3.2 (vector field and flow, informal).
