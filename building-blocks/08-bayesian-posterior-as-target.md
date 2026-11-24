# 08. Bayesian posterior as the target density

- **Layer:** Probability
- **Depends on:** [01. Multivariable calculus](01-multivariable-calculus.md), [04. Probability densities](04-probability-densities.md), [05. Change of variables and Jacobian determinants](05-change-of-variables-and-jacobians.md)
- **Used by:** [10. Monte Carlo integration](10-monte-carlo-integration.md), [14. Metropolis–Hastings algorithm](14-metropolis-hastings-algorithm.md), [27. Phase-space target and momentum resampling](27-phase-space-target-and-momentum-resampling.md), [39. Hierarchical funnel and reparameterization](39-hierarchical-funnel-and-reparameterization.md)

## Role in HMC

The principal application of HMC and NUTS is sampling from the posterior distribution of a Bayesian model. The form of the posterior explains the two constraints under which every MCMC method operates: the normalizing constant is unavailable, and only pointwise evaluations of the unnormalized log density, and for HMC its gradient, can be computed.

## Required knowledge

- **Bayes' theorem.** For parameters $\theta$ and data $y$, $p(\theta\mid y) = p(y\mid\theta)\,p(\theta)/p(y)$. The posterior density is proportional to the product of likelihood and prior.
- **Marginal likelihood.** $p(y) = \int p(y\mid\theta)\,p(\theta)\,d\theta$ is a $d$-dimensional integral. Evaluation on a grid with $n$ points per dimension costs $n^d$ evaluations, which is infeasible beyond a few dimensions.
- **Unnormalized target.** The sampler has access to $\tilde\pi(\theta) = p(y\mid\theta)\,p(\theta)$ only. Admissible algorithms use ratios $\tilde\pi(\theta')/\tilde\pi(\theta)$ or, equivalently, differences of $\log\tilde\pi$.
- **Log posterior.** $\log\tilde\pi(\theta) = \log p(y\mid\theta) + \log p(\theta)$, which for independent observations is a sum over data points. Computation is carried out on the log scale to avoid numerical underflow.
- **Gradient of the log posterior.** HMC requires $\nabla_\theta\log\tilde\pi(\theta)$ at every integrator step. The reader must be able to derive it for a simple model, for example a Gaussian or logistic regression. In Stan, PyMC and NumPyro the gradient is supplied by automatic differentiation.
- **Inferential target.** The quantities of interest are posterior expectations $\mathbb{E}[h(\theta)\mid y] = \int h(\theta)\,p(\theta\mid y)\,d\theta$ (block [06](06-expectation-and-limit-theorems.md)).
- **Conditions imposed by HMC.** The parameters must be continuous, and the log density must be differentiable on $\mathbb{R}^d$ after constrained parameters are transformed (block [05](05-change-of-variables-and-jacobians.md)). Discrete parameters cannot be sampled by HMC directly.
- **Correspondence with physics.** The potential energy of HMC is defined as $U(q) = -\log\tilde\pi(q)$, with $q$ denoting the parameter vector (block [23](23-canonical-distribution.md)).

## Not required

- Conjugate priors, prior elicitation, model checking and model comparison.
- The internal operation of automatic differentiation.

## Sources

- BDA3: opening of Ch. 10 and the passage "Log densities" (unnormalized posterior on the log scale); §10.1 (posterior expectations).
- MacKay (2003): §29.1, eqs. (29.11)–(29.12) (cost of evaluating the normalizing constant).
- Neal (2011): eq. (3.4) (potential energy defined from prior and likelihood).
- Thomas and Tu (2021): §2 (posterior proportional to likelihood times prior); §5.2.1–5.2.3 (derivation of log posteriors and gradients for regression models).
- Hoffman and Gelman (2014): §1–2 (log density and gradient as the only inputs; automatic differentiation cited).
