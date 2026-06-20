---
title: "MS Lesion Evaluation Framework"
excerpt: "Lesion-centric evaluation framework (MSEval) that exposes failure modes hidden by standard Dice metrics in MS lesion segmentation models. Published at IEEE IJCNN @ WCCI 2026.<br/>"
collection: portfolio
---

## The problem

Standard MS lesion segmentation metrics (Dice, HD95) are computed voxel-by-voxel across the full scan. Large lesions dominate aggregate scores because they contribute far more voxels than small ones. A model can miss dozens of small lesions entirely and still look competitive. SAMSEG had a 65.2% false negative rate on one dataset while staying in that range.

Existing matching methods make this harder to see. They enforce strict 1:1 correspondence between predicted lesions and ground truth. When a large lesion fragments into several predictions, or several small lesions collapse into one, the method records raw FP and FN counts rather than flagging those as distinct failure patterns. The metric gives you a number. It doesn't tell you what the model got wrong.

## What was built

MSEval: a six-stage lesion-centric evaluation pipeline with N:M lesion matching, size-stratified reporting, and per-lesion metrics. Synthetic MRI data (FLAIR, T1, T2) was generated for threshold hyperparameter tuning. Custom loss functions were included as demonstration cases to show how MSEval reveals differences between training objectives that aggregate Dice scores hide.

**Stage 1.** Binary masks decomposed into discrete lesion instances via 3D connected component analysis.

**Stage 2.** Bounding box tests reject spatially disjoint pairs. Three overlap metrics computed for the rest: IoU, Intersection over GT Area (IoA_GT), and Intersection over Predicted Area (IoA_Pred).

**Stage 3.** Matching score = max(IoU, IoA_GT, IoA_Pred); pairs retained above threshold τ. N:M configurations accepted. IoA_Pred captures 1:N splits (one GT lesion fragmented into many predictions). IoA_GT captures N:1 merges (many GT lesions collapsed into one prediction).

**Stage 4.** Matched pairs organized into clusters by correspondence type: 1:1, 1:N, N:1, N:M.

**Stage 5.** Per-pair: lesion-specific Dice, HD95, and correspondence type. Scan-level: precision, recall, F1, and FP load.

**Stage 6.** Size bins: Very Small (0–10 voxels), Small (10–100), Medium (100–400), Large (>400). Performance reported per bin.

## What it shows

Six models benchmarked on MSSEG-1 and MSLesSeg (T2-FLAIR, real clinical data): nnU-Net, SegResNet, SwinUNETR, LST-AI, SAMSEG, mindGlide.

Best F1 for very small lesions: 0.34. For large lesions: 0.92–1.00. Models sitting at 53–58% aggregate Dice still left roughly 30% of GT lesion load as false negatives. The aggregate score says almost nothing about small-lesion detection.

N:M matching also exposed failures that don't show up anywhere in voxel-wise reporting. One nnU-Net cluster had 4 GT lesions mapped to 12 predicted components with an aggregated Dice of 0.00.

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
