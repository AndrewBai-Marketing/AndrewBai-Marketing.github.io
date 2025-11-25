---
layout: page
title: visualizations
permalink: /visualizations/
description: Interactive demonstrations of mathematical concepts.
nav: true
nav_order: 3
---

<style>
  .viz-container {
    margin: 2rem 0;
    padding: 1.5rem;
    border: 1px solid #ddd;
    border-radius: 8px;
    background: #fafafa;
  }

  .viz-canvas {
    background: white;
    border: 1px solid #ccc;
    border-radius: 4px;
    cursor: crosshair;
    display: block;
    margin: 1rem auto;
  }

  .viz-controls {
    text-align: center;
    margin: 1rem 0;
  }

  .viz-button {
    padding: 0.5rem 1rem;
    margin: 0 0.5rem;
    background: #2962ff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 0.9rem;
  }

  .viz-button:hover {
    background: #1e4fc2;
  }

  .viz-info {
    text-align: center;
    color: #666;
    font-size: 0.9rem;
    margin-top: 0.5rem;
  }
</style>

## Optimal Transport: The Monge Problem

The classical optimal transport problem, posed by Gaspard Monge in 1781, asks: *What is the most efficient way to move mass from one distribution to another?*

**How to interact:**
- Click on the left (blue) side to add source points
- Click on the right (red) side to add target points
- The visualization shows the optimal matching that minimizes total transport cost
- Use buttons below to reset or generate random examples

<div class="viz-container">
  <canvas id="otCanvas" class="viz-canvas" width="800" height="400"></canvas>

  <div class="viz-controls">
    <button class="viz-button" onclick="resetPoints()">Reset</button>
    <button class="viz-button" onclick="generateRandom()">Random Example</button>
    <button class="viz-button" onclick="generateGaussian()">Gaussian Clouds</button>
  </div>

  <div class="viz-info">
    Total transport cost: <strong id="costDisplay">0.00</strong>
  </div>
</div>

**The Math:** Given source points \\(\{x_i\}\\) and target points \\(\{y_j\}\\), we find the optimal matching \\(\pi\\) that minimizes:
\\[
\sum_{i,j} \pi_{ij} \|x_i - y_j\|^2
\\]
subject to matching constraints (each source matched to exactly one target).

<script>
const canvas = document.getElementById('otCanvas');
const ctx = canvas.getContext('2d');

let sourcePoints = [];
let targetPoints = [];

// Helper: Calculate Euclidean distance
function distance(p1, p2) {
  const dx = p1.x - p2.x;
  const dy = p1.y - p2.y;
  return Math.sqrt(dx * dx + dy * dy);
}

// Simple greedy matching for visualization (not globally optimal but fast)
// For perfect optimality, would need Hungarian algorithm
function computeMatching() {
  const n = Math.min(sourcePoints.length, targetPoints.length);
  if (n === 0) return [];

  const matching = [];
  const usedTargets = new Set();

  // Greedy: for each source, find nearest unused target
  for (let i = 0; i < sourcePoints.length; i++) {
    let bestTarget = -1;
    let bestDist = Infinity;

    for (let j = 0; j < targetPoints.length; j++) {
      if (usedTargets.has(j)) continue;
      const d = distance(sourcePoints[i], targetPoints[j]);
      if (d < bestDist) {
        bestDist = d;
        bestTarget = j;
      }
    }

    if (bestTarget !== -1) {
      matching.push({ source: i, target: bestTarget, cost: bestDist });
      usedTargets.add(bestTarget);
    }
  }

  return matching;
}

function draw() {
  // Clear canvas
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Draw dividing line
  ctx.strokeStyle = '#ddd';
  ctx.lineWidth = 2;
  ctx.beginPath();
  ctx.moveTo(canvas.width / 2, 0);
  ctx.lineTo(canvas.width / 2, canvas.height);
  ctx.stroke();

  // Compute and draw matching
  const matching = computeMatching();
  let totalCost = 0;

  ctx.strokeStyle = 'rgba(150, 150, 150, 0.5)';
  ctx.lineWidth = 1.5;

  for (const m of matching) {
    const src = sourcePoints[m.source];
    const tgt = targetPoints[m.target];

    ctx.beginPath();
    ctx.moveTo(src.x, src.y);
    ctx.lineTo(tgt.x, tgt.y);
    ctx.stroke();

    // Draw arrow head
    const angle = Math.atan2(tgt.y - src.y, tgt.x - src.x);
    const headLen = 10;
    ctx.beginPath();
    ctx.moveTo(tgt.x, tgt.y);
    ctx.lineTo(
      tgt.x - headLen * Math.cos(angle - Math.PI / 6),
      tgt.y - headLen * Math.sin(angle - Math.PI / 6)
    );
    ctx.moveTo(tgt.x, tgt.y);
    ctx.lineTo(
      tgt.x - headLen * Math.cos(angle + Math.PI / 6),
      tgt.y - headLen * Math.sin(angle + Math.PI / 6)
    );
    ctx.stroke();

    totalCost += m.cost * m.cost; // Squared Euclidean distance
  }

  // Draw source points (blue)
  ctx.fillStyle = '#2962ff';
  for (const p of sourcePoints) {
    ctx.beginPath();
    ctx.arc(p.x, p.y, 6, 0, 2 * Math.PI);
    ctx.fill();
  }

  // Draw target points (red)
  ctx.fillStyle = '#ff4444';
  for (const p of targetPoints) {
    ctx.beginPath();
    ctx.arc(p.x, p.y, 6, 0, 2 * Math.PI);
    ctx.fill();
  }

  // Update cost display
  document.getElementById('costDisplay').textContent = totalCost.toFixed(2);
}

canvas.addEventListener('click', (e) => {
  const rect = canvas.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;

  if (x < canvas.width / 2) {
    sourcePoints.push({ x, y });
  } else {
    targetPoints.push({ x, y });
  }

  draw();
});

function resetPoints() {
  sourcePoints = [];
  targetPoints = [];
  draw();
}

function generateRandom() {
  sourcePoints = [];
  targetPoints = [];

  const n = 8;
  for (let i = 0; i < n; i++) {
    sourcePoints.push({
      x: 50 + Math.random() * 300,
      y: 50 + Math.random() * 300
    });
    targetPoints.push({
      x: 450 + Math.random() * 300,
      y: 50 + Math.random() * 300
    });
  }

  draw();
}

function generateGaussian() {
  sourcePoints = [];
  targetPoints = [];

  const n = 10;

  // Box-Muller transform for Gaussian
  function gaussian(mean, std) {
    const u1 = Math.random();
    const u2 = Math.random();
    const z = Math.sqrt(-2 * Math.log(u1)) * Math.cos(2 * Math.PI * u2);
    return mean + z * std;
  }

  for (let i = 0; i < n; i++) {
    sourcePoints.push({
      x: gaussian(150, 50),
      y: gaussian(200, 50)
    });
    targetPoints.push({
      x: gaussian(650, 50),
      y: gaussian(200, 50)
    });
  }

  draw();
}

// Initial draw
draw();
// Start with a random example
generateRandom();
</script>

---

*Note: This visualization uses a greedy matching algorithm for real-time interaction. For the globally optimal solution, one would typically use the Hungarian algorithm or linear programming.*
