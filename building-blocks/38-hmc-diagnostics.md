# 38. HMC-specific diagnostics: divergences, energy, tree depth

- **Layer:** Diagnostics
- **Depends on:** [18. MCMC output analysis](18-mcmc-diagnostics.md), [23. Canonical distribution and energy level sets](23-canonical-distribution.md), [26. Energy error and the modified (shadow) Hamiltonian](26-energy-error-and-shadow-hamiltonian.md), [36. The No-U-Turn Sampler (NUTS)](36-nuts-algorithm.md)
- **Used by:** [39. Hierarchical funnel and reparameterization](39-hierarchical-funnel-and-reparameterization.md)

## Role in HMC

HMC and NUTS produce, in addition to the draws, quantities that indicate whether the sampler has explored the target: the energy error of each trajectory, the energy of each state and the depth of each tree. These diagnostics detect failures that the general diagnostics of block [18](18-mcmc-diagnostics.md) may not detect, and each of them is explained by a specific part of the theory.

## Required knowledge

- **Divergent transition.** A transition is divergent when the energy error along its numerical trajectory exceeds a large threshold; Hoffman and Gelman, and Betancourt, use the value 1000. By block [26](26-energy-error-and-shadow-hamiltonian.md), the energy error is bounded while the step size is below the local stability limit. A divergence therefore indicates a region in which the curvature of the log density is too high for the step size.
- **Significance of divergences.** Trajectories that enter such a region are terminated, so the region is visited less often than the target requires. Estimates are then biased. A divergence is evidence of bias and not only of inefficiency.
- **Remedies for divergences.** A larger target acceptance value lowers the step size (block [37](37-step-size-adaptation-by-dual-averaging.md)). If divergences persist, the model is reparameterized (block [39](39-hierarchical-funnel-and-reparameterization.md)).
- **Energy diagnostic.** Let $E_n = H(q_n,p_n)$ be the energy recorded at iteration $n$. Momentum resampling changes the energy by a random amount; the Hamiltonian flow does not change it. If the changes $E_n - E_{n-1}$ are small relative to the spread of the energies $E_n$, the chain explores the energy distribution slowly.
- **E-BFMI.** The energy Bayesian fraction of missing information is estimated by

  $$\widehat{\text{E-BFMI}} = \frac{\sum_{n=1}^{N}(E_n - E_{n-1})^2}{\sum_{n=0}^{N}(E_n - \bar E)^2}.$$

  Betancourt reports values below 0.3 as problematic. The graphical form of the diagnostic overlays the histogram of the energy changes on the histogram of the centered energies. Low values occur for heavy-tailed targets.
- **Tree depth.** An iteration of NUTS that reaches the maximum tree depth was terminated before the U-turn criterion was met. Frequent occurrence indicates that the trajectories required by the target are longer than the limit permits, as in regions where the gradient is very small. The consequence is reduced efficiency.
- **General diagnostics.** $\hat R$, effective sample size and Monte Carlo standard error (block [18](18-mcmc-diagnostics.md)) apply unchanged.
- **Interpretation.** The absence of warnings does not prove that the chain has converged.

## Not required

- The derivation of E-BFMI from the decomposition of the variance of the energy.
- The theory of geometric ergodicity that relates tail behavior of the target to these diagnostics.

## Sources

- Betancourt (2017): §5.1 and §6.2 (divergences and their interpretation); §6.1 (E-BFMI and the threshold 0.3); §6.3 (limits of diagnostics).
- Betancourt (2016), *Diagnosing Suboptimal Cotangent Disintegrations in HMC*: §3.1–3.2 (E-BFMI, energy histograms); §4 (examples, including heavy tails).
- Betancourt and Girolami (2015): §III.A.1 (divergences in hierarchical models).
- Hoffman and Gelman (2014): eq. 8 (energy-error threshold); Betancourt (2016), *Identifying the Optimal Integration Time in HMC*: §4.2.
- Livingstone et al. (2019), *On the Geometric Ergodicity of HMC*: §7.4 (rapid gradient growth and divergences; vanishing gradients and maximum tree depth).
- Martin (2024): Ch. 10, "Diagnosing the samples" (divergences in PyMC; energy plots are not covered there).
