---
title: "Mixed-Precision Quantization"
excerpt: "Mixed-precision quantization framework for DNNs and LLMs using RL-based per-layer bit-width search, extending ANT and OliVe. Research work at eBrain Lab, NYU Abu Dhabi.<br/>"
collection: portfolio
---

## The problem

Most quantization tools apply the same bit-width to every layer. Fine for small networks. With LLMs and large DNNs it breaks down. Sensitive layers (attention heads, first and last layers) don't tolerate aggressive compression well. Others barely notice dropping from FP32 to INT4. Uniform precision is either too conservative or too aggressive, and there's no obvious manual path through that tradeoff at scale.

The goal was automating per-layer bit-width selection: let the search find good configurations instead of guessing or falling back to uniform settings.

## What was built

A mixed-precision quantization framework developed at eBrain Lab (NYU Abu Dhabi), extending ANT (Adaptive Numerical Data Type, MICRO'22) and OliVe (ISCA'23). An RL policy and evolutionary search explore configurations automatically under real hardware constraints (memory, latency). Coverage spans DNNs (VGG16, ResNet18, ResNet50, Inception-V3, ViT on ImageNet) and LLMs (BERT, GPT-2, OPT, Bloom).

## How it works

**Mixed-precision search.** A hardware-aware RL policy assigns bit-widths per layer. At each step it reads layer sensitivity metrics and outputs a precision level. Reward is the actual accuracy-efficiency tradeoff on hardware, not a proxy metric. Evolutionary search runs alongside, covering configurations the RL agent misses.

**ANT for DNNs.** ANT encodes weights and activations using adaptive numerical data types matched to each layer's actual distribution, rather than a fixed grid. Custom CUDA kernels implement this at inference time.

**OliVe for LLMs.** LLM activations contain outlier values with magnitudes far outside normal. Standard INT8 quantization either clips them (losing information) or scales to fit them (wasting precision everywhere else). OliVe pairs each outlier with an adjacent normal value and encodes the pair locally. No global coordination across the accelerator needed.

**MLflow tracking.** All experiments ran through an MLflow pipeline logging accuracy, energy, and latency. Running a new configuration meant queuing a run, not rebuilding a spreadsheet.

## Results

- +15% model performance over baseline quantization configurations
- +8.3% energy and memory efficiency
- 43% reduction in hyperparameter search time

## Key design decisions

**Hardware-aware rewards in RL.** Using actual hardware latency and memory as reward signals, rather than FLOP counts, means the policy learns what runs fast on real chips. Proxy metrics optimize for the proxy.

**Outlier-victim pairing instead of global sparsity encoding.** Prior approaches separate outliers from normal values globally, which requires coordination hardware and adds overhead. OVP pairs each outlier with its neighbor and handles them together at the kernel level. No orchestration controller.

**Separate approaches for DNNs vs LLMs.** The quantization problem is different in vision models versus transformers. ANT addresses the weight distribution problem in CNNs and ViTs. OliVe addresses the activation outlier problem specific to attention. Trying to unify them would mean neither works as well.

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
