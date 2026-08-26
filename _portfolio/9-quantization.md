---
title: "Mixed-Precision Quantization"
excerpt: "Mixed-precision quantization framework for DNNs and LLMs using RL-based per-layer bit-width search, extending ANT and OliVe. Research work at eBrain Lab, NYU Abu Dhabi.<br/>"
collection: portfolio
---

<style>
.quant-viz { --accent: #d98324; --quant-ink: #1a1c20; --quant-muted: #6b7280; --quant-line: #e6e8ec; --quant-card: #f7f8fa; --quant-good: #17a673; --quant-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; margin: 1.2em 0; }
.quant-viz * { box-sizing: border-box; }
.quant-stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; }
.quant-stat { background: var(--quant-card); border: 1px solid var(--quant-line); border-radius: 10px; padding: 16px; }
.quant-stat .n { font-size: 24px; font-weight: 700; color: var(--accent); letter-spacing: -.02em; }
.quant-stat .l { font-size: 12px; color: var(--quant-muted); margin-top: 3px; line-height: 1.4; }
.quant-layers { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.quant-layerpanel { border: 1px solid var(--quant-line); border-radius: 10px; padding: 15px; background: var(--quant-card); }
.quant-layerpanel h4 { margin: 0 0 3px; font-size: 14px; color: var(--quant-ink); }
.quant-layerpanel .sub { font-size: 11.5px; color: var(--quant-muted); margin-bottom: 12px; }
.quant-stack { display: flex; flex-direction: column; gap: 5px; }
.quant-lyr { display: grid; grid-template-columns: 74px 1fr 46px; align-items: center; gap: 8px; }
.quant-lyr .name { font-size: 11px; color: var(--quant-muted); font-family: var(--quant-mono); }
.quant-lyr .bar { height: 20px; border-radius: 4px; display: flex; align-items: center; padding-left: 8px; font-size: 10.5px; font-weight: 700; color: #fff; }
.quant-lyr .tag { font-size: 10.5px; font-family: var(--quant-mono); font-weight: 700; text-align: right; }
.b-fp32 { background: #1a1c20; } .b-int8 { background: var(--accent); } .b-int4 { background: #e7b980; color: #6b4a12 !important; }
.t-fp32 { color: #1a1c20; } .t-int8 { color: var(--accent); } .t-int4 { color: #b07a1e; }
.quant-legend { display: flex; gap: 14px; flex-wrap: wrap; margin-top: 12px; font-size: 11px; color: var(--quant-muted); }
.quant-legend span { display: inline-flex; align-items: center; gap: 5px; }
.quant-legend i { width: 11px; height: 11px; border-radius: 3px; display: inline-block; }
.quant-steps { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; }
.quant-step { background: var(--quant-card); border: 1px solid var(--quant-line); border-radius: 10px; padding: 14px; }
.quant-step .sn { font-family: var(--quant-mono); font-size: 11px; color: var(--accent); font-weight: 700; }
.quant-step .sh { font-weight: 700; font-size: 14px; margin: 5px 0 4px; color: var(--quant-ink); }
.quant-step .sd { font-size: 12.5px; color: var(--quant-muted); line-height: 1.5; }
.quant-split { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.quant-panel { border: 1px solid var(--quant-line); border-radius: 10px; padding: 15px; }
.quant-panel .hd { display: flex; align-items: center; justify-content: space-between; gap: 8px; margin-bottom: 4px; }
.quant-panel h4 { margin: 0; font-size: 14px; color: var(--quant-ink); }
.quant-panel .badge { font-size: 10.5px; font-weight: 700; letter-spacing: .04em; text-transform: uppercase; padding: 2px 8px; border-radius: 999px; background: rgba(217,131,36,.12); color: var(--accent); white-space: nowrap; }
.quant-panel .scope { font-size: 11.5px; color: var(--quant-muted); margin: 0 0 8px; }
.quant-panel ul { margin: 0; padding-left: 15px; }
.quant-panel li { font-size: 12.5px; color: var(--quant-muted); margin-bottom: 7px; line-height: 1.5; }
.quant-panel li b { color: var(--quant-ink); font-weight: 600; }
.quant-cover { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.quant-covcol { border: 1px solid var(--quant-line); border-radius: 10px; padding: 14px; }
.quant-covcol .ct { font-size: 12px; font-weight: 700; color: var(--quant-ink); margin-bottom: 9px; }
.quant-chips { display: flex; flex-wrap: wrap; gap: 6px; }
.quant-chip { font-family: var(--quant-mono); font-size: 11.5px; color: var(--quant-ink); background: var(--quant-card); border: 1px solid var(--quant-line); border-radius: 999px; padding: 3px 10px; }
.quant-note { font-size: 11.5px; color: var(--quant-muted); margin-top: 12px; line-height: 1.5; }
@media (max-width: 620px) { .quant-viz .quant-stats, .quant-viz .quant-layers, .quant-viz .quant-steps, .quant-viz .quant-split, .quant-viz .quant-cover { grid-template-columns: 1fr; } }
</style>

## The problem

Most quantization tools apply the same bit-width to every layer. Fine for small networks. With LLMs and large DNNs it breaks down. Sensitive layers (attention heads, first and last layers) don't tolerate aggressive compression well. Others barely notice dropping from FP32 to INT4. Uniform precision is either too conservative or too aggressive, and there's no obvious manual path through that tradeoff at scale.

The goal was automating per-layer bit-width selection: let the search find good configurations instead of guessing or falling back to uniform settings.

<div class="quant-viz">
<div class="quant-layers">
  <div class="quant-layerpanel">
    <h4>Uniform precision</h4>
    <div class="sub">Every layer gets the same bit-width, sensitive or not.</div>
    <div class="quant-stack">
      <div class="quant-lyr"><span class="name">first</span><div class="bar b-int8" style="width:70%">INT8</div><span class="tag t-int8">INT8</span></div>
      <div class="quant-lyr"><span class="name">attention</span><div class="bar b-int8" style="width:70%">INT8</div><span class="tag t-int8">INT8</span></div>
      <div class="quant-lyr"><span class="name">hidden</span><div class="bar b-int8" style="width:70%">INT8</div><span class="tag t-int8">INT8</span></div>
      <div class="quant-lyr"><span class="name">hidden</span><div class="bar b-int8" style="width:70%">INT8</div><span class="tag t-int8">INT8</span></div>
      <div class="quant-lyr"><span class="name">last</span><div class="bar b-int8" style="width:70%">INT8</div><span class="tag t-int8">INT8</span></div>
    </div>
  </div>
  <div class="quant-layerpanel">
    <h4>Mixed precision</h4>
    <div class="sub">Sensitive layers kept higher, tolerant layers dropped to INT4.</div>
    <div class="quant-stack">
      <div class="quant-lyr"><span class="name">first</span><div class="bar b-fp32" style="width:100%">FP32</div><span class="tag t-fp32">FP32</span></div>
      <div class="quant-lyr"><span class="name">attention</span><div class="bar b-fp32" style="width:100%">FP32</div><span class="tag t-fp32">FP32</span></div>
      <div class="quant-lyr"><span class="name">hidden</span><div class="bar b-int4" style="width:40%">INT4</div><span class="tag t-int4">INT4</span></div>
      <div class="quant-lyr"><span class="name">hidden</span><div class="bar b-int4" style="width:40%">INT4</div><span class="tag t-int4">INT4</span></div>
      <div class="quant-lyr"><span class="name">last</span><div class="bar b-fp32" style="width:100%">FP32</div><span class="tag t-fp32">FP32</span></div>
    </div>
  </div>
</div>
<div class="quant-legend">
  <span><i class="b-fp32"></i> FP32, sensitive layers kept high</span>
  <span><i class="b-int8"></i> INT8</span>
  <span><i class="b-int4"></i> INT4, tolerant layers compressed</span>
</div>
</div>

## How it works

<div class="quant-viz">
<div class="quant-steps">
  <div class="quant-step"><div class="sn">01</div><div class="sh">Mixed-precision search</div><div class="sd">A hardware-aware RL policy reads per-layer sensitivity metrics and assigns a bit-width per layer, rewarded on measured hardware latency and memory rather than FLOP-count proxies, so it learns what actually runs fast on real chips. Evolutionary search runs alongside to cover configurations the RL agent misses.</div></div>
  <div class="quant-step"><div class="sn">02</div><div class="sh">ANT for DNNs</div><div class="sd">Adaptive numerical data types match each layer's actual weight and activation distribution rather than a fixed grid. Custom CUDA kernels implement this at inference time.</div></div>
  <div class="quant-step"><div class="sn">03</div><div class="sh">OliVe for LLMs</div><div class="sd">Activation outliers are paired with an adjacent normal value and encoded locally, so INT8 neither clips them nor wastes precision everywhere else. No global coordination needed.</div></div>
  <div class="quant-step"><div class="sn">04</div><div class="sh">MLflow tracking</div><div class="sd">Every experiment logs accuracy, energy, and latency through an MLflow pipeline. Running a new configuration means queuing a run, not rebuilding a spreadsheet.</div></div>
</div>
</div>

## Results

<div class="quant-viz">
<div class="quant-stats">
  <div class="quant-stat"><div class="n">+15%</div><div class="l">Model performance over baseline quantization configurations</div></div>
  <div class="quant-stat"><div class="n">+8.3%</div><div class="l">Energy and memory efficiency</div></div>
  <div class="quant-stat"><div class="n">43%</div><div class="l">Reduction in hyperparameter search time</div></div>
</div>
</div>

## Key design decisions

<div class="quant-viz">
<div class="quant-split">
  <div class="quant-panel">
    <div class="hd"><h4>ANT</h4><span class="badge">DNNs</span></div>
    <p class="scope">Addresses the weight distribution problem in CNNs and ViTs.</p>
    <ul>
      <li><b>Adaptive numerical data types</b> matched per layer to the actual distribution.</li>
      <li><b>Custom CUDA kernels</b> implement the encoding at inference time.</li>
    </ul>
  </div>
  <div class="quant-panel">
    <div class="hd"><h4>OliVe</h4><span class="badge">LLMs</span></div>
    <p class="scope">Addresses the activation outlier problem specific to attention.</p>
    <ul>
      <li><b>Outlier-victim pairing</b> for activation outliers, encoded locally at the kernel level.</li>
      <li><b>No global coordination</b> controller across the accelerator, unlike prior methods that separate outliers globally and pay a coordination-hardware overhead.</li>
    </ul>
  </div>
</div>
</div>

<div class="quant-viz">
<div class="quant-cover">
  <div class="quant-covcol">
    <div class="ct">DNNs on ImageNet</div>
    <div class="quant-chips">
      <span class="quant-chip">VGG16</span>
      <span class="quant-chip">ResNet18</span>
      <span class="quant-chip">ResNet50</span>
      <span class="quant-chip">Inception-V3</span>
      <span class="quant-chip">ViT</span>
    </div>
  </div>
  <div class="quant-covcol">
    <div class="ct">LLMs</div>
    <div class="quant-chips">
      <span class="quant-chip">BERT</span>
      <span class="quant-chip">GPT-2</span>
      <span class="quant-chip">OPT</span>
      <span class="quant-chip">Bloom</span>
    </div>
  </div>
</div>
<div class="quant-note">Research work at eBrain Lab, NYU Abu Dhabi, extending ANT (MICRO'22) and OliVe (ISCA'23).</div>
</div>

**Technologies:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Python</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">PyTorch</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">CUDA</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">MLflow</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">AWQ</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">GPTQ</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">SmoothQuant</span>

**Concepts / Algorithms:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Mixed-Precision Quantization</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Reinforcement Learning</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Evolutionary Search</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Post-Training Quantization</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">LLM Inference Optimization</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Outlier-Victim Pair Encoding</span>
