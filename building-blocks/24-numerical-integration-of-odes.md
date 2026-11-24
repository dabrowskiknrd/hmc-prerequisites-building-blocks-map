# 24. Numerical integration of ODEs: Euler's method, order, stability

- **Layer:** Numerical integration
- **Depends on:** [02. Linear algebra](02-linear-algebra.md), [03. Ordinary differential equations and flows](03-ordinary-differential-equations-and-flows.md)
- **Used by:** [25. Leapfrog (Störmer–Verlet) integrator](25-leapfrog-integrator.md)

## Role in HMC

Hamilton's equations for a posterior density have no closed-form solution, so the flow is replaced by a numerical integrator. The concepts of this block state how the error of an integrator depends on the step size and when the numerical solution becomes unstable. The failure of Euler's method on Hamiltonian systems shows which properties the integrator of HMC must have.

## Required knowledge

- **One-step integrator.** A map $\psi_h$ that approximates the flow $\varphi_h$ over a step of size $h$. The numerical trajectory is $z_{n+1} = \psi_h(z_n)$.
- **Explicit Euler method.** $z_{n+1} = z_n + h\,f(z_n)$. For Hamilton's equations: $q' = q + h\,M^{-1}p$ and $p' = p - h\,\nabla U(q)$, both evaluated at the old state.
- **Local error and order.** An integrator has order $\nu$ when $\psi_h(z) - \varphi_h(z) = O(h^{\nu+1})$. This difference is the local error.
- **Global error.** Over a fixed time interval of length $T$, the integrator takes $T/h$ steps. For a stable integrator of order $\nu$ the accumulated error is $O(h^\nu)$. The Euler method has order 1, and the leapfrog integrator has order 2.
- **Linear stability.** On the harmonic oscillator $\dot q = p$, $\dot p = -\omega^2q$, one step of a linear integrator is multiplication by a $2\times2$ matrix $A_h$. The numerical solution remains bounded only if no eigenvalue of $A_h$ has modulus greater than 1.
- **Failure of the Euler method.** For the Euler method the eigenvalues of $A_h$ are $1\pm ih\omega$, with modulus $\sqrt{1+h^2\omega^2}\gt 1$. The numerical solution spirals outward for every step size, and the energy grows without bound. The determinant of $A_h$ is $1+h^2\omega^2$, so the method does not preserve volume. The method is also not reversible.
- **Consequence for HMC.** An integrator that does not preserve volume would require a Jacobian determinant in the acceptance probability. An integrator whose energy error grows along the trajectory gives low acceptance probabilities for long trajectories.
- **Symplectic Euler method.** $p' = p - h\,\nabla U(q)$, followed by $q' = q + h\,M^{-1}p'$ with the updated momentum. Each of the two updates is a shear map, so the method preserves volume. Its order is 1, and it is not reversible. The leapfrog integrator corrects both deficiencies (block [25](25-leapfrog-integrator.md)).
- **Cost.** The cost of an integrator in HMC is the number of evaluations of $\nabla U$.

## Not required

- Runge–Kutta, multistep and implicit methods.
- Adaptive step-size control, which is incompatible with the reversibility that HMC requires.

## Sources

- Neal (2011): §2.3, eqs. (2.24)–(2.27) and Fig. 1 (Euler and modified Euler); statement of local and global error orders.
- Bou-Rabee and Sanz-Serna (2018): §3.1–3.2, eqs. (3.1)–(3.5) and Theorem 3.1 (integrators, order, convergence); Example 3.1 and eqs. (3.22)–(3.25) (stability; Euler on the oscillator).
- Leimkuhler and Matthews (2015): §2.1, eqs. (2.10)–(2.12) (local and global error); §2.3.2 and eqs. (2.18)–(2.19) (Euler and symplectic Euler); introduction of Ch. 4 (stability threshold).
- Barbu and Zhu (2020): §9.4.1–9.4.2; Murphy (2025): eqs. (12.72)–(12.74); Hanada and Matsuura (2022): eq. (5.4).
