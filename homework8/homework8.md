# CosimoLombardi2031075


# Comparative Study: Mean and Variance Calculator vs Bernoulli Random-Walk Simulation

##  Introduction

This document provides an academic comparison between two JavaScript-based simulations:

1. **Mean and Variance Calculator** — a deterministic tool computing statistical moments from user input.
2. **Bernoulli Random-Walk Simulation** — a stochastic simulation illustrating the probabilistic evolution of a system over multiple trials, following Bernoulli and Binomial principles.

The analysis explores their conceptual connections, mathematical foundations, and implementation details, highlighting analogies related to the **Law of Large Numbers (LLN)**, **Binomial coefficients**, **Pascal’s triangle**, and **combinatorial sequences** such as **Fibonacci**.

---

##  Conceptual Framework

| Aspect | Mean & Variance Calculator | Random-Walk Simulation |
|--------|-----------------------------|------------------------|
| **Nature** | Deterministic computation of descriptive statistics. | Stochastic simulation of repeated Bernoulli trials. |
| **Objective** | Compute mean and variance of a given dataset. | Model a system’s cumulative score over n weeks and m attackers. |
| **Underlying Model** | Empirical data analysis. | Random walk governed by Binomial distribution. |
| **Law of Large Numbers** | Mean stabilizes as sample size grows. | Distribution converges to theoretical Binomial shape as n → ∞. |

Both programs empirically reflect **statistical convergence**: one through data aggregation, the other through probability simulation.

---

##  Mathematical Interpretation

###  Mean and Variance Calculator

For a data vector \( X = [x_1, x_2, \dots, x_n] \):

\[
\bar{X} = \frac{1}{n} \sum_{i=1}^n x_i, \quad
Var(X) = \frac{1}{n-1} \sum_{i=1}^n (x_i - \bar{X})^2
\]

This expresses the **empirical estimation** of population parameters and embodies the **Law of Large Numbers** through convergence of \( \bar{X} \) to \( E[X] \).

###  Random-Walk Simulation

Each week represents a **Bernoulli trial**:

\[
X_i = 
\begin{cases} 
+1 & \text{if secure (no breach)} \\
-1 & \text{if breached}
\end{cases}
\]

with probability \( P(X_i = -1) = q = 1 - (1-p)^m \).

The total score after n weeks:

\[
S_n = \sum_{i=1}^n X_i = n - 2k
\]

follows a **Binomial distribution**:

\[
P(S_n = n-2k) = \binom{n}{k} q^k (1-q)^{n-k}
\]

This directly connects to **Pascal’s triangle**, **Binomial coefficients**, and the **Binomial expansion** \( (q + (1-q))^n = 1 \).

---

##  JavaScript Code Structure Comparison

###  Architectural Design

| Feature | Mean/Variance | Random-Walk |
|----------|----------------|--------------|
| **Input** | User-provided numeric list. | Parameters: n (weeks), m (attackers), p (breach probability), number of runs. |
| **Core Computation** | Simple aggregation (sum, mean, squared deviation). | Monte Carlo simulation using `Math.random()`. |
| **Randomness** | None (deterministic). | Essential (Bernoulli sampling). |
| **Output Type** | Textual mean and variance values. | Dynamic canvas plots (trajectories + histogram). |
| **Convergence Mechanism** | Increasing sample size. | Increasing number of runs (R) and trials (n). |

###  Function-Level Comparison

| Function | Mean/Variance Calculator | Random-Walk Simulation |
|-----------|--------------------------|------------------------|
| `mean()` | Computes arithmetic mean. | N/A (expected value derived from theoretical q). |
| `variance()` | Computes unbiased variance. | Variance implicit in Binomial variance \(4nq(1-q)\). |
| `addValue()` | Appends new numeric entry to dataset. | N/A (random draws performed programmatically). |
| `simulateWalks()` | N/A | Iterates through n steps × R simulations using `Math.random()`. |
| `drawWalks()` | N/A | Uses Canvas API to visualize trajectories and distributions. |

The **Mean/Variance code** operates on static data vectors and computes deterministic results, while the **Random-Walk code** dynamically generates probabilistic data using random sampling.

---

##  Mathematical Analogies and Extensions

| Concept | Connection to Mean/Variance | Connection to Random-Walk |
|----------|-----------------------------|---------------------------|
| **Bernoulli Process** | Implicit — user data may represent outcomes of trials. | Explicit — each trial simulated with `Math.random()`. |
| **Law of Large Numbers** | Sample mean converges to population mean. | Empirical frequency converges to Binomial distribution. |
| **Binomial Coefficients** | None. | Directly defines outcome probabilities. |
| **Pascal’s Triangle** | Not used. | Encodes recursive computation of Binomial coefficients. |
| **Binomial Expansion** | Not used. | Foundation of probability normalization. |
| **Fibonacci Sequence** | None. | Related in constrained random-walk path counting. |
| **Combinatorial Coefficients** | Implicitly in sample counting. | Explicitly through \( \binom{n}{k} \) combinatorics. |

---

##  JavaScript Logic and Algorithmic Depth

### Mean & Variance Calculator
- **Algorithmic complexity:** \( O(n) \).
- **Key operations:** summation, squaring, averaging.
- **Focus:** precise arithmetic manipulation, no stochastic components.
- **Purpose:** empirical validation of LLN via user-generated input variability.

### Random-Walk Simulation
- **Algorithmic complexity:** \( O(nR) \), where R = number of simulated runs.
- **Key operations:** random sampling, accumulation, histogram construction.
- **Focus:** probabilistic convergence to Binomial distribution.
- **Purpose:** visualize stochastic structure and link to combinatorial mathematics.

---

##  Theoretical and Didactic Implications

Both simulations serve as **didactic demonstrations of convergence** in probability and statistics.

- The **Mean/Variance Calculator** represents **descriptive stability**: sample moments approach theoretical moments as data size grows.
- The **Random-Walk Simulation** represents **distributional stability**: empirical frequencies converge to the theoretical Binomial law.

Their mathematical unity lies in the **Law of Large Numbers**, which underpins both.

---

##  Connections to Combinatorial Structures

- **Binomial coefficients (\( \binom{n}{k} \))** form the algebraic skeleton of the random-walk distribution.
- **Pascal’s triangle** represents recursive computation of these coefficients.
- **Binomial expansion** ensures the normalization condition of total probability.
- **Fibonacci sequence** arises when random walks are constrained by adjacency or boundary conditions, linking stochastic recurrence to combinatorial growth.

---

##  Conclusion

The two codes, though distinct in implementation, share a **common mathematical core**.  
Both explore how **repeated trials**, whether deterministic or random, exhibit convergence towards **expected theoretical values**.

| Category | Mean/Variance | Random-Walk |
|-----------|----------------|--------------|
| **Type** | Deterministic tool | Stochastic simulation |
| **Mathematical Core** | Sample statistics | Bernoulli trials & Binomial law |
| **Convergence** | LLN (empirical mean → E[X]) | LLN (distribution → Binomial law) |
| **Combinatorial Link** | Minimal | Strong (Pascal, Binomial, Fibonacci) |

Thus, the **Mean and Variance Calculator** exemplifies *analytical computation* of statistical measures, while the **Bernoulli Random-Walk Simulation** embodies *probabilistic modeling*, visually revealing the structure of the **Binomial process**, the **Law of Large Numbers**, and its deep **combinatorial relationships**.


