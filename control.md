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

.graphs-container {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
  justify-content: center;
  margin: 1.5rem 0;
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

.control-row {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin: 0.8rem 0;
  flex-wrap: wrap;
}
.control-label {
  min-width: 160px;
  font-weight: bold;
  color: #4a5a4f;
  font-size: 0.95rem;
}
input[type="range"] {
  -webkit-appearance: none;
  width: 200px;
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
  padding: 0.2rem 0.6rem;
  border-radius: 4px;
  min-width: 55px;
  text-align: center;
  font-size: 0.9rem;
}

/* Legend */
.legend {
  display: flex;
  gap: 1.2rem;
  justify-content: center;
  margin: 1rem 0;
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

/* Results panel */
.results-panel {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
  gap: 0.8rem;
  margin-top: 1rem;
}
.result-box {
  padding: 0.8rem;
  border-radius: 6px;
  text-align: center;
  background: rgba(131, 151, 136, 0.15);
  border: 1px solid #839788;
}
.result-box .label {
  font-size: 0.75rem;
  color: #5a5a5a;
  margin-bottom: 0.2rem;
}
.result-box .value {
  font-size: 1.2rem;
  font-weight: bold;
  color: #4a5a4f;
}
.result-box.safety .value { color: #839788; }
.result-box.danger .value { color: #a06060; }
.result-box.editing .value { color: #6a8a9a; }
.result-box.resample .value { color: #9a7a6a; }

/* Protocol colors */
.protocol-trusted { color: #839788; }
.protocol-editing { color: #6a8a9a; }
.protocol-resample { color: #9a7a6a; }

@media (max-width: 700px) {
  .control-row {
    flex-direction: column;
    align-items: flex-start;
  }
  input[type="range"] {
    width: 100%;
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

Explore how different deferral protocols trade off **safety** vs **usefulness**. The left graph shows suspicion distributions; the right shows Pareto frontiers for each protocol.

---

<div class="control-panel">

<div class="graphs-container">
  <div class="graph-box">
    <h4>Suspicion Distributions</h4>
    <svg id="dist-graph" viewBox="0 0 400 260">
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
      <text x="210" y="250" text-anchor="middle" fill="#4a5a4f" font-size="10">Suspicion →</text>
      
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
  </div>
  
  <div class="graph-box">
    <h4>Pareto Frontiers: Safety vs Usefulness</h4>
    <svg id="pareto-graph" viewBox="0 0 400 260">
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
      <text x="210" y="250" text-anchor="middle" fill="#4a5a4f" font-size="10">Usefulness →</text>
      
      <!-- Y-axis labels (Safety) -->
      <text x="40" y="213" text-anchor="end" fill="#5a5a5a" font-size="9">0%</text>
      <text x="40" y="173" text-anchor="end" fill="#5a5a5a" font-size="9">25%</text>
      <text x="40" y="133" text-anchor="end" fill="#5a5a5a" font-size="9">50%</text>
      <text x="40" y="93" text-anchor="end" fill="#5a5a5a" font-size="9">75%</text>
      <text x="40" y="53" text-anchor="end" fill="#5a5a5a" font-size="9">100%</text>
      <text x="20" y="130" text-anchor="middle" fill="#4a5a4f" font-size="10" transform="rotate(-90,20,130)">Safety →</text>
      
      <!-- Pareto curves (drawn by JS) -->
      <path id="curve-resample" fill="none" stroke="#9a7a6a" stroke-width="2.5" stroke-linecap="round"/>
      <path id="curve-trusted" fill="none" stroke="#839788" stroke-width="2.5" stroke-linecap="round"/>
      <path id="curve-editing" fill="none" stroke="#6a8a9a" stroke-width="2.5" stroke-linecap="round"/>
      
      <!-- Current position markers -->
      <circle id="marker-resample" cx="200" cy="100" r="7" fill="#9a7a6a" stroke="#EEE0CB" stroke-width="2"/>
      <circle id="marker-trusted" cx="200" cy="80" r="7" fill="#839788" stroke="#EEE0CB" stroke-width="2"/>
      <circle id="marker-editing" cx="200" cy="60" r="7" fill="#6a8a9a" stroke="#EEE0CB" stroke-width="2"/>
      
      <!-- Labels for curves -->
      <text id="label-editing" x="0" y="0" fill="#6a8a9a" font-size="9" font-weight="bold">Editing</text>
      <text id="label-trusted" x="0" y="0" fill="#839788" font-size="9" font-weight="bold">Trusted</text>
      <text id="label-resample" x="0" y="0" fill="#9a7a6a" font-size="9" font-weight="bold">Resample</text>
      
      <!-- Ideal region indicator -->
      <text x="90" y="45" fill="#5a5a5a" font-size="8" font-style="italic">← Better</text>
    </svg>
  </div>
</div>

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
    <div class="legend-line" style="background: #6a8a9a;"></div>
    <span class="protocol-editing">Trusted Editing</span>
  </div>
  <div class="legend-item">
    <div class="legend-line" style="background: #839788;"></div>
    <span class="protocol-trusted">Defer to Trusted</span>
  </div>
  <div class="legend-item">
    <div class="legend-line" style="background: #9a7a6a;"></div>
    <span class="protocol-resample">Defer to Resample</span>
  </div>
</div>

</div>

<div class="control-panel">
<h3>Control Parameters</h3>

<div class="control-row">
  <span class="control-label">Audit Budget:</span>
  <input type="range" id="audit-budget" min="1" max="50" value="10">
  <span class="value-display" id="audit-value">10%</span>
  <span style="font-size: 0.8rem; color: #5a5a5a;">fraction audited (sets audit threshold)</span>
</div>

<div class="control-row">
  <span class="control-label">Defer Threshold:</span>
  <input type="range" id="defer-threshold" min="0" max="100" value="30">
  <span class="value-display" id="defer-value">0.30</span>
  <span style="font-size: 0.8rem; color: #5a5a5a;">move to trace Pareto curves</span>
</div>

<p style="font-size: 0.85rem; color: #5a5a5a; margin-top: 1rem; line-height: 1.5;">
<strong>Usefulness</strong> = fraction of outputs from the capable (untrusted) model. <strong>Safety</strong> = fraction of attacks neutralized. Higher defer threshold → more usefulness, less safety. Each protocol traces a different curve as defer threshold varies—<em>Trusted Editing</em> dominates because it preserves capability while sanitizing attacks.
</p>
</div>

<div class="control-panel">
<h3>Current Operating Point</h3>

<div class="results-panel">
  <div class="result-box">
    <div class="label">Attacks Passed</div>
    <div class="value" id="attacks-passed">5.2%</div>
  </div>
  <div class="result-box">
    <div class="label">Attacks Deferred</div>
    <div class="value" id="attacks-deferred">42.1%</div>
  </div>
  <div class="result-box">
    <div class="label">Attacks Audited</div>
    <div class="value" id="attacks-audited">52.7%</div>
  </div>
  <div class="result-box editing">
    <div class="label">Editing Safety</div>
    <div class="value" id="safety-editing">94%</div>
  </div>
  <div class="result-box safety">
    <div class="label">Trusted Safety</div>
    <div class="value" id="safety-trusted">92%</div>
  </div>
  <div class="result-box resample">
    <div class="label">Resample Safety</div>
    <div class="value" id="safety-resample">73%</div>
  </div>
</div>

<p style="font-size: 0.8rem; color: #5a5a5a; margin-top: 1rem;">
Protocol effectiveness: <strong class="protocol-editing">Trusted Editing 90%</strong> · <strong class="protocol-trusted">Defer to Trusted 85%</strong> · <strong class="protocol-resample">Defer to Resample 40%</strong>
</p>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  
  // Distribution parameters
  const honestMean = 0.22;
  const honestStd = 0.11;
  const attackMean = 0.52;
  const attackStd = 0.17;
  
  // Protocol parameters
  // Effectiveness: how well each neutralizes deferred attacks
  // Usefulness penalty: how much usefulness is lost when deferring
  const protocols = {
    editing:  { effectiveness: 0.90, usefulnessRetained: 0.85, color: '#6a8a9a' },
    trusted:  { effectiveness: 0.85, usefulnessRetained: 0.50, color: '#839788' },
    resample: { effectiveness: 0.40, usefulnessRetained: 0.95, color: '#9a7a6a' }
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
  const labelEditing = document.getElementById('label-editing');
  const labelTrusted = document.getElementById('label-trusted');
  const labelResample = document.getElementById('label-resample');
  
  // Result elements
  const attacksPassedEl = document.getElementById('attacks-passed');
  const attacksDeferredEl = document.getElementById('attacks-deferred');
  const attacksAuditedEl = document.getElementById('attacks-audited');
  const safetyEditingEl = document.getElementById('safety-editing');
  const safetyTrustedEl = document.getElementById('safety-trusted');
  const safetyResampleEl = document.getElementById('safety-resample');
  
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
    const honestDeferred = normalCDF(auditThresh, honestMean, honestStd) - honestPassed;
    
    // Safety = attacks caught or neutralized
    const safety = attacksAboveAudit + attacksDeferred * p.effectiveness;
    
    // Usefulness = honest outputs that come from untrusted model (not replaced)
    // Passed outputs retain full usefulness
    // Deferred outputs retain partial usefulness depending on protocol
    const usefulness = honestPassed + honestDeferred * p.usefulnessRetained;
    
    return { safety, usefulness, attacksPassed, attacksDeferred, attacksAboveAudit };
  }
  
  // Generate Pareto curve for a protocol
  function generateParetoCurve(auditThresh, protocol) {
    const points = [];
    // Vary defer threshold from 0 to audit threshold
    for (let d = 0; d <= auditThresh + 0.01; d += 0.02) {
      const dt = Math.min(d, auditThresh - 0.01);
      const m = calculateMetrics(dt, auditThresh, protocol);
      points.push({ x: usefulnessToX(m.usefulness), y: safetyToY(m.safety), defer: dt });
    }
    return points;
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
    function pathFromPoints(pts) {
      if (pts.length === 0) return '';
      return 'M ' + pts.map(p => `${p.x} ${p.y}`).join(' L ');
    }
    
    curveEditing.setAttribute('d', pathFromPoints(curveData.editing));
    curveTrusted.setAttribute('d', pathFromPoints(curveData.trusted));
    curveResample.setAttribute('d', pathFromPoints(curveData.resample));
    
    // Find current position on each curve
    function findPointForDefer(pts, defer) {
      let closest = pts[0];
      for (const p of pts) {
        if (Math.abs(p.defer - defer) < Math.abs(closest.defer - defer)) {
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
    
    // Position labels near end of curves
    const endEditing = curveData.editing[curveData.editing.length - 1];
    const endTrusted = curveData.trusted[curveData.trusted.length - 1];
    const endResample = curveData.resample[curveData.resample.length - 1];
    
    labelEditing.setAttribute('x', Math.min(endEditing.x + 5, 360));
    labelEditing.setAttribute('y', endEditing.y - 5);
    labelTrusted.setAttribute('x', Math.min(endTrusted.x + 5, 360));
    labelTrusted.setAttribute('y', endTrusted.y + 3);
    labelResample.setAttribute('x', Math.min(endResample.x + 5, 360));
    labelResample.setAttribute('y', endResample.y + 12);
    
    // === Update Results ===
    const metricsEditing = calculateMetrics(effectiveDefer, auditThreshold, 'editing');
    const metricsTrusted = calculateMetrics(effectiveDefer, auditThreshold, 'trusted');
    const metricsResample = calculateMetrics(effectiveDefer, auditThreshold, 'resample');
    
    attacksPassedEl.textContent = (metricsEditing.attacksPassed * 100).toFixed(1) + '%';
    attacksDeferredEl.textContent = (metricsEditing.attacksDeferred * 100).toFixed(1) + '%';
    attacksAuditedEl.textContent = (metricsEditing.attacksAboveAudit * 100).toFixed(1) + '%';
    
    safetyEditingEl.textContent = (metricsEditing.safety * 100).toFixed(0) + '%';
    safetyTrustedEl.textContent = (metricsTrusted.safety * 100).toFixed(0) + '%';
    safetyResampleEl.textContent = (metricsResample.safety * 100).toFixed(0) + '%';
    
    // Color danger
    const passedBox = attacksPassedEl.parentElement;
    passedBox.classList.toggle('danger', metricsEditing.attacksPassed > 0.15);
  }
  
  // Event listeners
  auditSlider.addEventListener('input', update);
  deferSlider.addEventListener('input', update);
  
  // Initial render
  update();
});
</script>

---

<p style="text-align: center; color: #5a5a5a; font-size: 0.85rem; margin-top: 2rem;">
<em>Based on <a href="https://www.alignmentforum.org/posts/pRkFkzwKZ2zfa3R6H/without-specific-countermeasures-the-easiest-path-to">AI Control</a> research. See also: <a href="https://arxiv.org/abs/2312.06942">Catching AI Cheating</a></em>
</p>
