# 11. Markov chains and transition kernels

- **Layer:** Markov chain theory
- **Depends on:** [02. Linear algebra](02-linear-algebra.md), [04. Probability densities](04-probability-densities.md)
- **Used by:** [12. Invariant distribution, detailed balance and reversibility](12-invariant-distribution-and-detailed-balance.md)

## Role in HMC

Every MCMC method, including HMC and NUTS, is a rule that generates the next state from the current state. Such a rule is a Markov transition kernel, and the sequence of states is a Markov chain. The correctness of a sampler is a property of its kernel.

## Required knowledge

- **Markov chain.** A sequence of random states $q_0,q_1,q_2,\dots$ is a Markov chain when the conditional distribution of $q_{n+1}$ given $q_0,\dots,q_n$ depends on $q_n$ only. The chain is time-homogeneous when this conditional distribution is the same for every $n$.
- **Transition matrix (finite state space).** $P(i,j)$ is the probability of moving from state $i$ to state $j$; every row sums to 1. The $n$-step transition probabilities are the entries of $P^n$ (Chapman–Kolmogorov equation). If the row vector $\mu_0$ is the distribution of $q_0$, the distribution of $q_n$ is $\mu_0P^n$.
- **Transition kernel (continuous state space).** $T(q'\mid q)$ is the density of the next state $q'$ given the current state $q$, with $\int T(q'\mid q)\,dq' = 1$. If $q_n$ has density $\mu_n$, then $q_{n+1}$ has density $\mu_{n+1}(q') = \int \mu_n(q)\,T(q'\mid q)\,dq$.
- **Kernels without a density.** A kernel may leave the state unchanged with positive probability, as in a rejection, or may move the state by a deterministic map. Such kernels are described by the probability $K(q,A)$ of moving from $q$ into a set $A$. Both cases occur in HMC.
- **Kernel as algorithm.** A sampling algorithm is specified completely by the procedure that draws $q'$ given $q$. Analysis of the algorithm is analysis of the kernel defined by this procedure.
- **Composition and mixture.** Applying a kernel $T_1$ and then a kernel $T_2$ defines a new kernel, the composition. Selecting one of several kernels at random at each step defines a mixture. One HMC iteration is a composition of two kernels (block [29](29-hmc-algorithm-and-invariance.md)).
- **Dependence between states.** Consecutive states of a Markov chain are dependent. Averages over a chain therefore carry, in general, less information than averages over the same number of independent draws (block [18](18-mcmc-diagnostics.md)).

## Not required

- Detailed classification of states, hitting times and exit distributions.
- The measure-theoretic definition of a kernel beyond the notation $K(q,A)$.

## Sources

- Durrett (2016), *Essentials of Stochastic Processes*: §1.1, eq. (1.1) (Markov property, transition matrix); §1.2, eq. (1.2) and Theorem 1.1 (Chapman–Kolmogorov); eq. (1.7) (evolution of the marginal distribution).
- Robert and Casella (2004): Definitions 6.2 and 6.4, Lemma 6.7 (transition kernel on a general state space).
- MacKay (2003): eq. (29.39) (evolution of a density under a transition density).
- Betancourt (2017): §2.1 (Markov transition density).
