# CosimoLombardi2031075

## Various Averages (Measures of Location) and Their Applications

## Abstract

Measures of location, commonly known as averages, play a central role in descriptive statistics and data analysis. They provide single-value summaries that represent the center or typical value of a dataset. However, not all averages are equally appropriate for all kinds of data. This research explores the main measures of location—mean, median, mode, geometric mean, harmonic mean, and trimmed mean—examining their mathematical properties, interpretive implications, and the contexts in which they are most useful. The study also discusses the influence of outliers, data distribution, and scale of measurement on the choice of an appropriate average.

---

## Introduction

In statistics, the goal of summarizing data is often to reduce a dataset to a representative single number that reflects its “center.” This central value, or *measure of location*, provides insight into where most data points tend to cluster. While the arithmetic mean is the most commonly used measure, it is not universally applicable. For example, data that contain extreme outliers or that are ordinal in nature may require alternative measures such as the median or mode.

The choice of average depends on the data’s distribution, scale, and the analytical purpose. Misuse of an average can lead to misleading conclusions. Therefore, understanding the conceptual and mathematical basis of different measures of location is essential for sound statistical reasoning.

---

## The Arithmetic Mean

### Definition and Formula

The **arithmetic mean** is the sum of all observations divided by their count:

**Formula:**  
mean = (x₁ + x₂ + ... + xₙ) / n

where x₁, x₂, ..., xₙ are the data points and n is the total number of observations.

### Properties

- Sensitive to all values: every data point influences the mean.  
- Affected by outliers: extreme values can distort the mean significantly.  
- Applicable to interval and ratio scales.  
- Additive: the mean of combined datasets is a weighted average of their individual means.

### Appropriate Use

Use the arithmetic mean when:
- Data are symmetrically distributed.
- Outliers are minimal or have been appropriately managed.
- The variable is measured on an interval or ratio scale (e.g., income, height, temperature).

### Example

If the monthly incomes of five employees are 2000, 2200, 2300, 2500, and 20000,  
then:

mean = (2000 + 2200 + 2300 + 2500 + 20000) / 5 = 5800  

This mean value is misleading because most employees earn far less than 5800. Hence, the median (discussed below) may be more suitable.

---

## The Median

### Definition

The **median** is the middle value when data are arranged in order.  
For an odd number of observations, it is the central value;  
for an even number, it is the average of the two central values.

### Properties

- Not affected by extreme values.  
- Useful for skewed distributions.  
- Appropriate for ordinal, interval, or ratio data.

### Appropriate Use

Use the median when:
- Data are skewed (e.g., income, property prices).
- The dataset contains outliers.
- The data are ordinal (ranked data such as satisfaction ratings).

### Example

Using the previous income data: 2000, 2200, 2300, 2500, 20000  
→ Median = 2300  

This is a more representative measure of the “typical” income in the group.

---

## The Mode

### Definition

The **mode** is the value that occurs most frequently in a dataset.

### Properties

- Applicable to all scales of measurement, including nominal data.  
- Not unique: a dataset can be unimodal, bimodal, or multimodal.  
- Insensitive to outliers.

### Appropriate Use

The mode is suitable when:
- Data are categorical (e.g., most common brand preference).
- One wants to identify the most typical or popular category.
- The distribution is highly irregular or qualitative.

### Example

In a dataset of colors chosen by customers: {Red, Blue, Blue, Green, Blue, Red},  
the mode is **Blue**, indicating it is the most preferred color.

---

## The Geometric Mean

### Definition and Formula

The **geometric mean** is the nth root of the product of n positive observations.

**Formula:**  
G = (x₁ × x₂ × ... × xₙ) ^ (1 / n)

### Properties

- Used for multiplicative relationships (e.g., growth rates).  
- Less affected by extreme values than the arithmetic mean.  
- Defined only for positive numbers.

### Appropriate Use

The geometric mean is appropriate when:
- Data represent rates of change or ratios (e.g., returns on investment, growth indices).  
- The analysis involves percentage changes or log-normal distributions.

### Example

For annual growth rates of 10%, 20%, and 30%:

G = (1.10 × 1.20 × 1.30)^(1/3) - 1 = 0.198 (or 19.8%)  

This gives a more accurate measure of average growth than the arithmetic mean (20%).

---

## The Harmonic Mean

### Definition and Formula

The **harmonic mean** is the reciprocal of the arithmetic mean of the reciprocals.

**Formula:**  
H = n / (1/x₁ + 1/x₂ + ... + 1/xₙ)

### Properties

- Appropriate for rates such as speed, efficiency, or density.  
- Heavily influenced by small values.  
- Always less than or equal to the arithmetic mean.

### Appropriate Use

Use the harmonic mean when:
- The data are rates or ratios measured over a constant quantity (e.g., average speed over equal distances).  
- Combining rates of the same kind (e.g., price/earnings ratios).

### Example

If a car travels 60 km/h for half the trip and 40 km/h for the other half:

H = 2 / (1/60 + 1/40) = 48 km/h

---

## The Trimmed Mean

### Definition

The **trimmed mean** is the arithmetic mean after removing a fixed percentage of the highest and lowest values.

### Properties

- Reduces influence of outliers.  
- Balances sensitivity and robustness.  
- Common in robust statistics.

### Appropriate Use

The trimmed mean is ideal for:
- Datasets with occasional extreme values.  
- Situations requiring a compromise between mean and median.  
- Applications in economics, sports scores, and experimental data.

### Example

For exam scores: {30, 40, 50, 60, 70, 80, 90, 100},  
a 25% trimmed mean removes the two highest and lowest scores.  
Remaining values: {50, 60, 70, 80} → Mean = 65.

---

## Comparative Summary

| Measure | Sensitive to Outliers | Scale Type | Common Applications |
|----------|----------------------|-------------|----------------------|
| Arithmetic Mean | Yes | Interval/Ratio | Symmetric quantitative data |
| Median | No | Ordinal/Interval | Skewed data, outliers |
| Mode | No | Nominal/Ordinal | Categorical data |
| Geometric Mean | Moderate | Ratio | Growth rates, indices |
| Harmonic Mean | Yes (to small values) | Ratio | Rates, ratios |
| Trimmed Mean | Partially | Interval/Ratio | Robust estimation |

---

## Conclusion

Different measures of location provide different perspectives on the same dataset. The arithmetic mean is efficient but fragile in the presence of outliers; the median is robust but ignores the magnitude of extreme values; and the mode is conceptually simple but statistically limited. The geometric and harmonic means are indispensable in multiplicative or rate-based contexts, while the trimmed mean offers a balanced alternative.



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

    <div id="wholeInput" style="display:none;">
      <h3>Enter all numbers separated by commas:</h3>
      <textarea id="vectorInput" rows="3" placeholder="e.g. 1, 2, 3, 4, 5"></textarea>
      <button onclick="processWholeVector()">Calculate</button>
    </div>

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
    let values = [];

    function showWholeInput() {
      document.getElementById('wholeInput').style.display = 'block';
      document.getElementById('stepInput').style.display = 'none';
      document.getElementById('result').style.display = 'none';
    }

    function showStepInput() {
      document.getElementById('stepInput').style.display = 'block';
      document.getElementById('wholeInput').style.display = 'none';
      document.getElementById('result').style.display = 'none';
      values = [];
      document.getElementById('valuesList').innerText = 'Current data: []';
    }

    function resetValues() {
      values = [];
      document.getElementById('valuesList').innerText = 'Current data: []';
      document.getElementById('result').style.display = 'none';
      document.getElementById('singleValue').value = '';
    }

    function processWholeVector() {
      const input = document.getElementById('vectorInput').value;
      const arr = input.split(',')
        .map(x => parseFloat(x.trim()))
        .filter(x => !isNaN(x));
      if (arr.length === 0) {
        alert("Please enter valid numbers.");
        return;
      }
      showResults(arr);
    }

    function addValue() {
      const val = parseFloat(document.getElementById('singleValue').value);
      if (!isNaN(val)) {
        values.push(val);
        document.getElementById('singleValue').value = '';
        document.getElementById('valuesList').innerText = `Current data: [${values.join(', ')}]`;
        showResults(values); // Calcola subito mean e variance
      } else {
        alert("Please enter a valid number.");
      }
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

    function showResults(arr) {
      const m = mean(arr).toFixed(4);
      const v = arr.length > 1 ? variance(arr).toFixed(4) : 'N/A (need ≥2 values)';
      const res = `
        <strong>Data:</strong> [${arr.join(', ')}]<br>
        <strong>Mean:</strong> ${m}<br>
        <strong>Variance:</strong> ${v}
      `;
      const box = document.getElementById('result');
      box.innerHTML = res;
      box.style.display = 'block';
    }
  </script>
</body>

## Code of the demo
![Code of the demo](./carbon(2).png)



##  Code explanation and comparision between the two techniques

This part document analyzes the design and implementation of the **Mean and Variance Calculator** built in JavaScript and HTML.  
The program allows users to compute **mean** and **variance** of a dataset using two different input modes:

1. **Whole Vector Input** – all numbers entered at once (comma-separated)
2. **Step-by-Step Input** – values added one by one, with real-time computation

Both approaches achieve the same statistical result, but they differ in terms of user interaction, computation flow, and potential performance implications.

---

##  Overview of the Program

The program is composed of a single HTML file containing:

- A structured **user interface** built with HTML and CSS  
- A **JavaScript module** handling data collection, mean/variance computation, and dynamic output updates  

The calculator dynamically updates the displayed results without requiring page reloads, using **event-driven programming** and DOM manipulation.

---

##  Functional Description

###  Input Modes

- **Whole Vector Mode:**  
  - The user types all values in a single text area.  
  - The program splits the string on commas, converts values to floats, filters invalid entries, and computes the results once.

- **Step-by-Step Mode:**  
  - The user adds a new number using a numeric input field.  
  - After each insertion, the dataset (`values[]`) grows dynamically.  
  - The mean and variance are **recomputed and displayed instantly**, providing a real-time update.

---

##  Mathematical Computation

### Mean

The arithmetic mean is calculated as:

```
mean = (sum of all values) / (number of values)
```

In JavaScript:
```js
function mean(arr) {
  const sum = arr.reduce((a, b) => a + b, 0);
  return sum / arr.length;
}
```

### Variance

The sample variance is computed using:
```
variance = Σ(xi - mean)² / (n - 1)
```

In JavaScript:
```js
function variance(arr) {
  if (arr.length < 2) return 0;
  const m = mean(arr);
  const squaredDiffs = arr.map(x => Math.pow(x - m, 2));
  return squaredDiffs.reduce((a, b) => a + b, 0) / (arr.length - 1);
}
```

---

##  Step-by-Step Code Explanation

###  Structure and Initialization

At the top, the script defines:
```js
let values = [];
```
This array stores the dataset, dynamically updated depending on user input mode.

---

###  User Interface Logic

The HTML defines two separate sections (`div`s):
- `#wholeInput` → for vector input  
- `#stepInput` → for one-by-one input  

The visibility of these sections is toggled by the functions:
```js
function showWholeInput() { ... }
function showStepInput() { ... }
```
Each simply hides one section and displays the other, allowing mode switching.

---

###  Input Handling

#### (a) Whole Vector Mode
The function:
```js
function processWholeVector() { ... }
```
performs the following steps:

1. Reads the text area value (`vectorInput`).
2. Splits the string on commas.
3. Converts entries to floats (`parseFloat`).
4. Filters invalid numbers (`!isNaN(x)`).
5. Computes and displays mean and variance via `showResults()`.

---

#### (b) Step-by-Step Mode
For real-time updates:
```js
function addValue() {
  const val = parseFloat(document.getElementById('singleValue').value);
  if (!isNaN(val)) {
    values.push(val);
    document.getElementById('singleValue').value = '';
    document.getElementById('valuesList').innerText = `Current data: [${values.join(', ')}]`;
    showResults(values);
  }
}
```

Here:
- Each number is validated and appended to the dataset.  
- The input field is cleared for the next value.  
- The updated list of values is displayed immediately.  
- Mean and variance are **recomputed instantly**, keeping results live.

A `resetValues()` function allows the user to clear the dataset.

---

###  Computation Display

Results are presented inside a styled `<div>`:
```js
function showResults(arr) {
  const m = mean(arr).toFixed(4);
  const v = arr.length > 1 ? variance(arr).toFixed(4) : 'N/A (need ≥2 values)';
  const res = `
    <strong>Data:</strong> [${arr.join(', ')}]<br>
    <strong>Mean:</strong> ${m}<br>
    <strong>Variance:</strong> ${v}
  `;
  const box = document.getElementById('result');
  box.innerHTML = res;
  box.style.display = 'block';
}
```
The function uses string interpolation to dynamically build the HTML result block.

---

##  Comparative Analysis: Input Techniques

| Criterion | Whole Vector Input | Step-by-Step Input |
|------------|--------------------|--------------------|
| **Ease of Use** | Simple for short datasets; allows pasting from spreadsheets. | Better for progressive entry or live data collection. |
| **Computation Timing** | Mean/variance calculated once, at the end. | Recalculated after every input (real-time). |
| **Performance** | O(n) total computation per run. | O(n²) cumulative cost (each addition recalculates over all values). |
| **Responsiveness** | Static; single output. | Dynamic and interactive; shows learning or experiment evolution. |
| **Error Handling** | Easy to validate entire vector at once. | Requires continuous validation per value. |
| **User Experience** | Efficient for bulk data input. | Ideal for iterative entry or educational demonstration. |
| **Best Use Case** | Predefined data sets, e.g., from CSV or research data. | Live experiments, manual measurements, teaching. |

---

##  Computational Considerations

- **Time Complexity:**  
  - Whole vector mode → Single pass (O(n)).  
  - Step-by-step mode → Recomputes mean and variance on each insertion → cumulative O(n²).  
  However, for small or educational datasets, this overhead is negligible.

- **Memory Usage:**  
  Both methods maintain the full dataset in memory (`values[]`), so memory complexity is O(n) in both cases.

- **Optimization Potential:**  
  Real-time computation can be improved to O(1) per update using **incremental formulas** for mean and variance (e.g., Welford’s algorithm).

---

##  Conclusion

Both input techniques are valid, and the optimal choice depends on the **context**:

- **Whole Vector Input** excels when handling pre-collected data where efficiency and simplicity are desired.
- **Step-by-Step Input** is more interactive and pedagogical, providing immediate feedback for each new data point.

The provided implementation demonstrates both modes using clean, modular JavaScript, emphasizing **clarity, correctness, and usability** over raw computational performance.











Choosing an appropriate measure of location depends on both the statistical properties of the data and the purpose of analysis. Awareness of these nuances enhances the accuracy and interpretability of statistical summaries, preventing common pitfalls in data-driven decision-making.



