# Books on HMC, NUTS and MCMC

Books from my library (`~/Documents/eBooks`, collections `eBooks - Purchased` and `eBooks - Open Access`) that explain Hamiltonian Monte Carlo (HMC), the No-U-Turn Sampler (NUTS) or Markov chain Monte Carlo (MCMC), or the symplectic integrators that HMC relies on.

Author is the first author and Year is the year in the library file name. "Where" comes from each book's table of contents.

## Explain HMC

| Book | First author | Year | Where | Why | Folder |
|---|---|---|---|---|---|
| Bayesian Data Analysis, 3rd ed. | A. Gelman | 2021 | Ch. 11–12; §12.4 Hamiltonian Monte Carlo, §12.5 HMC for a hierarchical model, §12.6 Stan | The statistician's explanation of HMC, placed after Gibbs and Metropolis; also discusses NUTS and Stan. | Open Access/CRC Press |
| Probabilistic Machine Learning: Advanced Topics | K. P. Murphy | 2025 | Ch. 12 Markov chain Monte Carlo; §12.5 HMC (Hamiltonian mechanics, integrating Hamilton's equations, the HMC algorithm, tuning, Riemann manifold HMC) | Compact modern treatment that puts HMC in context with Metropolis–Hastings, Gibbs, Langevin and stochastic-gradient MCMC; mentions NUTS. | Open Access/MIT Press |
| Monte Carlo Methods | A. Barbu | 2020 | Ch. 9 Hamiltonian and Langevin Monte Carlo (Hamiltonian mechanics, symplectic structure, volume preservation) | The longest HMC treatment in the library, building up from Hamiltonian mechanics; no NUTS. | Purchased/Springer |
| MCMC from Scratch | M. Hanada | 2022 | Ch. 3–4 MCMC and Metropolis; §5.1 The HMC Algorithm (intuition from physics, leapfrog, univariate and multivariate HMC, tuning, debugging) | A physicist's hands-on build-up of MCMC and HMC from first principles; no NUTS. | Purchased/Springer |
| Information Theory, Inference, and Learning Algorithms | D. J. C. MacKay | 2003 | Ch. 29 Monte Carlo Methods; §30.1 Hamiltonian Monte Carlo | Short, intuitive classic on Metropolis, Gibbs and HMC. | Open Access/Cambridge |
| Bayesian Analysis with Python, 3rd ed. | O. Martin | 2024 | Ch. 10 Inference Engines (Metropolis–Hastings, Hamiltonian Monte Carlo, Sequential Monte Carlo, diagnosing the samples) | Practitioner's introduction to the samplers behind PyMC, including NUTS and sample diagnostics. | Purchased/Packt |
| An Introduction to Bayesian Inference, Methods and Computation | N. Heard | 2021 | §5.3 Markov Chain Monte Carlo; §5.4 Hamiltonian Markov Chain Monte Carlo | Brief, mathematical summary of MCMC and HMC. | Purchased/Springer |
| Bayesian Reasoning and Machine Learning | D. Barber | 2025 | Ch. 27: §27.3 Gibbs Sampling, §27.4 MCMC, §27.5.1 Hybrid Monte Carlo | Short machine-learning view of sampling, with HMC under its older name. | Open Access/Cambridge |
| Molecular Dynamics | B. Leimkuhler | 2015 | Ch. 2–3 numerical and geometric integrators (symplectic property, symplectic integrators); hybrid Monte Carlo method (p. 417) | Background on the symplectic integrators that HMC relies on. | Purchased/Springer |

## Explain MCMC (little or no HMC)

| Book | First author | Year | Where | Why | Folder |
|---|---|---|---|---|---|
| Monte Carlo Statistical Methods, 2nd ed. | C. P. Robert | 2004 | Metropolis–Hastings and Gibbs chapters; Ch. 10 The Multi-Stage Gibbs Sampler; Ch. 12 Diagnosing Convergence | The standard reference on classical MCMC; does not cover HMC. | Purchased/Springer |
| Lectures on Monte Carlo Theory | P. Lorek | 2025 | §6.3 Metropolis and Metropolis–Hastings algorithms, the Gibbs sampler | Theory-oriented lectures on Monte Carlo and MCMC. | Purchased/Springer |
| Explorations in Monte Carlo Methods, 2nd ed. | R. W. Shonkwiler | 2024 | Ch. 3 Markov Chain Monte Carlo; Ch. 6 More on Markov Chain Monte Carlo | Gentle introduction to Monte Carlo and MCMC. | Purchased/Springer |
| A First Course in Bayesian Statistical Methods | P. D. Hoff | 2009 | Ch. 10 Nonconjugate priors and Metropolis–Hastings algorithms | Introductory Bayesian text that motivates Gibbs and Metropolis–Hastings from applied problems. | Purchased/Springer |
| Essentials of Stochastic Processes, 3rd ed. | R. Durrett | 2016 | Ch. 1 Markov Chains: Stationary Distributions, Detailed Balance Condition, §1.5.1 Reversibility, §1.5.2 The Metropolis–Hastings Algorithm, Limit Behavior | The shortest rigorous route from stationary distributions and detailed balance to why Metropolis–Hastings works. | Purchased/Springer |
| Deep Learning | C. M. Bishop | 2024 | Ch. 14 Sampling: §14.2 Markov Chain Monte Carlo, §14.3 Langevin Sampling | Clear textbook chapter on Metropolis, Gibbs and Langevin sampling; HMC is only mentioned. | Purchased/Springer |
| Markov Chains, 2nd ed. | P. Brémaud | 2020 | Ch. 11 Monte Carlo Markov Chains: §11.2 Monte Carlo Markov Chain Sampling (convergence rates in MCMC, variance of Monte Carlo estimators), §11.3 The Gibbs Sampler, §11.4 Exact sampling | Rigorous chapter on MCMC within a Markov chain textbook, with the Metropolis algorithm as a worked example, convergence rates and exact sampling. | Purchased/Springer |
| Large Sample Techniques for Statistics, 2nd ed. | J. Jiang | 2022 | Ch. 15 Markov-Chain Monte Carlo: §15.2 The Gibbs sampler, §15.3 The Metropolis–Hastings algorithm, §15.4 Monte Carlo EM algorithm, §15.5 Convergence rates of Gibbs samplers | Theory-oriented chapter that includes convergence rates of Gibbs samplers. | Purchased/Springer |
| An Introduction to Bayesian Analysis | J. K. Ghosh | 2006 | Ch. 7 Bayesian Computations: §7.3 Monte Carlo Sampling; §7.4 Markov Chain Monte Carlo Methods (Markov chains in MCMC, Metropolis–Hastings algorithm, Gibbs sampling, Rao-Blackwellization, examples, convergence issues) | A Bayesian textbook's account of MCMC, from the Markov chain background to Metropolis–Hastings, Gibbs sampling and convergence issues. | Purchased/Springer |
| Statistics and Analysis of Scientific Data, 3rd ed. | M. Bonamente | 2022 | Ch. 21 Introduction to Markov Chains; Ch. 22 Markov Chain Monte Carlo (§22.3 The Metropolis–Hastings MCMC, §22.4 The Gibbs Sampler, §22.5–22.8 convergence and the Geweke, Gelman–Rubin and Raftery–Lewis diagnostics) | A scientist's introduction to MCMC that gives the most space to convergence diagnostics. | Purchased/Springer |
| Machine Learning Methods | H. Li | 2024 | Ch. 19 Markov Chain Monte Carlo Method: §19.1 Monte Carlo Method, §19.2 Markov Chain, §19.4 Metropolis–Hasting Algorithm, §19.5 Gibbs Sampling | Step-by-step machine-learning chapter that builds MCMC from Monte Carlo and Markov chain basics. | Purchased/Springer |
| Probability Theory, 3rd ed. | A. Klenke | 2020 | §18.2 Coupling and Convergence Theorem; §18.3 Markov Chain Monte Carlo Method (Metropolis algorithm, Gibbs sampler, perfect sampling) | Short, rigorous treatment inside a measure-theoretic probability textbook, placed directly after the convergence theorem for Markov chains. | Purchased/Springer |

## Explain symplectic integrators (background for HMC)

| Book | First author | Year | Where | Why | Folder |
|---|---|---|---|---|---|
| Numerical Methods for Ordinary Differential Equations | D. F. Griffiths | 2010 | Ch. 14 Geometric Integration Part I—Invariants; Ch. 15 Geometric Integration Part II—Hamiltonian Dynamics (§15.1 Symplectic Maps, §15.2 Hamiltonian ODEs, §15.3 Approximating Hamiltonian ODEs, §15.4 Modified Equations) | Short introduction to symplectic maps and to numerical methods for Hamiltonian ODEs. | Purchased/Springer |
| Numerical Approximation of Ordinary Differential Problems | R. D'Ambrosio | 2023 | Ch. 8 Geometric Numerical Integration (§8.3 Preservation of Linear and Quadratic Invariants, §8.4 Symplectic Methods, §8.5 Symmetric Methods, §8.6 Backward Error Analysis, §8.6.3 Long-Term Analysis of Symplectic Methods) | More advanced chapter on symplectic and symmetric methods and on backward error analysis, the theory behind the modified Hamiltonian. | Purchased/Springer |

## Also mention MCMC

Found by keyword counts only; chapters not checked.

- Markov Chains — R. Douc, 2018 (Purchased/Springer)
- An Introduction to Sequential Monte Carlo — N. Chopin, 2020 (Purchased/Springer)
- The Bayesian Choice, 2nd ed. — C. P. Robert, 2007 (Purchased/Springer)
- Bayesian Compendium, 2nd ed. — M. van Oijen, 2024 (Purchased/Springer)
- Machine Learning Algorithms in Depth — V. Smolyakov, 2024 (Purchased/Manning)
- Algorithms for Validation — M. J. Kochenderfer, 2025 (Open Access/MIT Press); also mentions HMC and NUTS
- Classical and Bayesian Statistical Approaches in Infectious Disease Data Analysis — N. M. Khan, 2026 (Open Access/Springer); also mentions HMC and NUTS
