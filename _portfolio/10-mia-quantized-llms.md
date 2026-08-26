---
title: "Membership Inference Attacks on Quantized LLMs"
excerpt: "NYU Abu Dhabi capstone testing whether post-training quantization protects LLMs against membership inference attacks, evaluated on Pythia-12B across six PILE domains. Currently under peer review at a security-focused academic venue.<br/>"
collection: portfolio
---

<style>
.mia-viz { --mia-accent: #c0392b; --mia-ink: #1a1c20; --mia-muted: #6b7280; --mia-line: #e6e8ec; --mia-card: #f7f8fa; --mia-good: #17a673; --mia-warn: #c77700; --mia-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; margin: 1.2em 0; }
.mia-viz * { box-sizing: border-box; }
.mia-stats { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }
.mia-stat { background: var(--mia-card); border: 1px solid var(--mia-line); border-radius: 10px; padding: 14px; }
.mia-stat .n { font-size: 22px; font-weight: 700; color: var(--mia-ink); letter-spacing: -.02em; }
.mia-stat .n.accent { color: var(--mia-accent); }
.mia-stat .l { font-size: 12px; color: var(--mia-muted); margin-top: 2px; }
.mia-verdict { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.mia-vcard { border: 1px solid var(--mia-line); border-radius: 10px; padding: 16px; background: var(--mia-card); }
.mia-vcard.good { border-left: 3px solid var(--mia-good); }
.mia-vcard.warn { border-left: 3px solid var(--mia-accent); }
.mia-vcard .badge { font-size: 10.5px; font-weight: 700; letter-spacing: .04em; text-transform: uppercase; padding: 2px 7px; border-radius: 999px; display: inline-block; }
.mia-vcard .badge.good { background: rgba(23,166,115,.12); color: var(--mia-good); }
.mia-vcard .badge.warn { background: rgba(192,57,43,.1); color: var(--mia-accent); }
.mia-vcard .vh { font-size: 15px; font-weight: 700; color: var(--mia-ink); margin: 9px 0 3px; }
.mia-vcard .vs { font-size: 12.5px; color: var(--mia-muted); line-height: 1.45; }
.mia-feat { background: var(--mia-card); border: 1px solid var(--mia-line); border-radius: 10px; padding: 15px; }
.mia-feat .ftitle { display: flex; align-items: baseline; justify-content: space-between; margin-bottom: 10px; }
.mia-feat .ftitle .fn { font-size: 13px; font-weight: 600; color: var(--mia-ink); }
.mia-feat .ftitle .ft { font-family: var(--mia-mono); font-size: 20px; font-weight: 700; color: var(--mia-accent); }
.mia-segbar { display: flex; height: 26px; border-radius: 6px; overflow: hidden; border: 1px solid var(--mia-line); }
.mia-seg { display: flex; align-items: center; justify-content: center; font-size: 11px; font-weight: 700; color: #fff; white-space: nowrap; }
.mia-seg.s1 { background: #c0392b; } .mia-seg.s2 { background: #7f8c9b; } .mia-seg.s3 { background: #5a6b7a; } .mia-seg.s4 { background: #9aa4b2; } .mia-seg.s5 { background: #34495e; }
.mia-legend { display: flex; flex-wrap: wrap; gap: 8px 16px; margin-top: 11px; }
.mia-legend .li { display: flex; align-items: center; gap: 6px; font-size: 12px; color: var(--mia-muted); }
.mia-legend .sw { width: 11px; height: 11px; border-radius: 3px; flex: none; }
.mia-legend .sw.s1 { background: #c0392b; } .mia-legend .sw.s2 { background: #7f8c9b; } .mia-legend .sw.s3 { background: #5a6b7a; } .mia-legend .sw.s4 { background: #9aa4b2; } .mia-legend .sw.s5 { background: #34495e; }
.mia-legend .li b { color: var(--mia-ink); font-weight: 600; }
.mia-chips { display: flex; flex-wrap: wrap; gap: 8px; }
.mia-chip { background: var(--mia-card); border: 1px solid var(--mia-line); border-radius: 999px; padding: 6px 12px; font-size: 12.5px; color: var(--mia-ink); }
.mia-chip .k { color: var(--mia-muted); }
.mia-spec { display: grid; grid-template-columns: repeat(4, 1fr); gap: 9px; margin-top: 11px; }
.mia-specitem { background: var(--mia-card); border: 1px solid var(--mia-line); border-radius: 10px; padding: 12px; }
.mia-specitem .sk { font-size: 11px; color: var(--mia-muted); text-transform: uppercase; letter-spacing: .04em; }
.mia-specitem .sv { font-size: 13px; font-weight: 600; color: var(--mia-ink); margin-top: 3px; }
.mia-method { border: 1px solid var(--mia-line); border-radius: 10px; padding: 16px; }
.mia-method .mh { display: flex; align-items: center; gap: 8px; margin-bottom: 8px; }
.mia-method .mh .badge { font-size: 10.5px; font-weight: 700; letter-spacing: .04em; text-transform: uppercase; padding: 2px 7px; border-radius: 999px; background: rgba(199,119,0,.12); color: var(--mia-warn); }
.mia-method .mh .mt { font-size: 14px; font-weight: 700; color: var(--mia-ink); }
.mia-method .arrow { display: flex; align-items: center; gap: 10px; flex-wrap: wrap; margin: 12px 0; }
.mia-method .box { flex: 1; min-width: 130px; background: var(--mia-card); border: 1px solid var(--mia-line); border-radius: 8px; padding: 11px 12px; font-size: 12.5px; color: var(--mia-muted); line-height: 1.4; }
.mia-method .box b { color: var(--mia-ink); font-weight: 600; }
.mia-method .box.crossed { text-decoration: line-through; text-decoration-color: var(--mia-accent); text-decoration-thickness: 2px; }
.mia-method .sep { font-family: var(--mia-mono); color: var(--mia-accent); font-weight: 700; font-size: 15px; }
.mia-method .attrs { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 4px; }
.mia-method .attrs span { font-size: 11.5px; color: var(--mia-muted); background: var(--mia-card); border: 1px solid var(--mia-line); border-radius: 6px; padding: 3px 8px; }
.mia-axis { border: 1px solid var(--mia-line); border-radius: 10px; padding: 16px; }
.mia-axis .track { display: grid; grid-template-columns: repeat(4, 1fr); gap: 0; margin: 8px 0 14px; }
.mia-axis .node { text-align: center; position: relative; padding-top: 22px; }
.mia-axis .node:before { content: ""; position: absolute; top: 6px; left: 0; right: 0; height: 3px; background: linear-gradient(90deg, var(--mia-good), var(--mia-good) 66%, var(--mia-accent)); }
.mia-axis .node:first-child:before { left: 50%; }
.mia-axis .node:last-child:before { right: 50%; }
.mia-axis .node .pt { position: absolute; top: 2px; left: 50%; transform: translateX(-50%); width: 11px; height: 11px; border-radius: 50%; background: #fff; border: 2px solid var(--mia-good); }
.mia-axis .node.weak .pt { border-color: var(--mia-accent); }
.mia-axis .node .lvl { font-family: var(--mia-mono); font-size: 13px; font-weight: 700; color: var(--mia-ink); }
.mia-axis .node .st { font-size: 11px; margin-top: 2px; color: var(--mia-good); font-weight: 600; }
.mia-axis .node.weak .st { color: var(--mia-accent); }
.mia-axis .note { font-size: 12.5px; color: var(--mia-muted); line-height: 1.45; }
.mia-axis .note b { color: var(--mia-ink); font-weight: 600; }
.mia-note { font-size: 12.5px; color: var(--mia-muted); line-height: 1.45; margin-top: 10px; }
.mia-note b { color: var(--mia-ink); font-weight: 600; }
@media (max-width: 620px) { .mia-viz .mia-stats, .mia-viz .mia-spec { grid-template-columns: repeat(2,1fr); } .mia-viz .mia-verdict { grid-template-columns: 1fr; } .mia-viz .mia-axis .track { grid-template-columns: 1fr 1fr; gap: 14px 0; } .mia-viz .mia-method .arrow { flex-direction: column; align-items: stretch; } }
</style>

## The problem

Post-training quantization (AWQ, GPTQ, bitsandbytes NF4/LLM.int8()) is usually framed purely as a compression and efficiency technique. Whether it also changes a model's exposure to membership inference attacks, where an attacker tries to determine if a specific example was in the training set, was largely untested. If quantization happened to weaken that signal as a side effect, that would matter for anyone deploying compressed models trained on sensitive data. If it didn't, that assumption needed to stop being taken for granted.

## Key finding

<div class="mia-viz">
<div class="mia-verdict">
  <div class="mia-vcard good">
    <span class="badge good">Signal preserved</span>
    <div class="vh">4-bit and above: p &lt; 0.05</div>
    <div class="vs">All three quantization frameworks (AWQ, GPTQ, bitsandbytes) keep a statistically significant membership signal at 4-bit precision and above. Compression alone does not protect training-data privacy.</div>
  </div>
  <div class="mia-vcard warn">
    <span class="badge warn">Weakened, not eliminated</span>
    <div class="vh">2-bit / 3-bit GPTQ</div>
    <div class="vs">Only the most aggressive GPTQ settings measurably weaken the membership signal. Even there the signal is reduced, not removed, so the exposure does not go away.</div>
  </div>
</div>
</div>

## What was built

<div class="mia-viz">
<div class="mia-feat">
  <div class="ftitle"><span class="fn">Membership-signal vector, per sequence</span><span class="ft">36 features</span></div>
  <div class="mia-segbar">
    <div class="mia-seg s1" style="width:2.78%" title="Token-level loss (1)"></div>
    <div class="mia-seg s2" style="width:30.56%">Min-K% · 11</div>
    <div class="mia-seg s3" style="width:30.56%">Max-K% · 11</div>
    <div class="mia-seg s4" style="width:2.78%" title="ZLIB compression ratio (1)"></div>
    <div class="mia-seg s5" style="width:33.33%">Perturbation ppl · 12</div>
  </div>
  <div class="mia-legend">
    <div class="li"><span class="sw s1"></span><span>Token-level loss <b>1</b></span></div>
    <div class="li"><span class="sw s2"></span><span>Min-K% scores <b>11</b></span></div>
    <div class="li"><span class="sw s3"></span><span>Max-K% scores <b>11</b></span></div>
    <div class="li"><span class="sw s4"></span><span>ZLIB compression ratio <b>1</b></span></div>
    <div class="li"><span class="sw s5"></span><span>Perturbation-based perplexity <b>12</b></span></div>
  </div>
</div>

<div class="mia-chips" style="margin-top:12px">
  <div class="mia-chip"><span class="k">Model:</span> Pythia-12B</div>
  <div class="mia-chip"><span class="k">Framework:</span> extends Maini et al. Dataset Inference</div>
  <div class="mia-chip"><span class="k">Tests:</span> two-sample t-test + Cohen's d</div>
</div>

<div class="mia-spec">
  <div class="mia-specitem"><div class="sk">Domains</div><div class="sv">6 PILE domains</div></div>
  <div class="mia-specitem"><div class="sk">Compute</div><div class="sv">SLURM HPC cluster</div></div>
  <div class="mia-specitem"><div class="sk">Hardware</div><div class="sv">NVIDIA A100 80GB</div></div>
  <div class="mia-specitem"><div class="sk">Score</div><div class="sv">Linear regressor</div></div>
</div>

<div class="mia-chips" style="margin-top:9px">
  <div class="mia-chip">Wikipedia</div>
  <div class="mia-chip">ArXiv</div>
  <div class="mia-chip">GitHub</div>
  <div class="mia-chip">Common Crawl</div>
  <div class="mia-chip">HackerNews</div>
  <div class="mia-chip">Math</div>
</div>

<div class="mia-note">An IID-controlled dataset-inference pipeline: a linear regressor combines the 36 features above into a single membership score per sequence, and the t-test with Cohen's d checks whether that score separates members from non-members across quantization configurations.</div>
</div>

<div class="mia-viz">
<div class="mia-axis">
  <div class="track">
    <div class="node"><span class="pt"></span><span class="lvl">FP</span><span class="st">preserved</span></div>
    <div class="node"><span class="pt"></span><span class="lvl">4-bit</span><span class="st">preserved</span></div>
    <div class="node weak"><span class="pt"></span><span class="lvl">3-bit</span><span class="st">weakening</span></div>
    <div class="node weak"><span class="pt"></span><span class="lvl">2-bit</span><span class="st">weakening</span></div>
  </div>
  <div class="note">From full precision down to 4-bit, the membership signal stays statistically significant (p &lt; 0.05) across all three frameworks. The <b>weakening</b> only appears at the most aggressive GPTQ 3-bit and 2-bit settings, and even there the signal is reduced rather than eliminated.</div>
</div>
</div>

## A methodological finding along the way

<div class="mia-viz">
<div class="mia-method">
  <div class="mh"><span class="badge">Artifact ruled out</span><span class="mt">Corpus-level leakage was distribution shift, not memorization</span></div>
  <div class="arrow">
    <div class="box crossed">Apparent reading: large cross-corpus leakage differences reflect <b>true memorization</b></div>
    <div class="sep">&rarr;</div>
    <div class="box">Actual cause: <b>residual distribution shift</b> between member and non-member splits</div>
  </div>
  <div class="vs" style="font-size:12.5px;color:var(--mia-muted);line-height:1.45">Diagnosed with a secondary spaCy analysis of five corpus-level attributes:</div>
  <div class="attrs">
    <span>7-gram overlap</span>
    <span>Named-entity density</span>
    <span>Mean dependency length</span>
    <span>Lexical specificity</span>
    <span>Vocabulary diversity</span>
  </div>
  <div class="mia-note">A pitfall worth flagging for anyone evaluating membership inference attacks across heterogeneous corpora.</div>
</div>
</div>

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
