# 04. Probability densities: joint, marginal, conditional, unnormalized

- **Layer:** Probability
- **Depends on:** [01. Multivariable calculus](01-multivariable-calculus.md)
- **Used by:** [05. Change of variables and Jacobian determinants](05-change-of-variables-and-jacobians.md), [06. Expectation, variance and limit theorems](06-expectation-and-limit-theorems.md), [07. Multivariate Gaussian distribution](07-multivariate-gaussian-distribution.md), [08. Bayesian posterior as the target density](08-bayesian-posterior-as-target.md), [11. Markov chains and transition kernels](11-markov-chains-and-transition-kernels.md), [16. Gibbs sampling and auxiliary variables](16-gibbs-sampling-and-auxiliary-variables.md), [23. Canonical distribution and energy level sets](23-canonical-distribution.md), [34. Sampling a state from a trajectory](34-sampling-a-state-from-a-trajectory.md)

## Role in HMC

The object that every MCMC method targets is a probability density on $\mathbb{R}^d$ that is known only up to a multiplicative constant. HMC constructs a joint density over positions and momenta whose marginal over positions is the target, and it draws the momentum from a conditional density. Each of these statements uses the elementary calculus of densities.

## Required knowledge

- **Density.** A density $\pi$ on $\mathbb{R}^d$ satisfies $\pi(q)\ge 0$ and $\int \pi(q)\,dq = 1$. The probability of a region $A$ is $\int_A \pi(q)\,dq$. Probability mass is therefore the product of density and volume (block [09](09-typical-set-and-high-dimensional-geometry.md)).
- **Unnormalized density.** If $\pi = \tilde\pi/Z$ with $Z = \int\tilde\pi(q)\,dq$, then the ratio $\pi(q')/\pi(q) = \tilde\pi(q')/\tilde\pi(q)$ does not depend on $Z$. Equivalently, $\log\pi$ is determined up to an additive constant, and its gradient does not depend on $Z$.
- **Joint, marginal, conditional.** For a joint density $\pi(q,p)$, the marginal is $\pi(q) = \int \pi(q,p)\,dp$ and the conditional is $\pi(p\mid q) = \pi(q,p)/\pi(q)$. Hence $\pi(q,p) = \pi(p\mid q)\,\pi(q)$.
- **Independence.** $q$ and $p$ are independent when $\pi(q,p) = \pi(q)\,\pi(p)$.
- **Two sampling consequences.** If $(q,p)$ is a draw from $\pi(q,p)$, then $q$ alone is a draw from the marginal $\pi(q)$. If $q$ is a draw from $\pi(q)$ and $p$ is then drawn from $\pi(p\mid q)$, the pair $(q,p)$ is a draw from the joint density. HMC uses the second statement to add a momentum and the first to discard it.
- **Elementary distributions.** The uniform distribution on an interval and on a finite set; the Bernoulli distribution, realized as the event $u \lt a$ for $u$ uniform on $[0,1]$ (accept–reject decisions); the categorical distribution with given weights (selection of a state from a trajectory); the Gaussian distribution (block [07](07-multivariate-gaussian-distribution.md)).
- **Indicator function.** $\mathbb{1}[A]$ equals 1 when $A$ holds and 0 otherwise. A density proportional to $\mathbb{1}[q\in C]$ is the uniform distribution on the set $C$.

## Not required

- Measure-theoretic foundations: $\sigma$-algebras, densities with respect to general reference measures, Radon–Nikodym derivatives, and disintegration. These are needed only for the rigorous geometric formulation (Betancourt et al., 2017).

## Sources

- Betancourt (2017): §1, §1.1 (target density and parameter space); §3.2 (joint density $\pi(q,p) = \pi(p\mid q)\,\pi(q)$ and marginalization).
- Neal (2011): eqs. (3.1)–(3.3) and §3.2 (factorized joint density; the position marginal is the target).
- MacKay (2003), *Information Theory, Inference, and Learning Algorithms*: eqs. (29.8)–(29.9) (unnormalized density and normalizing constant).
- Gelman et al. (2021), *Bayesian Data Analysis* (BDA3): Ch. 10–11 use joint, marginal and conditional densities without defining them.
