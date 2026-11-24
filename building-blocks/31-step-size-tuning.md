# 31. Step size: stability, scaling with dimension, optimal acceptance rate

- **Layer:** Tuning of HMC
- **Depends on:** [15. Random-walk Metropolis and its scaling](15-random-walk-metropolis-and-its-scaling.md), [26. Energy error and the modified (shadow) Hamiltonian](26-energy-error-and-shadow-hamiltonian.md), [29. The HMC algorithm and its invariance proof](29-hmc-algorithm-and-invariance.md)
- **Used by:** [37. Step-size adaptation by dual averaging](37-step-size-adaptation-by-dual-averaging.md)

## Role in HMC

The step size determines both the cost of a trajectory and the probability that the trajectory is accepted. The theory of this block yields a criterion that does not depend on the target: the step size is chosen so that the average acceptance probability equals a fixed value. This criterion is the objective of the automatic adaptation in NUTS implementations (block [37](37-step-size-adaptation-by-dual-averaging.md)).

## Required knowledge

- **Trade-off.** For a fixed integration time $T = L\varepsilon$, the number of gradient evaluations is $T/\varepsilon$. A smaller step size increases the cost. A larger step size increases the energy error and lowers the acceptance probability.
- **Stability limit.** The step size must satisfy $\varepsilon\,\omega_{\max}\lt 2$, where $\omega_{\max}$ is the largest frequency of the dynamics (block [25](25-leapfrog-integrator.md)). Above this limit the trajectories are unstable, and the acceptance probability is close to 0.
- **Model for the analysis.** The target consists of $d$ independent, identically distributed components, and the chain is stationary. The energy error is then a sum of $d$ independent terms.
- **Scaling with dimension.** By block [26](26-energy-error-and-shadow-hamiltonian.md), the mean and the variance of $\Delta H$ are proportional to $d\,\varepsilon^4$. Both remain bounded as $d\to\infty$ exactly when $\varepsilon = \ell\,d^{-1/4}$ for a constant $\ell$. By the central limit theorem, $\Delta H$ is then approximately $N(\ell^4\Sigma/2,\ \ell^4\Sigma)$ for a constant $\Sigma$ that depends on the target, and the expected acceptance probability is $a(\ell) = 2\,\Phi\big(-\ell^2\sqrt{\Sigma}/2\big)$.
- **Cost.** The number of leapfrog steps per unit of integration time grows as $d^{1/4}$. With a gradient cost proportional to $d$, the computation per independent draw grows as $d^{5/4}$. The corresponding rate for random-walk Metropolis is $d^2$ (block [15](15-random-walk-metropolis-and-its-scaling.md)).
- **Optimal acceptance rate.** The efficiency is proportional to $\ell\,a(\ell)$, the accepted distance per gradient evaluation. Its maximum is attained at $a\approx 0.651$, for every target in the model class. The corresponding values are 0.234 for random-walk Metropolis and 0.574 for the Metropolis-adjusted Langevin algorithm.
- **Relaxed range.** Betancourt, Byrne and Girolami (2014) bound the cost from below and from above for general targets. The lower bound is minimized at an average acceptance probability of 0.651 and the upper bound at 0.801, and both bounds are flat between approximately 0.6 and 0.9. Values in the upper part of this range are preferred in finite dimension.
- **Limits of the criterion.** The analysis assumes a stable integrator and a static integration time. It does not apply formally to the dynamic integration times of NUTS. When divergences occur, the target acceptance value is raised, which lowers the step size (block [38](38-hmc-diagnostics.md)).

## Not required

- The proofs of the scaling limit and of the optimality result.
- Integrators with more stages per step, designed to exceed the efficiency of the leapfrog integrator.

## Sources

- Beskos et al. (2013): Theorem 3.6, eqs. (3.7)–(3.8) (scaling $d^{-1/4}$ and limiting acceptance probability); eqs. (4.1)–(4.3), Theorems 4.1–4.2 (optimal acceptance rate 0.651); eq. (2.4) (comparison with random-walk Metropolis and MALA).
- Bou-Rabee and Sanz-Serna (2018): §7.1–7.3, eqs. (7.1)–(7.2), Theorems 7.1–7.2.
- Betancourt, Byrne and Girolami (2014): Theorem 9 and Fig. 2 (range 0.6–0.9); §4 (limits of the criterion).
- Neal (2011): §4.2 (stability and step size); §4.4, eqs. (4.17)–(4.22) (informal scaling analysis).
- Betancourt (2017): §5.3; Betancourt and Girolami (2015): §III.A.1.
