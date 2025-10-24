# CosimoLombardi2031075

## Convergence to Probability

In probability theory and statistics, the phrase **“converge to the probability”** does **not** refer to classical notions of convergence in analysis, such as the Weierstrass ε–δ definition for limits of functions. Instead, it refers to a **stochastic concept of convergence**, describing how sequences of random variables or empirical frequencies behave as the number of observations or trials increases.

This concept is central in understanding why long-run empirical measurements approximate theoretical probabilities, and it forms the basis for much of statistical inference.

---

##  Intuitive Meaning

To “converge to the probability” generally means that as we repeat a **random experiment** many times:

- The **empirical frequency** (the proportion of times an event occurs) approaches the **theoretical probability** of that event.
- In other words, the average outcome over many trials stabilizes around the expected probability.

### Example: Coin Toss

- Let us flip a fair coin repeatedly.
- Define a sequence of indicator random variables X1, X2, ..., Xn such that:

```
X_i = 1 if the i-th toss is heads
X_i = 0 if the i-th toss is tails
```

- The probability of heads is:

```
P(X_i = 1) = 0.5
```

- Define the **sample average** after n tosses:

```
S_n = (X_1 + X_2 + ... + X_n) / n
```

- The law of large numbers states:

```
S_n → 0.5 as n → ∞
```

- Here, “→ 0.5” is **convergence in probability** (or almost surely, depending on the form of the law of large numbers), meaning the observed proportion of heads will stabilize near 0.5 as the number of tosses grows.

---

##  Formal Notions of Probabilistic Convergence

Unlike classical convergence in analysis, there are **several types of convergence for random variables**:

###  Convergence in Probability

A sequence of random variables Xn **converges in probability** to a constant p if, for every ε > 0:

```
P(|X_n - p| > ε) → 0 as n → ∞
```

- Intuition: The probability that Xn deviates significantly from p goes to zero as n increases.
- Example: Sn (sample average of coin tosses) converges in probability to 0.5.

###  Almost Sure Convergence

A sequence Xn **converges almost surely** to p if:

```
P(lim_{n→∞} X_n = p) = 1
```

- This is a stronger notion: with probability 1, the sequence of outcomes eventually stays arbitrarily close to p.

###  Convergence in Distribution

A sequence Xn **converges in distribution** to a random variable X if the cumulative distribution functions F_{Xn}(x) converge to F_X(x) at all continuity points of F_X.

- This is weaker than almost sure convergence and convergence in probability.
- Often used for limiting distributions (e.g., central limit theorem).

---

##  Relation to the Law of Large Numbers (LLN)

The statement that a sequence of trials **converges to the probability** is formalized by the **law of large numbers**:

###  Weak Law of Large Numbers

- Let X1, X2, ..., Xn be independent and identically distributed (i.i.d.) random variables with expected value μ = E[Xi].
- Then the sample average:

```
S_n = (X_1 + X_2 + ... + X_n) / n
```

**converges in probability** to μ:

```
S_n → μ as n → ∞
```

###  Strong Law of Large Numbers

- Under the same assumptions, the sample average **converges almost surely** to μ:

```
P(lim_{n→∞} S_n = μ) = 1
```

- This guarantees that in a single infinite sequence of trials, the relative frequency will settle at the true probability.

---

##  Key Differences from Classical Convergence

| Classical Convergence | Convergence to Probability |
|---------------------|---------------------------|
| Sequence of numbers | Sequence of random variables |
| Limit defined via ε–δ neighborhoods | Limit defined via probabilistic statements (e.g., P(|X_n - p| > ε) → 0) |
| Deterministic | Stochastic / random |
| Example: f(x) → L as x → a | Example: sample average S_n → true probability p as n → ∞ |

- Classical convergence is deterministic and precise; probabilistic convergence deals with tendencies over repeated trials.

---

##  Practical Implications

- In simulations or experiments, “converging to probability” explains why **empirical frequencies approximate theoretical probabilities**.
- This concept is crucial in:
  - Monte Carlo methods
  - Statistical estimation
  - Predictive modeling
- It also underlies the idea that **probabilities are long-run relative frequencies**.

---

##  Summary

- **Convergence to probability** refers to the stabilization of observed outcomes (frequencies, averages) around the theoretical probability as the number of trials grows.
- It is **probabilistic**, not classical, convergence.
- It is formalized through concepts such as **convergence in probability** and **almost sure convergence** and is central to the **law of large numbers**.
- This convergence justifies why probabilities measured empirically in repeated experiments are reliable estimates of theoretical probabilities.

# Demo
# Bernoulli Trajectories Demo

This interactive demo simulates multiple sample paths of repeated Bernoulli trials. Each path represents a sequence of successes over a fixed number of trials, with a given probability `p` of success. The demo visualizes both the trajectories and the distribution of final successes.

## Features

- **Trajectories**: Shows the cumulative number of successes for each simulated path.
- **Mean line**: Displays the expected number of successes over trials (`N * p`).
- **Final successes histogram**: Empirical distribution of successes at the end of all paths.
- **Gaussian overlay**: CLT approximation of the final success distribution.

## User Controls

- **Trials (N)**: Number of Bernoulli trials per path.
- **Paths**: Number of sample paths to simulate.
- **p (success probability)**: Probability of success in each trial.
- **Animate**: Toggle animation to show trajectories in real time.
- **Batch delay**: Delay in milliseconds between animation batches.

## How It Works

1. **Generate Paths**: For each path, simulate `N` Bernoulli trials with probability `p` and compute cumulative successes.
2. **Draw Trajectories**: Plot each path with light blue lines to visualize randomness.
3. **Plot Mean Line**: Overlay the theoretical expected successes (`t * p`) as a solid blue line.
4. **Compute Histogram**: Count final successes across all paths and display as red bars.
5. **Overlay Gaussian**: Use the Central Limit Theorem to overlay a green Gaussian curve approximating the histogram.

## Educational Purpose

- Demonstrates the **law of large numbers**: trajectories tend to approach the expected mean as the number of trials increases.
- Illustrates the **Central Limit Theorem**: distribution of final successes approximates a Gaussian with mean `N*p` and variance `N*p*(1-p)`.

## Notes

- Trajectories are cumulative counts of successes.
- The histogram scales relative to the highest bin.
- Gaussian overlay is scaled to match the histogram for visual comparison.
- ASCII and browser-safe implementation with high-DPI canvas support.

Use the controls above to explore how the probability `p` affects the distribution of successes, the convergence of paths, and the CLT approximation.

<head>
<meta charset="utf-8" />
<title>LLN — Bernoulli Frequencies (f(n) = S_n / n)</title>
<meta name="viewport" content="width=device-width,initial-scale=1" />
<style>
  body { font-family: system-ui, -apple-system, "Segoe UI", Roboto, Arial; background:#f7fbff; color:#0b2445; padding:20px; }
  .card { background:#fff; border-radius:10px; padding:16px; box-shadow:0 6px 18px rgba(11,37,69,0.06); max-width:1100px; margin:0 auto; }
  h1 { margin:0 0 8px; font-size:18px; }
  .row { display:flex; gap:12px; align-items:center; margin-top:10px; flex-wrap:wrap; }
  label { font-weight:600; font-size:13px; margin-right:6px; }
  input[type=number], input[type=range], select { padding:6px 8px; border-radius:8px; border:1px solid #e6eefc; }
  button { padding:8px 12px; border-radius:8px; border:0; background:#2563eb; color:white; font-weight:700; cursor:pointer; }
  canvas { background: linear-gradient(180deg,#ffffff,#f1f7ff); border-radius:8px; display:block; margin-top:12px; width:100%; max-width:100%; height:520px; }
  .meta { font-size:13px; color:#446; margin-top:8px; }
  .legend { display:flex; gap:12px; margin-top:8px; flex-wrap:wrap; }
  .legend div { display:flex; align-items:center; gap:6px; font-size:13px; }
  .swatch { width:18px; height:8px; border-radius:3px; display:inline-block; }
</style>
</head>
<body>
  <div class="card">
    <h1>Legge dei Grandi Numeri — Frequenze relative f(n) = S<sub>n</sub>/n</h1>
    <p class="meta">Simula più traiettorie di prove Bernoulli(p) e mostra come le frequenze relative finali si concentrano intorno a p. Istogramma verticale sulla destra.</p>

    <div class="row">
      <label>Trials (N)</label>
      <input id="N" type="number" min="1" max="2000" value="200" />
      <label>Paths (m)</label>
      <input id="paths" type="number" min="1" max="2000" value="200" />
      <label>p (success prob)</label>
      <input id="pprob" type="number" min="0" max="1" step="0.01" value="0.5" />
      <label>Animate</label>
      <select id="animate">
        <option value="true">Yes (animated)</option>
        <option value="false">No (instant)</option>
      </select>
      <label>Batch delay ms</label>
      <input id="delay" type="number" min="0" max="2000" value="10" />
      <button id="run">Run</button>
      <button id="clear">Clear</button>
    </div>

    <canvas id="plot"></canvas>

    <div class="legend">
      <div><span class="swatch" style="background:rgba(20,110,230,0.18)"></span> traiettorie f(t) (leggere)</div>
      <div><span class="swatch" style="background:rgba(20,110,230,1)"></span> linea media teorica (y = p)</div>
      <div><span class="swatch" style="background:rgba(220,40,40,0.9)"></span> istogramma di f(N)</div>
    </div>

    <div class="meta" id="info"></div>
  </div>

<script>
/* ----------------------------------------------------------
   Modificato per visualizzare la Legge dei Grandi Numeri (LLN)
   - Traiettorie di frequenze relative f(t) = S_t / t (y in [0,1])
   - Linea orizzontale y = p
   - Istogramma verticale sulla destra per f(N)
   ---------------------------------------------------------- */

// helpers
function randBernoulli(p){ return Math.random() < p ? 1 : 0; }
function gaussianPdf(x, mu, sigma){ return Math.exp(-0.5*((x-mu)/sigma)**2) / (Math.sqrt(2*Math.PI)*sigma); }

// canvas
const canvas = document.getElementById('plot');
const ctx = canvas.getContext('2d');

function resizeCanvas(){
  const cssW = canvas.clientWidth;
  const cssH = 520;
  const ratio = window.devicePixelRatio || 1;
  canvas.width = Math.floor(cssW * ratio);
  canvas.height = Math.floor(cssH * ratio);
  canvas.style.height = cssH + 'px';
  ctx.setTransform(ratio,0,0,ratio,0,0);
}
window.addEventListener('resize', resizeCanvas);
resizeCanvas();

function clearCanvas(){
  ctx.clearRect(0,0,canvas.width,canvas.height);
}

// layout: main plot area on left, histogram area on right
function getLayout(){
  const totalW = canvas.clientWidth;
  const totalH = parseInt(canvas.style.height);
  const histWidth = Math.min(220, Math.floor(totalW * 0.28)); // right panel for histogram
  const mainWidth = totalW - histWidth - 18; // small gap
  const margins = { left: 60, right: 12, top: 30, bottom: 60 };
  const main = { x: 0, y: 0, w: mainWidth, h: totalH, margins };
  const hist = { x: mainWidth + 18, y: 0, w: histWidth, h: totalH, margins: { left: 12, right: 12, top: 30, bottom: 60 } };
  return { main, hist, totalW, totalH };
}

function drawMainAxes(main, N){
  const w = main.w, h = main.h;
  const m = main.margins;
  ctx.save();
  // background grid
  ctx.strokeStyle = '#e6eefc';
  ctx.lineWidth = 1;
  for(let i=0;i<=5;i++){
    const yy = m.top + (h - m.top - m.bottom) * i/5;
    ctx.beginPath();
    ctx.moveTo(m.left, yy);
    ctx.lineTo(m.left + (w - m.left - m.right), yy);
    ctx.stroke();
  }
  // axes
  ctx.strokeStyle = '#a9c2f7';
  ctx.lineWidth = 1.2;
  ctx.beginPath();
  ctx.moveTo(m.left, m.top);
  ctx.lineTo(m.left, h - m.bottom);
  ctx.lineTo(m.left + (w - m.left - m.right), h - m.bottom);
  ctx.stroke();

  ctx.fillStyle = '#38506b';
  ctx.font = '12px system-ui';
  ctx.fillText('Tries →', m.left + (w - m.left - m.right) - 40, h - m.bottom + 18);
  ctx.fillText('Relative freq →', m.left - 58, m.top + 10);

  // y ticks: 0..1
  for(let i=0;i<=5;i++){
    const val = (1 - i/5).toFixed(2);
    const yy = m.top + (h - m.top - m.bottom) * i/5;
    ctx.fillText(String(val), m.left - 46, yy + 4);
  }

  // x ticks: 0, N/4, N/2, 3N/4, N
  const xs = [0, Math.floor(N/4), Math.floor(N/2), Math.floor(3*N/4), N];
  for(const xi of xs){
    const xx = m.left + (w - m.left - m.right) * (xi / N);
    ctx.fillText(String(xi), xx - 8, h - m.bottom + 18);
  }

  ctx.restore();
}

function makeMainMapper(main, N){
  const w = main.w, h = main.h, m = main.margins;
  return {
    x: x => m.left + (w - m.left - m.right) * (x / N),
    y: y => m.top + (h - m.top - m.bottom) * (1 - y) // y in [0,1]
  };
}

function drawHistAxes(hist){
  const w = hist.w, h = hist.h, m = hist.margins;
  ctx.save();
  // outline for histogram area
  ctx.strokeStyle = '#d6e6ff';
  ctx.lineWidth = 1;
  ctx.strokeRect(hist.x + m.left - 1, m.top - 1, w - m.left - m.right + 2, h - m.top - m.bottom + 2);
  // label
  ctx.fillStyle = '#38506b';
  ctx.font = '12px system-ui';
  ctx.fillText('Distribution of f(N)', hist.x + 8, m.top + 12);
  ctx.restore();
}

// simulate one path returning sequence of frequencies f(t) length N+1
function simulateOnePathFreq(N, p){
  const seq = new Array(N+1);
  let cum = 0;
  seq[0] = 0; // convention f(0)=0
  for(let t=1;t<=N;t++){
    cum += randBernoulli(p);
    seq[t] = cum / t; // frequency relative
  }
  return seq;
}

// main function
async function runSimulation(opts){
  const N = opts.N, paths = opts.paths, p = opts.p, animate = opts.animate, delay = opts.delay;
  resizeCanvas();
  clearCanvas();

  const layout = getLayout();
  const main = layout.main, hist = layout.hist;
  drawMainAxes(main, N);
  drawHistAxes(hist);

  const map = makeMainMapper(main, N);

  // draw theoretical horizontal line y = p
  ctx.save();
  ctx.strokeStyle = 'rgba(20,110,230,1)';
  ctx.lineWidth = 2;
  ctx.setLineDash([6,4]);
  ctx.beginPath();
  ctx.moveTo(map.x(0), map.y(p));
  ctx.lineTo(map.x(N), map.y(p));
  ctx.stroke();
  ctx.setLineDash([]);
  ctx.restore();

  // draw trajectories
  ctx.save();
  ctx.lineWidth = 1.0;
  const alpha = 0.12;
  const batch = 5;
  let idx = 0;
  const finals = new Array(paths).fill(0);

  function drawPath(seq){
    ctx.beginPath();
    ctx.strokeStyle = `rgba(20,110,230,${alpha})`;
    ctx.moveTo(map.x(0), map.y(seq[0]));
    for(let t=1;t<=N;t++){
      ctx.lineTo(map.x(t), map.y(seq[t]));
    }
    ctx.stroke();
  }

  if(!animate){
    for(let i=0;i<paths;i++){
      const seq = simulateOnePathFreq(N, p);
      finals[i] = seq[N];
      drawPath(seq);
    }
  } else {
    while(idx < paths){
      const to = Math.min(idx + batch, paths);
      for(let i=idx;i<to;i++){
        const seq = simulateOnePathFreq(N, p);
        finals[i] = seq[N];
        drawPath(seq);
      }
      idx = to;
      document.getElementById('info').textContent = `Simulated ${idx}/${paths} paths...`;
      await new Promise(r => setTimeout(r, delay));
    }
  }
  ctx.restore();

  // compute histogram of finals (f(N)) on [0,1]
  const bins = 40; // number of bins
  const counts = new Array(bins).fill(0);
  for(const v of finals){
    // clamp edge case v==1
    const clamped = Math.max(0, Math.min(1, v));
    const bin = Math.min(bins-1, Math.floor(clamped * bins));
    counts[bin]++;
  }
  const maxCount = Math.max(...counts, 1);

  // draw vertical histogram in hist area, bars horizontal width proportional to count
  const hw = hist.w, hh = hist.h, hm = hist.margins;
  const histInnerW = hw - hm.left - hm.right;
  const histInnerH = hh - hm.top - hm.bottom;
  const binHeight = histInnerH / bins;

  // for alignment, we draw histogram from top (1.0) to bottom (0.0)
  ctx.save();
  for(let b=0;b<bins;b++){
    const cnt = counts[b];
    if(cnt === 0) continue;
    const yTop = hm.top + b * binHeight;
    const barW = (cnt / maxCount) * (histInnerW - 8);
    const xLeft = hist.x + hm.left + (histInnerW - 8) - barW; // align to right within hist area
    ctx.fillStyle = 'rgba(220,40,40,0.9)';
    ctx.fillRect(xLeft, yTop + 2, barW, Math.max(1, binHeight - 4));
  }
  // draw y ticks inside histogram for reference (0..1)
  ctx.fillStyle = '#38506b';
  ctx.font = '11px system-ui';
  for(let i=0;i<=4;i++){
    const frac = 1 - i/4;
    const by = hm.top + (1 - frac) * histInnerH;
    ctx.fillText(frac.toFixed(2), hist.x + 8, by + 4);
    ctx.beginPath();
    ctx.strokeStyle = '#edf4ff';
    ctx.moveTo(hist.x + hm.left, by);
    ctx.lineTo(hist.x + hm.left + histInnerW, by);
    ctx.stroke();
  }
  ctx.restore();

  // draw small marker for theoretical p on histogram (horizontal line across histogram)
  const pPos = hm.top + (1 - p) * histInnerH;
  ctx.save();
  ctx.strokeStyle = 'rgba(20,110,230,1)';
  ctx.lineWidth = 2;
  ctx.beginPath();
  ctx.moveTo(hist.x + hm.left - 2, pPos);
  ctx.lineTo(hist.x + hm.left + histInnerW + 2, pPos);
  ctx.stroke();
  ctx.restore();

  // show summary
  const empiricalMean = finals.reduce((a,b)=>a+b,0) / paths;
  document.getElementById('info').innerHTML =
    `Paths: ${paths}, Trials N: ${N}, p: ${p.toFixed(3)} — empirical mean f(N): ${empiricalMean.toFixed(4)}.`;

  return { finals, counts, bins };
}

// UI wiring
document.getElementById('run').addEventListener('click', async () => {
  const N = Math.max(1, Math.min(2000, parseInt(document.getElementById('N').value) || 200));
  const paths = Math.max(1, Math.min(2000, parseInt(document.getElementById('paths').value) || 200));
  const p = Math.max(0, Math.min(1, parseFloat(document.getElementById('pprob').value) || 0.5));
  const animate = (document.getElementById('animate').value === 'true');
  const delay = Math.max(0, parseInt(document.getElementById('delay').value) || 10);
  document.getElementById('info').textContent = 'Running simulation...';
  await runSimulation({ N, paths, p, animate, delay });
});

document.getElementById('clear').addEventListener('click', () => {
  resizeCanvas();
  clearCanvas();
  document.getElementById('info').textContent = 'Cleared.';
});

// init
document.getElementById('info').textContent = 'Ready. Set parameters and click Run.';
</script>
</body>




# Code and explanation
![Code of the demo](./carbon(1).png)



This part of the document provides a detailed, research-style explanation of the HTML/JavaScript code for simulating Bernoulli trajectories, plotting their cumulative successes, and visualizing the Gaussian approximation via the Central Limit Theorem (CLT). The goal is to interpret the code as a formal study in stochastic processes and computational statistics.

---

##  Overview

The application simulates **repeated Bernoulli trials**, where each trial results in "success" with probability `p` or "failure" with probability `1-p`. For a given number of trials `N` and paths `paths`, the program:

1. Generates multiple independent sample paths of cumulative successes.
2. Plots trajectories of cumulative successes vs trial number.
3. Computes and plots the histogram of final successes.
4. Overlays a Gaussian curve approximating the distribution of final successes (CLT).

This structure allows users to visually explore how empirical distributions converge to theoretical predictions.

---

##  Simulation of Bernoulli Trials

### Random Bernoulli Generator

```js
function randBernoulli(p) {
  return Math.random() < p ? 1 : 0;
}
```

* Simulates a single Bernoulli trial.
* Returns `1` for success, `0` for failure.
* Fundamental for stochastic path generation.

### Path Simulation

```js
function simulateOnePath(){
  const seq = new Array(N+1);
  let cum = 0;
  seq[0] = 0;
  for(let t=1; t<=N; t++){
    cum += randBernoulli(p);
    seq[t] = cum;
  }
  return seq;
}
```

* Generates **cumulative successes** for `N` trials.
* Stores sequence in `seq`.
* Enables trajectory plotting.

> Each trajectory represents a **stochastic path** of successes, demonstrating variance in outcomes even with identical probabilities.

---

##  Visualization and Canvas Utilities

### Canvas Setup

```js
function resizeCanvas(){
  const cssW = canvas.clientWidth;
  const cssH = 420;
  const ratio = window.devicePixelRatio || 1;
  canvas.width = Math.floor(cssW * ratio);
  canvas.height = Math.floor(cssH * ratio);
  ctx.setTransform(ratio,0,0,ratio,0,0);
}
```

* Ensures high-resolution rendering for modern displays.
* Maintains aspect ratio and clarity when resizing.

### Axes and Grid

```js
function drawAxes(margins, N, maxY){
  // Draw horizontal and vertical grids
  // Add axis labels and tick marks
}
```

* Provides **visual context** for trajectories and histograms.
* Labels the axes: `Tries →` and `Successes →`.
* Includes y-ticks and x-ticks for interpretation.

---

##  Plotting Trajectories

```js
function drawPath(seq, colorAlpha=0.12){
  ctx.beginPath();
  ctx.strokeStyle = `rgba(20,110,230,${colorAlpha})`;
  ctx.moveTo(map.x(0), map.y(seq[0]));
  for(let t=1; t<=N; t++){
    ctx.lineTo(map.x(t), map.y(seq[t]));
  }
  ctx.stroke();
}
```

* Draws each path with transparency for visual clarity.
* **Mean line** is plotted separately as `y = t * p`, representing expected cumulative successes.

```js
ctx.strokeStyle = 'rgba(20,110,230,1)';
ctx.lineWidth = 2;
ctx.beginPath();
ctx.moveTo(map.x(0), map.y(0));
for(let t=0; t<=N; t++){
  ctx.lineTo(map.x(t), map.y(t * p));
}
ctx.stroke();
```

> This illustrates **expected vs realized outcomes** in Bernoulli processes.

---

##  Histogram of Final Successes

```js
const bins = new Array(N+1).fill(0);
for(const v of finals) bins[v]++;
```

* Aggregates the final success counts from all paths.
* Represents the **empirical distribution** of the number of successes.

```js
ctx.fillStyle = 'rgba(220,40,40,0.9)';
ctx.fillRect(centerX - binWidthPx*0.4, barTopY, binWidthPx*0.8, barH);
```

* Draws histogram bars proportional to frequency.
* Red bars highlight the distribution of terminal outcomes.

---

##  Gaussian Approximation (CLT)

```js
function gaussianPdf(x, mu, sigma){
  return Math.exp(-0.5*((x-mu)/sigma)**2) / (Math.sqrt(2*Math.PI)*sigma);
}
```

* Evaluates the Gaussian PDF for overlay.
* Parameters:

  * `mu = N * p` (expected successes)
  * `sigma = sqrt(N * p * (1-p))` (standard deviation)

```js
for(let k=0; k<=N; k++){
  const pdfVal = gaussianPdf(k, mu, sigma);
  const expectedCount = pdfVal * paths;
  theoCounts[k] = expectedCount;
}
```

* Approximates the expected frequency of each final success count.
* Drawn as a green curve overlaying the histogram.

> Demonstrates **convergence of empirical distribution to theoretical CLT prediction**.

---

##  User Interaction

* **Inputs:** Number of trials (`N`), number of paths, success probability (`p`), animation toggle, batch delay.
* **Run button:** Initiates simulation and visualization.
* **Clear button:** Resets the canvas.

```js
document.getElementById('run').addEventListener('click', async () => {
  await runSimulation({ N, paths, p, animate, delay });
});
```

> Allows real-time experimentation with stochastic behavior and CLT convergence.

---

##  Research Significance

* **Stochastic trajectories:** Illustrate variability in repeated Bernoulli experiments.
* **Histogram vs Gaussian:** Demonstrates **law of large numbers** and **CLT** in practice.
* **Interactive simulation:** Enables empirical validation of probabilistic theory.
* **Educational utility:** Bridges theory (probability) and computation (simulation and visualization).

This framework could support a **university-level study** on empirical probability distributions, convergence properties, and statistical approximation methods.

---

##  Conclusion

The code provides a comprehensive tool to:

1. Simulate stochastic Bernoulli trials.
2. Visualize both individual trajectories and aggregate distributions.
3. Empirically demonstrate the **convergence of sample statistics** to theoretical expectations.

By integrating JavaScript simulations with dynamic visualizations, this implementation becomes a powerful educational and research platform for **probability theory and statistical learning**.

