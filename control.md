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
After years of development, Anthropode released Cladius 6-7. The code-writing perfomance is substantially better than existing models, across the board. Early adopters are able to write code faster than the best engineers, spot bugs before they ship, and automate the tedious parts of software development. Thanks to this, they can relax in their homes, and get to finally write code they *actually* like!

Cladius' fanbase grows at an astonishing pace, and leaderboard after leaderboard becomes obsolete. The few living Turing-test skeptics find more absurd arguments to hold their position. The team at the AI-enabled software development IDE, Curser, soon makes it available on their platform. Many have it as their default.

<blockquote class="twitter-tweet" data-theme="light"><p lang="en" dir="ltr">me: can you center this div<br>the unreleased Chinese base model I accidentally picked on openrouter: <a href="https://t.co/n77tXfs16X">pic.twitter.com/n77tXfs16X</a></p>&mdash; Hero Thousandfaces (@1thousandfaces_) <a href="https://twitter.com/1thousandfaces_/status/1978519212537552901?ref_src=twsrc%5Etfw">October 15, 2025</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

But some people can't sleep at night. Surgeries fail, planes fall, innocent people go to jail. Imperfect safety has rarely stopped us. But AI is different, because it can *suddenly stop* being safe. How do you guarantee something will continue being as safe as you expected it to be, even during deployment? More importantly:

**How do you use an AI system you can't fully trust?**

This is the core problem of **AI Control**. And surprisingly, we can reason about it with mathematical precision. What follows is a journey through the key ideas: what "safety" even means, the protocols that trade off safety against usefulness, and why the quality of your monitoring system determines *almost* everything.

---

## What Does Safety Even Mean?

Even before deployment, you need to settle for some measure of safety. If an AI tries to attack you, how do you quantify "safe enough"?

Here's the setup: Your AI system runs for many rounds, thousands or millions of lines of code per day. A trusted monitor watches each output and flags anything suspicious. If it catches an attack, the system shuts down. But some attacks *will* slip through undetected.

**What's the probability that *nothing* slips through?**

We need two numbers to reason about this:

- ***c*** ∈ [0,1] — the probability your monitor catches an attack
- ***p*** ∈ [0,1] — the probability the AI attacks on any given round

In a single round, there are three basic outcomes:

| Outcome | Probability | Result |
|---------|-------------|--------|
| No attack | 1 − *p* | System continues |
| Attack caught | *p* · *c* | System shuts down |
| Attack succeeds | *p* · (1−*c*) | Undetected — continues |

You might wonder what happened with the fourth outcome: no attack. However, rounds without attacks are safe by definition. We only care about what happens *when the AI attacks*. And each attack is essentially an independent coin flip:

- Attack 1: caught with prob *c*, slips through with prob (1−*c*)
- Attack 2: caught with prob *c*, slips through with prob (1−*c*)
- ... and so on, until some attack gets caught (shutdown)

We are assuming we shutdown the AI as soon as we catch it red handed. So let's count: how many attacks slip through before we catch one?

Call this *K*, the number of successful (undetected) attacks before shutdown.

- *K* = 0 means the first attack is caught. **P(*K* = 0) = *c***
- *K* = 1 means one slips through, then we catch the second. **P(*K* = 1) = (1−*c*) · *c***
- *K* = 2 means two slip through, third caught. **P(*K* = 2) = (1−*c*)² · *c***

The pattern: **P(*K* = *k*) = (1−*c*)<sup>*k*</sup> · *c*** people into probability will recognize this is the **Geometric(*c*)** distribution.

**Safety** = probability of zero undetected attacks = P(*K* = 0) = ***c***

In our 2-parameter statistical model, we can model safety as **E[*K*] = (1−*c*) / *c***.

An interesting result of our mathematical simplification is the lack of *p* to calculate safety. This is because *p* only affects *how many rounds* pass between attacks, not what happens when attacks occur. In most cases, **Safety depends entirely on your monitor quality.**

This tells us what to optimize: not just "make the AI less likely to attack" but "make damn sure you catch attacks when they happen."

<div class="control-panel">
<h4 style="text-align: center; margin-top: 0;">Geometric Distribution of Undetected Attacks</h4>

<div class="params-row">
  <div class="param-group">
    <span class="param-label">Catch Rate <em>c</em>:</span>
    <input type="range" id="safety-catch-rate" min="1" max="99" value="80">
    <span class="value-display" id="safety-catch-value">0.80</span>
  </div>
</div>

<svg id="geometric-graph" viewBox="0 0 500 240" style="width: 100%; max-width: 500px; display: block; margin: 0 auto;">
  <!-- Grid -->
  <g stroke="#d4c4b0" stroke-width="1" stroke-dasharray="3,3">
    <line x1="60" y1="40" x2="480" y2="40"/>
    <line x1="60" y1="90" x2="480" y2="90"/>
    <line x1="60" y1="140" x2="480" y2="140"/>
  </g>
  
  <!-- Axes -->
  <line x1="60" y1="190" x2="480" y2="190" stroke="#4a5a4f" stroke-width="2"/>
  <line x1="60" y1="190" x2="60" y2="20" stroke="#4a5a4f" stroke-width="2"/>
  
  <!-- X-axis labels -->
  <text x="90" y="208" text-anchor="middle" fill="#5a5a5a" font-size="11">0</text>
  <text x="150" y="208" text-anchor="middle" fill="#5a5a5a" font-size="11">1</text>
  <text x="210" y="208" text-anchor="middle" fill="#5a5a5a" font-size="11">2</text>
  <text x="270" y="208" text-anchor="middle" fill="#5a5a5a" font-size="11">3</text>
  <text x="330" y="208" text-anchor="middle" fill="#5a5a5a" font-size="11">4</text>
  <text x="390" y="208" text-anchor="middle" fill="#5a5a5a" font-size="11">5</text>
  <text x="450" y="208" text-anchor="middle" fill="#5a5a5a" font-size="11">6+</text>
  <text x="270" y="232" text-anchor="middle" fill="#4a5a4f" font-size="12">K = undetected attacks before shutdown</text>
  
  <!-- Y-axis labels -->
  <text x="52" y="193" text-anchor="end" fill="#5a5a5a" font-size="10">0</text>
  <text x="52" y="93" text-anchor="end" fill="#5a5a5a" font-size="10">0.5</text>
  <text x="52" y="43" text-anchor="end" fill="#5a5a5a" font-size="10">1.0</text>
  <text x="25" y="115" text-anchor="middle" fill="#4a5a4f" font-size="11" transform="rotate(-90,25,115)">P(K = k)</text>
  
  <!-- Bars -->
  <g id="prob-bars"></g>
</svg>

<div style="display: flex; gap: 2rem; justify-content: center; flex-wrap: wrap; margin-top: 1rem;">
  <div style="text-align: center;">
    <div style="font-size: 0.8rem; color: #666;">Safety = P(K=0)</div>
    <div style="font-family: 'Inconsolata', monospace; font-size: 1.8rem; font-weight: bold; color: #839788;" id="safety-result">80%</div>
  </div>
  <div style="text-align: center;">
    <div style="font-size: 0.8rem; color: #666;">E[K] = expected undetected</div>
    <div style="font-family: 'Inconsolata', monospace; font-size: 1.8rem; font-weight: bold; color: #a06060;" id="expected-uncaught">0.25</div>
  </div>
</div>

</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const catchSlider = document.getElementById('safety-catch-rate');
  const catchValueEl = document.getElementById('safety-catch-value');
  const probBarsG = document.getElementById('prob-bars');
  const safetyResultEl = document.getElementById('safety-result');
  const expectedUncaughtEl = document.getElementById('expected-uncaught');
  
  function geometricPMF(k, c) {
    return Math.pow(1 - c, k) * c;
  }
  
  function update() {
    const c = parseInt(catchSlider.value) / 100;
    catchValueEl.textContent = c.toFixed(2);
    
    safetyResultEl.textContent = (c * 100).toFixed(0) + '%';
    expectedUncaughtEl.textContent = ((1 - c) / c).toFixed(2);
    
    const barWidth = 40;
    const barGap = 20;
    const barStartX = 70;
    const barBaseY = 190;
    const maxBarHeight = 150;
    
    let barsHTML = '';
    for (let k = 0; k <= 6; k++) {
      const prob = k < 6 ? geometricPMF(k, c) : Math.pow(1-c, 6);
      const barHeight = prob * maxBarHeight;
      const x = barStartX + k * (barWidth + barGap);
      const y = barBaseY - barHeight;
      
      const color = k === 0 ? '#839788' : '#a06060';
      const opacity = k === 0 ? 1 : Math.max(0.3, 0.8 - k * 0.1);
      
      barsHTML += `<rect x="${x}" y="${y}" width="${barWidth}" height="${barHeight}" fill="${color}" opacity="${opacity}" rx="3"/>`;
      if (prob > 0.01) {
        barsHTML += `<text x="${x + barWidth/2}" y="${y - 5}" text-anchor="middle" fill="#4a5a4f" font-size="10" font-weight="bold">${(prob * 100).toFixed(0)}%</text>`;
      }
    }
    probBarsG.innerHTML = barsHTML;
  }
  
  catchSlider.addEventListener('input', update);
  update();
});
</script>

---

## The cost of safety

But if it all came down to catching every attack, why not use your trusted model all the time? Another key assumption of control is that the trusted model is less capable than the untrusted one. We want to use Cladius 6-7 because it's "better" but "unsafer" than its predecesor. Whenever the trusted monitor flags something, it needs to be handled, and even if you don't throw it out entirely, you will produce "worse" outputs.

So we can't have it all. We face a fundamental tradeoff between safety and usefulness. The interesting question becomes: which protocol gives us the best rate?

Different strategies handle suspicious outputs differently. There are many specialized ones in the literature, depending on the specific task at hand (not everything is code generation), but a few recurring ones are variations on:
- **Trusted Editing**: A trusted (but less capable) model reviews and edits flagged outputs
- **Defer to Trusted**: Route flagged tasks entirely to the trusted model
- **Resample**: Just ask the untrusted model to try again

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
  
  // Find audit threshold given budget (based on honest distribution only)
  function findAuditThreshold(budget) {
    // Audit budget = top X% of honest distribution gets audited
    let low = 0, high = 1;
    for (let i = 0; i < 20; i++) {
      const mid = (low + high) / 2;
      const honestExceedProb = 1 - normalCDF(mid, honestMean, honestStd);
      if (honestExceedProb > budget / 100) low = mid;
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

## Enter the Red Team

Here's the uncomfortable part: everything above assumed a naive attacker.

We modeled the attack distribution as fixed — but what if the AI is smart? What if it *learns* to evade your monitor? A sophisticated attacker produces outputs that look less suspicious, intentionally blending in with honest work. The attack distribution shifts left toward the honest one. Your carefully tuned thresholds? Now they're cutting through the *wrong* part of the overlap.

This is why AI control is fundamentally a *game*, not a one-time engineering problem. The red team (simulating a misaligned AI) tries to find attack strategies that slip past blue team defenses. The blue team hardens their protocols. Rinse, repeat.

Let's see what happens as the attacker gets better. Crank up "evasiveness" (attacks that look more innocent) and "consistency" (less variance in attack behavior) and watch your safety margins erode:

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
  
  // Find audit threshold given budget (based on honest distribution only)
  // This is FIXED regardless of attack distribution changes
  function findAuditThreshold(budget) {
    let low = 0, high = 1;
    for (let i = 0; i < 20; i++) {
      const mid = (low + high) / 2;
      const honestExceedProb = 1 - normalCDF(mid, honestMean, honestStd);
      if (honestExceedProb > budget / 100) low = mid;
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
  // Audit threshold is FIXED based on honest distribution only
  const baseAuditThresh = findAuditThreshold(FIXED_AUDIT_BUDGET);
  const baseSafetyEditing = calculateMetrics(FIXED_DEFER_THRESHOLD, baseAuditThresh, 'editing', baseAttackMean, baseAttackStd).safety;
  
  // Draw baseline ghost curves once
  function drawGhostCurves() {
    const auditThresh = baseAuditThresh; // Fixed, doesn't change with attack policy
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
    
    // Audit threshold is FIXED (based on honest distribution only)
    const auditThreshold = baseAuditThresh;
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

## The Monitor's Dial

So everything hinges on the monitor. How do we make it better? And once we have one, how do we tune it?

This is where the **ROC curve** comes in — the classic tool for reasoning about classifiers. It shows you the tradeoff between catching real attacks (true positive rate) and falsely flagging honest work (false positive rate). Every point on the curve is a different threshold you could set.

Think of it as a dial. Turn it one way: catch more attacks, but trigger more false alarms. Turn it the other way: fewer interruptions, but more attacks slip through. You can't escape the tradeoff — but a *better monitor* (higher AUC) gives you a better frontier to choose from.

Drag the point along the curve to see how your choice of threshold affects both detection and the resulting safety-usefulness tradeoff:

<style>
.roc-container {
  display: flex;
  gap: 1.5rem;
  flex-wrap: wrap;
  justify-content: center;
  align-items: flex-start;
}
.roc-graph, .roc-pareto {
  flex: 1;
  min-width: 300px;
  max-width: 400px;
}
.roc-graph h4, .roc-pareto h4 {
  text-align: center;
  color: #4a5a4f;
  margin: 0 0 0.5rem 0;
  font-size: 0.95rem;
}
.monitor-slider-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}
.monitor-slider-row {
  display: flex;
  align-items: center;
  gap: 1rem;
  width: 100%;
  max-width: 400px;
}
.monitor-slider-label {
  font-size: 0.8rem;
  min-width: 50px;
  text-align: center;
  color: #4a5a4f;
  font-weight: bold;
}
.monitor-auc-display {
  font-family: "Inconsolata", monospace;
  font-size: 1.1rem;
  font-weight: bold;
  padding: 0.3rem 0.8rem;
  border-radius: 6px;
  background: #4a5a4f;
  color: #EEE0CB;
}
.roc-stats {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 0.5rem;
  margin-top: 1rem;
}
.roc-stat {
  text-align: center;
  padding: 0.6rem;
  border-radius: 6px;
  background: rgba(131, 151, 136, 0.1);
}
.roc-stat .label {
  font-size: 0.7rem;
  color: #666;
  text-transform: uppercase;
}
.roc-stat .value {
  font-family: "Inconsolata", monospace;
  font-size: 1.1rem;
  font-weight: bold;
  color: #4a5a4f;
}
.drag-hint {
  text-align: center;
  font-size: 0.75rem;
  color: #888;
  margin-top: 0.5rem;
}
</style>

<div class="control-panel">

<div class="monitor-slider-container">
  <div class="monitor-slider-row">
    <span class="monitor-slider-label">Weak</span>
    <input type="range" id="monitor-quality-slider" min="50" max="99" value="85">
    <span class="monitor-slider-label">Strong</span>
  </div>
  <div class="monitor-auc-display" id="auc-display">AUC: 0.85</div>
</div>

<div class="roc-container">
  <div class="roc-graph">
    <h4>ROC Curve (drag the point)</h4>
    <svg id="roc-svg" viewBox="0 0 300 300" style="width:100%; touch-action:none;">
      <!-- Grid -->
      <g stroke="#d4c4b0" stroke-width="1" stroke-dasharray="3,3">
        <line x1="50" y1="50" x2="270" y2="50"/>
        <line x1="50" y1="105" x2="270" y2="105"/>
        <line x1="50" y1="160" x2="270" y2="160"/>
        <line x1="50" y1="215" x2="270" y2="215"/>
        <line x1="105" y1="50" x2="105" y2="270"/>
        <line x1="160" y1="50" x2="160" y2="270"/>
        <line x1="215" y1="50" x2="215" y2="270"/>
      </g>
      
      <!-- Diagonal (random classifier) -->
      <line x1="50" y1="270" x2="270" y2="50" stroke="#ccc" stroke-width="1" stroke-dasharray="5,5"/>
      
      <!-- Axes -->
      <line x1="50" y1="270" x2="270" y2="270" stroke="#4a5a4f" stroke-width="2"/>
      <line x1="50" y1="270" x2="50" y2="50" stroke="#4a5a4f" stroke-width="2"/>
      
      <!-- Labels -->
      <text x="160" y="295" text-anchor="middle" fill="#4a5a4f" font-size="11">False Positive Rate →</text>
      <text x="15" y="160" text-anchor="middle" fill="#4a5a4f" font-size="11" transform="rotate(-90,15,160)">True Positive Rate →</text>
      
      <text x="50" y="285" text-anchor="middle" fill="#888" font-size="9">0</text>
      <text x="160" y="285" text-anchor="middle" fill="#888" font-size="9">0.5</text>
      <text x="270" y="285" text-anchor="middle" fill="#888" font-size="9">1</text>
      <text x="40" y="273" text-anchor="end" fill="#888" font-size="9">0</text>
      <text x="40" y="163" text-anchor="end" fill="#888" font-size="9">0.5</text>
      <text x="40" y="53" text-anchor="end" fill="#888" font-size="9">1</text>
      
      <!-- ROC curve -->
      <path id="roc-curve" fill="none" stroke="#839788" stroke-width="3" stroke-linecap="round"/>
      
      <!-- Draggable point -->
      <circle id="roc-point" cx="100" cy="100" r="10" fill="#839788" stroke="#EEE0CB" stroke-width="2" style="cursor:grab;"/>
      
      <!-- Point label -->
      <text id="roc-point-label" x="100" y="85" text-anchor="middle" fill="#4a5a4f" font-size="9" font-weight="bold"></text>
    </svg>
    <div class="drag-hint">↔ Drag point to adjust threshold</div>
  </div>
  
  <div class="roc-pareto">
    <h4>Resulting Safety-Usefulness Tradeoff</h4>
    <svg id="roc-pareto-svg" viewBox="0 0 300 300" style="width:100%;">
      <!-- Grid -->
      <g stroke="#d4c4b0" stroke-width="1" stroke-dasharray="3,3">
        <line x1="50" y1="50" x2="270" y2="50"/>
        <line x1="50" y1="105" x2="270" y2="105"/>
        <line x1="50" y1="160" x2="270" y2="160"/>
        <line x1="50" y1="215" x2="270" y2="215"/>
        <line x1="105" y1="50" x2="105" y2="270"/>
        <line x1="160" y1="50" x2="160" y2="270"/>
        <line x1="215" y1="50" x2="215" y2="270"/>
      </g>
      
      <!-- Axes -->
      <line x1="50" y1="270" x2="270" y2="270" stroke="#4a5a4f" stroke-width="2"/>
      <line x1="50" y1="270" x2="50" y2="50" stroke="#4a5a4f" stroke-width="2"/>
      
      <!-- Labels -->
      <text x="160" y="295" text-anchor="middle" fill="#4a5a4f" font-size="11">Usefulness →</text>
      <text x="15" y="160" text-anchor="middle" fill="#4a5a4f" font-size="11" transform="rotate(-90,15,160)">Safety →</text>
      
      <text x="50" y="285" text-anchor="middle" fill="#888" font-size="9">0%</text>
      <text x="160" y="285" text-anchor="middle" fill="#888" font-size="9">50%</text>
      <text x="270" y="285" text-anchor="middle" fill="#888" font-size="9">100%</text>
      <text x="40" y="273" text-anchor="end" fill="#888" font-size="9">0%</text>
      <text x="40" y="163" text-anchor="end" fill="#888" font-size="9">50%</text>
      <text x="40" y="53" text-anchor="end" fill="#888" font-size="9">100%</text>
      
      <!-- Pareto curve -->
      <path id="pareto-curve" fill="none" stroke="#839788" stroke-width="3" stroke-linecap="round"/>
      
      <!-- Current operating point -->
      <circle id="pareto-point" cx="200" cy="100" r="8" fill="#839788" stroke="#EEE0CB" stroke-width="2"/>
    </svg>
    
    <div class="roc-stats">
      <div class="roc-stat">
        <div class="label">True Positive</div>
        <div class="value" id="stat-tpr">75%</div>
      </div>
      <div class="roc-stat">
        <div class="label">False Positive</div>
        <div class="value" id="stat-fpr">10%</div>
      </div>
      <div class="roc-stat">
        <div class="label">Safety</div>
        <div class="value" id="stat-safety">82%</div>
      </div>
      <div class="roc-stat">
        <div class="label">Usefulness</div>
        <div class="value" id="stat-useful">91%</div>
      </div>
    </div>
  </div>
</div>

</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const rocSvg = document.getElementById('roc-svg');
  const rocCurve = document.getElementById('roc-curve');
  const rocPoint = document.getElementById('roc-point');
  const rocPointLabel = document.getElementById('roc-point-label');
  const paretoCurve = document.getElementById('pareto-curve');
  const paretoPoint = document.getElementById('pareto-point');
  const monitorSlider = document.getElementById('monitor-quality-slider');
  const aucDisplay = document.getElementById('auc-display');
  
  let currentAUC = 0.85;
  let currentThreshold = 0.5;
  let isDragging = false;
  
  // ROC curve parameterized by AUC
  // Higher AUC = curve bows more toward top-left = higher TPR for same FPR
  function rocTPR(fpr, auc) {
    const c = 1 / (2 * (1 - auc) + 0.02);
    return Math.pow(fpr, 1/c);
  }
  
  function fprToX(fpr) { return 50 + fpr * 220; }
  function tprToY(tpr) { return 270 - tpr * 220; }
  function xToFpr(x) { return Math.max(0, Math.min(1, (x - 50) / 220)); }
  
  function drawROC() {
    let d = `M ${fprToX(0)} ${tprToY(rocTPR(0, currentAUC))}`;
    for (let fpr = 0.02; fpr <= 1; fpr += 0.02) {
      const tpr = rocTPR(fpr, currentAUC);
      d += ` L ${fprToX(fpr)} ${tprToY(tpr)}`;
    }
    rocCurve.setAttribute('d', d);
  }
  
  function drawParetoCurve() {
    let d = '';
    let first = true;
    for (let t = 0; t <= 1; t += 0.02) {
      const f = 1 - t;
      const tp = rocTPR(f, currentAUC);
      const s = tp * 0.9 + 0.1 * (1 - f);
      const u = (1 - f) * 0.85 + 0.15;
      const x = 50 + u * 220;
      const y = 270 - s * 220;
      d += first ? `M ${x} ${y}` : ` L ${x} ${y}`;
      first = false;
    }
    paretoCurve.setAttribute('d', d);
  }
  
  function updateFromThreshold() {
    const fpr = 1 - currentThreshold;
    const tpr = rocTPR(fpr, currentAUC);
    
    const x = fprToX(fpr);
    const y = tprToY(tpr);
    
    rocPoint.setAttribute('cx', x);
    rocPoint.setAttribute('cy', y);
    rocPointLabel.setAttribute('x', x);
    rocPointLabel.setAttribute('y', y - 18);
    rocPointLabel.textContent = `τ=${currentThreshold.toFixed(2)}`;
    
    // Update stats
    document.getElementById('stat-tpr').textContent = (tpr * 100).toFixed(0) + '%';
    document.getElementById('stat-fpr').textContent = (fpr * 100).toFixed(0) + '%';
    
    const safety = tpr * 0.9 + 0.1 * (1 - fpr);
    const usefulness = (1 - fpr) * 0.85 + 0.15;
    
    document.getElementById('stat-safety').textContent = (safety * 100).toFixed(0) + '%';
    document.getElementById('stat-useful').textContent = (usefulness * 100).toFixed(0) + '%';
    
    // Update pareto point position
    const px = 50 + usefulness * 220;
    const py = 270 - safety * 220;
    paretoPoint.setAttribute('cx', px);
    paretoPoint.setAttribute('cy', py);
  }
  
  function updateAUCDisplay() {
    aucDisplay.textContent = `AUC: ${currentAUC.toFixed(2)}`;
  }
  
  // Drag handling
  function startDrag(e) {
    isDragging = true;
    rocPoint.style.cursor = 'grabbing';
    e.preventDefault();
  }
  
  function doDrag(e) {
    if (!isDragging) return;
    
    const rect = rocSvg.getBoundingClientRect();
    const scaleX = 300 / rect.width;
    const clientX = e.touches ? e.touches[0].clientX : e.clientX;
    const x = (clientX - rect.left) * scaleX;
    
    const fpr = xToFpr(x);
    currentThreshold = 1 - fpr;
    currentThreshold = Math.max(0.05, Math.min(0.95, currentThreshold));
    updateFromThreshold();
  }
  
  function endDrag() {
    isDragging = false;
    rocPoint.style.cursor = 'grab';
  }
  
  rocPoint.addEventListener('mousedown', startDrag);
  rocPoint.addEventListener('touchstart', startDrag);
  document.addEventListener('mousemove', doDrag);
  document.addEventListener('touchmove', doDrag);
  document.addEventListener('mouseup', endDrag);
  document.addEventListener('touchend', endDrag);
  
  // Monitor quality slider
  monitorSlider.addEventListener('input', () => {
    currentAUC = parseInt(monitorSlider.value) / 100;
    updateAUCDisplay();
    drawROC();
    drawParetoCurve();
    updateFromThreshold();
  });
  
  updateAUCDisplay();
  drawROC();
  drawParetoCurve();
  updateFromThreshold();
});
</script>

---

## Why This Matters

These aren't theoretical games.

As AI systems become more capable, we face a genuine dilemma: the models that can do the most useful work are also the hardest to trust. We can't wait until we've "solved alignment" to deploy powerful AI — the economic and competitive pressures are too strong. But we also can't deploy systems that might subvert us without *any* safeguards.

AI Control offers a middle path. Instead of asking "is this AI aligned?" (a question we might not be able to answer), we ask "can we use this AI safely even if it isn't?" That reframing turns an impossibly vague alignment question into a concrete engineering problem with measurable safety guarantees.

The key insights from this framework:

1. **Safety = catch rate.** The probability of an undetected attack depends only on your monitor quality, not on how often the AI "tries" to attack.

2. **Tradeoffs are real.** There's no free lunch — better safety costs usefulness. But the right protocols can give you a much better Pareto frontier.

3. **It's adversarial.** You're not optimizing against a fixed distribution. The attack policy will improve. Your defenses need to anticipate this.

4. **The monitor is everything.** Better monitoring research expands what's possible. This is where the leverage is.

The window for getting this right isn't infinite. We need people thinking carefully about control *before* the systems arrive that require it.

<p style="text-align: center; color: #5a5a5a; font-size: 0.85rem; margin-top: 2rem;">
<em>Based on <a href="https://arxiv.org/abs/2312.06942">AI Control: Improving Safety Despite Intentional Subversion</a> (Greenblatt et al., 2023)</em>
</p>
