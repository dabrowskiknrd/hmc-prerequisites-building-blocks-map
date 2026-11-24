# 26. Energy error and the modified (shadow) Hamiltonian

- **Layer:** Numerical integration
- **Depends on:** [06. Expectation, variance and limit theorems](06-expectation-and-limit-theorems.md), [22. Symplectic maps](22-symplectic-maps.md), [23. Canonical distribution and energy level sets](23-canonical-distribution.md), [24. Numerical integration of ODEs](24-numerical-integration-of-odes.md), [25. Leapfrog (Störmer–Verlet) integrator](25-leapfrog-integrator.md)
- **Used by:** [28. Metropolis correction for a deterministic proposal](28-metropolis-correction-for-deterministic-proposals.md), [31. Step size](31-step-size-tuning.md), [38. HMC-specific diagnostics](38-hmc-diagnostics.md)

## Role in HMC

The acceptance probability of HMC depends on the numerical trajectory only through the energy error. The modified Hamiltonian explains why this error remains small for trajectories of many steps, which is the property that permits distant proposals with high acceptance probability. The statistical properties of the energy error are the basis of step-size tuning, and the breakdown of the bounded-error regime is the origin of divergent transitions.

## Required knowledge

- **Energy error.** For a numerical trajectory of $L$ leapfrog steps started at $z$, the energy error is $\Delta H = H(\Phi_\varepsilon^L(z)) - H(z)$.
- **Backward error analysis.** A numerical integrator applied to $\dot z = f(z)$ produces, up to terms of arbitrarily high order in the step size, the exact solution of a modified equation $\dot z = \tilde f_\varepsilon(z)$.
- **Modified Hamiltonian.** For a symplectic integrator applied to a Hamiltonian system, the modified equation is again Hamiltonian. For the leapfrog integrator its Hamiltonian is $\tilde H_\varepsilon = H + \varepsilon^2 G + O(\varepsilon^4)$ for a function $G$ determined by $U$ and $K$.
- **Bounded energy error.** The numerical trajectory conserves $\tilde H_\varepsilon$ to high accuracy. Since $\tilde H_\varepsilon - H = O(\varepsilon^2)$, the value of $H$ along the numerical trajectory oscillates within a band of width $O(\varepsilon^2)$ and does not drift as the number of steps increases. A non-symplectic integrator such as the Euler method has no modified Hamiltonian, and its energy error accumulates.
- **Limits of the statement.** The series that defines $\tilde H_\varepsilon$ does not converge in general, and the statement holds up to exponentially small terms. It fails when the step size exceeds the local stability limit; the numerical trajectory then leaves the region of the initial energy, which is observed as a divergence (block [38](38-hmc-diagnostics.md)).
- **Expectation identity.** Let $z$ be distributed according to the canonical distribution (block [23](23-canonical-distribution.md)), and let the integrator be a volume-preserving bijection. Then $\mathbb{E}[e^{-\Delta H}] = 1$. By Jensen's inequality, $\mathbb{E}[\Delta H]\ge 0$.
- **Magnitude in stationarity.** For the leapfrog integrator the standard deviation of $\Delta H$ is of order $\varepsilon^2$, and the mean of $\Delta H$ is of order $\varepsilon^4$. For a target of $d$ independent components the energy errors of the components add, so the mean and the variance of $\Delta H$ are both proportional to $d\,\varepsilon^4$. This scaling determines the dependence of the step size on dimension (block [31](31-step-size-tuning.md)).

## Not required

- The Baker–Campbell–Hausdorff derivation of $\tilde H_\varepsilon$, Lie derivatives and Poisson brackets, and the explicit form of $G$.
- Proofs of the long-time energy estimates.

## Sources

- Bou-Rabee and Sanz-Serna (2018): §4.3–4.4, eqs. (4.1)–(4.10), Theorems 4.3–4.4 and Proposition 4.1 (modified equations, modified Hamiltonian, exact form for the harmonic oscillator; the long-time result is cited, not proved); Theorem 6.1, eqs. (6.2)–(6.4) (mean energy error); Propositions 6.2–6.3 (Gaussian formulas).
- Leimkuhler and Matthews (2015): §3.1, eq. (3.1), and §3.3 (formal derivation).
- Betancourt (2017): §5.1, Fig. 28 (oscillating energy error); §5.3 (modified level sets and divergences).
- Betancourt, Byrne and Girolami (2014), *Optimizing the Integrator Step Size for HMC*: §2.3.2, Lemma 5 ($\mathbb{E}[e^{\Delta}] = 1$ in that paper's sign convention).
- Beskos et al. (2013), *Optimal tuning of the Hybrid Monte Carlo algorithm*: Lemma 3.3, eqs. (3.5)–(3.6).
- Neal (2011): §4.4, eqs. (4.17)–(4.19).
