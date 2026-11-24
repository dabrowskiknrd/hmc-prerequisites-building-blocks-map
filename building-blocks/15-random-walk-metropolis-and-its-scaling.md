# 15. Random-walk Metropolis and its scaling

- **Layer:** MCMC algorithms
- **Depends on:** [07. Multivariate Gaussian distribution](07-multivariate-gaussian-distribution.md), [09. Typical set and high-dimensional geometry](09-typical-set-and-high-dimensional-geometry.md), [14. Metropolis–Hastings algorithm](14-metropolis-hastings-algorithm.md)
- **Used by:** [31. Step size](31-step-size-tuning.md), [32. Integration time (trajectory length)](32-integration-time.md)

## Role in HMC

Random-walk Metropolis is the baseline against which HMC is defined and measured. Its cost grows rapidly with dimension and with the disparity of scales in the target. The quantitative form of this inefficiency states the problem that the Hamiltonian proposal solves.

## Required knowledge

- **Algorithm.** The proposal is $Q(q'\mid q) = N(q'\mid q,\sigma^2 I)$, or $N(q'\mid q,\Sigma)$ for a chosen covariance. The proposal is symmetric, so the acceptance probability is $\min\big(1,\pi(q')/\pi(q)\big)$.
- **Trade-off in the proposal scale.** A large $\sigma$ produces proposals outside the typical set, which are rejected. A small $\sigma$ produces accepted proposals that are close to the current state.
- **Diffusive motion.** Successive accepted moves have independent directions. After $T$ moves of length $\varepsilon$ the chain has traveled a distance of order $\varepsilon\sqrt T$ (block [06](06-expectation-and-limit-theorems.md)). To traverse a region of length $L$ the chain needs of order $(L/\varepsilon)^2$ iterations.
- **Dependence on the scales of the target.** The move length $\varepsilon$ is bounded by the smallest standard deviation $\sigma_{\min}$ of the target, and the distance to traverse is the largest standard deviation $\sigma_{\max}$. The number of iterations per nearly independent draw is therefore of order $(\sigma_{\max}/\sigma_{\min})^2$. For correlated targets the relevant small scale is a conditional standard deviation, not a marginal one.
- **Dependence on dimension.** For a $d$-dimensional target the efficient proposal scale decreases as $2.38/\sqrt d$ times the target scale. The corresponding acceptance rate is approximately 0.44 for $d=1$ and approximately 0.23 for large $d$. The computation time per independent draw grows as $d^2$; the corresponding growth for HMC is $d^{5/4}$ (block [31](31-step-size-tuning.md)).
- **Statement of the problem solved by HMC.** An efficient proposal must cover a distance proportional to the number of elementary steps, not to its square root, and must remain in the typical set. HMC achieves this by retaining the direction of motion over many steps (a momentum) and by using the gradient of the log density to bend the path along the typical set.

## Not required

- The diffusion-limit proofs of the optimal acceptance rate.
- Adaptive Metropolis algorithms and Langevin (MALA) proposals, which the sources treat as separate methods.

## Sources

- MacKay (2003): §29.4, eq. (29.32) and Exercise 29.3 (the $(L/\varepsilon)^2$ rule); the passage on the Metropolis method in high dimensions.
- BDA3: §12.2 (scale $2.4/\sqrt d$; acceptance rates 0.44 and 0.23).
- Murphy (2025): §12.2.3.2 (variance $2.38^2/D$; acceptance rate 0.234).
- Neal (2011): §3.3 (random-walk behavior compared with HMC on a correlated Gaussian); §4.4 (growth of cost as $d^2$ against $d^{5/4}$).
- Betancourt (2017): §2.4 (failure of random-walk Metropolis in high dimension).
- Betancourt (2019): §3.1; Hoffman and Gelman (2014): §1.
