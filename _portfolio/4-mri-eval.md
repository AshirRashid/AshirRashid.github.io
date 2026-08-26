---
title: "MS Lesion Evaluation Framework"
excerpt: "Lesion-centric evaluation framework (MSEval) that exposes failure modes hidden by standard Dice metrics in MS lesion segmentation models. Published at IEEE IJCNN @ WCCI 2026.<br/>"
collection: portfolio
---

<style>
.mri-viz { --mri-accent: #2f8fb0; --mri-ink: #1a1c20; --mri-muted: #6b7280; --mri-line: #e6e8ec; --mri-card: #f7f8fa; --mri-good: #17a673; --mri-warn: #c77700; --mri-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; margin: 1.2em 0; }
.mri-viz * { box-sizing: border-box; }
.mri-stats { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }
.mri-stat { background: var(--mri-card); border: 1px solid var(--mri-line); border-radius: 10px; padding: 14px; }
.mri-stat .n { font-size: 22px; font-weight: 700; color: var(--mri-ink); letter-spacing: -.02em; }
.mri-stat .n.accent { color: var(--mri-accent); }
.mri-stat .n.warn { color: var(--mri-warn); }
.mri-stat .n.good { color: var(--mri-good); }
.mri-stat .l { font-size: 12px; color: var(--mri-muted); margin-top: 2px; }
.mri-cmp { display: grid; gap: 14px; }
.mri-metric .mname { font-size: 13px; font-weight: 600; color: var(--mri-ink); margin-bottom: 6px; }
.mri-track { display: grid; grid-template-columns: 132px 1fr; align-items: center; gap: 8px; margin-bottom: 6px; }
.mri-track .who { font-size: 12px; color: var(--mri-muted); }
.mri-barwrap { background: var(--mri-card); border: 1px solid var(--mri-line); border-radius: 6px; height: 24px; overflow: hidden; }
.mri-bar { height: 100%; display: flex; align-items: center; justify-content: flex-end; padding-right: 8px; font-size: 11.5px; font-weight: 700; color: #fff; min-width: 34px; }
.mri-bar.hi { background: var(--mri-accent); }
.mri-bar.lo { background: #d98a4f; }
.mri-bar.mid { background: #9aa4b2; }
.mri-binlegend { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 4px; }
.mri-binlegend .chip { font-family: var(--mri-mono); font-size: 11px; color: var(--mri-muted); background: var(--mri-card); border: 1px solid var(--mri-line); border-radius: 999px; padding: 3px 9px; }
.mri-steps { display: grid; grid-template-columns: repeat(3, 1fr); gap: 9px; }
.mri-step { background: var(--mri-card); border: 1px solid var(--mri-line); border-radius: 10px; padding: 13px; }
.mri-step .sn { font-family: var(--mri-mono); font-size: 11px; color: var(--mri-accent); font-weight: 700; }
.mri-step .sh { font-weight: 700; font-size: 14px; margin: 5px 0 3px; color: var(--mri-ink); }
.mri-step .sd { font-size: 12px; color: var(--mri-muted); line-height: 1.45; }
.mri-callout { background: #eef6f9; border: 1px solid #cfe6ee; border-left: 4px solid var(--mri-accent); border-radius: 10px; padding: 15px 17px; }
.mri-callout .flow { display: flex; align-items: center; gap: 12px; flex-wrap: wrap; margin-bottom: 9px; }
.mri-callout .box { background: #fff; border: 1px solid var(--mri-line); border-radius: 8px; padding: 9px 13px; text-align: center; }
.mri-callout .box .bn { font-size: 20px; font-weight: 700; color: var(--mri-ink); line-height: 1; }
.mri-callout .box .bl { font-size: 11px; color: var(--mri-muted); margin-top: 3px; }
.mri-callout .arr { font-size: 18px; color: var(--mri-accent); font-weight: 700; }
.mri-callout .dice { margin-left: auto; text-align: center; }
.mri-callout .dice .dn { font-family: var(--mri-mono); font-size: 22px; font-weight: 700; color: var(--mri-warn); line-height: 1; }
.mri-callout .dice .dl { font-size: 11px; color: var(--mri-muted); margin-top: 3px; }
.mri-callout .note { font-size: 12.5px; color: var(--mri-muted); line-height: 1.45; }
.mri-callout .note b { color: var(--mri-ink); font-weight: 600; }
.mri-chips { display: flex; flex-wrap: wrap; gap: 8px; align-items: center; }
.mri-chips .grouplbl { font-size: 11px; font-weight: 700; letter-spacing: .04em; text-transform: uppercase; color: var(--mri-muted); margin-right: 4px; }
.mri-chips .mchip { font-family: var(--mri-mono); font-size: 12px; color: var(--mri-ink); background: var(--mri-card); border: 1px solid var(--mri-line); border-radius: 999px; padding: 4px 11px; }
.mri-chips .mchip.data { background: #eef6f9; border-color: #cfe6ee; color: var(--mri-accent); font-weight: 600; }
@media (max-width: 620px) { .mri-viz .mri-stats { grid-template-columns: repeat(2,1fr); } .mri-viz .mri-steps { grid-template-columns: repeat(2,1fr); } .mri-viz .mri-track { grid-template-columns: 96px 1fr; } }
</style>

## The problem

Standard MS lesion segmentation metrics (Dice, HD95) are computed voxel-by-voxel across the full scan. Large lesions dominate aggregate scores because they contribute far more voxels than small ones. A model can miss dozens of small lesions entirely and still look competitive. SAMSEG had a 65.2% false negative rate on one dataset while staying in that range.

<div class="mri-viz">
<div class="mri-stats">
  <div class="mri-stat"><div class="n warn">65.2<span style="font-size:13px">%</span></div><div class="l">SAMSEG false negative rate on one dataset</div></div>
  <div class="mri-stat"><div class="n">0.34</div><div class="l">Best F1, very small lesions</div></div>
  <div class="mri-stat"><div class="n good">0.92 to 1.00</div><div class="l">Best F1, large lesions</div></div>
  <div class="mri-stat"><div class="n accent">~7<span style="font-size:13px">%</span></div><div class="l">Balanced-accuracy gain from custom losses</div></div>
</div>
</div>

Existing matching methods make this harder to see. They enforce strict 1:1 correspondence between predicted lesions and ground truth. When a large lesion fragments into several predictions, or several small lesions collapse into one, the method records raw FP and FN counts rather than flagging those as distinct failure patterns. The metric gives you a number. It doesn't tell you what the model got wrong.

## What was built

MSEval: a six-stage lesion-centric evaluation pipeline with N:M lesion matching, size-stratified reporting, and per-lesion metrics. Synthetic MRI data (FLAIR, T1, T2) was generated for threshold hyperparameter tuning. Custom loss functions (Generalized Surface Loss, ContourSoftDice variants, targeting boundary/region weaknesses and annotator disagreement) were included as demonstration cases, improving balanced accuracy by ~7% and showing how MSEval reveals differences between training objectives that aggregate Dice scores hide.

<div class="mri-viz">
<div class="mri-steps">
  <div class="mri-step"><div class="sn">STAGE 1</div><div class="sh">Connected components</div><div class="sd">Binary masks decomposed into discrete lesion instances via 3D connected component analysis.</div></div>
  <div class="mri-step"><div class="sn">STAGE 2</div><div class="sh">Overlap metrics</div><div class="sd">Bounding box tests reject spatially disjoint pairs. Three overlap metrics for the rest: IoU, Intersection over GT Area (IoA_GT), and Intersection over Predicted Area (IoA_Pred).</div></div>
  <div class="mri-step"><div class="sn">STAGE 3</div><div class="sh">Matching + threshold</div><div class="sd">Score = max(IoU, IoA_GT, IoA_Pred); pairs retained above threshold τ, N:M configurations accepted. IoA_Pred captures 1:N splits (one GT lesion fragmented into many); IoA_GT captures N:1 merges (many collapsed into one).</div></div>
  <div class="mri-step"><div class="sn">STAGE 4</div><div class="sh">Cluster by type</div><div class="sd">Matched pairs organized into clusters by correspondence type: 1:1, 1:N, N:1, N:M.</div></div>
  <div class="mri-step"><div class="sn">STAGE 5</div><div class="sh">Per-pair + scan metrics</div><div class="sd">Per-pair: lesion-specific Dice, HD95, correspondence type. Scan-level: precision, recall, F1, FP load.</div></div>
  <div class="mri-step"><div class="sn">STAGE 6</div><div class="sh">Size bins</div><div class="sd">Reported per bin: Very Small (0 to 10 voxels), Small (10 to 100), Medium (100 to 400), Large (&gt;400), so small-lesion detection is separable.</div></div>
</div>
</div>

## What it shows

Six models benchmarked on MSSEG-1 and MSLesSeg (T2-FLAIR, real clinical data): nnU-Net, SegResNet, SwinUNETR, LST-AI, SAMSEG, mindGlide.

<div class="mri-viz">
<div class="mri-chips">
  <span class="grouplbl">Models</span>
  <span class="mchip">nnU-Net</span>
  <span class="mchip">SegResNet</span>
  <span class="mchip">SwinUNETR</span>
  <span class="mchip">LST-AI</span>
  <span class="mchip">SAMSEG</span>
  <span class="mchip">mindGlide</span>
  <span class="grouplbl" style="margin-left:6px">Datasets</span>
  <span class="mchip data">MSSEG-1</span>
  <span class="mchip data">MSLesSeg</span>
</div>
</div>

Best F1 for very small lesions: 0.34. For large lesions: 0.92 to 1.00. Models sitting at 53 to 58% aggregate Dice still left roughly 30% of GT lesion load as false negatives. The aggregate score says almost nothing about small-lesion detection.

<div class="mri-viz">
<div class="mri-cmp">
  <div class="mri-metric">
    <div class="mname">Best F1 by lesion size</div>
    <div class="mri-track"><span class="who">Very Small</span><div class="mri-barwrap"><div class="mri-bar lo" style="width:34%">0.34</div></div></div>
    <div class="mri-track"><span class="who">Large</span><div class="mri-barwrap"><div class="mri-bar hi" style="width:100%">0.92 to 1.00</div></div></div>
  </div>
  <div class="mri-binlegend">
    <span class="chip">Very Small: 0 to 10 voxels</span>
    <span class="chip">Small: 10 to 100</span>
    <span class="chip">Medium: 100 to 400</span>
    <span class="chip">Large: &gt;400</span>
  </div>
</div>
</div>

N:M matching also exposed failures that don't show up anywhere in voxel-wise reporting. One nnU-Net cluster had 4 GT lesions mapped to 12 predicted components with an aggregated Dice of 0.00.

<div class="mri-viz">
<div class="mri-callout">
  <div class="flow">
    <div class="box"><div class="bn">4</div><div class="bl">GT lesions</div></div>
    <span class="arr">&rarr;</span>
    <div class="box"><div class="bn">12</div><div class="bl">predicted components</div></div>
    <div class="dice"><div class="dn">0.00</div><div class="dl">aggregated Dice</div></div>
  </div>
  <div class="note">One <b>nnU-Net N:M cluster</b>. Strict 1:1 matching would have logged this as scattered false positives and false negatives. N:M matching names it for what it is: a many-to-many breakdown that voxel-wise reporting hides entirely.</div>
</div>
</div>

## Key design decisions

**Three overlap metrics.** IoU alone can't distinguish a split from a merge. IoA_GT catches merges (many GT lesions absorbed into one prediction); IoA_Pred catches splits (one GT lesion fragmented into many). Both directions of failure needed a dedicated metric.

**N:M over 1:1 matching.** Strict 1:1 correspondence turns every split and merge into unlabeled FP/FN counts. N:M matching makes those patterns visible and distinguishable by type, which is the point.

**Size-stratified reporting.** One large lesion in the aggregate can absorb the score impact of many missed small ones. Binning by size gives small-lesion detection its own measurement, separate from overall performance.

Published at IEEE IJCNN @ WCCI 2026.

**Technologies:**
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Python</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">PyTorch</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">nnU-Net</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">SimpleITK</span>

**Concepts / Algorithms:**
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Lesion-Centric Evaluation</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Connected Component Analysis</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">N:M Lesion Matching</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Size-Stratified Metrics</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">MS Lesion Segmentation</span>
