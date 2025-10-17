# Convergence to Probability

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

<style>
  body { font-family: system-ui, -apple-system, "Segoe UI", Roboto, Arial; background:#f7fbff; color:#0b2445; padding:20px; }
  .card { background:#fff; border-radius:10px; padding:16px; box-shadow:0 6px 18px rgba(11,37,69,0.06); max-width:1100px; margin:0 auto; }
  h1 { margin:0 0 8px; font-size:18px; }
  .row { display:flex; gap:12px; align-items:center; margin-top:10px; flex-wrap:wrap; }
  label { font-weight:600; font-size:13px; margin-right:6px; }
  input[type=number], input[type=range], select { padding:6px 8px; border-radius:8px; border:1px solid #e6eefc; }
  button { padding:8px 12px; border-radius:8px; border:0; background:#2563eb; color:white; font-weight:700; cursor:pointer; }
  canvas { background: linear-gradient(180deg,#ffffff,#f1f7ff); border-radius:8px; display:block; margin-top:12px; width:100%; max-width:100%; height:420px; }
  .meta { font-size:13px; color:#446; margin-top:8px; }
  .legend { display:flex; gap:12px; margin-top:8px; flex-wrap:wrap; }
  .legend div { display:flex; align-items:center; gap:6px; font-size:13px; }
  .swatch { width:18px; height:8px; border-radius:3px; display:inline-block; }
</style>
</head>
<body>
  <div class="card">
    <h1>Bernoulli Trajectories — "Number of successes" vs "Number of tries"</h1>
    <p class="meta">Simulate multiple sample paths of repeated Bernoulli trials (success prob <code>p</code>), plot trajectories (#successes over trials) and show the empirical final-success histogram with Gaussian (CLT) overlay.</p>

    <div class="row">
      <label>Trials (N)</label>
      <input id="N" type="number" min="1" max="2000" value="200" />
      <label>Paths</label>
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
      <div><span class="swatch" style="background:rgba(20,110,230,0.18)"></span> trajectories (light)</div>
      <div><span class="swatch" style="background:rgba(20,110,230,1)"></span> mean line (np)</div>
      <div><span class="swatch" style="background:rgba(220,40,40,0.9)"></span> histogram (final successes)</div>
      <div><span class="swatch" style="background:rgba(40,200,40,0.9)"></span> Gaussian overlay</div>
    </div>

    <div class="meta" id="info"></div>
  </div>

<script>
/*
  Trajectories simulation and plot
  - Generates 'paths' sample paths of length N with Bernoulli(p)
  - Plots each trajectory: x=trial index (0..N), y=cumulative successes (0..N)
  - Computes histogram of final successes across paths
  - Overlays Gaussian (CLT) approx for final successes:
      mean = N*p, variance = N*p*(1-p)
  - Options: animate vs instant
*/

// ---- helpers ----
function randBernoulli(p){ return Math.random() < p ? 1 : 0; }
function linspace(a,b,n){ const out=[]; if(n===1) return [a]; for(let i=0;i<n;i++) out.push(a + (b-a)*i/(n-1)); return out; }
function gaussianPdf(x, mu, sigma){ return Math.exp(-0.5*((x-mu)/sigma)**2) / (Math.sqrt(2*Math.PI)*sigma); }

// ---- canvas plotting utilities ----
const canvas = document.getElementById('plot');
const ctx = canvas.getContext('2d');

function resizeCanvas(){
  // make canvas high-res for crispness
  const cssW = canvas.clientWidth;
  const cssH = 420;
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

// draw axes and grid
function drawAxes(margins, N, maxY){
  const w = canvas.clientWidth;
  const h = parseInt(canvas.style.height);
  ctx.save();
  ctx.strokeStyle = '#e6eefc';
  ctx.lineWidth = 1;
  // grid lines (y)
  for(let i=0;i<=5;i++){
    const yy = margins.top + (h - margins.top - margins.bottom) * i/5;
    ctx.beginPath();
    ctx.moveTo(margins.left, yy);
    ctx.lineTo(w - margins.right, yy);
    ctx.stroke();
  }
  // axes
  ctx.strokeStyle = '#a9c2f7';
  ctx.lineWidth = 1.2;
  ctx.beginPath();
  ctx.moveTo(margins.left, margins.top);
  ctx.lineTo(margins.left, h - margins.bottom);
  ctx.lineTo(w - margins.right, h - margins.bottom);
  ctx.stroke();
  // labels
  ctx.fillStyle = '#38506b';
  ctx.font = '12px system-ui';
  ctx.fillText('Tries →', w - margins.right - 40, h - margins.bottom + 18);
  ctx.fillText('Successes →', margins.left - 68, margins.top + 10);
  // y ticks
  for(let i=0;i<=5;i++){
    const val = Math.round(maxY * (1 - i/5));
    const yy = margins.top + (h - margins.top - margins.bottom) * i/5;
    ctx.fillText(String(val), margins.left - 38, yy + 4);
  }
  // x ticks: 0, N/4, N/2, 3N/4, N
  const xs = [0, Math.floor(N/4), Math.floor(N/2), Math.floor(3*N/4), N];
  for(const xi of xs){
    const xx = margins.left + (w - margins.left - margins.right) * (xi / N);
    ctx.fillText(String(xi), xx - 8, h - margins.bottom + 18);
  }

  ctx.restore();
}

// map data coords -> canvas pixel
function makeMapper(margins, N, maxY){
  const w = canvas.clientWidth, h = parseInt(canvas.style.height);
  return {
    x: x => margins.left + (w - margins.left - margins.right) * (x / N),
    y: y => margins.top + (h - margins.top - margins.bottom) * (1 - (y / maxY))
  };
}

// ---- main simulation & plot ----
async function runSimulation(opts){
  // options
  const N = opts.N; // number of trials
  const paths = opts.paths;
  const p = opts.p;
  const animate = opts.animate;
  const delay = opts.delay;

  const margins = { left: 70, right: 40, top: 30, bottom: 50 };
  const maxY = N; // successes range 0..N
  resizeCanvas();
  clearCanvas();
  drawAxes(margins, N, maxY);
  const map = makeMapper(margins, N, maxY);

  // draw expected mean line (np)
  ctx.save();
  ctx.strokeStyle = 'rgba(20,110,230,1)';
  ctx.lineWidth = 2;
  ctx.beginPath();
  ctx.moveTo(map.x(0), map.y(0));
  // mean successes grows linearly: mean(t) = t * p
  for(let t=0;t<=N;t++){
    const meanY = t * p;
    ctx.lineTo(map.x(t), map.y(meanY));
  }
  ctx.stroke();
  ctx.restore();

  // storage for final successes
  const finals = new Array(paths).fill(0);

  // To reduce overdraw, draw trajectories in lighter color
  ctx.save();
  ctx.lineWidth = 1.2;

  // If not animating, we'll draw all trajectories faintly and then overlay histogram+gauss
  // If animating, draw in batches with small pause to visualize growth
  const batch = 5; // how many paths per immediate iteration (if animating)
  let pathIndex = 0;

  // pre-generate all paths if not animating? We'll generate on the fly
  function simulateOnePath(){
    const seq = new Array(N+1);
    let cum = 0;
    seq[0] = 0;
    for(let t=1;t<=N;t++){
      cum += randBernoulli(p);
      seq[t] = cum;
    }
    return seq;
  }

  function drawPath(seq, colorAlpha=0.12){
    ctx.beginPath();
    ctx.strokeStyle = `rgba(20,110,230,${colorAlpha})`;
    ctx.moveTo(map.x(0), map.y(seq[0]));
    for(let t=1;t<=N;t++){
      ctx.lineTo(map.x(t), map.y(seq[t]));
    }
    ctx.stroke();
  }

  // animate or instant
  if(!animate){
    // instant: draw all trajectories quickly
    for(let i=0;i<paths;i++){
      const seq = simulateOnePath();
      finals[i] = seq[N];
      drawPath(seq, 0.12);
    }
    ctx.restore();
  } else {
    // animated: draw in small batches so UI remains responsive
    ctx.restore();
    while(pathIndex < paths){
      const to = Math.min(pathIndex + batch, paths);
      for(let i=pathIndex;i<to;i++){
        const seq = simulateOnePath();
        finals[i] = seq[N];
        drawPath(seq, 0.12);
      }
      pathIndex = to;
      // update progress text
      document.getElementById('info').textContent = `Simulated ${pathIndex}/${paths} paths...`;
      // yield to UI
      await new Promise(r => setTimeout(r, delay));
    }
  }

  // after drawing trajectories, compute histogram of finals
  // bins are integers 0..N
  const bins = new Array(N+1).fill(0);
  for(const v of finals) bins[v]++;

  // plot histogram (bars) at bottom area with red color
  // compute scaling for histogram: find max bin count to scale height
  const maxBin = Math.max(...bins);
  // we draw histogram in a separate overlay area: map y from 0..maxY already, but histogram height scale proportional to counts
  // We'll normalize a histogram bar height to at most 40% of canvas height
  const histMaxHeight = (canvas.clientHeight - margins.top - margins.bottom) * 0.35;
  ctx.save();
  for(let k=0;k<=N;k++){
    if(bins[k] === 0) continue;
    const binCount = bins[k];
    const binWidthPx = Math.max(2, ( (canvas.clientWidth - margins.left - margins.right) / (N+1) ));
    const centerX = map.x(k);
    const barH = (binCount / maxBin) * histMaxHeight;
    const barTopY = (canvas.clientHeight - margins.bottom) - barH;
    ctx.fillStyle = 'rgba(220,40,40,0.9)';
    ctx.fillRect(centerX - binWidthPx*0.4, barTopY, binWidthPx*0.8, barH);
  }
  ctx.restore();

  // compute and overlay Gaussian curve (CLT) for final successes
  // mean = N*p, var = N*p*(1-p)
  const mu = N * p;
  const sigma = Math.sqrt(N * p * (1 - p));
  // To align pdf with histogram, scale pdf by (paths * binWidth)
  const binWidth = 1; // integer bins
  const scaleFactor = paths * binWidth; // scaling of pdf so area ~ paths
  // But we must map pdf values to pixel heights matching histogram scaling used above.
  // We'll compute theoretical counts for each integer k and draw a green polyline connecting them,
  // convert counts to pixel heights using same histMaxHeight and maxBin.
  const theoCounts = new Array(N+1).fill(0);
  let theoMax = 0;
  for(let k=0;k<=N;k++){
    const pdfVal = gaussianPdf(k, mu, sigma);
    const expectedCount = pdfVal * paths; // approximate expected number of paths landing on value k
    theoCounts[k] = expectedCount;
    if(expectedCount > theoMax) theoMax = expectedCount;
  }
  // We want to draw gaussian overlay scaled to histogram's pixel scale; compute factor to convert counts -> pixels:
  const countToPx = (maxBin === 0) ? 0 : (histMaxHeight / maxBin);

  // draw gaussian overlay as smooth curve
  ctx.save();
  ctx.lineWidth = 2.4;
  ctx.strokeStyle = 'rgba(40,180,40,0.95)';
  ctx.beginPath();
  let first = true;
  for(let k=0;k<=N;k++){
    const cnt = theoCounts[k];
    const barH = cnt * countToPx;
    const y = (canvas.clientHeight - margins.bottom) - barH;
    const x = map.x(k);
    if(first){ ctx.moveTo(x, y); first=false; } else ctx.lineTo(x, y);
  }
  ctx.stroke();
  ctx.restore();

  // draw small label showing mu and sigma and some metrics
  document.getElementById('info').innerHTML =
    `Paths: ${paths}, Trials N: ${N}, p: ${p.toFixed(2)} — empirical mean of final successes: ${(finals.reduce((a,b)=>a+b,0)/paths).toFixed(3)}; theoretical mean: ${mu.toFixed(3)}; σ = ${sigma.toFixed(3)}.`;

  // done
  return { finals, bins, mu, sigma };
}

// ---- UI wiring ----
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

// initialize example
document.getElementById('info').textContent = 'Ready. Set parameters and click Run.';
</script>
