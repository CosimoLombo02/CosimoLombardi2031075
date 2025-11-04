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
<title>Mean and Variance Calculator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f6f8fa;
      color: #222;
      margin: 0;
      padding: 2rem;
    }
    h1 {
      text-align: center;
      color: #2c3e50;
    }
    .container {
      max-width: 600px;
      margin: 2rem auto;
      background: #fff;
      border-radius: 12px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      padding: 2rem;
    }
    button {
      background: #0078d7;
      color: white;
      border: none;
      padding: 10px 20px;
      border-radius: 6px;
      cursor: pointer;
      margin: 0.5rem 0;
    }
    button:hover {
      background: #005fa3;
    }
    input, textarea {
      width: 100%;
      padding: 10px;
      border-radius: 6px;
      border: 1px solid #ccc;
      margin-bottom: 1rem;
      font-size: 1rem;
    }
    .result {
      background: #ecf0f1;
      border-radius: 8px;
      padding: 1rem;
      margin-top: 1rem;
      font-family: monospace;
    }
    .values-list {
      font-family: monospace;
      color: #333;
      margin-bottom: 1rem;
    }
  </style>

<body>
  <h1>Mean and Variance Calculator</h1>
  <div class="container">
    <h3>Choose Input Mode:</h3>
    <button onclick="showWholeInput()">Enter Full Vector</button>
    <button onclick="showStepInput()">Enter Values One by One</button>

    <!-- Full vector mode -->
    <div id="wholeInput" style="display:none;">
      <h3>Enter all numbers separated by commas:</h3>
      <textarea id="vectorInput" rows="3" placeholder="e.g. 1, 2, 3, 4, 5"></textarea>
      <button onclick="processWholeVector()">Calculate</button>
    </div>

    <!-- Step-by-step (incremental) mode -->
    <div id="stepInput" style="display:none;">
      <h3>Insert values one by one:</h3>
      <input id="singleValue" type="number" placeholder="Enter a number" />
      <button onclick="addValue()">Add Value</button>
      <button onclick="resetValues()">Reset</button>
      <div class="values-list" id="valuesList">Current data: []</div>
    </div>

    <div class="result" id="result" style="display:none;"></div>
  </div>

  <script>
    // Common variables
    let values = [];

    // For incremental mode (Welford)
    let n = 0;
    let meanInc = 0;
    let M2 = 0;

    // --- UI switching ---
    function showWholeInput() {
      document.getElementById('wholeInput').style.display = 'block';
      document.getElementById('stepInput').style.display = 'none';
      document.getElementById('result').style.display = 'none';
    }

    function showStepInput() {
      document.getElementById('stepInput').style.display = 'block';
      document.getElementById('wholeInput').style.display = 'none';
      document.getElementById('result').style.display = 'none';
      resetValues();
    }

    function resetValues() {
      values = [];
      n = 0;
      meanInc = 0;
      M2 = 0;
      document.getElementById('valuesList').innerText = 'Current data: []';
      document.getElementById('result').style.display = 'none';
      document.getElementById('singleValue').value = '';
    }

    // --- Full vector mode (classic computation) ---
    function processWholeVector() {
      const input = document.getElementById('vectorInput').value;
      const arr = input.split(',')
        .map(x => parseFloat(x.trim()))
        .filter(x => !isNaN(x));
      if (arr.length === 0) {
        alert("Please enter valid numbers.");
        return;
      }
      showResultsClassic(arr);
    }

    function mean(arr) {
      const sum = arr.reduce((a, b) => a + b, 0);
      return sum / arr.length;
    }

    function variance(arr) {
      if (arr.length < 2) return 0;
      const m = mean(arr);
      const squaredDiffs = arr.map(x => Math.pow(x - m, 2));
      return squaredDiffs.reduce((a, b) => a + b, 0) / (arr.length - 1);
    }

    function showResultsClassic(arr) {
      const m = mean(arr).toFixed(4);
      const v = arr.length > 1 ? variance(arr).toFixed(4) : 'N/A (need ≥2 values)';
      const res = `
        <strong>Mode:</strong> Full Vector<br>
        <strong>Data:</strong> [${arr.join(', ')}]<br>
        <strong>Mean:</strong> ${m}<br>
        <strong>Variance:</strong> ${v}
      `;
      const box = document.getElementById('result');
      box.innerHTML = res;
      box.style.display = 'block';
    }

    // --- Step-by-step mode (incremental, Welford) ---
    function addValue() {
      const val = parseFloat(document.getElementById('singleValue').value);
      if (!isNaN(val)) {
        n += 1;
        const delta = val - meanInc;
        meanInc += delta / n;
        const delta2 = val - meanInc;
        M2 += delta * delta2;

        values.push(val);
        document.getElementById('singleValue').value = '';
        document.getElementById('valuesList').innerText = `Current data: [${values.join(', ')}]`;

        const variance = n > 1 ? (M2 / (n - 1)) : 0;
        const res = `
          <strong>Mode:</strong> Incremental (Welford)<br>
          <strong>Data:</strong> [${values.join(', ')}]<br>
          <strong>Mean:</strong> ${meanInc.toFixed(4)}<br>
          <strong>Variance:</strong> ${n > 1 ? variance.toFixed(4) : 'N/A (need ≥2 values)'}
        `;
        const box = document.getElementById('result');
        box.innerHTML = res;
        box.style.display = 'block';
      } else {
        alert("Please enter a valid number.");
      }
    }
  </script>
</body>


# Code of the demo and explanation
![code](./carbon(3).png)




The JavaScript code implements two different ways to compute the mean and variance of numerical data:

1. **Full Vector Mode (Classical computation)** — all data points are entered at once, and the program computes the mean and variance using their standard definitions.
2. **Step-by-Step Mode (Incremental computation)** — data points are entered one by one, and the algorithm updates the mean and variance using **Welford’s online algorithm**, which is numerically stable and efficient.

Both modes display the computed statistics interactively in a web page.

---

##  Global Variables and Initialization

```js
let values = [];
let n = 0;
let meanInc = 0;
let M2 = 0;
```

- `values`: stores the list of all input values (for display purposes).
- `n`: the number of values processed so far.
- `meanInc`: the current running mean (updated incrementally).
- `M2`: the sum of squared deviations from the mean, used to compute variance efficiently.

These variables are reset each time the user switches modes or presses the **Reset** button.

---

##  User Interface (UI) Control

The functions below manage the visibility of the two input modes and reset the state:

```js
function showWholeInput() { ... }
function showStepInput() { ... }
function resetValues() { ... }
```

- `showWholeInput()` and `showStepInput()` toggle between the two modes by showing or hiding specific `<div>` elements.
- `resetValues()` clears all stored data and restores the default state of the calculator.

---

##  Full Vector Mode — Classical Computation

In this mode, the user provides a list of numbers separated by commas.  
The program parses them, filters out invalid entries, and applies the standard statistical definitions.

###  Parsing and Validation

```js
function processWholeVector() {
  const input = document.getElementById('vectorInput').value;
  const arr = input.split(',')
    .map(x => parseFloat(x.trim()))
    .filter(x => !isNaN(x));
  ...
}
```

This code:
- Splits the input string by commas,
- Converts each piece to a number using `parseFloat`,
- Filters out invalid or empty entries.

If no valid numbers are entered, an alert message is shown.

###  Mean and Variance (Classical Definitions)

```js
function mean(arr) {
  const sum = arr.reduce((a, b) => a + b, 0);
  return sum / arr.length;
}

function variance(arr) {
  if (arr.length < 2) return 0;
  const m = mean(arr);
  const squaredDiffs = arr.map(x => Math.pow(x - m, 2));
  return squaredDiffs.reduce((a, b) => a + b, 0) / (arr.length - 1);
}
```

- **Mean**: the arithmetic average, computed as the sum of all elements divided by their count.  
- **Variance**: computed using the *unbiased* estimator (division by *n - 1*), which corrects for bias when estimating from samples.

Both functions are written using functional programming methods (`map`, `reduce`), which improve readability.

###  Displaying the Results

```js
function showResultsClassic(arr) {
  const m = mean(arr).toFixed(4);
  const v = arr.length > 1 ? variance(arr).toFixed(4) : 'N/A (need ≥2 values)';
  ...
}
```

The `.toFixed(4)` method ensures that results are displayed with four decimal places for consistency.  
The results are shown dynamically in the `#result` box.

---

##  Step-by-Step Mode — Incremental Computation (Welford’s Algorithm)

This mode is the key educational feature of the app.  
It implements **Welford’s online algorithm**, which updates mean and variance with each new data point, without storing all values or recalculating from scratch.

###  Incremental Update Logic

```js
function addValue() {
  const val = parseFloat(document.getElementById('singleValue').value);
  if (!isNaN(val)) {
    n += 1;
    const delta = val - meanInc;
    meanInc += delta / n;
    const delta2 = val - meanInc;
    M2 += delta * delta2;
    ...
  }
}
```

Each time a new value is entered:
1. The count `n` increases by 1.
2. `delta` represents the difference between the new value and the previous mean.
3. The mean is updated as:

   ```
   mean_n = mean_{n-1} + delta / n
   ```

4. The second difference `delta2` is the deviation from the *new* mean.
5. The accumulated squared deviation `M2` is updated as:

   ```
   M2_n = M2_{n-1} + delta * delta2
   ```

These formulas match exactly the recurrence relations described in Welford’s algorithm,  
ensuring numerical stability even for large data streams.

###  Computing the Variance

After updating `M2`, the variance is computed as:

```
sample variance = M2 / (n - 1)
```

For `n = 1`, the variance is undefined, so the code displays `"N/A (need ≥2 values)"`.

###  Why Welford’s Algorithm Matters

The incremental method is **O(1)** in time and space per observation:
- It doesn’t require storing the entire dataset.
- It avoids catastrophic cancellation errors caused by large numbers with small variance.
- It is ideal for *streaming* data or continuous sensor readings.

---

##  Displaying Incremental Results

After each new value is added, the function dynamically updates the output box:

```js
const variance = n > 1 ? (M2 / (n - 1)) : 0;
const res = `
  <strong>Mode:</strong> Incremental (Welford)<br>
  <strong>Data:</strong> [${values.join(', ')}]<br>
  <strong>Mean:</strong> ${meanInc.toFixed(4)}<br>
  <strong>Variance:</strong> ${n > 1 ? variance.toFixed(4) : 'N/A (need ≥2 values)'}
`;
```

This provides immediate visual feedback showing how the mean and variance evolve as new data points are added.

---

##  Summary of Algorithms Implemented

| Mode | Formula Type | Requires All Data? | Numerical Stability | Typical Use Case |
|------|---------------|--------------------|---------------------|------------------|
| Full Vector | Classical mean & variance | Yes | Moderate | Small, static datasets |
| Incremental | Welford’s recurrence | No | Excellent | Streaming or real-time data |

---

##  Computational Complexity

| Operation | Time Complexity | Space Complexity |
|------------|----------------|------------------|
| Classical mean/variance | O(n) | O(n) |
| Welford incremental update | O(1) | O(1) |

---







