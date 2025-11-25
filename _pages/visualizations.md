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
    min-height: 600px;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    border-radius: 20px;
    overflow: hidden;
    margin: 2rem 0;
    box-shadow: 0 20px 60px rgba(0,0,0,0.3);
  }

  .viz-header {
    position: absolute;
    top: 3rem;
    left: 3rem;
    right: 3rem;
    z-index: 10;
    color: white;
  }

  .viz-header h1 {
    font-size: 3rem;
    font-weight: 700;
    margin: 0 0 1rem 0;
    letter-spacing: -0.02em;
    color: white;
  }

  .viz-header p {
    font-size: 1.25rem;
    opacity: 0.9;
    max-width: 600px;
    line-height: 1.6;
    margin: 0;
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
    background: rgba(255, 255, 255, 0.1);
    padding: 1rem 1.5rem;
    border-radius: 50px;
    border: 1px solid rgba(255, 255, 255, 0.2);
  }

  .viz-button {
    padding: 0.75rem 1.5rem;
    background: rgba(255, 255, 255, 0.2);
    color: white;
    border: 1px solid rgba(255, 255, 255, 0.3);
    border-radius: 25px;
    cursor: pointer;
    font-size: 0.95rem;
    font-weight: 500;
    transition: all 0.3s ease;
    backdrop-filter: blur(10px);
  }

  .viz-button:hover {
    background: rgba(255, 255, 255, 0.3);
    transform: translateY(-2px);
    box-shadow: 0 10px 20px rgba(0,0,0,0.2);
  }

  .viz-info {
    position: absolute;
    top: 3rem;
    right: 3rem;
    color: white;
    font-size: 1rem;
    z-index: 10;
    backdrop-filter: blur(10px);
    background: rgba(255, 255, 255, 0.1);
    padding: 1rem 1.5rem;
    border-radius: 15px;
    border: 1px solid rgba(255, 255, 255, 0.2);
  }

  .viz-info strong {
    font-size: 1.5rem;
    display: block;
    margin-top: 0.25rem;
  }

  .math-section {
    margin-top: 3rem;
    padding: 2rem;
    background: rgba(102, 126, 234, 0.05);
    border-radius: 15px;
    border-left: 4px solid #667eea;
  }

  @media (max-width: 768px) {
    .viz-header h1 {
      font-size: 2rem;
    }
    .viz-header {
      top: 2rem;
      left: 2rem;
      right: 2rem;
    }
    .viz-controls {
      flex-direction: column;
      gap: 0.5rem;
    }
    .viz-info {
      top: auto;
      bottom: 8rem;
      right: 2rem;
    }
  }
</style>

<div class="viz-hero">
  <div class="viz-header">
    <h1>Optimal Transport</h1>
    <p>Watch mass flow gracefully from one distribution to another, following paths of minimal cost.</p>
  </div>

  <canvas id="otCanvas" class="viz-canvas"></canvas>

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

class Particle {
  constructor(x, y, targetX, targetY, isSource) {
    this.startX = x;
    this.startY = y;
    this.x = x;
    this.y = y;
    this.targetX = targetX;
    this.targetY = targetY;
    this.isSource = isSource;
    this.progress = 0;
    this.speed = 0.008 + Math.random() * 0.004;
    this.radius = 4;
    this.hue = isSource ? 200 : 320;
    this.pulsePhase = Math.random() * Math.PI * 2;
  }

  update() {
    if (!animating) return;

    this.progress += this.speed;
    if (this.progress >= 1) this.progress = 0;

    // Smooth easing
    const eased = this.progress < 0.5
      ? 2 * this.progress * this.progress
      : 1 - Math.pow(-2 * this.progress + 2, 2) / 2;

    this.x = this.startX + (this.targetX - this.startX) * eased;
    this.y = this.startY + (this.targetY - this.startY) * eased;
  }

  draw() {
    const pulse = Math.sin(time * 2 + this.pulsePhase) * 0.3 + 1;
    const radius = this.radius * pulse;

    // Glow effect
    const gradient = ctx.createRadialGradient(this.x, this.y, 0, this.x, this.y, radius * 4);
    gradient.addColorStop(0, `hsla(${this.hue}, 70%, 60%, 0.8)`);
    gradient.addColorStop(0.5, `hsla(${this.hue}, 70%, 50%, 0.3)`);
    gradient.addColorStop(1, `hsla(${this.hue}, 70%, 40%, 0)`);

    ctx.fillStyle = gradient;
    ctx.beginPath();
    ctx.arc(this.x, this.y, radius * 4, 0, Math.PI * 2);
    ctx.fill();

    // Core
    ctx.fillStyle = `hsl(${this.hue}, 80%, 70%)`;
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

  // Create dark gradient background
  const grad = ctx.createLinearGradient(0, 0, canvas.width, canvas.height);
  grad.addColorStop(0, 'rgba(102, 126, 234, 0.05)');
  grad.addColorStop(1, 'rgba(118, 75, 162, 0.05)');
  ctx.fillStyle = grad;
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  // Update and draw particles
  for (const p of particles) {
    p.update();
  }

  // Draw connection lines with gradient
  const sources = particles.filter(p => p.isSource);
  const targets = particles.filter(p => !p.isSource);

  if (sources.length > 0 && targets.length > 0) {
    const matching = computeMatching(sources, targets);
    let totalCost = 0;

    for (const m of matching) {
      const src = sources[m.source];
      const tgt = targets[m.target];

      const gradient = ctx.createLinearGradient(src.x, src.y, tgt.x, tgt.y);
      gradient.addColorStop(0, 'hsla(200, 70%, 60%, 0.3)');
      gradient.addColorStop(1, 'hsla(320, 70%, 60%, 0.3)');

      ctx.strokeStyle = gradient;
      ctx.lineWidth = 2;
      ctx.beginPath();
      ctx.moveTo(src.x, src.y);
      ctx.lineTo(tgt.x, tgt.y);
      ctx.stroke();

      totalCost += m.cost * m.cost;
    }

    document.getElementById('costDisplay').textContent = totalCost.toFixed(0);
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
