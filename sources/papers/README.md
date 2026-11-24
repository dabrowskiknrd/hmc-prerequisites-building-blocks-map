# Papers on HMC and NUTS

Important papers explaining Hamiltonian Monte Carlo (HMC) and the No-U-Turn Sampler (NUTS).

PDFs marked ✅ are already downloaded to `~/Documents/eBooks/Great Papers/HMC`.

Year is the publication year (journal or book); for papers that exist only on arXiv, the year first posted.

## Start here

| | Paper | Authors | Year | Why | Link |
|---|---|---|---|---|---|
| ✅ | A Conceptual Introduction to Hamiltonian Monte Carlo | M. Betancourt | 2017 | Best first read: builds the geometric intuition for how HMC works, when it succeeds and when it fails, without heavy mathematics. | [arXiv:1701.02434](https://arxiv.org/abs/1701.02434) |
| ✅ | MCMC using Hamiltonian dynamics | R. M. Neal | 2011 | The classic review (*Handbook of MCMC*, ch. 5): derives HMC from Hamiltonian dynamics and covers the leapfrog integrator, tuning and variations. | [arXiv:1206.1901](https://arxiv.org/abs/1206.1901) |
| ✅ | Markov Chain Monte Carlo and Numerical Differential Equations | J. M. Sanz-Serna | 2014 | Self-contained tutorial that follows the prerequisite chain of HMC in order: discrete Markov chains (stationary distributions, detailed balance, ergodic theorem), random-walk Metropolis, MALA, Hamiltonian dynamics, the canonical density and the Verlet integrator, then HMC with a complete proof that it preserves the target. Does not cover NUTS. | [doi:10.1007/978-3-319-01300-8_2](https://doi.org/10.1007/978-3-319-01300-8_2) · [PDF](https://sanzserna.org/wp-content/uploads/2014/01/916_cetraro.pdf) |
| ✅ | The No-U-Turn Sampler: Adaptively Setting Path Lengths in Hamiltonian Monte Carlo | M. D. Hoffman, A. Gelman | 2014 | Introduces NUTS: removes the need to choose the number of leapfrog steps, and adapts the step size automatically. | [arXiv:1111.4246](https://arxiv.org/abs/1111.4246) |
| ✅ | The Convergence of Markov Chain Monte Carlo Methods: From the Metropolis Method to Hamiltonian Monte Carlo | M. Betancourt | 2019 | History of MCMC from Metropolis to HMC; shows why HMC was needed, through the interplay of statistics and physics. | [arXiv:1706.01520](https://arxiv.org/abs/1706.01520) |
| ✅ | Learning Hamiltonian Monte Carlo in R | S. Thomas, W. Tu | 2021 | Explains HMC in statisticians' language and implements it step by step in R. | [arXiv:2006.16194](https://arxiv.org/abs/2006.16194) |

## Origin

| | Paper | Authors | Year | Why | Link |
|---|---|---|---|---|---|
| | Hybrid Monte Carlo | S. Duane, A. D. Kennedy, B. J. Pendleton, D. Roweth | 1987 | The original paper: introduces the method (as "hybrid Monte Carlo") in lattice field theory, combining molecular dynamics with a Metropolis accept/reject step. | [doi:10.1016/0370-2693(87)91197-X](https://doi.org/10.1016/0370-2693(87)91197-X) |

## Theory and geometry

| | Paper | Authors | Year | Why | Link |
|---|---|---|---|---|---|
| ✅ | The Geometry of Hamiltonian Monte Carlo | M. Betancourt, L. C. Stein | 2011 | Short early geometric view: uses symplectic geometry to characterise which Hamiltonians are admissible for HMC. | [arXiv:1112.4118](https://arxiv.org/abs/1112.4118) |
| ✅ | The Geometric Foundations of Hamiltonian Monte Carlo | M. Betancourt, S. Byrne, S. Livingstone, M. Girolami | 2017 | The rigorous counterpart of the *Conceptual Introduction*: formal foundations of HMC through measures on smooth manifolds. | [arXiv:1410.5110](https://arxiv.org/abs/1410.5110) |
| ✅ | Geometric integrators and the Hamiltonian Monte Carlo method | N. Bou-Rabee, J. M. Sanz-Serna | 2018 | Detailed survey of the numerical integrators behind HMC: why volume preservation and reversibility matter, Verlet and its alternatives, and behaviour in high dimension. | [arXiv:1711.05337](https://arxiv.org/abs/1711.05337) |
| ✅ | On the Geometric Ergodicity of Hamiltonian Monte Carlo | S. Livingstone, M. Betancourt, S. Byrne, M. Girolami | 2019 | Gives conditions under which HMC is, and is not, geometrically ergodic, depending on the tails of the target and the integration time. | [arXiv:1601.08057](https://arxiv.org/abs/1601.08057) |
| ✅ | An Introduction to Hamiltonian Monte Carlo Method for Sampling | N. K. Vishnoi | 2021 | Short, proof-oriented introduction: shows that idealized HMC preserves the target and converges for strongly convex, smooth potentials. | [arXiv:2108.12107](https://arxiv.org/abs/2108.12107) |
| ✅ | Riemann manifold Langevin and Hamiltonian Monte Carlo methods | M. Girolami, B. Calderhead | 2011 | Introduces Riemann manifold HMC, which adapts to the local geometry of the target; the PDF includes the published discussion. | [doi:10.1111/j.1467-9868.2010.00765.x](https://doi.org/10.1111/j.1467-9868.2010.00765.x) · [PDF](https://sethna.lassp.cornell.edu/pubPDF/GirolamiComment.pdf) |
| ✅ | Generalizing the No-U-Turn Sampler to Riemannian Manifolds | M. Betancourt | 2013 | Explains the dynamical reason the no-U-turn criterion works and extends it to Riemann manifold HMC. | [arXiv:1304.1920](https://arxiv.org/abs/1304.1920) |
| ✅ | On the convergence of dynamic implementations of Hamiltonian Monte Carlo and No U-Turn Samplers | A. Durmus, S. Gruffaz, M. Kailas, E. Saksman, M. Vihola | 2026 | Rigorous treatment of NUTS as a case of "dynamic HMC": proves it leaves the target invariant and gives conditions for ergodicity. | [arXiv:2307.03460](https://arxiv.org/abs/2307.03460) |

## Tuning and diagnostics

| | Paper | Authors | Year | Why | Link |
|---|---|---|---|---|---|
| ✅ | Optimal tuning of the Hybrid Monte Carlo algorithm | A. Beskos, N. Pillai, G. Roberts, J. M. Sanz-Serna, A. Stuart | 2013 | Proves that the step size should scale as d^(-1/4) in high dimension; the source of the 0.651 target acceptance rate. | [arXiv:1001.4460](https://arxiv.org/abs/1001.4460) |
| ✅ | Optimizing the Integrator Step Size for Hamiltonian Monte Carlo | M. Betancourt, S. Byrne, M. Girolami | 2014 | Geometric criterion for tuning the step size; relaxes the target acceptance rate from 0.651 to roughly 0.6–0.9. | [arXiv:1411.6669](https://arxiv.org/abs/1411.6669) |
| ✅ | Identifying the Optimal Integration Time in Hamiltonian Monte Carlo | M. Betancourt | 2016 | Shows how the geometry of HMC identifies the optimal integration time (trajectory length); introduces Exhaustive HMC. | [arXiv:1601.00225](https://arxiv.org/abs/1601.00225) |
| ✅ | Diagnosing Suboptimal Cotangent Disintegrations in Hamiltonian Monte Carlo | M. Betancourt | 2016 | Source of the energy diagnostic (E-BFMI): shows how a poor choice of momentum distribution shows up in observable diagnostics. | [arXiv:1604.00695](https://arxiv.org/abs/1604.00695) |
| ✅ | Hamiltonian Monte Carlo for Hierarchical Models | M. Betancourt, M. Girolami | 2015 | Explains the funnel-shaped pathologies of hierarchical models and how HMC copes with them, including centered and non-centered parameterisations. | [arXiv:1312.0906](https://arxiv.org/abs/1312.0906) |
| ✅ | GIST: Gibbs self-tuning for locally adaptive Hamiltonian Monte Carlo | N. Bou-Rabee, B. Carpenter, M. Marsden | 2026 | A framework that Gibbs-samples the tuning parameters; NUTS, multinomial HMC and randomized HMC are special cases, which gives a clean view of why NUTS is valid. | [arXiv:2404.15253](https://arxiv.org/abs/2404.15253) |

## Implementation

| | Paper | Authors | Year | Why | Link |
|---|---|---|---|---|---|
| ✅ | Composable Effects for Flexible and Accelerated Probabilistic Programming in NumPyro | D. Phan, N. Pradhan, M. Jankowiak | 2019 | Describes an iterative (non-recursive) formulation of NUTS that can be JIT-compiled; useful when implementing NUTS yourself. | [arXiv:1912.11554](https://arxiv.org/abs/1912.11554) |
