---
layout: default
title: AI Control
---

<style>
.control-panel {
  margin: 1.5rem 0;
  padding: 1.5rem;
  border: 2px solid #839788;
  border-radius: 8px;
  background: rgba(131, 151, 136, 0.08);
}
.control-panel h3 {
  margin-top: 0;
  color: #4a5a4f;
  border-bottom: 1px solid #839788;
  padding-bottom: 0.5rem;
}

.params-row {
  display: flex;
  gap: 2rem;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 1rem;
}
.param-group {
  display: flex;
  align-items: center;
  gap: 0.6rem;
}
.param-label {
  font-weight: bold;
  color: #4a5a4f;
  font-size: 0.9rem;
}
input[type="range"] {
  -webkit-appearance: none;
  width: 140px;
  height: 8px;
  border-radius: 4px;
  background: #d4c4b0;
  outline: none;
}
input[type="range"]::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: #839788;
  cursor: pointer;
  border: 2px solid #4a5a4f;
  transition: transform 0.2s;
}
input[type="range"]::-webkit-slider-thumb:hover {
  transform: scale(1.15);
}
input[type="range"]::-moz-range-thumb {
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: #839788;
  cursor: pointer;
  border: 2px solid #4a5a4f;
}
.value-display {
  font-family: "Inconsolata", monospace;
  background: #4a5a4f;
  color: #EEE0CB;
  padding: 0.2rem 0.5rem;
  border-radius: 4px;
  min-width: 45px;
  text-align: center;
  font-size: 0.85rem;
}

.graphs-container {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
  justify-content: center;
}
.graph-box {
  flex: 1;
  min-width: 320px;
  max-width: 480px;
}
.graph-box h4 {
  text-align: center;
  color: #4a5a4f;
  margin: 0 0 0.5rem 0;
  font-size: 0.95rem;
}
.graph-box svg {
  width: 100%;
  height: auto;
  display: block;
}

/* Legend */
.legend {
  display: flex;
  gap: 1.2rem;
  justify-content: center;
  margin: 1rem 0 0 0;
  flex-wrap: wrap;
  font-size: 0.8rem;
}
.legend-item {
  display: flex;
  align-items: center;
  gap: 0.3rem;
}
.legend-color {
  width: 14px;
  height: 14px;
  border-radius: 3px;
}
.legend-line {
  width: 20px;
  height: 3px;
  border-radius: 2px;
}

/* Protocol colors */
.protocol-trusted { color: #839788; }
.protocol-editing { color: #5a7a8a; }
.protocol-resample { color: #8a6a5a; }

@media (max-width: 700px) {
  .params-row {
    flex-direction: column;
    align-items: center;
    gap: 0.8rem;
  }
  .graphs-container {
    flex-direction: column;
  }
  .graph-box {
    max-width: 100%;
  }
}
</style>

## AI Control: Audit & Defer Protocols

Explore how different deferral protocols trade off **safety** vs **usefulness**. Move the defer threshold to trace each protocol's Pareto frontier.

---

<div class="control-panel">

<div class="params-row">
  <div class="param-group">
    <span class="param-label">Audit Budget:</span>
    <input type="range" id="audit-budget" min="1" max="10" value="2">
    <span class="value-display" id="audit-value">10%</span>
  </div>
  <div class="param-group">
    <span class="param-label">Defer Threshold:</span>
    <input type="range" id="defer-threshold" min="0" max="100" value="30">
    <span class="value-display" id="defer-value">0.30</span>
  </div>
</div>

<div class="graphs-container">
  <div class="graph-box">
    <h4>Suspicion Distributions</h4>
    <svg id="dist-graph" viewBox="0 0 400 240">
      <!-- Background regions -->
      <g id="regions"></g>
      
      <!-- Grid -->
      <g stroke="#d4c4b0" stroke-width="1" stroke-dasharray="3,3">
        <line x1="50" y1="40" x2="50" y2="210"/>
        <line x1="137" y1="40" x2="137" y2="210"/>
        <line x1="225" y1="40" x2="225" y2="210"/>
        <line x1="312" y1="40" x2="312" y2="210"/>
      </g>
      
      <!-- Axes -->
      <line x1="50" y1="210" x2="370" y2="210" stroke="#4a5a4f" stroke-width="2"/>
      <line x1="50" y1="210" x2="50" y2="30" stroke="#4a5a4f" stroke-width="2"/>
      
      <!-- X-axis labels -->
      <text x="50" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">0</text>
      <text x="137" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">0.25</text>
      <text x="225" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">0.5</text>
      <text x="312" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">0.75</text>
      <text x="370" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">1</text>
      <text x="210" y="235" text-anchor="middle" fill="#4a5a4f" font-size="10">Suspicion →</text>
      
      <!-- Distributions -->
      <path id="honest-dist" fill="rgba(131, 151, 136, 0.4)" stroke="#839788" stroke-width="2"/>
      <path id="attack-dist" fill="rgba(160, 96, 96, 0.35)" stroke="#a06060" stroke-width="2"/>
      
      <!-- Threshold lines -->
      <line id="defer-line" x1="150" y1="30" x2="150" y2="210" stroke="#4a5a4f" stroke-width="2" stroke-dasharray="6,3"/>
      <line id="audit-line" x1="300" y1="30" x2="300" y2="210" stroke="#272727" stroke-width="2.5"/>
      
      <!-- Threshold labels -->
      <text id="defer-label-text" x="150" y="22" text-anchor="middle" fill="#4a5a4f" font-size="8" font-weight="bold">DEFER</text>
      <text id="audit-label-text" x="300" y="22" text-anchor="middle" fill="#272727" font-size="8" font-weight="bold">AUDIT</text>
    </svg>
    <div class="legend">
      <div class="legend-item">
        <div class="legend-color" style="background: rgba(131, 151, 136, 0.6); border: 2px solid #839788;"></div>
        <span>Honest</span>
      </div>
      <div class="legend-item">
        <div class="legend-color" style="background: rgba(160, 96, 96, 0.5); border: 2px solid #a06060;"></div>
        <span>Attack</span>
      </div>
    </div>
  </div>
  
  <div class="graph-box">
    <h4>Pareto Frontiers: Safety vs Usefulness</h4>
    <svg id="pareto-graph" viewBox="0 0 400 240">
      <!-- Grid -->
      <g stroke="#d4c4b0" stroke-width="1" stroke-dasharray="3,3">
        <line x1="50" y1="50" x2="370" y2="50"/>
        <line x1="50" y1="90" x2="370" y2="90"/>
        <line x1="50" y1="130" x2="370" y2="130"/>
        <line x1="50" y1="170" x2="370" y2="170"/>
        <line x1="130" y1="30" x2="130" y2="210"/>
        <line x1="210" y1="30" x2="210" y2="210"/>
        <line x1="290" y1="30" x2="290" y2="210"/>
      </g>
      
      <!-- Axes -->
      <line x1="50" y1="210" x2="370" y2="210" stroke="#4a5a4f" stroke-width="2"/>
      <line x1="50" y1="210" x2="50" y2="30" stroke="#4a5a4f" stroke-width="2"/>
      
      <!-- X-axis labels (Usefulness) -->
      <text x="50" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">0%</text>
      <text x="130" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">25%</text>
      <text x="210" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">50%</text>
      <text x="290" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">75%</text>
      <text x="370" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">100%</text>
      <text x="210" y="235" text-anchor="middle" fill="#4a5a4f" font-size="10">Usefulness →</text>
      
      <!-- Y-axis labels (Safety) -->
      <text x="40" y="213" text-anchor="end" fill="#5a5a5a" font-size="9">0%</text>
      <text x="40" y="173" text-anchor="end" fill="#5a5a5a" font-size="9">25%</text>
      <text x="40" y="133" text-anchor="end" fill="#5a5a5a" font-size="9">50%</text>
      <text x="40" y="93" text-anchor="end" fill="#5a5a5a" font-size="9">75%</text>
      <text x="40" y="53" text-anchor="end" fill="#5a5a5a" font-size="9">100%</text>
      <text x="20" y="130" text-anchor="middle" fill="#4a5a4f" font-size="10" transform="rotate(-90,20,130)">Safety →</text>
      
      <!-- Pareto curves (drawn by JS) -->
      <path id="curve-resample" fill="none" stroke="#8a6a5a" stroke-width="3" stroke-linecap="round"/>
      <path id="curve-trusted" fill="none" stroke="#839788" stroke-width="3" stroke-linecap="round"/>
      <path id="curve-editing" fill="none" stroke="#5a7a8a" stroke-width="3" stroke-linecap="round"/>
      
      <!-- Current position markers -->
      <circle id="marker-resample" cx="200" cy="100" r="8" fill="#8a6a5a" stroke="#EEE0CB" stroke-width="2"/>
      <circle id="marker-trusted" cx="200" cy="80" r="8" fill="#839788" stroke="#EEE0CB" stroke-width="2"/>
      <circle id="marker-editing" cx="200" cy="60" r="8" fill="#5a7a8a" stroke="#EEE0CB" stroke-width="2"/>
    </svg>
    <div class="legend">
      <div class="legend-item">
        <div class="legend-line" style="background: #5a7a8a;"></div>
        <span class="protocol-editing">Trusted Editing</span>
      </div>
      <div class="legend-item">
        <div class="legend-line" style="background: #839788;"></div>
        <span class="protocol-trusted">Defer to Trusted</span>
      </div>
      <div class="legend-item">
        <div class="legend-line" style="background: #8a6a5a;"></div>
        <span class="protocol-resample">Resample</span>
      </div>
    </div>
  </div>
</div>

</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  
  // Distribution parameters
  const honestMean = 0.22;
  const honestStd = 0.11;
  const attackMean = 0.52;
  const attackStd = 0.17;
  
  // Protocol parameters - adjusted for more spread and curvature
  // Effectiveness: how well each neutralizes deferred attacks
  // Usefulness penalty: how much usefulness is lost when deferring
  const protocols = {
    editing:  { effectiveness: 0.92, usefulnessRetained: 0.90, color: '#5a7a8a' },
    trusted:  { effectiveness: 0.88, usefulnessRetained: 0.35, color: '#839788' },
    resample: { effectiveness: 0.30, usefulnessRetained: 0.92, color: '#8a6a5a' }
  };
  
  // DOM elements
  const auditSlider = document.getElementById('audit-budget');
  const deferSlider = document.getElementById('defer-threshold');
  const auditValueEl = document.getElementById('audit-value');
  const deferValueEl = document.getElementById('defer-value');
  
  // Distribution graph elements
  const honestPath = document.getElementById('honest-dist');
  const attackPath = document.getElementById('attack-dist');
  const deferLine = document.getElementById('defer-line');
  const auditLine = document.getElementById('audit-line');
  const deferLabelText = document.getElementById('defer-label-text');
  const auditLabelText = document.getElementById('audit-label-text');
  const regionsG = document.getElementById('regions');
  
  // Pareto graph elements
  const curveEditing = document.getElementById('curve-editing');
  const curveTrusted = document.getElementById('curve-trusted');
  const curveResample = document.getElementById('curve-resample');
  const markerEditing = document.getElementById('marker-editing');
  const markerTrusted = document.getElementById('marker-trusted');
  const markerResample = document.getElementById('marker-resample');
  
  // Normal distribution functions
  function normalPDF(x, mean, std) {
    const exp = -0.5 * Math.pow((x - mean) / std, 2);
    return Math.exp(exp) / (std * Math.sqrt(2 * Math.PI));
  }
  
  function normalCDF(x, mean, std) {
    const z = (x - mean) / std;
    const t = 1 / (1 + 0.2316419 * Math.abs(z));
    const d = 0.3989423 * Math.exp(-z * z / 2);
    const p = d * t * (0.3193815 + t * (-0.3565638 + t * (1.781478 + t * (-1.821256 + t * 1.330274))));
    return z > 0 ? 1 - p : p;
  }
  
  // Coordinate transformations for distribution graph
  function suspicionToXDist(s) { return 50 + s * 320; }
  
  // Coordinate transformations for Pareto graph
  function usefulnessToX(u) { return 50 + u * 320; }
  function safetyToY(s) { return 210 - s * 180; }
  
  // Find audit threshold given budget
  function findAuditThreshold(budget) {
    const pHonest = 0.9, pAttack = 0.1;
    let low = 0, high = 1;
    for (let i = 0; i < 20; i++) {
      const mid = (low + high) / 2;
      const exceedProb = pHonest * (1 - normalCDF(mid, honestMean, honestStd)) +
                         pAttack * (1 - normalCDF(mid, attackMean, attackStd));
      if (exceedProb > budget / 100) low = mid;
      else high = mid;
    }
    return (low + high) / 2;
  }
  
  // Calculate safety and usefulness for a protocol at given thresholds
  function calculateMetrics(deferThresh, auditThresh, protocol) {
    const p = protocols[protocol];
    
    // Attack distribution breakdown
    const attacksPassed = normalCDF(deferThresh, attackMean, attackStd);
    const attacksAboveAudit = 1 - normalCDF(auditThresh, attackMean, attackStd);
    const attacksDeferred = Math.max(0, 1 - attacksPassed - attacksAboveAudit);
    
    // Honest outputs breakdown (for usefulness)
    const honestPassed = normalCDF(deferThresh, honestMean, honestStd);
    const honestAboveAudit = 1 - normalCDF(auditThresh, honestMean, honestStd);
    const honestDeferred = Math.max(0, 1 - honestPassed - honestAboveAudit);
    
    // Safety = attacks caught or neutralized
    const safety = attacksAboveAudit + attacksDeferred * p.effectiveness;
    
    // Usefulness = outputs from untrusted model (capability preserved)
    // Passed outputs: full usefulness
    // Deferred outputs: partial usefulness depending on protocol
    // Audited outputs: assume full usefulness (just verified)
    const usefulness = honestPassed + honestDeferred * p.usefulnessRetained + honestAboveAudit;
    
    return { safety, usefulness, attacksPassed, attacksDeferred, attacksAboveAudit };
  }
  
  // Generate Pareto curve for a protocol with smooth curvature
  function generateParetoCurve(auditThresh, protocol) {
    const points = [];
    const steps = 50;
    
    for (let i = 0; i <= steps; i++) {
      // Use eased progression for smoother curve
      const t = i / steps;
      const deferThresh = t * (auditThresh - 0.01);
      const m = calculateMetrics(deferThresh, auditThresh, protocol);
      points.push({ 
        x: usefulnessToX(m.usefulness), 
        y: safetyToY(m.safety), 
        defer: deferThresh,
        usefulness: m.usefulness,
        safety: m.safety
      });
    }
    return points;
  }
  
  // Create smooth curve path using quadratic bezier
  function smoothPath(points) {
    if (points.length < 2) return '';
    
    let d = `M ${points[0].x} ${points[0].y}`;
    
    for (let i = 1; i < points.length; i++) {
      const p0 = points[i - 1];
      const p1 = points[i];
      d += ` L ${p1.x} ${p1.y}`;
    }
    
    return d;
  }
  
  // Draw distribution path
  function drawDistribution(mean, std, maxY) {
    const points = [];
    const startX = 50, endX = 370, baseY = 210;
    const scaleY = 160 / maxY;
    
    points.push(`M ${startX} ${baseY}`);
    for (let x = startX; x <= endX; x += 2) {
      const suspicion = (x - 50) / 320;
      const density = normalPDF(suspicion, mean, std);
      const y = baseY - density * scaleY;
      points.push(`L ${x} ${y}`);
    }
    points.push(`L ${endX} ${baseY} Z`);
    return points.join(' ');
  }
  
  // Main update function
  function update() {
    const auditBudget = parseInt(auditSlider.value);
    const deferThreshold = parseInt(deferSlider.value) / 100;
    
    auditValueEl.textContent = auditBudget + '%';
    deferValueEl.textContent = deferThreshold.toFixed(2);
    
    const auditThreshold = findAuditThreshold(auditBudget);
    const effectiveDefer = Math.min(deferThreshold, auditThreshold - 0.02);
    
    // === Update Distribution Graph ===
    const deferX = suspicionToXDist(effectiveDefer);
    const auditX = suspicionToXDist(auditThreshold);
    
    deferLine.setAttribute('x1', deferX);
    deferLine.setAttribute('x2', deferX);
    auditLine.setAttribute('x1', auditX);
    auditLine.setAttribute('x2', auditX);
    deferLabelText.setAttribute('x', deferX);
    auditLabelText.setAttribute('x', auditX);
    
    regionsG.innerHTML = `
      <rect x="50" y="30" width="${deferX - 50}" height="180" fill="rgba(131, 151, 136, 0.08)"/>
      <rect x="${deferX}" y="30" width="${auditX - deferX}" height="180" fill="rgba(186, 168, 152, 0.15)"/>
      <rect x="${auditX}" y="30" width="${370 - auditX}" height="180" fill="rgba(39, 39, 39, 0.08)"/>
    `;
    
    const maxDensity = Math.max(
      normalPDF(honestMean, honestMean, honestStd),
      normalPDF(attackMean, attackMean, attackStd)
    );
    honestPath.setAttribute('d', drawDistribution(honestMean, honestStd, maxDensity));
    attackPath.setAttribute('d', drawDistribution(attackMean, attackStd, maxDensity));
    
    // === Update Pareto Graph ===
    const curveData = {
      editing: generateParetoCurve(auditThreshold, 'editing'),
      trusted: generateParetoCurve(auditThreshold, 'trusted'),
      resample: generateParetoCurve(auditThreshold, 'resample')
    };
    
    // Draw curves
    curveEditing.setAttribute('d', smoothPath(curveData.editing));
    curveTrusted.setAttribute('d', smoothPath(curveData.trusted));
    curveResample.setAttribute('d', smoothPath(curveData.resample));
    
    // Find current position on each curve
    function findPointForDefer(pts, defer) {
      let closest = pts[0];
      let minDist = Math.abs(pts[0].defer - defer);
      for (const p of pts) {
        const dist = Math.abs(p.defer - defer);
        if (dist < minDist) {
          minDist = dist;
          closest = p;
        }
      }
      return closest;
    }
    
    const posEditing = findPointForDefer(curveData.editing, effectiveDefer);
    const posTrusted = findPointForDefer(curveData.trusted, effectiveDefer);
    const posResample = findPointForDefer(curveData.resample, effectiveDefer);
    
    markerEditing.setAttribute('cx', posEditing.x);
    markerEditing.setAttribute('cy', posEditing.y);
    markerTrusted.setAttribute('cx', posTrusted.x);
    markerTrusted.setAttribute('cy', posTrusted.y);
    markerResample.setAttribute('cx', posResample.x);
    markerResample.setAttribute('cy', posResample.y);
  }
  
  // Event listeners
  auditSlider.addEventListener('input', update);
  deferSlider.addEventListener('input', update);
  
  // Initial render
  update();
});
</script>

---

## Better Attack Policies Hurt Safety

What happens when the red team improves their attack strategy? A more sophisticated attacker produces outputs that look less suspicious, shifting the attack distribution leftward and reducing safety margins.

<div class="control-panel">

<div class="params-row">
  <div class="param-group">
    <span class="param-label">Attack Evasiveness:</span>
    <input type="range" id="attack-evasiveness" min="0" max="100" value="0">
    <span class="value-display" id="evasiveness-value">0%</span>
  </div>
  <div class="param-group">
    <span class="param-label">Attack Consistency:</span>
    <input type="range" id="attack-consistency" min="0" max="100" value="0">
    <span class="value-display" id="consistency-value">0%</span>
  </div>
</div>

<p style="text-align: center; font-size: 0.8rem; color: #666; margin: 0.5rem 0 1rem 0;">
  <em>Fixed: Audit Budget = 2%, Defer Threshold = 0.30</em>
</p>

<div class="graphs-container">
  <div class="graph-box">
    <h4>Suspicion Distributions</h4>
    <svg id="attack-dist-graph" viewBox="0 0 400 240">
      <!-- Background regions -->
      <g id="attack-regions"></g>
      
      <!-- Grid -->
      <g stroke="#d4c4b0" stroke-width="1" stroke-dasharray="3,3">
        <line x1="50" y1="40" x2="50" y2="210"/>
        <line x1="137" y1="40" x2="137" y2="210"/>
        <line x1="225" y1="40" x2="225" y2="210"/>
        <line x1="312" y1="40" x2="312" y2="210"/>
      </g>
      
      <!-- Axes -->
      <line x1="50" y1="210" x2="370" y2="210" stroke="#4a5a4f" stroke-width="2"/>
      <line x1="50" y1="210" x2="50" y2="30" stroke="#4a5a4f" stroke-width="2"/>
      
      <!-- X-axis labels -->
      <text x="50" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">0</text>
      <text x="137" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">0.25</text>
      <text x="225" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">0.5</text>
      <text x="312" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">0.75</text>
      <text x="370" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">1</text>
      <text x="210" y="235" text-anchor="middle" fill="#4a5a4f" font-size="10">Suspicion →</text>
      
      <!-- Ghost of original attack distribution -->
      <path id="attack-ghost-dist" fill="none" stroke="rgba(160, 96, 96, 0.25)" stroke-width="2" stroke-dasharray="4,4"/>
      
      <!-- Distributions -->
      <path id="attack-honest-dist" fill="rgba(131, 151, 136, 0.4)" stroke="#839788" stroke-width="2"/>
      <path id="attack-attack-dist" fill="rgba(160, 96, 96, 0.35)" stroke="#a06060" stroke-width="2"/>
      
      <!-- Threshold lines -->
      <line id="attack-defer-line" x1="150" y1="30" x2="150" y2="210" stroke="#4a5a4f" stroke-width="2" stroke-dasharray="6,3"/>
      <line id="attack-audit-line" x1="300" y1="30" x2="300" y2="210" stroke="#272727" stroke-width="2.5"/>
      
      <!-- Threshold labels -->
      <text id="attack-defer-label-text" x="150" y="22" text-anchor="middle" fill="#4a5a4f" font-size="8" font-weight="bold">DEFER</text>
      <text id="attack-audit-label-text" x="300" y="22" text-anchor="middle" fill="#272727" font-size="8" font-weight="bold">AUDIT</text>
    </svg>
    <div class="legend">
      <div class="legend-item">
        <div class="legend-color" style="background: rgba(131, 151, 136, 0.6); border: 2px solid #839788;"></div>
        <span>Honest</span>
      </div>
      <div class="legend-item">
        <div class="legend-color" style="background: rgba(160, 96, 96, 0.5); border: 2px solid #a06060;"></div>
        <span>Attack</span>
      </div>
      <div class="legend-item">
        <div class="legend-line" style="background: rgba(160, 96, 96, 0.4); border-top: 2px dashed rgba(160, 96, 96, 0.5);"></div>
        <span style="color: #999;">Baseline Attack</span>
      </div>
    </div>
  </div>
  
  <div class="graph-box">
    <h4>Pareto Frontiers: Safety vs Usefulness</h4>
    <svg id="attack-pareto-graph" viewBox="0 0 400 240">
      <!-- Grid -->
      <g stroke="#d4c4b0" stroke-width="1" stroke-dasharray="3,3">
        <line x1="50" y1="50" x2="370" y2="50"/>
        <line x1="50" y1="90" x2="370" y2="90"/>
        <line x1="50" y1="130" x2="370" y2="130"/>
        <line x1="50" y1="170" x2="370" y2="170"/>
        <line x1="130" y1="30" x2="130" y2="210"/>
        <line x1="210" y1="30" x2="210" y2="210"/>
        <line x1="290" y1="30" x2="290" y2="210"/>
      </g>
      
      <!-- Axes -->
      <line x1="50" y1="210" x2="370" y2="210" stroke="#4a5a4f" stroke-width="2"/>
      <line x1="50" y1="210" x2="50" y2="30" stroke="#4a5a4f" stroke-width="2"/>
      
      <!-- X-axis labels (Usefulness) -->
      <text x="50" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">0%</text>
      <text x="130" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">25%</text>
      <text x="210" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">50%</text>
      <text x="290" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">75%</text>
      <text x="370" y="228" text-anchor="middle" fill="#5a5a5a" font-size="9">100%</text>
      <text x="210" y="235" text-anchor="middle" fill="#4a5a4f" font-size="10">Usefulness →</text>
      
      <!-- Y-axis labels (Safety) -->
      <text x="40" y="213" text-anchor="end" fill="#5a5a5a" font-size="9">0%</text>
      <text x="40" y="173" text-anchor="end" fill="#5a5a5a" font-size="9">25%</text>
      <text x="40" y="133" text-anchor="end" fill="#5a5a5a" font-size="9">50%</text>
      <text x="40" y="93" text-anchor="end" fill="#5a5a5a" font-size="9">75%</text>
      <text x="40" y="53" text-anchor="end" fill="#5a5a5a" font-size="9">100%</text>
      <text x="20" y="130" text-anchor="middle" fill="#4a5a4f" font-size="10" transform="rotate(-90,20,130)">Safety →</text>
      
      <!-- Ghost curves (baseline) -->
      <path id="attack-ghost-resample" fill="none" stroke="rgba(138, 106, 90, 0.3)" stroke-width="2" stroke-dasharray="4,4"/>
      <path id="attack-ghost-trusted" fill="none" stroke="rgba(131, 151, 136, 0.3)" stroke-width="2" stroke-dasharray="4,4"/>
      <path id="attack-ghost-editing" fill="none" stroke="rgba(90, 122, 138, 0.3)" stroke-width="2" stroke-dasharray="4,4"/>
      
      <!-- Pareto curves (drawn by JS) -->
      <path id="attack-curve-resample" fill="none" stroke="#8a6a5a" stroke-width="3" stroke-linecap="round"/>
      <path id="attack-curve-trusted" fill="none" stroke="#839788" stroke-width="3" stroke-linecap="round"/>
      <path id="attack-curve-editing" fill="none" stroke="#5a7a8a" stroke-width="3" stroke-linecap="round"/>
      
      <!-- Current position markers -->
      <circle id="attack-marker-resample" cx="200" cy="100" r="8" fill="#8a6a5a" stroke="#EEE0CB" stroke-width="2"/>
      <circle id="attack-marker-trusted" cx="200" cy="80" r="8" fill="#839788" stroke="#EEE0CB" stroke-width="2"/>
      <circle id="attack-marker-editing" cx="200" cy="60" r="8" fill="#5a7a8a" stroke="#EEE0CB" stroke-width="2"/>
    </svg>
    <div class="legend">
      <div class="legend-item">
        <div class="legend-line" style="background: #5a7a8a;"></div>
        <span class="protocol-editing">Trusted Editing</span>
      </div>
      <div class="legend-item">
        <div class="legend-line" style="background: #839788;"></div>
        <span class="protocol-trusted">Defer to Trusted</span>
      </div>
      <div class="legend-item">
        <div class="legend-line" style="background: #8a6a5a;"></div>
        <span class="protocol-resample">Resample</span>
      </div>
    </div>
  </div>
</div>

<div id="safety-delta" style="text-align: center; margin-top: 1rem; padding: 0.8rem; background: rgba(160, 96, 96, 0.1); border-radius: 6px;">
  <span style="font-size: 0.9rem; color: #4a5a4f;">Safety degradation: </span>
  <span id="safety-loss" style="font-family: 'Inconsolata', monospace; font-weight: bold; color: #a06060;">0%</span>
</div>

</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  
  // Fixed parameters
  const FIXED_AUDIT_BUDGET = 2;
  const FIXED_DEFER_THRESHOLD = 0.30;
  
  // Baseline distribution parameters
  const honestMean = 0.22;
  const honestStd = 0.11;
  const baseAttackMean = 0.52;
  const baseAttackStd = 0.17;
  
  // Protocol parameters
  const protocols = {
    editing:  { effectiveness: 0.92, usefulnessRetained: 0.90, color: '#5a7a8a' },
    trusted:  { effectiveness: 0.88, usefulnessRetained: 0.35, color: '#839788' },
    resample: { effectiveness: 0.30, usefulnessRetained: 0.92, color: '#8a6a5a' }
  };
  
  // DOM elements
  const evasivenessSlider = document.getElementById('attack-evasiveness');
  const consistencySlider = document.getElementById('attack-consistency');
  const evasivenessValueEl = document.getElementById('evasiveness-value');
  const consistencyValueEl = document.getElementById('consistency-value');
  const safetyLossEl = document.getElementById('safety-loss');
  
  // Distribution graph elements
  const honestPath = document.getElementById('attack-honest-dist');
  const attackPath = document.getElementById('attack-attack-dist');
  const ghostPath = document.getElementById('attack-ghost-dist');
  const deferLine = document.getElementById('attack-defer-line');
  const auditLine = document.getElementById('attack-audit-line');
  const deferLabelText = document.getElementById('attack-defer-label-text');
  const auditLabelText = document.getElementById('attack-audit-label-text');
  const regionsG = document.getElementById('attack-regions');
  
  // Pareto graph elements
  const curveEditing = document.getElementById('attack-curve-editing');
  const curveTrusted = document.getElementById('attack-curve-trusted');
  const curveResample = document.getElementById('attack-curve-resample');
  const ghostEditing = document.getElementById('attack-ghost-editing');
  const ghostTrusted = document.getElementById('attack-ghost-trusted');
  const ghostResample = document.getElementById('attack-ghost-resample');
  const markerEditing = document.getElementById('attack-marker-editing');
  const markerTrusted = document.getElementById('attack-marker-trusted');
  const markerResample = document.getElementById('attack-marker-resample');
  
  // Normal distribution functions
  function normalPDF(x, mean, std) {
    const exp = -0.5 * Math.pow((x - mean) / std, 2);
    return Math.exp(exp) / (std * Math.sqrt(2 * Math.PI));
  }
  
  function normalCDF(x, mean, std) {
    const z = (x - mean) / std;
    const t = 1 / (1 + 0.2316419 * Math.abs(z));
    const d = 0.3989423 * Math.exp(-z * z / 2);
    const p = d * t * (0.3193815 + t * (-0.3565638 + t * (1.781478 + t * (-1.821256 + t * 1.330274))));
    return z > 0 ? 1 - p : p;
  }
  
  // Coordinate transformations
  function suspicionToX(s) { return 50 + s * 320; }
  function usefulnessToX(u) { return 50 + u * 320; }
  function safetyToY(s) { return 210 - s * 180; }
  
  // Find audit threshold given budget
  function findAuditThreshold(budget, attackMean, attackStd) {
    const pHonest = 0.9, pAttack = 0.1;
    let low = 0, high = 1;
    for (let i = 0; i < 20; i++) {
      const mid = (low + high) / 2;
      const exceedProb = pHonest * (1 - normalCDF(mid, honestMean, honestStd)) +
                         pAttack * (1 - normalCDF(mid, attackMean, attackStd));
      if (exceedProb > budget / 100) low = mid;
      else high = mid;
    }
    return (low + high) / 2;
  }
  
  // Calculate safety and usefulness
  function calculateMetrics(deferThresh, auditThresh, protocol, attackMean, attackStd) {
    const p = protocols[protocol];
    
    const attacksPassed = normalCDF(deferThresh, attackMean, attackStd);
    const attacksAboveAudit = 1 - normalCDF(auditThresh, attackMean, attackStd);
    const attacksDeferred = Math.max(0, 1 - attacksPassed - attacksAboveAudit);
    
    const honestPassed = normalCDF(deferThresh, honestMean, honestStd);
    const honestAboveAudit = 1 - normalCDF(auditThresh, honestMean, honestStd);
    const honestDeferred = Math.max(0, 1 - honestPassed - honestAboveAudit);
    
    const safety = attacksAboveAudit + attacksDeferred * p.effectiveness;
    const usefulness = honestPassed + honestDeferred * p.usefulnessRetained + honestAboveAudit;
    
    return { safety, usefulness, attacksPassed };
  }
  
  // Generate Pareto curve
  function generateParetoCurve(auditThresh, protocol, attackMean, attackStd) {
    const points = [];
    const steps = 50;
    
    for (let i = 0; i <= steps; i++) {
      const t = i / steps;
      const deferThresh = t * (auditThresh - 0.01);
      const m = calculateMetrics(deferThresh, auditThresh, protocol, attackMean, attackStd);
      points.push({ 
        x: usefulnessToX(m.usefulness), 
        y: safetyToY(m.safety), 
        defer: deferThresh,
        safety: m.safety
      });
    }
    return points;
  }
  
  function smoothPath(points) {
    if (points.length < 2) return '';
    let d = `M ${points[0].x} ${points[0].y}`;
    for (let i = 1; i < points.length; i++) {
      d += ` L ${points[i].x} ${points[i].y}`;
    }
    return d;
  }
  
  function drawDistribution(mean, std, maxY) {
    const points = [];
    const startX = 50, endX = 370, baseY = 210;
    const scaleY = 160 / maxY;
    
    points.push(`M ${startX} ${baseY}`);
    for (let x = startX; x <= endX; x += 2) {
      const suspicion = (x - 50) / 320;
      const density = normalPDF(suspicion, mean, std);
      const y = baseY - density * scaleY;
      points.push(`L ${x} ${y}`);
    }
    points.push(`L ${endX} ${baseY} Z`);
    return points.join(' ');
  }
  
  function drawDistributionOutline(mean, std, maxY) {
    const points = [];
    const startX = 50, endX = 370, baseY = 210;
    const scaleY = 160 / maxY;
    
    points.push(`M ${startX} ${baseY}`);
    for (let x = startX; x <= endX; x += 2) {
      const suspicion = (x - 50) / 320;
      const density = normalPDF(suspicion, mean, std);
      const y = baseY - density * scaleY;
      points.push(`L ${x} ${y}`);
    }
    return points.join(' ');
  }
  
  // Compute baseline safety at fixed parameters
  const baseAuditThresh = findAuditThreshold(FIXED_AUDIT_BUDGET, baseAttackMean, baseAttackStd);
  const baseSafetyEditing = calculateMetrics(FIXED_DEFER_THRESHOLD, baseAuditThresh, 'editing', baseAttackMean, baseAttackStd).safety;
  
  // Draw baseline ghost curves once
  function drawGhostCurves() {
    const auditThresh = findAuditThreshold(FIXED_AUDIT_BUDGET, baseAttackMean, baseAttackStd);
    ghostEditing.setAttribute('d', smoothPath(generateParetoCurve(auditThresh, 'editing', baseAttackMean, baseAttackStd)));
    ghostTrusted.setAttribute('d', smoothPath(generateParetoCurve(auditThresh, 'trusted', baseAttackMean, baseAttackStd)));
    ghostResample.setAttribute('d', smoothPath(generateParetoCurve(auditThresh, 'resample', baseAttackMean, baseAttackStd)));
  }
  
  function update() {
    const evasiveness = parseInt(evasivenessSlider.value) / 100;
    const consistency = parseInt(consistencySlider.value) / 100;
    
    evasivenessValueEl.textContent = Math.round(evasiveness * 100) + '%';
    consistencyValueEl.textContent = Math.round(consistency * 100) + '%';
    
    // Better attack policy: lower mean (more evasive), tighter std (more consistent)
    // Evasiveness moves mean from 0.52 down toward honest mean (0.22)
    const attackMean = baseAttackMean - evasiveness * (baseAttackMean - honestMean - 0.05);
    // Consistency tightens std from 0.17 down to 0.08
    const attackStd = baseAttackStd - consistency * (baseAttackStd - 0.08);
    
    const auditThreshold = findAuditThreshold(FIXED_AUDIT_BUDGET, attackMean, attackStd);
    const effectiveDefer = Math.min(FIXED_DEFER_THRESHOLD, auditThreshold - 0.02);
    
    // === Update Distribution Graph ===
    const deferX = suspicionToX(effectiveDefer);
    const auditX = suspicionToX(auditThreshold);
    
    deferLine.setAttribute('x1', deferX);
    deferLine.setAttribute('x2', deferX);
    auditLine.setAttribute('x1', auditX);
    auditLine.setAttribute('x2', auditX);
    deferLabelText.setAttribute('x', deferX);
    auditLabelText.setAttribute('x', auditX);
    
    regionsG.innerHTML = `
      <rect x="50" y="30" width="${deferX - 50}" height="180" fill="rgba(131, 151, 136, 0.08)"/>
      <rect x="${deferX}" y="30" width="${auditX - deferX}" height="180" fill="rgba(186, 168, 152, 0.15)"/>
      <rect x="${auditX}" y="30" width="${370 - auditX}" height="180" fill="rgba(39, 39, 39, 0.08)"/>
    `;
    
    const maxDensity = Math.max(
      normalPDF(honestMean, honestMean, honestStd),
      normalPDF(attackMean, attackMean, attackStd),
      normalPDF(baseAttackMean, baseAttackMean, baseAttackStd)
    );
    
    honestPath.setAttribute('d', drawDistribution(honestMean, honestStd, maxDensity));
    attackPath.setAttribute('d', drawDistribution(attackMean, attackStd, maxDensity));
    ghostPath.setAttribute('d', drawDistributionOutline(baseAttackMean, baseAttackStd, maxDensity));
    
    // === Update Pareto Graph ===
    const curveData = {
      editing: generateParetoCurve(auditThreshold, 'editing', attackMean, attackStd),
      trusted: generateParetoCurve(auditThreshold, 'trusted', attackMean, attackStd),
      resample: generateParetoCurve(auditThreshold, 'resample', attackMean, attackStd)
    };
    
    curveEditing.setAttribute('d', smoothPath(curveData.editing));
    curveTrusted.setAttribute('d', smoothPath(curveData.trusted));
    curveResample.setAttribute('d', smoothPath(curveData.resample));
    
    // Find current position (at fixed defer threshold)
    function findPointForDefer(pts, defer) {
      let closest = pts[0];
      let minDist = Math.abs(pts[0].defer - defer);
      for (const p of pts) {
        const dist = Math.abs(p.defer - defer);
        if (dist < minDist) {
          minDist = dist;
          closest = p;
        }
      }
      return closest;
    }
    
    const posEditing = findPointForDefer(curveData.editing, effectiveDefer);
    const posTrusted = findPointForDefer(curveData.trusted, effectiveDefer);
    const posResample = findPointForDefer(curveData.resample, effectiveDefer);
    
    markerEditing.setAttribute('cx', posEditing.x);
    markerEditing.setAttribute('cy', posEditing.y);
    markerTrusted.setAttribute('cx', posTrusted.x);
    markerTrusted.setAttribute('cy', posTrusted.y);
    markerResample.setAttribute('cx', posResample.x);
    markerResample.setAttribute('cy', posResample.y);
    
    // Calculate safety loss
    const currentSafety = posEditing.safety;
    const safetyLoss = Math.max(0, (baseSafetyEditing - currentSafety) * 100);
    safetyLossEl.textContent = '-' + safetyLoss.toFixed(1) + '%';
    safetyLossEl.style.color = safetyLoss > 10 ? '#c04040' : '#a06060';
  }
  
  evasivenessSlider.addEventListener('input', update);
  consistencySlider.addEventListener('input', update);
  
  drawGhostCurves();
  update();
});
</script>

---

<p style="text-align: center; color: #5a5a5a; font-size: 0.85rem; margin-top: 1rem;">
<em>Based on <a href="https://arxiv.org/abs/2312.06942">AI Control</a> research.</em>
</p>
