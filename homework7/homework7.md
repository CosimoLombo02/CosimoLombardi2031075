

# CosimoLombardi2031075

---

## Homework7

A server receives weekly security updates for `n` weeks. Each week there are `m` attackers, each with a probability `p` of breaching the system independently. For every week:

* Assign `+1` if **no attacker** succeeds (the server remains secure),
* Assign `−1` if **at least one attacker** succeeds (the server is breached).

Each trajectory represents the cumulative score of these ±1 events over `n` weeks — a **random walk**. By repeating this experiment for many simulated trajectories, we can count how many end at each possible final score, then compare the empirical distribution with the **binomial distribution** predicted by theory.

---



###  Probability of a Secure Week

If each of the `m` attackers acts independently with breach probability `p`, then the probability that the system remains secure for a week is:

[
P_{\text{secure}} = (1 - p)^m
]

and the probability that at least one breach occurs is

[
P_{\text{breach}} = 1 - (1 - p)^m
]

###  Distribution of Secure Weeks

Over `n` weeks, each week is an independent Bernoulli trial with success probability `P_secure`. The number of secure weeks `K` therefore follows a **binomial distribution**:

[
K \sim \text{Binomial}(n, P_{\text{secure}})
]

###  Mapping to a Random Walk

Each secure week corresponds to a step of `+1`, and each breach corresponds to `−1`. After `n` weeks, the total cumulative score is:

[
S = (+1)K + (-1)(n - K) = 2K - n
]

Thus, the possible values of `S` are `−n, −n + 2, …, n − 2, n`. Each score maps to a unique number of secure weeks via `K = (S + n)/2`.

###  Theoretical Distribution of Total Scores

The theoretical probability that the total score equals `s` is the probability that `K = (s + n)/2` in the binomial distribution:

[
P(S = s) = {n \choose k} P_{\text{secure}}^{k} (1 - P_{\text{secure}})^{n - k}, \quad k = \frac{s + n}{2}
]

###  Asymptotic Behavior

* As `n → ∞`, the binomial distribution converges (by the Central Limit Theorem) to a **normal distribution** with
  [
  \mu = n P_{\text{secure}}, \quad \sigma^2 = n P_{\text{secure}} (1 - P_{\text{secure}})
  ]
* As `m` increases, `P_secure = (1 − p)^m` decreases rapidly, so breaches dominate and the distribution shifts left.
* As `p → 0`, `P_secure → 1`, concentrating the distribution at high positive scores.

---

##  Simulation Strategy

To simulate the experiment:

1. Compute `P_secure = (1 − p)^m`.
2. For each trajectory (repeat `nTrials` times):

   * Initialize `score = 0`.
   * Repeat `n` times:

     * Draw a uniform random variable `u ∼ Uniform(0,1)`.
     * If `u < 1 − P_secure`, a breach occurs → `score -= 1`.
     * Otherwise → `score += 1`.
   * Record the final score in a frequency table.
3. Normalize the counts by dividing by the total number of trials to obtain empirical probabilities.
4. Compute the theoretical binomial probabilities for the same set of possible scores.
5. Compare the two distributions.

This process directly parallels the concept of a **biased random walk**.

---

##  Computational Considerations

* **Complexity**: Each trajectory involves `n` random draws, so the overall cost is `O(n × nTrials)`.
* **Numerical stability**: Computing `{n \choose k}` via factorials can overflow for large `n`; the implementation uses an iterative product to avoid this.
* **Efficiency**: For very large simulations, one could sample `K` directly from a binomial distribution instead of simulating each week, but the current method illustrates the random-walk process clearly.

---
## Demo and code

<meta charset="UTF-8">
  <title>HMWK 7 – Security Random Walk Simulation</title>
  <script src="https://cdn.plot.ly/plotly-2.26.0.min.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    #chart {
      width: 800px;
      height: 500px;
      margin: 20px auto;
    }
  </style>

<body class="bg-gray-100 min-h-screen flex flex-col items-center p-6">

  <h1 class="text-3xl font-bold text-gray-800 mb-4">
    HMWK 7 – Security Random Walk Simulation
  </h1>

  <!-- GUI input panel -->
  <div class="bg-white shadow-md rounded-2xl p-6 w-full max-w-lg mb-6">
    <div class="grid grid-cols-2 gap-4 mb-4">
      <label class="flex flex-col">
        <span class="font-semibold text-sm text-gray-700 mb-1">Weeks (n):</span>
        <input id="nWeeks" type="number" value="20" min="1" class="border rounded-lg px-2 py-1">
      </label>
      <label class="flex flex-col">
        <span class="font-semibold text-sm text-gray-700 mb-1">Attackers (m):</span>
        <input id="mAttackers" type="number" value="10" min="1" class="border rounded-lg px-2 py-1">
      </label>
      <label class="flex flex-col">
        <span class="font-semibold text-sm text-gray-700 mb-1">Breach prob (p):</span>
        <input id="p" type="number" step="0.01" value="0.05" min="0" max="1" class="border rounded-lg px-2 py-1">
      </label>
      <label class="flex flex-col">
        <span class="font-semibold text-sm text-gray-700 mb-1">Trajectories:</span>
        <input id="nTrials" type="number" value="10000" min="100" class="border rounded-lg px-2 py-1">
      </label>
    </div>
    <button id="runBtn" class="bg-blue-600 text-white px-4 py-2 rounded-lg hover:bg-blue-700 transition">
      Run Simulation
    </button>
  </div>

  <!-- Chart container -->
  <div id="chart" class="bg-white rounded-2xl shadow-md p-4"></div>

  <script>
    // ---- Math helpers ----
    function combination(n, k) {
      if (k < 0 || k > n) return 0;
      k = Math.min(k, n - k);
      let c = 1;
      for (let i = 0; i < k; i++) c = (c * (n - i)) / (i + 1);
      return c;
    }

    function binomialPMF(k, n, p) {
      return combination(n, k) * Math.pow(p, k) * Math.pow(1 - p, n - k);
    }

    // ---- Simulation core ----
    function simulateSecurity(nWeeks, mAttackers, p, nTrials) {
      const secureProb = Math.pow(1 - p, mAttackers);
      const scoreCounts = new Map();

      for (let t = 0; t < nTrials; t++) {
        let score = 0;
        for (let week = 0; week < nWeeks; week++) {
          const breached = Math.random() < 1 - secureProb;
          score += breached ? -1 : +1;
        }
        scoreCounts.set(score, (scoreCounts.get(score) || 0) + 1);
      }

      const results = [];
      for (let totalScore = -nWeeks; totalScore <= nWeeks; totalScore += 2) {
        const freq = (scoreCounts.get(totalScore) || 0) / nTrials;
        const k = (totalScore + nWeeks) / 2;
        const expected = binomialPMF(k, nWeeks, secureProb);
        results.push({ totalScore, freq, expected });
      }
      return results;
    }

    // ---- Fixed chart layout ----
    const layout = {
      title: 'Distribution of Total Scores – Random Walk vs Binomial',
      xaxis: { title: 'Total Score (2·#SecureWeeks − n)' },
      yaxis: { title: 'Probability' },
      legend: { orientation: 'h' },
      margin: { t: 50, l: 60, r: 30, b: 60 }
    };

    const traceSim = { x: [], y: [], type: 'bar', name: 'Simulated', opacity: 0.6 };
    const traceBinom = { x: [], y: [], type: 'scatter', mode: 'lines+markers', name: 'Binomial (Expected)', line: { width: 2 } };

    // Create empty chart (fixed size)
    Plotly.newPlot('chart', [traceSim, traceBinom], layout, { responsive: false });

    // ---- Run simulation & update ----
    document.getElementById('runBtn').addEventListener('click', () => {
      const nWeeks = parseInt(document.getElementById('nWeeks').value);
      const mAttackers = parseInt(document.getElementById('mAttackers').value);
      const p = parseFloat(document.getElementById('p').value);
      const nTrials = parseInt(document.getElementById('nTrials').value);

      const data = simulateSecurity(nWeeks, mAttackers, p, nTrials);
      const scores = data.map(d => d.totalScore);
      const simFreqs = data.map(d => d.freq);
      const binomVals = data.map(d => d.expected);

      Plotly.react('chart', [
        { ...traceSim, x: scores, y: simFreqs },
        { ...traceBinom, x: scores, y: binomVals }
      ], layout, { responsive: false });
    });
  </script>





![code](./carbon(4).png)





##  Step-by-Step Explanation of the JavaScript Code

Below is a walkthrough of the JavaScript logic in the provided HTML file.

##  Included Libraries

* **Plotly.js**: used for rendering the chart (simulated histogram vs theoretical binomial curve).
* **TailwindCSS**: provides simple, responsive styling.
* The chart container (`#chart`) has fixed width and height so that the plot size remains constant when data updates.

###  Mathematical Helper Functions

```js
function combination(n, k) {
  if (k < 0 || k > n) return 0;
  k = Math.min(k, n - k);
  let c = 1;
  for (let i = 0; i < k; i++) c = (c * (n - i)) / (i + 1);
  return c;
}
```

Computes the binomial coefficient ( {n \choose k} ) using an iterative approach to prevent overflow.

```js
function binomialPMF(k, n, p) {
  return combination(n, k) * Math.pow(p, k) * Math.pow(1 - p, n - k);
}
```

Computes the binomial probability mass function.

###  Core Simulation Function

```js
function simulateSecurity(nWeeks, mAttackers, p, nTrials) {
  const secureProb = Math.pow(1 - p, mAttackers);
  const scoreCounts = new Map();

  for (let t = 0; t < nTrials; t++) {
    let score = 0;
    for (let week = 0; week < nWeeks; week++) {
      const breached = Math.random() < 1 - secureProb;
      score += breached ? -1 : +1;
    }
    scoreCounts.set(score, (scoreCounts.get(score) || 0) + 1);
  }

  const results = [];
  for (let totalScore = -nWeeks; totalScore <= nWeeks; totalScore += 2) {
    const freq = (scoreCounts.get(totalScore) || 0) / nTrials;
    const k = (totalScore + nWeeks) / 2;
    const expected = binomialPMF(k, nWeeks, secureProb);
    results.push({ totalScore, freq, expected });
  }
  return results;
}
```

This function simulates all trajectories and computes both empirical and theoretical probabilities.

###  Chart Initialization

```js
const layout = {
  title: 'Distribution of Total Scores – Random Walk vs Binomial',
  xaxis: { title: 'Total Score (2·#SecureWeeks − n)' },
  yaxis: { title: 'Probability' },
  legend: { orientation: 'h' },
  margin: { t: 50, l: 60, r: 30, b: 60 }
};

const traceSim = { x: [], y: [], type: 'bar', name: 'Simulated', opacity: 0.6 };
const traceBinom = { x: [], y: [], type: 'scatter', mode: 'lines+markers', name: 'Binomial (Expected)', line: { width: 2 } };
Plotly.newPlot('chart', [traceSim, traceBinom], layout, { responsive: false });
```

The chart is created once with fixed layout; later updates only change the data arrays.

###  Updating Data on Button Click

```js
document.getElementById('runBtn').addEventListener('click', () => {
  const nWeeks = parseInt(document.getElementById('nWeeks').value);
  const mAttackers = parseInt(document.getElementById('mAttackers').value);
  const p = parseFloat(document.getElementById('p').value);
  const nTrials = parseInt(document.getElementById('nTrials').value);

  const data = simulateSecurity(nWeeks, mAttackers, p, nTrials);
  const scores = data.map(d => d.totalScore);
  const simFreqs = data.map(d => d.freq);
  const binomVals = data.map(d => d.expected);

  Plotly.react('chart', [
    { ...traceSim, x: scores, y: simFreqs },
    { ...traceBinom, x: scores, y: binomVals }
  ], layout, { responsive: false });
});
```

When the user clicks **Run Simulation**, the function reads parameters from the GUI, runs the simulation, and updates the chart using `Plotly.react()` — preserving the fixed layout and size.

---

##  Interpretation of Results

* The **bars** show the empirical frequencies from simulation.
* The **line** represents the theoretical binomial probabilities.
* As `n` and `m` increase, the simulated histogram converges toward the binomial curve, validating the model.

---

### Key Takeaways

* The weekly security model naturally maps to a **biased random walk**.
* The end-of-period distribution of total scores is analytically binomial.
* Monte Carlo simulations empirically verify the convergence of random walks to their binomial (and asymptotically normal) limit.




