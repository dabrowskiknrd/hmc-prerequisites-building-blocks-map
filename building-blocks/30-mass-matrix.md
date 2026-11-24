# 30. Mass matrix (Euclidean metric)

- **Layer:** Tuning of HMC
- **Depends on:** [02. Linear algebra](02-linear-algebra.md), [05. Change of variables and Jacobian determinants](05-change-of-variables-and-jacobians.md), [07. Multivariate Gaussian distribution](07-multivariate-gaussian-distribution.md), [29. The HMC algorithm and its invariance proof](29-hmc-algorithm-and-invariance.md)
- **Used by:** [39. Hierarchical funnel and reparameterization](39-hierarchical-funnel-and-reparameterization.md)

## Role in HMC

The mass matrix is the tuning parameter that adapts HMC to the scales and linear correlations of the target. A suitable choice makes the stability limit of the integrator and the required trajectory length uniform over all directions. Stan, PyMC and NumPyro estimate the mass matrix during warm-up.

## Required knowledge

- **Definition.** The kinetic energy is $K(p) = \tfrac12 p^\top M^{-1}p$ with $M$ symmetric positive definite. The momentum is drawn from $N(0,M)$, and Hamilton's equations are $\dot q = M^{-1}p$, $\dot p = -\nabla U(q)$. The sources also call $M$ the Euclidean metric.
- **Equivalence with a linear transformation.** Let $M = A^\top A$. HMC on $q$ with mass matrix $M$ and momentum $p$ produces the same trajectories as HMC with identity mass matrix on the transformed parameters $q' = Aq$ with momentum $p' = (A^\top)^{-1}p$. The mass matrix is therefore a linear change of variables of the target (block [05](05-change-of-variables-and-jacobians.md)), applied implicitly.
- **Optimal choice for a Gaussian target.** If $M^{-1}$ equals the covariance matrix of the target, the transformed parameters $q'$ have identity covariance. All directions then have the same scale, the stability limit is the same in every direction, and a trajectory of fixed length traverses every direction.
- **Cost without adaptation.** With identity mass matrix the step size is bounded by the smallest standard deviation $\sigma_{\min}$ of the target, and the trajectory must cover the largest standard deviation $\sigma_{\max}$. The required number of leapfrog steps is of order $\sigma_{\max}/\sigma_{\min}$.
- **Diagonal mass matrix.** A diagonal $M$ rescales each parameter separately. It is equivalent to a separate step size $\varepsilon_i$ for each parameter, with $m_i = 1/\varepsilon_i^2$ up to a common factor.
- **Dense mass matrix.** A dense $M$ additionally removes linear correlations. It requires a factorization of $M$ and matrix–vector products of cost $O(d^2)$ per leapfrog step.
- **Estimation.** $M^{-1}$ is set to an estimate of the posterior variances, or of the posterior covariance matrix, computed from draws obtained during warm-up, and is held fixed afterwards. Betancourt (2017, §4.2.1) states that the estimate is refined over successive warm-up windows; the consulted NUTS papers do not describe the procedure.
- **Limitation.** A constant mass matrix corrects global scales and linear correlations only. It cannot correct curvature that varies with position, as in the funnel of hierarchical models (block [39](39-hierarchical-funnel-and-reparameterization.md)).
- **Convention.** The sources name the same matrix, the covariance of the momentum, with different symbols: $M$ in Neal (2011) and BDA3, and $\Sigma$ in Barbu and Zhu (2020), Betancourt and Girolami (2015) and Murphy (2025). Neal (2011, §4.1) uses $\Sigma$ for the covariance of $q$, which corresponds to $M^{-1}$.

## Not required

- Position-dependent metrics: Riemannian manifold HMC and the Fisher information metric (Girolami and Calderhead, 2011, §4–6), and the SoftAbs metric.

## Sources

- Neal (2011): §4.1, eqs. (4.1)–(4.3) (equivalence with a linear transformation); eqs. (4.8)–(4.16) (per-variable step sizes).
- Betancourt (2017): §4.2.1 (Euclidean–Gaussian kinetic energy; rotation and rescaling; estimation in warm-up).
- Betancourt and Girolami (2015): §III.A; Bou-Rabee and Sanz-Serna (2018): eq. (3.27) and Proposition 3.1 (Gaussian model with general mass matrix).
- Barbu and Zhu (2020): Lemma 9.1 and §9.5.4; BDA3: §12.4.
- Betancourt et al. (2017), *The Geometric Foundations of HMC*: §3.1.3 (the Euclidean metric as the prevalent choice in applications).
- Girolami and Calderhead (2011), *Riemann manifold Langevin and HMC methods*: §7.1.4, p. 137 (different step sizes along different directions are encoded in the mass matrix; a unit mass matrix is adequate when the posterior standard deviations have similar magnitudes); §10.1.1, p. 153 (the step size of HMC is restricted by the smallest marginal variance).
