# 33. U-turn criterion

- **Layer:** No-U-Turn Sampler
- **Depends on:** [01. Multivariable calculus](01-multivariable-calculus.md), [02. Linear algebra](02-linear-algebra.md), [32. Integration time (trajectory length)](32-integration-time.md)
- **Used by:** [35. Trajectory doubling and termination](35-trajectory-doubling-and-termination.md), [36. The No-U-Turn Sampler (NUTS)](36-nuts-algorithm.md)

## Role in HMC

The U-turn criterion is the rule by which NUTS decides that a trajectory is long enough. It detects the point at which continued integration would begin to reduce the distance between the two ends of the trajectory. The criterion replaces the user-specified number of leapfrog steps.

## Required knowledge

- **Idea.** A trajectory is extended as long as its ends continue to move apart. Extension stops when one end begins to move toward the other.
- **One-sided form.** Let $q_0$ be the initial position and $(q(t),p(t))$ the state at time $t$. With identity mass matrix, $\dot q = p$. Hence

  $$\frac{d}{dt}\,\tfrac12\lVert q(t)-q_0\rVert^2 = (q(t)-q_0)\cdot p(t).$$

  A negative value means that the trajectory is moving toward its starting point.
- **Two-sided form.** NUTS extends a trajectory in both directions of time. Let $(q^-,p^-)$ and $(q^+,p^+)$ be the states at its backward and forward ends. The trajectory satisfies the U-turn criterion when

  $$(q^+-q^-)\cdot p^-\lt 0\quad\text{or}\quad(q^+-q^-)\cdot p^+\lt 0.$$

  Each condition states that further integration at the corresponding end would reduce the distance between the ends.
- **Cost.** The criterion requires two inner products and uses only the end states. No intermediate state must be stored for it.
- **Mass matrix.** With a mass matrix $M$ the velocity is $M^{-1}p$. The generalized criterion replaces $p^\pm$ by $M^{-1}p^\pm$ and the displacement $q^+-q^-$ by the sum $\rho$ of the momenta over the states of the trajectory. For identity mass matrix the sum of the momenta is proportional to a discrete approximation of $q^+-q^-$, so the generalized criterion reduces to the two-sided form. Stan uses the generalized criterion.
- **Insufficiency of the criterion alone.** The rule "integrate forward until the criterion is met and propose the final state" does not define a valid sampler. The trajectory started from the proposed state with reversed momentum does not in general stop at the original state, so the proposal is not reversible. Blocks [34](34-sampling-a-state-from-a-trajectory.md) and [35](35-trajectory-doubling-and-termination.md) give the construction that restores validity.
- **Status.** The criterion is a heuristic for the integration time of block [32](32-integration-time.md). It is not derived as an optimum.

## Not required

- The analysis of turning points under harmonic motion and the Riemannian generalization of the criterion.
- The virial-based termination criterion of Exhaustive HMC.

## Sources

- Hoffman and Gelman (2014): §3.1, eq. 3 (derivative of the squared distance); eq. 9 (two-sided condition).
- Betancourt (2017): Appendix A.4.2, eq. (3) (criterion) and the generalized form with $M^{-1}p$ and $\rho$; Appendix A.5 (use in Stan).
- Betancourt (2013), *Generalizing the No-U-Turn Sampler to Riemannian Manifolds*: section "The No U-Turn Sampler".
- Betancourt (2016), *Identifying the Optimal Integration Time in HMC*: §3.3.
- Bou-Rabee, Carpenter and Marsden (2026): eqs. (11) and (33). The sources print the two inequalities with different sign and conjunction conventions; the form above follows Hoffman and Gelman.
- BDA3: §12.4 (qualitative description).
