# 29. The HMC algorithm and its invariance proof

- **Layer:** Hamiltonian Monte Carlo
- **Depends on:** [12. Invariant distribution, detailed balance and reversibility](12-invariant-distribution-and-detailed-balance.md), [13. Convergence and ergodicity of Markov chains](13-convergence-and-ergodicity.md), [15. Random-walk Metropolis and its scaling](15-random-walk-metropolis-and-its-scaling.md), [27. Phase-space target and momentum resampling](27-phase-space-target-and-momentum-resampling.md), [28. Metropolis correction for a deterministic proposal](28-metropolis-correction-for-deterministic-proposals.md)
- **Used by:** [30. Mass matrix (Euclidean metric)](30-mass-matrix.md), [31. Step size](31-step-size-tuning.md), [32. Integration time (trajectory length)](32-integration-time.md), [34. Sampling a state from a trajectory](34-sampling-a-state-from-a-trajectory.md), [36. The No-U-Turn Sampler (NUTS)](36-nuts-algorithm.md)

## Role in HMC

This block assembles the preceding constructions into the algorithm and into the proof that the algorithm leaves the target invariant. It is the reference point for the tuning problems of blocks [30](30-mass-matrix.md)–[32](32-integration-time.md) and for NUTS, which replaces the fixed number of steps and the accept–reject step.

## Required knowledge

- **Inputs.** A function that evaluates $U(q) = -\log\tilde\pi(q)$ and its gradient; a step size $\varepsilon$; a number of leapfrog steps $L$; a mass matrix $M$.
- **One iteration from the current position $q$.**
  1. Draw a momentum $p\sim N(0,M)$.
  2. Apply $L$ leapfrog steps of size $\varepsilon$ to obtain $(q',p') = \Phi_\varepsilon^L(q,p)$.
  3. Accept $q'$ as the next position with probability $\min\big(1,\ \exp(H(q,p) - H(q',p'))\big)$; otherwise the next position is $q$.
  4. Discard the momentum.
- **Cost.** The $L$ leapfrog steps of one iteration require $L+1$ evaluations of the gradient, of which the first is available from the preceding iteration.
- **Invariance proof.** Step 1 is a Gibbs update of the momentum and leaves the joint density $\pi(q,p)\propto e^{-H(q,p)}$ invariant (block [27](27-phase-space-target-and-momentum-resampling.md)). Steps 2 and 3, with the proposal completed by the momentum flip to $S\circ\Phi_\varepsilon^L(q,p)$, form a Metropolis–Hastings kernel that satisfies detailed balance with respect to $\pi(q,p)$ (block [28](28-metropolis-correction-for-deterministic-proposals.md)). The flip changes neither $q'$ nor $H$, and step 4 discards the momentum, so the algorithm omits the flip; the omission leaves the sequence of positions unchanged. A composition of kernels that each leave $\pi(q,p)$ invariant leaves $\pi(q,p)$ invariant (block [12](12-invariant-distribution-and-detailed-balance.md)). The marginal of $\pi(q,p)$ over $p$ is $\pi(q)$. Hence the positions have the invariant density $\pi(q)$.
- **Convergence.** Invariance must be supplemented by irreducibility (block [13](13-convergence-and-ergodicity.md)). Momentum resampling provides it in general. An exception is a trajectory length that matches a period of the dynamics: for a standard Gaussian target with unit mass and the exact flow, the integration time $T = \pi$ maps $q$ to $-q$, and $T = 2\pi$ returns the trajectory to its starting point; with the leapfrog integrator the same occurs when $L\arccos(1-\varepsilon^2/2)$ equals $\pi$ or $2\pi$, that is, for $L\varepsilon$ close to these values. Random variation of $\varepsilon$ or $L$ between iterations removes such resonances.
- **Comparison with random-walk Metropolis.** For integration times shorter than the time to the first turn (block [32](32-integration-time.md)), the proposal lies at a distance proportional to $L\varepsilon$ and is accepted with high probability, because the energy error is small. The motion between successive momentum draws is directed, not diffusive as in random-walk Metropolis (block [15](15-random-walk-metropolis-and-its-scaling.md)).
- **Case $L = 1$.** A single leapfrog step gives the proposal $q' = q - \tfrac{\varepsilon^2}{2}M^{-1}\nabla U(q) + \varepsilon\,M^{-1}p$ with $p\sim N(0,M)$, which is the proposal of the Metropolis-adjusted Langevin algorithm (MALA). The momentum is replaced after every step, and the chain moves diffusively.
- **Restrictions.** The parameters must be continuous and unconstrained, and the log density must be differentiable. The efficiency depends strongly on $\varepsilon$, $L$ and $M$.

## Not required

- Variants reviewed by Neal (2011, §5): constraint handling by reflection, windowed acceptance, tempered trajectories, partial momentum refreshment.
- Quantitative convergence rates (Wasserstein or total-variation bounds, coupling arguments).

## Sources

- Neal (2011): §3.2 and Fig. 2 (algorithm with R implementation); eqs. (3.7)–(3.12) (proof); §3.3 (illustrations); "Ergodicity of HMC" in §3.2.
- Thomas and Tu (2021): Algorithm 2; BDA3: §12.4; Murphy (2025): §12.5, Algorithm 12.3 and eq. (12.77); MacKay (2003): §30.1, Algorithm 30.1.
- Barbu and Zhu (2020): §9.5.2 and Theorem 9.1 (proof); Hanada and Matsuura (2022): §5.1 and §5.1.3 (proof); Heard (2021): §5.4, Algorithm 3.
- Bou-Rabee and Sanz-Serna (2018): Algorithm 5.3, Theorem 5.2, Remark 5.2 and §5.6–5.7 (periodicity for the standard normal target), Remarks 6.1–6.2 (one-step case).
- Duane et al. (1987), *Hybrid Monte Carlo*, is the original publication; it is listed in the sources folder and was not available for reading.
