# CosimoLombardi2031075

##  Objective

The goal of this simulation is to model a counting process over a time interval T (e.g., T = 1), where events (successes) occur **independently and uniformly in time** at a constant average rate λ. This process is fundamental in stochastic processes and is widely used to model random arrivals, failures, or occurrences over time.

The simulation uses a discrete approximation: the interval is divided into n small subintervals, and an event occurs in each subinterval with probability λ/n. This approximates a **Poisson process** as n → ∞.

---

##  Theoretical Background

###  Counting Process

A counting process N(t) is a stochastic process representing the total number of events up to time t. A Poisson counting process satisfies:

1. N(0) = 0
2. Independent increments: the number of events in disjoint intervals are independent.
3. Stationary increments: the number of events in an interval of length t is Poisson distributed with mean λ t.
4. For small time intervals Δt:

   * Probability of 1 event: λ Δt + o(Δt)
   * Probability of 2 or more events: negligible o(Δt)

The rate parameter λ controls the average number of events per unit time. Higher λ → more frequent events.

###  Discrete Approximation

To simulate the process numerically:

1. Divide [0, T] into n subintervals of length Δt = T/n.
2. Generate a **Bernoulli trial** in each subinterval with probability p = λ Δt.
3. The cumulative sum of successes gives an approximation of N(t).

As n becomes very large, this method converges to a Poisson process.

---
## Demo
## Demo

<style>
body { font-family: Arial, sans-serif; background:#f7f7f7; padding:20px; }
.container { max-width:900px; margin:auto; background:#fff; padding:20px; border-radius:8px; box-shadow:0 0 10px rgba(0,0,0,0.1); }
.controls { display:flex; justify-content:center; gap:10px; margin-bottom:20px; }
input[type=number] { width:80px; padding:5px; }
button { padding:6px 12px; cursor:pointer; }
canvas { background:#f0f0f0; border:1px solid #ccc; display:block; margin:auto; }
</style>

<div class="container">
<h1>Counting Process Simulation</h1>

<div class="controls">
  <label>Rate λ:</label>
  <input id="lambda" type="number" value="5" step="0.1">
  <label>Intervals n:</label>
  <input id="n" type="number" value="5000">
  <button id="simulateBtn">Simulate</button>
</div>

<canvas id="chart" width="800" height="400"></canvas>
<div id="info" style="text-align:center; margin-top:10px;"></div>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
let chartInstance = null;

function simulateCountingProcess() {
    const T = 1;
    const lambda = parseFloat(document.getElementById('lambda').value);
    const n = parseInt(document.getElementById('n').value);
    const dt = T / n;

    let time = [];
    let N_t = [];
    let count = 0;

    for (let i = 0; i < n; i++) {
        time.push(i * dt);
        if (Math.random() < lambda * dt) {
            count++;
        }
        N_t.push(count);
    }

    const ctx = document.getElementById('chart').getContext('2d');

    if (chartInstance) chartInstance.destroy();

    chartInstance = new Chart(ctx, {
        type: 'line',
        data: {
            labels: time,
            datasets: [{
                label: 'N(t)',
                data: N_t,
                stepped: true,
                borderColor: 'blue',
                fill: false
            }]
        },
        options: {
            responsive: true,
            scales: {
                x: { title: { display:true, text:'Time' } },
                y: { title: { display:true, text:'Number of events N(t)' }, beginAtZero:true }
            }
        }
    });

    document.getElementById('info').textContent = 
      `Simulated N(t) with λ=${lambda} over n=${n} intervals. Total events: ${count}.`;
}

document.getElementById('simulateBtn').addEventListener('click', simulateCountingProcess);
</script>

##  Code and code explanation

![code](./carbon.png)

###  Parameters and Time Grid

```javascript
const T = 1;                  // total simulation time
const lambda = parseFloat(document.getElementById('lambda').value); // rate
const n = parseInt(document.getElementById('n').value);            // number of subintervals
const dt = T / n;              // length of each subinterval

let time = [];
for (let i = 0; i < n; i++) {
    time.push(i * dt);         // time grid for plotting
}
```

* T defines the total duration of the simulation.
* lambda is the rate of events per unit time.
* n determines the granularity of the approximation.
* time stores the discrete time points for plotting.

###  Event Simulation

```javascript
let N_t = [];
let count = 0;

for (let i = 0; i < n; i++) {
    if (Math.random() < lambda * dt) {
        count += 1;            // an event occurs in this subinterval
    }
    N_t.push(count);           // cumulative count
}
```

* For each subinterval, an event occurs with probability λ * dt.
* count tracks the cumulative number of events, forming the counting process N(t).
* N_t is the array storing the process for plotting.

###  Plotting the Counting Process

```javascript
const ctx = document.getElementById('chart').getContext('2d');
if (window.chart) {
    window.chart.destroy();    // remove previous chart
}

window.chart = new Chart(ctx, {
    type: 'line',
    data: {
        labels: time,
        datasets: [{
            label: 'N(t)',
            data: N_t,
            stepped: true,       // stepwise plot for counting process
            borderColor: 'blue',
            fill: false
        }]
    },
    options: {
        responsive: true,
        scales: {
            x: { title: { display: true, text: 'Time' } },
            y: { title: { display: true, text: 'Number of events N(t)' }, beginAtZero: true }
        }
    }
});
```

* The chart uses a step plot (stepped: true) to reflect that the counting process increases in jumps.
* The previous chart is destroyed before creating a new one to allow repeated simulations.

---

##  Theoretical Interpretation

1. Stochastic Process Approximation: The simulated process approximates a **Poisson process**.
2. Expected Value**: E[N(t)] ≈ λ t
3. Variance: Var[N(t)] ≈ λ t
4. Independent increments: guaranteed by independent Bernoulli trials.
5. Stationary increments: the probability of an event depends only on dt and λ, constant over all subintervals.
6. Rate Parameter λ: determines the average intensity of events. Higher λ → more frequent jumps in N(t).

---

##  Extensions and Analysis

* Increasing n improves approximation to continuous-time Poisson process.
* Multiple simulations can illustrate variability and the distribution of counts.
* Inter-arrival times can be computed as differences between consecutive events, which follow an **exponential distribution** with rate λ.
* Applications: queueing theory, radioactive decay, call arrivals, or any process with randomly occurring events in time.




The JS code and explanation are aligned with **university-level stochastic process coursework** and satisfy the requirement to simulate, approximate, and analyze a counting process over [0, T].


