# 28. Metropolis correction for a deterministic proposal

- **Layer:** Hamiltonian Monte Carlo
- **Depends on:** [05. Change of variables and Jacobian determinants](05-change-of-variables-and-jacobians.md), [14. Metropolis–Hastings algorithm](14-metropolis-hastings-algorithm.md), [23. Canonical distribution and energy level sets](23-canonical-distribution.md), [25. Leapfrog (Störmer–Verlet) integrator](25-leapfrog-integrator.md), [26. Energy error and the modified (shadow) Hamiltonian](26-energy-error-and-shadow-hamiltonian.md)
- **Used by:** [29. The HMC algorithm and its invariance proof](29-hmc-algorithm-and-invariance.md)

## Role in HMC

A numerical trajectory does not conserve the Hamiltonian, so the leapfrog map does not leave the canonical distribution invariant. HMC removes the resulting bias by treating the end of the numerical trajectory as a Metropolis–Hastings proposal on phase space. This block states the conditions under which a deterministic map is a valid proposal and derives the acceptance probability of HMC.

## Required knowledge

- **Deterministic proposal.** The proposal from a state $z = (q,p)$ is $z' = G(z)$ for a fixed map $G$. The Metropolis–Hastings acceptance probability contains the probability of the reverse proposal. For a deterministic map this probability is nonzero only if $G(z') = z$.
- **Involution.** A map $G$ is an involution when $G(G(z)) = z$ for all $z$. The map $\Phi_\varepsilon^L$ alone is not an involution: applying it twice continues the trajectory forward.
- **Momentum flip.** Let $S(q,p) = (q,-p)$ and $G = S\circ\Phi_\varepsilon^L$. By the reversibility of the leapfrog integrator (block [25](25-leapfrog-integrator.md)), $G\circ G$ is the identity.
- **Acceptance probability.** For a differentiable involution $G$ and target density $\pi$, the acceptance probability

  $$a(z) = \min\Big(1,\ \frac{\pi(G(z))}{\pi(z)}\,\lvert\det G'(z)\rvert\Big)$$

  yields a kernel that leaves $\pi$ invariant.
- **Form in HMC.** The leapfrog map and the flip preserve volume, so $\lvert\det G'\rvert = 1$. With the canonical density $\pi(z)\propto e^{-H(z)}$ (block [23](23-canonical-distribution.md)), the acceptance probability is $a(z) = \min\big(1,\ e^{-\Delta H}\big)$ with $\Delta H = H(G(z)) - H(z)$. Since $K(-p) = K(p)$, the flip does not change $H$, and $\Delta H$ equals the energy error of block [26](26-energy-error-and-shadow-hamiltonian.md).
- **Sketch of the invariance argument.** Let $A$ be a small region of phase space and $B = G(A)$. The regions have equal volume, and $G$ maps $B$ back to $A$. The probability of a transition from $A$ to $B$ is $\pi(A)\min(1,\pi(B)/\pi(A)) = \min(\pi(A),\pi(B))$. This expression is symmetric in $A$ and $B$. Hence detailed balance holds with respect to $\pi$.
- **Rejection and the flip.** On rejection the chain remains at $z$. In standard HMC the momentum is replaced at the start of the next iteration, so the final flip has no effect on the positions and is omitted in implementations.
- **Division of roles.** Reversibility makes the proposal an involution. Volume preservation removes the Jacobian determinant. Approximate energy conservation makes the acceptance probability close to 1. The accuracy of the integrator affects the efficiency of HMC and does not affect its exactness.

## Not required

- The general theory of Metropolis–Hastings kernels on measurable spaces with Dirac measures.
- Delayed-rejection and extra-chance variants.

## Sources

- Neal (2011): §3.2, eq. (3.6) (acceptance probability); eqs. (3.7)–(3.12) (partition argument for detailed balance).
- Betancourt (2017): §5.2, Figs. 30–31 (momentum flip; a forward-only proposal has acceptance ratio zero).
- Bou-Rabee and Sanz-Serna (2018): Algorithm 5.1, eq. (5.6) and Proposition 5.1 (Metropolized reversible map with Jacobian determinant, full proof); Algorithm 5.3, Theorem 5.2 and eqs. (5.8)–(5.9) (numerical HMC). This source places the flip on rejection instead of on acceptance.
- Barbu and Zhu (2020): eq. (9.24) and Theorem 9.1; Hanada and Matsuura (2022): §5.1.3, eqs. (5.17)–(5.19).
- Bou-Rabee, Carpenter and Marsden (2026), *GIST*: §2.1, eqs. (1)–(3) and Lemma 2 (auxiliary-variable method with a measure-preserving involution).
