# CosimoLombardi2031075


---

###  Recurrence for the arithmetic mean

Given data `x_1, x_2, ..., x_n`, define:

```
mean_k = (1 / k) * (x_1 + x_2 + ... + x_k)
```

When a new value `x_n` arrives, the mean after *n* observations satisfies:

```
mean_n = (1 / n) * (x_1 + x_2 + ... + x_{n-1} + x_n)
       = ((n - 1) / n) * mean_{n-1} + (1 / n) * x_n
       = mean_{n-1} + (x_n - mean_{n-1}) / n
```

So the incremental update is:

```
mean_n = mean_{n-1} + (x_n - mean_{n-1}) / n
```

This is algebraically equivalent to recomputing the mean from scratch,  
but it only requires the previous mean and the new value.

---

###  Recurrence for variance using M2 (Welford’s method)

Define `M2_k` as the sum of squared deviations about the current mean:

```
M2_k = (x_1 - mean_k)^2 + (x_2 - mean_k)^2 + ... + (x_k - mean_k)^2
```

From `M2_k` we obtain:

```
population variance after k values: var_pop_k  = M2_k / k
sample (unbiased) variance after k values: var_samp_k = M2_k / (k - 1), for k >= 2
```

We claim the recurrence:

```
M2_n = M2_{n-1} + (x_n - mean_{n-1}) * (x_n - mean_n)
```

#### Proof (step-by-step)

1. Let `delta = x_n - mean_{n-1}`.  
2. From the mean update: `mean_n = mean_{n-1} + delta / n`,  
   equivalently `mean_{n-1} - mean_n = -delta / n`.  
3. Start with the definition:

   ```
   M2_n = (x_1 - mean_n)^2 + ... + (x_{n-1} - mean_n)^2 + (x_n - mean_n)^2
   ```
4. For `i < n`, write `x_i - mean_n = (x_i - mean_{n-1}) + (mean_{n-1} - mean_n)`.
5. Expand the square:

   ```
   (x_i - mean_n)^2 = (x_i - mean_{n-1})^2
                      + 2 * (x_i - mean_{n-1}) * (mean_{n-1} - mean_n)
                      + (mean_{n-1} - mean_n)^2
   ```
6. Summing over `i = 1..n-1`, the cross term vanishes because  
   the deviations from the previous mean sum to zero:

   ```
   sum_{i=1..n-1} (x_i - mean_{n-1}) = 0
   ```

   So:

   ```
   sum_{i=1..n-1} (x_i - mean_n)^2 =
       sum_{i=1..n-1} (x_i - mean_{n-1})^2
       + (n - 1) * (mean_{n-1} - mean_n)^2
   ```
7. Substitute back:

   ```
   M2_n = M2_{n-1} + (n - 1) * (mean_{n-1} - mean_n)^2 + (x_n - mean_n)^2
   ```
8. Use `mean_{n-1} - mean_n = -delta / n` and `x_n - mean_n = delta * (n - 1) / n`.
9. Simplify:

   ```
   (n - 1) * (mean_{n-1} - mean_n)^2 = delta^2 * (n - 1) / n^2
   (x_n - mean_n)^2 = delta^2 * (n - 1)^2 / n^2
   ```

   Adding them:

   ```
   delta^2 * (n - 1) / n^2 + delta^2 * (n - 1)^2 / n^2
   = delta^2 * (n - 1) * n / n^2
   = delta^2 * (n - 1) / n
   ```
10. Observe that:

    ```
    delta * (x_n - mean_n) = delta^2 * (n - 1) / n
    ```

    which matches the result above.
11. Therefore:

    ```
    M2_n = M2_{n-1} + delta * (x_n - mean_n)
          = M2_{n-1} + (x_n - mean_{n-1}) * (x_n - mean_n)
    ```

This proves the claimed recurrence.

---

###  How to get variances from M2

```
population variance after n values: var_pop_n  = M2_n / n
sample (unbiased) variance after n values: var_samp_n = M2_n / (n - 1), for n >= 2
```

Initialization for streaming:

```
mean_1 = x_1
M2_1   = 0
```

At each new observation `x_n`:

```
delta  = x_n - mean_{n-1}
mean_n = mean_{n-1} + delta / n
M2_n   = M2_{n-1} + delta * (x_n - mean_n)
```

---

**Numerical note :**  
Welford’s algorithm avoids catastrophic cancellation that can occur when computing  
(sum of squares) − (square of sum) for large means with small variance.  
The update uses only a single subtraction `delta` and two multiplications per step,  
keeping rounding errors small relative to the data scale.

**Complexity:**  
Time: O(1) per observation  
Memory: O(1) beyond storing the three scalars (`n`, `mean`, `M2`)

# Demo 

