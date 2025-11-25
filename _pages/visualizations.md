---
layout: page
title: visualizations
permalink: /visualizations/
description: Interactive demonstrations of mathematical concepts.
nav: true
nav_order: 3
---

<style>
  * {
    box-sizing: border-box;
  }

  .viz-hero {
    position: relative;
    width: 100%;
    min-height: min(600px, 80vh);
    background: linear-gradient(135deg, #f5f7fa 0%, #e8eef5 100%);
    border-radius: 20px;
    overflow: hidden;
    margin: 2rem 0;
    box-shadow: 0 10px 40px rgba(0,0,0,0.08);
    transition: background 0.3s ease, box-shadow 0.3s ease;
  }

  html[data-theme="dark"] .viz-hero {
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
    box-shadow: 0 10px 40px rgba(0,0,0,0.3);
  }

  .viz-header {
    position: absolute;
    top: 3rem;
    left: 3rem;
    right: 3rem;
    z-index: 10;
    color: #2c3e50;
  }

  .viz-header h1 {
    font-size: 3rem;
    font-weight: 700;
    margin: 0 0 1rem 0;
    letter-spacing: -0.02em;
    color: #1a1a1a;
    transition: color 0.3s ease;
  }

  html[data-theme="dark"] .viz-header h1 {
    color: #e2e8f0;
  }

  .viz-header p {
    font-size: 1.25rem;
    opacity: 0.7;
    max-width: 600px;
    line-height: 1.6;
    margin: 0;
    color: #4a5568;
    transition: color 0.3s ease;
  }

  html[data-theme="dark"] .viz-header p {
    color: #94a3b8;
  }

  .viz-canvas {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    cursor: pointer;
  }

  .viz-controls {
    position: absolute;
    bottom: 2rem;
    left: 50%;
    transform: translateX(-50%);
    z-index: 10;
    display: flex;
    gap: 1rem;
    backdrop-filter: blur(10px);
    background: rgba(255, 255, 255, 0.9);
    padding: 1rem 1.5rem;
    border-radius: 50px;
    border: 1px solid rgba(0, 0, 0, 0.08);
    transition: background 0.3s ease, border-color 0.3s ease;
  }

  html[data-theme="dark"] .viz-controls {
    background: rgba(30, 41, 59, 0.9);
    border-color: rgba(255, 255, 255, 0.1);
  }

  .viz-button {
    padding: 0.75rem 1.5rem;
    background: rgba(0, 0, 0, 0.04);
    color: #2c3e50;
    border: 1px solid rgba(0, 0, 0, 0.08);
    border-radius: 25px;
    cursor: pointer;
    font-size: 0.95rem;
    font-weight: 500;
    transition: all 0.3s ease;
  }

  html[data-theme="dark"] .viz-button {
    background: rgba(255, 255, 255, 0.1);
    color: #e2e8f0;
    border-color: rgba(255, 255, 255, 0.1);
  }

  .viz-button:hover {
    background: rgba(0, 0, 0, 0.08);
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
  }

  html[data-theme="dark"] .viz-button:hover {
    background: rgba(255, 255, 255, 0.2);
    box-shadow: 0 4px 12px rgba(0,0,0,0.3);
  }

  .viz-info {
    position: absolute;
    top: 3rem;
    right: 3rem;
    color: #2c3e50;
    font-size: 1rem;
    z-index: 10;
    backdrop-filter: blur(10px);
    background: rgba(255, 255, 255, 0.9);
    padding: 1rem 1.5rem;
    border-radius: 15px;
    border: 1px solid rgba(0, 0, 0, 0.08);
    transition: background 0.3s ease, border-color 0.3s ease, color 0.3s ease;
  }

  html[data-theme="dark"] .viz-info {
    background: rgba(30, 41, 59, 0.9);
    border-color: rgba(255, 255, 255, 0.1);
    color: #94a3b8;
  }

  .viz-info strong {
    font-size: 1.5rem;
    display: block;
    margin-top: 0.25rem;
    color: #1a1a1a;
    transition: color 0.3s ease;
  }

  html[data-theme="dark"] .viz-info strong {
    color: #e2e8f0;
  }

  .math-section {
    margin-top: 3rem;
    padding: 2rem;
    background: rgba(0, 0, 0, 0.02);
    border-radius: 15px;
    border-left: 4px solid #cbd5e0;
  }

  .distribution-label {
    position: absolute;
    bottom: 6rem;
    font-size: 0.85rem;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    color: #64748b;
    z-index: 10;
    transition: color 0.3s ease;
  }

  .distribution-label.source {
    left: 15%;
    color: #3b82f6;
  }

  html[data-theme="dark"] .distribution-label.source {
    color: #60a5fa;
  }

  .distribution-label.target {
    right: 15%;
    color: #64748b;
  }

  html[data-theme="dark"] .distribution-label.target {
    color: #94a3b8;
  }

  @media (max-width: 768px) {
    .viz-hero {
      min-height: min(500px, 70vh);
      border-radius: 12px;
      margin: 1rem 0;
    }
    .viz-header {
      position: relative;
      top: auto;
      left: auto;
      right: auto;
      padding: 1.5rem;
      text-align: center;
    }
    .viz-header h1 {
      font-size: 1.75rem;
    }
    .viz-header p {
      font-size: 1rem;
    }
    .viz-canvas {
      position: relative;
      height: 300px;
    }
    .viz-controls {
      position: relative;
      bottom: auto;
      left: auto;
      transform: none;
      flex-wrap: wrap;
      justify-content: center;
      margin: 1rem;
      padding: 0.75rem 1rem;
      border-radius: 15px;
    }
    .viz-button {
      padding: 0.6rem 1rem;
      font-size: 0.85rem;
    }
    .viz-info {
      position: relative;
      top: auto;
      right: auto;
      bottom: auto;
      margin: 1rem;
      text-align: center;
      display: inline-block;
    }
    .distribution-label {
      bottom: auto;
      top: 1rem;
      font-size: 0.7rem;
    }
    .distribution-label.source {
      left: 10%;
    }
    .distribution-label.target {
      right: 10%;
    }
  }
</style>

<div class="viz-hero">
  <div class="viz-header">
    <h1>Optimal Transport</h1>
    <p>Watch mass flow gracefully from one distribution to another, following paths of minimal cost.</p>
  </div>

  <canvas id="otCanvas" class="viz-canvas"></canvas>

  <div class="distribution-label source">Source μ</div>
  <div class="distribution-label target">Target ν</div>

  <div class="viz-info">
    Transport Cost
    <strong id="costDisplay">0.00</strong>
  </div>

  <div class="viz-controls">
    <button class="viz-button" onclick="resetPoints()">Reset</button>
    <button class="viz-button" onclick="generateRandom()">Random</button>
    <button class="viz-button" onclick="generateGaussian()">Gaussian</button>
    <button class="viz-button" onclick="toggleAnimation()">
      <span id="animToggle">Pause</span>
    </button>
  </div>
</div>

<div class="math-section">
  <p><strong>The Monge Problem (1781):</strong> Given source points \(\{x_i\}\) and target points \(\{y_j\}\), find the optimal matching \(\pi\) that minimizes total transport cost:</p>
  <p style="text-align: center; font-size: 1.1rem; margin: 1.5rem 0;">
  \[
  \min_{\pi} \sum_{i,j} \pi_{ij} \|x_i - y_j\|^2
  \]
  </p>
  <p>Click anywhere on the visualization to add particles. They'll automatically find their optimal transport partners and flow smoothly between distributions.</p>
</div>

<script>
const canvas = document.getElementById('otCanvas');
const ctx = canvas.getContext('2d');

// Responsive canvas sizing
function resizeCanvas() {
  const container = canvas.parentElement;
  canvas.width = container.clientWidth;
  canvas.height = container.clientHeight;
}
resizeCanvas();
window.addEventListener('resize', resizeCanvas);

let particles = [];
let animating = true;
let time = 0;
let lastMatchingUpdate = 0;
let cachedMatching = [];
let cachedTotalCost = 0;
const MATCHING_UPDATE_INTERVAL = 500; // Update matching every 500ms

// Dark mode detection
function isDarkMode() {
  return document.documentElement.getAttribute('data-theme') === 'dark';
}

class Particle {
  constructor(x, y, targetX, targetY, isSource) {
    this.startX = x;
    this.startY = y;
    this.x = x;
    this.y = y;
    this.targetX = targetX;
    this.targetY = targetY;
    this.isSource = isSource;
    this.progress = Math.random(); // Random starting phase for variety
    this.speed = 0.002 + Math.random() * 0.001; // Slowed down 4x
    this.radius = 4;
    this.isSource = isSource;
    this.pulsePhase = Math.random() * Math.PI * 2;
  }

  getColor() {
    if (this.isSource) {
      return isDarkMode() ? '#60a5fa' : '#3b82f6';
    }
    return isDarkMode() ? '#94a3b8' : '#64748b';
  }

  update() {
    if (!animating) return;

    this.progress += this.speed;
    if (this.progress >= 1) this.progress = 0;

    // Smooth ping-pong easing: goes 0->1->0 smoothly using sine wave
    // This prevents the teleportation effect
    const pingPong = Math.sin(this.progress * Math.PI);

    // Apply additional easing for smoother acceleration/deceleration
    const eased = pingPong * pingPong * (3 - 2 * pingPong);

    this.x = this.startX + (this.targetX - this.startX) * eased;
    this.y = this.startY + (this.targetY - this.startY) * eased;
  }

  draw() {
    const pulse = Math.sin(time * 2 + this.pulsePhase) * 0.2 + 1;
    const radius = this.radius * pulse;
    const color = this.getColor();

    // Subtle glow
    const gradient = ctx.createRadialGradient(this.x, this.y, 0, this.x, this.y, radius * 3);
    gradient.addColorStop(0, color + '40');
    gradient.addColorStop(0.5, color + '20');
    gradient.addColorStop(1, color + '00');

    ctx.fillStyle = gradient;
    ctx.beginPath();
    ctx.arc(this.x, this.y, radius * 3, 0, Math.PI * 2);
    ctx.fill();

    // Core
    ctx.fillStyle = color;
    ctx.beginPath();
    ctx.arc(this.x, this.y, radius, 0, Math.PI * 2);
    ctx.fill();
  }
}

function distance(p1, p2) {
  const dx = p1.x - p2.x;
  const dy = p1.y - p2.y;
  return Math.sqrt(dx * dx + dy * dy);
}

function computeMatching(sources, targets) {
  const matching = [];
  const usedTargets = new Set();

  for (let i = 0; i < sources.length; i++) {
    let bestTarget = -1;
    let bestDist = Infinity;

    for (let j = 0; j < targets.length; j++) {
      if (usedTargets.has(j)) continue;
      const d = distance(sources[i], targets[j]);
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

function animate() {
  time += 0.016;
  const now = Date.now();

  // Clear with subtle background (dark mode aware)
  const bgColor = isDarkMode() ? 'rgba(26, 26, 46, 0.3)' : 'rgba(255, 255, 255, 0.3)';
  ctx.fillStyle = bgColor;
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  // Update and draw particles
  for (const p of particles) {
    p.update();
  }

  // Draw connection lines (update matching less frequently)
  const sources = particles.filter(p => p.isSource);
  const targets = particles.filter(p => !p.isSource);

  if (sources.length > 0 && targets.length > 0) {
    // Only recompute matching every MATCHING_UPDATE_INTERVAL ms
    if (now - lastMatchingUpdate > MATCHING_UPDATE_INTERVAL) {
      cachedMatching = computeMatching(sources, targets);
      cachedTotalCost = 0;
      for (const m of cachedMatching) {
        cachedTotalCost += m.cost * m.cost;
      }
      lastMatchingUpdate = now;
      document.getElementById('costDisplay').textContent = cachedTotalCost.toFixed(0);
    }

    // Draw lines using cached matching
    const lineColor = isDarkMode() ? 'rgba(148, 163, 184, 0.3)' : 'rgba(148, 163, 184, 0.2)';
    for (const m of cachedMatching) {
      const src = sources[m.source];
      const tgt = targets[m.target];

      if (src && tgt) {
        ctx.strokeStyle = lineColor;
        ctx.lineWidth = 1.5;
        ctx.beginPath();
        ctx.moveTo(src.x, src.y);
        ctx.lineTo(tgt.x, tgt.y);
        ctx.stroke();
      }
    }
  }

  // Draw particles on top
  for (const p of particles) {
    p.draw();
  }

  requestAnimationFrame(animate);
}

canvas.addEventListener('click', (e) => {
  const rect = canvas.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;

  const isSource = x < canvas.width / 2;
  const startX = isSource ? x : canvas.width - x;
  const targetX = isSource ? canvas.width - x : x;

  particles.push(new Particle(startX, y, targetX, y, isSource));
});

function resetPoints() {
  particles = [];
}

function generateRandom() {
  particles = [];
  const n = 12;

  for (let i = 0; i < n; i++) {
    const y = 150 + Math.random() * (canvas.height - 300);
    const sourceX = 100 + Math.random() * (canvas.width * 0.3);
    const targetX = canvas.width - 100 - Math.random() * (canvas.width * 0.3);

    particles.push(new Particle(sourceX, y, targetX, y + (Math.random() - 0.5) * 100, true));
  }

  for (let i = 0; i < n; i++) {
    const y = 150 + Math.random() * (canvas.height - 300);
    const targetX = canvas.width - 100 - Math.random() * (canvas.width * 0.3);
    const sourceX = 100 + Math.random() * (canvas.width * 0.3);

    particles.push(new Particle(targetX, y, sourceX, y + (Math.random() - 0.5) * 100, false));
  }
}

function generateGaussian() {
  particles = [];
  const n = 15;

  function gaussian(mean, std) {
    const u1 = Math.random();
    const u2 = Math.random();
    return mean + std * Math.sqrt(-2 * Math.log(u1)) * Math.cos(2 * Math.PI * u2);
  }

  const centerY = canvas.height / 2;

  for (let i = 0; i < n; i++) {
    const sourceX = gaussian(canvas.width * 0.25, canvas.width * 0.08);
    const sourceY = gaussian(centerY, canvas.height * 0.15);
    const targetX = gaussian(canvas.width * 0.75, canvas.width * 0.08);
    const targetY = gaussian(centerY, canvas.height * 0.15);

    particles.push(new Particle(sourceX, sourceY, targetX, targetY, true));
  }

  for (let i = 0; i < n; i++) {
    const targetX = gaussian(canvas.width * 0.75, canvas.width * 0.08);
    const targetY = gaussian(centerY, canvas.height * 0.15);
    const sourceX = gaussian(canvas.width * 0.25, canvas.width * 0.08);
    const sourceY = gaussian(centerY, canvas.height * 0.15);

    particles.push(new Particle(targetX, targetY, sourceX, sourceY, false));
  }
}

function toggleAnimation() {
  animating = !animating;
  document.getElementById('animToggle').textContent = animating ? 'Pause' : 'Play';
}

// Start
generateGaussian();
animate();
</script>
