# 32. Integration time (trajectory length)

- **Layer:** Tuning of HMC
- **Depends on:** [15. Random-walk Metropolis and its scaling](15-random-walk-metropolis-and-its-scaling.md), [23. Canonical distribution and energy level sets](23-canonical-distribution.md), [29. The HMC algorithm and its invariance proof](29-hmc-algorithm-and-invariance.md)
- **Used by:** [33. U-turn criterion](33-u-turn-criterion.md)

## Role in HMC

The integration time $T = L\varepsilon$ determines how far a proposal lies from the current state. It is the tuning parameter of HMC with the largest effect on efficiency and the one that is hardest to set by hand. The difficulty of choosing it, and the fact that no single value is suitable for the whole target, are the motivation for the No-U-Turn Sampler.

## Required knowledge

- **Definition.** A trajectory of $L$ leapfrog steps of size $\varepsilon$ approximates the Hamiltonian flow over the time $T = L\varepsilon$. The cost of the trajectory is $L$ gradient evaluations.
- **Integration time too short.** The proposal is close to the current state. Because the momentum is replaced after every trajectory, successive short trajectories have independent directions, and the chain moves diffusively as in block [15](15-random-walk-metropolis-and-its-scaling.md).
- **Integration time too long.** On a bounded energy level set a trajectory eventually turns and approaches its starting point again. Gradient evaluations spent after this point do not increase the distance of the proposal.
- **Periodicity.** For a Gaussian target the trajectories are periodic (block [29](29-hmc-algorithm-and-invariance.md)). A fixed integration time near a multiple of the period produces proposals near the starting point for some directions, and exactly periodic trajectories violate irreducibility. Random variation of $T$ between iterations is the standard remedy.
- **Required length.** With a step size bounded by the smallest scale $\sigma_{\min}$ of the target, a trajectory must cover a distance of order $\sigma_{\max}$. Hence $L$ is of order $\sigma_{\max}/\sigma_{\min}$, compared with $(\sigma_{\max}/\sigma_{\min})^2$ iterations for random-walk Metropolis.
- **Dependence on the energy level.** The time required to traverse a level set depends on its energy. For the family $\pi_\beta(q)\propto\exp(-\lvert q\rvert^\beta)$ the optimal integration time scales as $H^{(2-\beta)/(2\beta)}$. For heavy-tailed targets, level sets of high energy require much longer trajectories. No single static integration time is therefore efficient for all level sets.
- **Trajectory as a sample of the level set.** A state selected uniformly from a trajectory approaches a draw from the microcanonical distribution on the level set as the trajectory grows (block [23](23-canonical-distribution.md)). The benefit of additional length diminishes once the trajectory has explored the level set.
- **Problem of adaptive selection.** Selecting $L$ by preliminary runs is costly. Selecting $L$ within an iteration from the states of the trajectory itself, for example by stopping at the first turn, does not in general yield a kernel that leaves the target invariant. A valid construction is given in blocks [33](33-u-turn-criterion.md)–[36](36-nuts-algorithm.md).

## Not required

- The virial-based exhaustion criterion and Exhaustive HMC.
- Convergence rates of temporal averages to spatial averages.

## Sources

- Neal (2011): §3.2 ("Ergodicity of HMC") and §4.2 (choice of trajectory length; random variation of $\varepsilon$ or $L$).
- Betancourt (2017): §4.3 (trade-off; energy-dependent optimal integration time for $\pi_\beta$).
- Betancourt (2016), *Identifying the Optimal Integration Time in HMC*: §1.2.2 (temporal and spatial averages).
- Bou-Rabee and Sanz-Serna (2018): Remark 5.2 and §5.6–5.7 (sensitivity to trajectory length; randomized duration).
- Hoffman and Gelman (2014): §1–2 (cost of a poorly chosen $L$).
- Betancourt et al. (2017): §3.3 (integration time as the fundamental parameter); BDA3: §12.4 (heuristic $\varepsilon L = 1$).
