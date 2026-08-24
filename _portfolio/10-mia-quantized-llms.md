---
title: "Membership Inference Attacks on Quantized LLMs"
excerpt: "NYU Abu Dhabi capstone testing whether post-training quantization protects LLMs against membership inference attacks, evaluated on Pythia-12B across six PILE domains. Currently under peer review at a security-focused academic venue.<br/>"
collection: portfolio
---

## The problem

Post-training quantization (AWQ, GPTQ, bitsandbytes NF4/LLM.int8()) is usually framed purely as a compression and efficiency technique. Whether it also changes a model's exposure to membership inference attacks, where an attacker tries to determine if a specific example was in the training set, was largely untested. If quantization happened to weaken that signal as a side effect, that would matter for anyone deploying compressed models trained on sensitive data. If it didn't, that assumption needed to stop being taken for granted.

## What was built

An IID-controlled dataset-inference pipeline, extending Maini et al.'s Dataset Inference framework, evaluated on Pythia-12B across six PILE domains (Wikipedia, ArXiv, GitHub, Common Crawl, HackerNews, Math). For each sequence, the pipeline computes a 36-feature membership-signal vector: token-level loss, 11 Min-K% and 11 Max-K% scores, ZLIB compression ratio, and 12 perturbation-based perplexity features. A linear regressor combines these into a membership score, with a two-sample t-test and Cohen's d used to test statistical significance across quantization configurations. Experiments ran on a SLURM-managed university HPC cluster (NVIDIA A100 80GB).

## Key finding

All three quantization frameworks preserve a statistically significant membership signal (p < 0.05) at 4-bit precision and above. Quantization does **not** meaningfully protect training-data privacy until the most aggressive GPTQ settings (2-bit/3-bit), which measurably weaken the signal without eliminating it.

## A methodological finding along the way

A secondary linguistic analysis (spaCy, five corpus-level attributes: 7-gram overlap, named-entity density, mean dependency length, lexical specificity, vocabulary diversity) tested whether large differences in leakage across corpora reflected true differences in memorization. They didn't: the apparent corpus-level effect turned out to be an artifact of residual distribution shift between member and non-member splits, not memorization itself, a pitfall worth flagging for anyone evaluating MIAs across heterogeneous corpora.

## Status

This is an NYU Abu Dhabi capstone project, currently under peer review at a security-focused academic venue (venue withheld per double-blind submission policy). Code and full results will be published once the review concludes.

**Technologies:**
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Python</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">PyTorch</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Hugging Face Transformers</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">spaCy</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">NumPy</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Pandas</span>

**Concepts / Algorithms:**
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">LLM</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">ML Security</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">ML Privacy</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Membership Inference Attacks</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Quantization (AWQ, GPTQ, bitsandbytes)</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Statistical Testing</span>
