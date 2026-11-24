# 02. Linear algebra: positive-definite matrices, eigenvalues, determinants

- **Layer:** Mathematical foundations
- **Depends on:** none (entry point)
- **Used by:** [03. Ordinary differential equations and flows](03-ordinary-differential-equations-and-flows.md), [05. Change of variables and Jacobian determinants](05-change-of-variables-and-jacobians.md), [07. Multivariate Gaussian distribution](07-multivariate-gaussian-distribution.md), [22. Symplectic maps](22-symplectic-maps.md), [24. Numerical integration of ODEs](24-numerical-integration-of-odes.md), [30. Mass matrix (Euclidean metric)](30-mass-matrix.md), [33. U-turn criterion](33-u-turn-criterion.md)

## Role in HMC

The kinetic energy of HMC is a quadratic form defined by a symmetric positive-definite mass matrix. Determinants measure the change of volume under a map, and volume preservation is the property that removes the Jacobian factor from the acceptance probability. Eigenvalues of covariance and Hessian matrices determine the largest stable step size and the number of integrator steps that a target requires.

## Required knowledge

- **Matrix algebra.** Transpose, inverse, block matrices, and the rules $(AB)^\top = B^\top A^\top$ and $(A^{-1})^\top = (A^\top)^{-1}$.
- **Dot product and norm.** $x\cdot y = x^\top y$ and $\lVert x\rVert = \sqrt{x^\top x}$. The dot product is negative exactly when the angle between $x$ and $y$ exceeds $90^\circ$; the U-turn criterion uses this sign.
- **Quadratic form.** For a symmetric matrix $A$, the function $p\mapsto \tfrac12 p^\top A p$ has gradient $Ap$.
- **Symmetric positive-definite matrix.** $M$ is positive definite when $x^\top M x \gt 0$ for every nonzero $x$. Equivalently, all eigenvalues of $M$ are positive. The inverse of a symmetric positive-definite matrix is symmetric positive definite. A covariance matrix is symmetric positive semidefinite.
- **Eigendecomposition.** A symmetric matrix satisfies $A = Q\Lambda Q^\top$ with $Q$ orthogonal and $\Lambda$ diagonal and real. The condition number $\kappa = \lambda_{\max}/\lambda_{\min}$ measures the disparity between the largest and smallest scales of a target. For a $2\times2$ real matrix, the reader must be able to compute the eigenvalues, including a complex-conjugate pair, and their modulus; the stability analysis of the leapfrog integrator uses this computation.
- **Determinant.** $\det(AB) = \det A\,\det B$, $\det A^\top = \det A$, and the determinant of a triangular matrix equals the product of its diagonal entries. The number $\lvert\det A\rvert$ is the factor by which the linear map $x\mapsto Ax$ scales volume.
- **Orthogonal matrix.** $Q^\top Q = I$; such a matrix represents a rotation or reflection and satisfies $\lvert\det Q\rvert = 1$.
- **Cholesky factorization.** A symmetric positive-definite $\Sigma$ satisfies $\Sigma = LL^\top$ with $L$ lower triangular. The factor $L$ is used to draw Gaussian vectors with covariance $\Sigma$ and to define the linear change of variables $q' = L^{-1}q$ that removes correlation and unequal scales.

## Not required

- Singular value decomposition, Jordan form, and numerical algorithms for factorizations.
- QR decomposition and Kronecker products, which occur only in worked examples of Thomas and Tu (2021).

## Sources

- Neal (2011): eq. (2.6) (quadratic kinetic energy); eqs. (2.16)–(2.22) (determinant rules in the volume and symplecticness arguments); §4.1, eqs. (4.1)–(4.3) (linear transformations, Cholesky factor); eqs. (4.4)–(4.7) (eigenvalues of the leapfrog map). The paper assumes this material as known.
- Betancourt (2017), *A Conceptual Introduction to HMC*: §4.2 (positive-definite, diagonal and dense metrics).
- Betancourt and Girolami (2015), *HMC for Hierarchical Models*: §III.A.1 (eigenvalue form of the stability bound).
