# CosimoLombardi2031075

## Comparative Study: Mean and Variance Calculator vs Bernoulli Random-Walk Simulation

## Introduction

This document provides an academic comparison between two JavaScript-based simulations:

1. **Mean and Variance Calculator** — a deterministic tool computing statistical moments from user input.
2. **Bernoulli Random-Walk Simulation** — a stochastic simulation illustrating the probabilistic evolution of a system over multiple trials, following Bernoulli and Binomial principles.

The analysis explores their conceptual connections, mathematical foundations, and implementation details, highlighting analogies related to the **Law of Large Numbers (LLN)**, **Binomial coefficients**, **Pascal’s triangle**, and **combinatorial sequences** such as **Fibonacci**.

---

## Conceptual Framework

| Aspect | Mean & Variance Calculator | Random-Walk Simulation |
|--------|-----------------------------|------------------------|
| **Nature** | Deterministic computation of descriptive statistics. | Stochastic simulation of repeated Bernoulli trials. |
| **Objective** | Compute mean and variance of a given dataset. | Model a system’s cumulative score over n weeks and m attackers. |
| **Underlying Model** | Empirical data analysis. | Random walk governed by Binomial distribution. |
| **Law of Large Numbers** | Mean stabilizes as sample size grows. | Distribution converges to theoretical Binomial shape as n increases. |

Both programs empirically reflect **statistical convergence**: one through data aggregation, the other through probability simulation.

---

## Mathematical Interpretation

### Mean and Variance Calculator

For a data vector X = [x1, x2, ..., xn]:

Mean: X̄ = (1/n) * Σ(xi)  
Variance: Var(X) = (1/(n-1)) * Σ(xi - X̄)²

This expresses the **empirical estimation** of population parameters and embodies the **Law of Large Numbers** through convergence of X̄ to E[X].

### Random-Walk Simulation

Each week represents a **Bernoulli trial**:

Xi = +1 if the server remains secure  
Xi = -1 if a breach occurs

The breach probability is q = 1 - (1 - p)^m.

The total score after n weeks is:

Sn = Σ(Xi) = n - 2k

The distribution of Sn follows a **Binomial law**:

P(Sn = n - 2k) = C(n, k) * q^k * (1 - q)^(n - k)

This connects directly to **Pascal’s triangle**, **Binomial coefficients**, and the **Binomial expansion** of (q + (1 - q))^n = 1.

---

## JavaScript Code Structure Comparison

### Architectural Design

| Feature | Mean/Variance | Random-Walk |
|----------|----------------|--------------|
| **Input** | User-provided numeric list. | Parameters: n (weeks), m (attackers), p (breach probability), and number of runs. |
| **Core Computation** | Simple aggregation (sum, mean, squared deviation). | Monte Carlo simulation using random sampling. |
| **Randomness** | None (deterministic). | Essential (Bernoulli process). |
| **Output Type** | Textual mean and variance. | Dynamic visual plots using Canvas (trajectories + histogram). |
| **Convergence Mechanism** | Increasing sample size. | Increasing number of runs (R) and trials (n). |

### Function-Level Comparison

| Function | Mean/Variance Calculator | Random-Walk Simulation |
|-----------|--------------------------|------------------------|
| `mean()` | Computes arithmetic mean. | N/A (expected value derived from q). |
| `variance()` | Computes unbiased variance. | Variance implicit in 4nq(1 - q). |
| `addValue()` | Appends a numeric entry. | N/A (values generated randomly). |
| `simulateWalks()` | N/A | Loops over n steps × R runs using Math.random(). |
| `drawWalks()` | N/A | Uses Canvas to plot random trajectories and distributions. |

The **Mean/Variance code** works with user data and produces fixed results, while the **Random-Walk code** generates stochastic data through repeated random sampling.

---

## Mathematical Analogies and Extensions

| Concept | Mean/Variance | Random-Walk |
|----------|----------------|-------------|
| **Bernoulli Process** | Implicit (user data may represent outcomes). | Explicit (each trial simulated). |
| **Law of Large Numbers** | Mean converges to population mean. | Frequencies converge to Binomial probabilities. |
| **Binomial Coefficients** | Not used. | Define outcome probabilities. |
| **Pascal’s Triangle** | Not applied. | Represents recursive structure of coefficients. |
| **Binomial Expansion** | Not applied. | Ensures total probability equals 1. |
| **Fibonacci Sequence** | None. | Related in restricted walk paths. |
| **Combinatorial Coefficients** | Implicit. | Explicit through C(n, k). |

---

## JavaScript Logic and Algorithmic Depth

### Mean & Variance Calculator
- **Algorithmic complexity:** O(n)
- **Key operations:** summation, squaring, averaging.
- **Focus:** precise arithmetic manipulation, no randomness.
- **Purpose:** empirical verification of LLN using static data.

### Random-Walk Simulation
- **Algorithmic complexity:** O(nR), with R = number of runs.
- **Key operations:** random sampling, accumulation, histogram calculation.
- **Focus:** probabilistic convergence to Binomial distribution.
- **Purpose:** visualize stochastic behavior and link to combinatorial mathematics.

---

## Theoretical and Didactic Implications

Both simulations demonstrate **convergence in probability and statistics**.

- The **Mean/Variance Calculator** shows convergence of sample statistics to theoretical expectations.
- The **Random-Walk Simulation** shows convergence of distributions to theoretical Binomial laws.

Their mathematical unity lies in the **Law of Large Numbers**, which governs both.

---

## Connections to Combinatorial Structures

- **Binomial coefficients (C(n, k))** form the foundation of the random-walk outcome probabilities.  
- **Pascal’s triangle** provides a recursive framework for these coefficients.  
- **Binomial expansion** ensures that the total probability across outcomes sums to one.  
- **Fibonacci sequence** appears when random walks are restricted, linking stochastic recurrence with combinatorial growth patterns.

---

## Conclusion

Despite distinct purposes, both codes share a **common probabilistic structure**.

| Category | Mean/Variance | Random-Walk |
|-----------|----------------|--------------|
| **Type** | Deterministic tool | Stochastic simulation |
| **Mathematical Core** | Sample statistics | Bernoulli and Binomial processes |
| **Convergence** | LLN (empirical mean → expected mean) | LLN (empirical distribution → theoretical Binomial) |
| **Combinatorial Link** | Minimal | Strong (Pascal, Binomial, Fibonacci) |

Thus, the **Mean and Variance Calculator** provides analytical understanding of empirical statistics, while the **Bernoulli Random-Walk Simulation** visually demonstrates how randomness and combinatorics intertwine to reveal fundamental probabilistic laws.

