# 39. Hierarchical funnel and reparameterization

- **Layer:** Diagnostics
- **Depends on:** [05. Change of variables and Jacobian determinants](05-change-of-variables-and-jacobians.md), [08. Bayesian posterior as the target density](08-bayesian-posterior-as-target.md), [30. Mass matrix (Euclidean metric)](30-mass-matrix.md), [38. HMC-specific diagnostics](38-hmc-diagnostics.md)
- **Used by:** none (end point)

## Role in HMC

Hierarchical models are the most common setting in which HMC and NUTS report divergences. Their posterior density has a region in which the scale of some parameters varies over orders of magnitude, so that no single step size and no constant mass matrix are adequate. The example connects the stability limit of the integrator, the limits of the mass matrix and the divergence diagnostic, and it introduces the standard remedy.

## Required knowledge

- **Hierarchical model.** Local parameters $\theta_1,\dots,\theta_n$ are given a common prior that depends on global parameters $\phi$: $\pi(\theta,\phi\mid y)\propto\prod_i\pi(y_i\mid\theta_i)\,\pi(\theta_i\mid\phi)\,\pi(\phi)$. In the normal case, $\theta_i\sim N(\mu,\tau^2)$ with $\phi = (\mu,\tau)$.
- **Funnel.** The scale of the local parameters is the global parameter $\tau$. For small $\tau$ the local parameters are confined to a narrow region of high density and small volume, the neck. For large $\tau$ they occupy a wide region of low density and large volume, the mouth. Both regions carry non-negligible probability mass.
- **Consequence for the integrator.** The largest frequency of the dynamics is of order $1/\tau$ in the neck. The stability limit of the leapfrog integrator (block [25](25-leapfrog-integrator.md)) therefore varies with position. A step size adapted to the bulk of the target exceeds the limit in the neck, and trajectories that enter the neck diverge (block [38](38-hmc-diagnostics.md)). A step size small enough for the neck is inefficient elsewhere.
- **Consequence for the mass matrix.** The correlation and scale structure depends on the position. A constant mass matrix is a single linear transformation and cannot remove it (block [30](30-mass-matrix.md)).
- **Centered parameterization.** The model is sampled in the variables $(\theta,\mu,\tau)$ with $\theta_i\sim N(\mu,\tau^2)$.
- **Non-centered parameterization.** The model is sampled in the variables $(\vartheta,\mu,\tau)$ with $\vartheta_i\sim N(0,1)$, and the local parameters are computed as $\theta_i = \mu + \tau\,\vartheta_i$. The prior of $\vartheta$ does not depend on $\tau$, so the prior dependence that produces the funnel is removed. The distribution of $\theta$ is unchanged; only the coordinates in which the sampler operates differ (block [05](05-change-of-variables-and-jacobians.md)).
- **Choice between the parameterizations.** The non-centered parameterization is preferable when the data inform the local parameters weakly. The centered parameterization is preferable when the data inform them strongly.
- **Unconstrained scale.** The sampler operates on $\log\tau$ and includes the Jacobian term of block [05](05-change-of-variables-and-jacobians.md).

## Not required

- Riemannian manifold HMC (Girolami and Calderhead, 2011, §6), which addresses position-dependent curvature by a position-dependent metric; the authors state a worst-case cost per update that is cubic in the dimension (§11, p. 153).
- Parameter expansion and model-specific Gibbs strategies for hierarchical models.

## Sources

- Betancourt and Girolami (2015): §I, eqs. (1)–(2) and Figs. 2–3 (hierarchical model and funnel); §II (failure of constant rotations and rescalings); §III.A.1 (stability and divergences); §II.B and §IV, eqs. (3)–(4) (centered and non-centered parameterizations).
- Betancourt (2017): §6.2; Betancourt (2016), *Diagnosing Suboptimal Cotangent Disintegrations in HMC*: §4.3.
- Neal (2011): §4.5; Thomas and Tu (2021): §5.2.3.
- BDA3: §12.5 (HMC for a hierarchical model; transformation to $\log\tau$).
- Martin (2024): Ch. 10 (funnel example; centered and non-centered models).
