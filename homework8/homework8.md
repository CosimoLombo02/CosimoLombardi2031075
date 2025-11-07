# CosimoLombardi2031075

##  Introduction

This document presents an integrated theoretical and computational analysis of two JavaScript-based simulations:

1. **Random Walk Security Simulation** — modeling weekly system security outcomes as a stochastic process driven by multiple attackers.  
2. **Mean and Variance Calculator** — a deterministic data analysis tool computing descriptive statistics over user inputs.

The discussion highlights the analogies between the *random walk simulation* and the *Bernoulli process* underlying the **Law of Large Numbers (LLN)**, and explores their shared mathematical foundations in the **binomial distribution**, **Pascal’s triangle**, **binomial expansion**, and **combinatorial coefficients**.  
Connections with the **Fibonacci sequence** and constrained random walks are also discussed.

---

##  Theoretical Framework

###  The Bernoulli Process and the Law of Large Numbers

Both simulations emerge from the same probabilistic foundation: the **Bernoulli process**.  
In the random walk simulation, each week’s outcome—secure or breached—is a Bernoulli trial with success probability:

**P_secure = (1 - p)^m**

where *p* is the breach probability per attacker, and *m* is the number of attackers.

Over *n* weeks, the number of secure outcomes *K* follows a **binomial distribution**:

**K ~ Binomial(n, P_secure)**

According to the **Law of Large Numbers**, as *n* increases, the empirical fraction *K / n* converges to *P_secure*.  
This mirrors the purpose of the mean–variance calculator: both simulate convergence — one statistically through repeated trials, the other deterministically through aggregation of data.

---

###  Random Walk Interpretation

Assigning +1 for a secure week and −1 for a breached week transforms the outcomes into a **one-dimensional random walk**.  
After *n* weeks, the cumulative score *S* is:

**S = (+1)K + (-1)(n - K) = 2K - n**

Thus, *S* can take values from *−n* to *n* in steps of 2. Each possible score corresponds to a unique number of secure weeks.

The theoretical probability that the total score equals *s* is:

**P(S = s) = C(n, k) × P_secure^k × (1 - P_secure)^(n - k)**  
where *k = (s + n) / 2* and *C(n, k)* denotes the binomial coefficient.

---

###  Binomial Coefficients and Pascal’s Triangle

The **binomial coefficients** *C(n, k)* count the number of distinct ways *k* secure outcomes can occur among *n* trials.  
They correspond directly to entries in **Pascal’s triangle**, where each coefficient satisfies:

**C(n, k) = C(n − 1, k − 1) + C(n − 1, k)**

This recursive property represents how random walk paths accumulate: the number of ways to reach a node depends on the number of paths leading to it from the previous level.  
Pascal’s triangle is therefore a visual map of all possible random walk trajectories.

---

###  Binomial Expansion and the Generating Function

The total probability across all outcomes satisfies:

**(P_secure + P_breach)^n = 1**

Expanding the left-hand side gives:

**Σ (from k = 0 to n) [C(n, k) × P_secure^k × P_breach^(n − k)] = 1**

This mirrors the **binomial expansion formula** *(a + b)^n*, where each term corresponds to one possible number of secure weeks.  
Hence, the random walk’s probability structure is mathematically equivalent to the algebraic structure of polynomial expansion.

---

###  Fibonacci Sequence and Restricted Random Walks

If the random walk is **restricted** — for example, it cannot go below zero (modeling a system that fails permanently after a breach) — the number of valid trajectories up to step *n* satisfies a **Fibonacci-like recurrence**:

**F_n = F_(n−1) + F_(n−2)**

Each valid path can be built from shorter allowed paths, leading to growth governed by Fibonacci numbers.  
This demonstrates how certain constrained stochastic processes naturally connect probability theory to combinatorial sequences.

---

##  Computational Comparison of JavaScript Implementations

###  Conceptual Differences

| Aspect | Random Walk Security Simulation | Mean–Variance Calculator |
|--------|---------------------------------|--------------------------|
| **Nature of Process** | Stochastic (Monte Carlo) | Deterministic (exact computation) |
| **Underlying Model** | Bernoulli process / Random walk | Statistical aggregation |
| **Goal** | Estimate empirical distribution of total scores | Compute descriptive statistics (mean, variance) |
| **Use of Randomness** | Yes, via Math.random() | No randomness |
| **Convergence Type** | Statistical convergence (Law of Large Numbers) | Numerical convergence (arithmetic mean) |

Despite their structural differences, both programs illustrate the same mathematical principle: the computation of averages and deviations from repeated sampling.

---

###  Structural Comparison in JavaScript

- The **random walk simulation** uses **nested loops** to simulate multiple trajectories, each consisting of *n* weekly Bernoulli draws.  
  Each trajectory’s final score is recorded, forming a histogram approximating the theoretical binomial distribution.

- The **mean–variance calculator** uses **array methods** such as `.map()` and `.reduce()` to compute mean and variance from user-provided data, reflecting deterministic aggregation instead of random sampling.

Both share:
- Iterative computation and dynamic visualization of data.  
- Use of accumulation logic (`for` loops or `reduce()`) to derive statistical summaries.  
- A clear demonstration of how empirical simulation approximates theoretical probability.

---

###  Computational Complexity

- **Random Walk Simulation:** time complexity is O(n × n_trials), since each trajectory involves *n* random draws.  
- **Mean–Variance Calculator:** time complexity is O(n), as it performs single-pass aggregation.

Thus, the first explores **statistical convergence** through repetition, while the second explores **numerical stability** over a fixed dataset.

---

##  Synthesis: Probabilistic and Combinatorial Insights

Both simulations are complementary examples of **probabilistic convergence**:

- The random walk simulation embodies the **Law of Large Numbers**, showing that empirical frequencies converge to theoretical probabilities.  
- The mean–variance calculator captures this convergence deterministically by computing mean and variance — the key descriptors of any distribution.

Both systems share the same combinatorial and statistical backbone:
- Weekly outcomes correspond to binomial coefficients.  
- The structure of Pascal’s triangle encodes all possible paths of cumulative outcomes.  
- The generating function *(P_secure + P_breach)^n* describes the same space algebraically.  
- Constrained walks produce Fibonacci recursions, linking stochastic processes with classical number sequences.

This synthesis illustrates the unity between probability, combinatorics, and computational simulation.

---

##  Conclusion

The comparison between the **Random Walk Security Simulation** and the **Mean–Variance Calculator** reveals a deep conceptual bridge between stochastic and deterministic computation.

The random walk translates probabilistic laws into a dynamic simulation of independent Bernoulli trials, while the mean–variance calculator deterministically summarizes the same structure through arithmetic operations.  
Both, however, rest upon the **binomial framework** and demonstrate convergence behavior consistent with the **Law of Large Numbers**.

The relationships among **binomial coefficients**, **Pascal’s triangle**, **binomial expansion**, and **Fibonacci recurrence** unify these models under a single mathematical theme — showing how random variability, combinatorial counting, and deterministic averages are manifestations of the same probabilistic principles.

---



