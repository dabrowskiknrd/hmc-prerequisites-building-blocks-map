# 37. Step-size adaptation by dual averaging

- **Layer:** No-U-Turn Sampler
- **Depends on:** [31. Step size](31-step-size-tuning.md), [36. The No-U-Turn Sampler (NUTS)](36-nuts-algorithm.md)
- **Used by:** none (end point)

## Role in HMC

Block [31](31-step-size-tuning.md) reduces the choice of the step size to a target value for the average acceptance probability. This block describes the procedure that finds the corresponding step size automatically during warm-up. Together with NUTS, it removes the need for manual tuning runs.

## Required knowledge

- **Problem.** Find the step size $\varepsilon$ at which the expected acceptance statistic equals a target value $\delta$. The statistic can be observed only with noise, one value per iteration.
- **Stochastic approximation (Robbins–Monro).** Let $H_t$ be a noisy observation with $h(x) = \mathbb{E}[H_t\mid x]$. The recursion $x_{t+1} = x_t - \eta_tH_t$ converges to a root of $h$ when the step sizes satisfy $\sum_t\eta_t = \infty$ and $\sum_t\eta_t^2\lt\infty$. For step-size adaptation, $x = \log\varepsilon$ and $H_t = \delta - \alpha_t$, where $\alpha_t$ is the acceptance statistic of iteration $t$.
- **Dual averaging.** Hoffman and Gelman use the scheme

  $$x_{t+1} = \mu - \frac{\sqrt t}{\gamma}\,\frac{1}{t+t_0}\sum_{i=1}^{t}H_i,\qquad \bar x_{t+1} = \eta_t\,x_{t+1} + (1-\eta_t)\,\bar x_t,\qquad \eta_t = t^{-\kappa}.$$

  The iterate $x_{t+1}$ is used in the next warm-up iteration, and the averaged iterate $\bar x$ converges. The reported settings are $\gamma = 0.05$, $t_0 = 10$, $\kappa = 0.75$ and $\mu = \log(10\,\varepsilon_1)$ for an initial step size $\varepsilon_1$.
- **Acceptance statistic for HMC.** $\alpha_t$ is the Metropolis acceptance probability $\min(1,e^{-\Delta H})$ of iteration $t$.
- **Acceptance statistic for NUTS.** NUTS has no accept–reject step. The statistic is the average, over the states $z$ added in the final doubling, of $\min\big(1,\ e^{-H(z)+H(z_0)}\big)$.
- **Assumption.** The expected acceptance statistic is assumed to be a non-increasing function of $\varepsilon$.
- **Target value.** The theoretical optimum for HMC is $\delta\approx0.65$ (block [31](31-step-size-tuning.md)). Hoffman and Gelman recommend $\delta = 0.6$ for NUTS. Implementations expose $\delta$ as a setting, for example `adapt_delta` in Stan and `target_accept` in PyMC, and a larger value yields a smaller step size.
- **Initial step size.** Starting from $\varepsilon = 1$, the step size is doubled or halved until the acceptance probability of a single leapfrog step crosses 0.5.
- **Restriction to warm-up.** During adaptation the kernel depends on the history of the chain, and invariance of the target is not guaranteed. After warm-up the step size is fixed at $\exp(\bar x)$, and only the subsequent draws are used for inference (block [18](18-mcmc-diagnostics.md)).

## Not required

- The convergence theory of stochastic approximation and of dual averaging.
- Adaptation of the step size within an iteration (locally adaptive samplers).

## Sources

- Hoffman and Gelman (2014): §3.2, eqs. 13–15 (stochastic approximation); eqs. 16–17 (dual averaging and its settings); eqs. 18–19 (acceptance statistics for HMC and NUTS); Algorithm 4 (initial step size); Algorithms 5–6 (HMC and NUTS with dual averaging); §4.4 (recommended $\delta$).
- Betancourt (2017): §5.3 (adaptation to a target average acceptance during warm-up).
- Betancourt and Girolami (2015): Figs. 5 and 9 (effect of raising the target value).
- Martin (2024), *Bayesian Analysis with Python*: Ch. 10 (`target_accept` in PyMC).
- BDA3: §12.4 (tuning restricted to warm-up).
