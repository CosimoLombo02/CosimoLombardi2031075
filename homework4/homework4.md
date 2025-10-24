# CosimoLombardi2031075
## Theoretical Background

### Bernoulli Trials

A **Bernoulli trial** is a random experiment with exactly two possible outcomes:
- *Success* (denoted by 1) with probability p,
- *Failure* (denoted by 0) with probability 1 - p.

Let X1, X2, ..., Xn be independent Bernoulli random variables, each with the same success probability p.  
Then the random sum

S_n = X1 + X2 + ... + Xn

represents the **total number of successes** after n trials.

### Relative Frequency and the Law of Large Numbers (LLN)

The **relative frequency** of successes after n trials is defined as

f_n = S_n / n = (X1 + X2 + ... + Xn) / n

The **Law of Large Numbers (LLN)** states that, as n becomes very large,

f_n converges in probability to p

i.e., the empirical mean f_n converges to the theoretical success probability p.

Intuitively, as we repeat the experiment many times, the fraction of successes observed stabilizes around the true value p.

### Connection to the Central Limit Theorem (CLT)

Although the LLN describes convergence in probability, it does not specify how *fast* this convergence happens.  
The **Central Limit Theorem (CLT)** states that

(S_n - n * p) / sqrt(n * p * (1 - p)) tends to a standard normal distribution as n becomes very large.

This implies that fluctuations of f_n around p scale roughly as

f_n ≈ p + O(1 / sqrt(n)),

so the variance of f_n decreases approximately like 1/n.

### Empirical Distribution and Convergence Visualization

When simulating many independent trajectories of f_n, we can empirically observe the LLN:

- For small n, the trajectories are noisy and vary significantly across realizations.  
- As n increases, the trajectories cluster around y = p.  
- The histogram of the final values f_N (after N trials) becomes increasingly concentrated near p.

This visualization provides both qualitative and quantitative evidence of the Law of Large Numbers.

















## Demo

<head>
<meta charset="utf-8" />
<title>Demo</title>
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
    <h1>Demo</h1>
    <p class="meta">Demo</p>

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
      <div><span class="swatch" style="background:rgba(20,110,230,0.18)"></span> trajectories f(t) </div>
      <div><span class="swatch" style="background:rgba(20,110,230,1)"></span> theoretical average (y = p)</div>
      <div><span class="swatch" style="background:rgba(220,40,40,0.9)"></span> historgram of f(N)</div>
    </div>

    <div class="meta" id="info"></div>
  </div>

<script>


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
![Code of the demo](./carbon.png)

##  Simulation of the Law of Large Numbers

We simulate the LLN by generating multiple **trajectories** of Bernoulli trials and plotting their **relative frequencies** over time.

## Conceptual Overview

1. Generate \( m \) independent trajectories, each consisting of \( n \) Bernoulli trials.
2. For each trajectory:
   - Compute the cumulative sum of successes \( S_t \) at trial \( t \).
   - Compute the relative frequency \( f_t = S_t / t \) at each step.
3. Plot all trajectories simultaneously to visualize convergence.
4. At the final trial \( n \), compute a histogram of the \( f_n \) values across all trajectories.
5. Overlay the theoretical probability \( p \) to illustrate the LLN.

---

## Detailed Code Explanation

The simulation is implemented in **JavaScript** with an interactive HTML interface. Below is a step-by-step explanation of the key components.

## Bernoulli Random Generator

```js
function randBernoulli(p) {
    return Math.random() < p ? 1 : 0;
}
```

- Generates a single Bernoulli outcome (1 for success, 0 for failure).
- Uses `Math.random()` which returns a uniform value in [0,1).
- Compares with `p` to determine success or failure.

---

## Simulation of a Single Trajectory

```js
function simulateOnePath(N, p) {
    let seq = new Array(N + 1);
    let cum = 0;
    seq[0] = 0; // zero successes at trial 0
    for (let t = 1; t <= N; t++) {
        cum += randBernoulli(p);
        seq[t] = cum / t; // relative frequency at trial t
    }
    return seq;
}
```

**Explanation:**

1. `seq` stores the relative frequency \( f_t \) at each trial.
2. `cum` keeps the cumulative number of successes.
3. Loop from `t = 1` to `N`:
   - Draw a Bernoulli outcome and add to `cum`.
   - Compute `f_t = cum / t` for the current trial.
4. Returns an array representing the trajectory of relative frequencies.

---

## Generating Multiple Trajectories

```js
let allTrajectories = [];
for (let i = 0; i < paths; i++) {
    allTrajectories.push(simulateOnePath(N, p));
}
```

- `paths` = number of trajectories \( m \).
- Each trajectory is stored as an array in `allTrajectories`.
- Ensures independence between trajectories.

---

## Plotting Trajectories

**Coordinate mapping:**

```js
function makeMapper(margins, N) {
    const w = canvas.clientWidth, h = parseInt(canvas.style.height);
    return {
        x: x => margins.left + (w - margins.left - margins.right) * (x / N),
        y: y => margins.top + (h - margins.top - margins.bottom) * (1 - y) // y in [0,1]
    };
}
```

- Maps trial index `t` and relative frequency `f_t` to canvas coordinates.
- Y-axis is inverted because canvas origin is top-left.
- Ensures consistent scaling for all trajectories.

**Drawing each trajectory:**

```js
function drawPath(seq, colorAlpha=0.1){
    ctx.beginPath();
    ctx.strokeStyle = `rgba(20,110,230,${colorAlpha})`;
    ctx.moveTo(map.x(0), map.y(seq[0]));
    for(let t=1; t<=N; t++){
        ctx.lineTo(map.x(t), map.y(seq[t]));
    }
    ctx.stroke();
}
```

- Loops through the sequence of `f_t` values.
- Uses semi-transparent blue lines to allow overlapping trajectories to show density visually.

---

## Histogram of Final Relative Frequencies

```js
let finalFrequencies = allTrajectories.map(traj => traj[N]);

let bins = new Array(binCount).fill(0);
finalFrequencies.forEach(f => {
    let index = Math.floor(f * binCount);
    bins[index]++;
});
```

- `finalFrequencies` stores \( f_N \) for each trajectory.
- The histogram is computed by binning these values.
- Allows visualization of empirical distribution convergence towards \( p \).

---

## Overlay of Theoretical Probability \( p \)

- A horizontal line at `y = p` is drawn across the canvas.
- Shows the convergence of trajectories and the histogram toward the theoretical success probability.

---

## Animation and Interaction

- Trajectories can be drawn **incrementally** in batches to create an animated effect.
- Uses `await new Promise(r => setTimeout(r, delay))` to allow visualization of convergence over time without freezing the UI.

---

##  Statistical Output

After simulation, the following statistics are displayed:

- Empirical mean of \( f_N \) across all trajectories.
- Standard deviation \( \sigma = \sqrt{p(1-p)/N} \).
- Histogram of final relative frequencies compared with theoretical probability \( p \).

These quantitatively verify the LLN and provide intuition about convergence rates.

---

##  Pedagogical Interpretation

- **Trajectories:** initially noisy, but cluster around \( p \) as \( n \) grows.
- **Histogram:** distribution of \( f_N \) becomes concentrated near \( p \) with increasing \( n \).
- **LLN demonstration:** visually and quantitatively illustrates the stabilization of relative frequency with sample size.
- **CLT insight:** variance of relative frequencies decreases as \( 1/N \).













