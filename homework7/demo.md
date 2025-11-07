<head>
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
</head>
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
</body>
