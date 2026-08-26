---
title: "Gmail RAG Pipeline"
excerpt: "Local semantic search over Gmail using whole-email BGE embeddings and ChromaDB, with no email content leaving the device. Benchmarked against a BM25 baseline. Foundation for the Agentic Process Discovery system.<br/>"
collection: portfolio
---

<style>
.emailai-viz { --ea-accent: #2f6df6; --ea-ink: #1a1c20; --ea-muted: #6b7280; --ea-line: #e6e8ec; --ea-card: #f7f8fa; --ea-good: #17a673; --ea-warn: #c77700; --ea-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; margin: 1.2em 0; }
.emailai-viz * { box-sizing: border-box; }
.ea-stats { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }
.ea-stat { background: var(--ea-card); border: 1px solid var(--ea-line); border-radius: 10px; padding: 14px; }
.ea-stat .n { font-size: 22px; font-weight: 700; color: var(--ea-ink); letter-spacing: -.02em; }
.ea-stat .n.good { color: var(--ea-good); }
.ea-stat .l { font-size: 12px; color: var(--ea-muted); margin-top: 2px; }
.ea-cmp { display: grid; gap: 14px; }
.ea-metric .mname { font-size: 13px; font-weight: 600; color: var(--ea-ink); margin-bottom: 6px; }
.ea-track { display: grid; grid-template-columns: 78px 1fr; align-items: center; gap: 8px; margin-bottom: 6px; }
.ea-track .who { font-size: 12px; color: var(--ea-muted); }
.ea-barwrap { background: var(--ea-card); border: 1px solid var(--ea-line); border-radius: 6px; height: 24px; overflow: hidden; }
.ea-bar { height: 100%; display: flex; align-items: center; justify-content: flex-end; padding-right: 8px; font-size: 11.5px; font-weight: 700; color: #fff; }
.ea-bar.sem { background: var(--ea-accent); }
.ea-bar.bm { background: #9aa4b2; }
.ea-demo { background: #0f1115; border: 1px solid #23262d; border-radius: 12px; overflow: hidden; }
.ea-demo .top { display: flex; align-items: center; gap: 6px; padding: 9px 12px; background: #171a20; border-bottom: 1px solid #23262d; }
.ea-demo .dot { width: 10px; height: 10px; border-radius: 50%; }
.ea-demo .r { background: #ff5f57; } .ea-demo .y { background: #febc2e; } .ea-demo .g { background: #28c840; }
.ea-demo .t { margin-left: 6px; font-size: 12px; color: #8b93a1; font-family: var(--ea-mono); }
.ea-demo .body { padding: 14px; }
.ea-q { display: flex; align-items: center; gap: 8px; background: #171a20; border: 1px solid #2b2f38; border-radius: 8px; padding: 10px 12px; }
.ea-q .mag { color: #8b93a1; }
.ea-q .qt { color: #e8ebf0; font-family: var(--ea-mono); font-size: 13px; }
.ea-res { margin-top: 11px; display: flex; flex-direction: column; gap: 7px; }
.ea-row { display: flex; align-items: flex-start; gap: 10px; padding: 9px 11px; background: #14171d; border: 1px solid #23262d; border-radius: 8px; }
.ea-row .sc { font-family: var(--ea-mono); font-size: 11.5px; color: #17a673; background: rgba(23,166,115,.12); border: 1px solid rgba(23,166,115,.3); border-radius: 5px; padding: 2px 6px; white-space: nowrap; }
.ea-row .subj { color: #e8ebf0; font-size: 13px; font-weight: 600; }
.ea-row .frm { color: #8b93a1; font-size: 11.5px; margin-top: 1px; }
.ea-demo .cap { color: #6b7280; font-size: 12px; margin-top: 11px; font-style: italic; }
.ea-steps { display: grid; grid-template-columns: repeat(4, 1fr); gap: 9px; }
.ea-step { background: var(--ea-card); border: 1px solid var(--ea-line); border-radius: 10px; padding: 13px; }
.ea-step .sn { font-family: var(--ea-mono); font-size: 11px; color: var(--ea-accent); font-weight: 700; }
.ea-step .sh { font-weight: 700; font-size: 14px; margin: 5px 0 3px; color: var(--ea-ink); }
.ea-step .sd { font-size: 12px; color: var(--ea-muted); line-height: 1.45; }
.ea-split { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.ea-panel { border: 1px solid var(--ea-line); border-radius: 10px; padding: 15px; }
.ea-panel h4 { margin: 0 0 4px; font-size: 14px; color: var(--ea-ink); }
.ea-panel .badge { font-size: 10.5px; font-weight: 700; letter-spacing: .04em; text-transform: uppercase; padding: 2px 7px; border-radius: 999px; }
.ea-panel .badge.good { background: rgba(23,166,115,.12); color: var(--ea-good); }
.ea-panel .badge.warn { background: rgba(199,119,0,.12); color: var(--ea-warn); }
.ea-panel ul { margin: 10px 0 0; padding-left: 15px; }
.ea-panel li { font-size: 12.5px; color: var(--ea-muted); margin-bottom: 6px; line-height: 1.45; }
.ea-panel li b { color: var(--ea-ink); font-weight: 600; }
.ea-try { background: #eef3ff; border: 1px solid #cfe0ff; border-radius: 10px; padding: 14px 16px; display: flex; align-items: center; justify-content: space-between; gap: 14px; flex-wrap: wrap; }
.ea-try .txt b { display: block; font-size: 14px; color: var(--ea-ink); }
.ea-try .txt span { font-size: 12.5px; color: var(--ea-muted); }
.ea-try .code { font-family: var(--ea-mono); font-size: 14px; background: #0f1115; color: #e8ebf0; padding: 9px 13px; border-radius: 7px; }
@media (max-width: 620px) { .emailai-viz .ea-stats, .emailai-viz .ea-steps { grid-template-columns: repeat(2,1fr); } .emailai-viz .ea-split { grid-template-columns: 1fr; } }
</style>

[![View Code on GitHub](https://img.shields.io/badge/View%20Code-GitHub-black?logo=github)](https://github.com/AshirRashid/schedule-ai)

## The problem

My inbox had become a useful but unsearchable archive. Investment newsletters, job threads, guitar deals, company research I'd saved for later. All there, but keyword search only returns what you already know to search for. And sending email content to an external API wasn't an option. Too much context in there that I didn't want leaving the machine.

The fix was local semantic search: whole-email BGE embeddings, ChromaDB, no external API calls, nothing leaving the device.

<div class="emailai-viz">
<div class="ea-stats">
  <div class="ea-stat"><div class="n good">0.849</div><div class="l">Recall@5 (0.688 for BM25)</div></div>
  <div class="ea-stat"><div class="n">60.7<span style="font-size:13px">ms</span></div><div class="l">Query latency, p50</div></div>
  <div class="ea-stat"><div class="n">13.5<span style="font-size:13px">/s</span></div><div class="l">Ingestion throughput</div></div>
  <div class="ea-stat"><div class="n good">$0</div><div class="l">Marginal cost per query</div></div>
</div>
</div>

## See it work

A query whose words never appear in the target email. Keyword search returns nothing useful; the semantic pipeline puts the right result first.

<div class="emailai-viz">
<div class="ea-demo">
  <div class="top"><span class="dot r"></span><span class="dot y"></span><span class="dot g"></span><span class="t">email-event-finder · localhost:7860</span></div>
  <div class="body">
    <div class="ea-q"><span class="mag">⌕</span><span class="qt">an email about a grant or research funding deadline</span></div>
    <div class="ea-res">
      <div class="ea-row"><span class="sc">0.725</span><div><div class="subj">Re: Grant renewal, documents due next Monday</div><div class="frm">research-office@university.edu</div></div></div>
      <div class="ea-row"><span class="sc">0.702</span><div><div class="subj">Re: Grant renewal, documents due the 30th</div><div class="frm">no-reply@conference-system.org</div></div></div>
      <div class="ea-row"><span class="sc">0.697</span><div><div class="subj">Re: Grant renewal, documents due end of week</div><div class="frm">hr@acme-labs.io</div></div></div>
    </div>
    <div class="cap">The correct email shares only the word "grant" with the query. BM25 scored 0 recall here. Semantic scored a perfect 1.0.</div>
  </div>
</div>
</div>

## What was built

A local ingestion-and-query pipeline: pull inbox email over the Gmail API, embed each message with a locally-run model, store the vectors in ChromaDB, and answer natural-language queries against them. There is no LLM generation step and no external API call anywhere in the pipeline.

<div class="emailai-viz">
<div class="ea-steps">
  <div class="ea-step"><div class="sn">01</div><div class="sh">Fetch</div><div class="sd">Gmail API, read-only OAuth. MIME tree walked recursively, HTML-to-text fallback, bad messages skipped.</div></div>
  <div class="ea-step"><div class="sn">02</div><div class="sh">Clean</div><div class="sd">Strip quoted replies and footer noise. Drop replies (In-Reply-To header), keep the thread opener.</div></div>
  <div class="ea-step"><div class="sn">03</div><div class="sh">Embed</div><div class="sd">Whole email into BGE-base-en-v1.5, 768-dim, L2-normalized, cosine. No chunking.</div></div>
  <div class="ea-step"><div class="sn">04</div><div class="sh">Store</div><div class="sd">Upsert into ChromaDB keyed by Gmail message ID. Re-runs update in place, no duplicates.</div></div>
</div>
</div>

## What it shows

The retrieval quality here is a measured number. An evidence pack in the repo (`eval/`) benchmarks the semantic pipeline against a BM25 bag-of-words baseline on a 60-email synthetic corpus with hand-verified ground truth, over 16 queries at k = 5.

<div class="emailai-viz">
<div class="ea-cmp">
  <div class="ea-metric">
    <div class="mname">Recall@5</div>
    <div class="ea-track"><span class="who">Semantic</span><div class="ea-barwrap"><div class="ea-bar sem" style="width:84.9%">0.849</div></div></div>
    <div class="ea-track"><span class="who">BM25</span><div class="ea-barwrap"><div class="ea-bar bm" style="width:68.8%">0.688</div></div></div>
  </div>
  <div class="ea-metric">
    <div class="mname">Precision@5</div>
    <div class="ea-track"><span class="who">Semantic</span><div class="ea-barwrap"><div class="ea-bar sem" style="width:46.3%">0.463</div></div></div>
    <div class="ea-track"><span class="who">BM25</span><div class="ea-barwrap"><div class="ea-bar bm" style="width:35.0%">0.350</div></div></div>
  </div>
  <div class="ea-metric">
    <div class="mname">MRR</div>
    <div class="ea-track"><span class="who">Semantic</span><div class="ea-barwrap"><div class="ea-bar sem" style="width:78.1%">0.781</div></div></div>
    <div class="ea-track"><span class="who">BM25</span><div class="ea-barwrap"><div class="ea-bar bm" style="width:68.2%">0.682</div></div></div>
  </div>
</div>
</div>

The gap shows up where semantic search is supposed to help: queries whose wording does not match the correct email's wording (the grant-deadline case above is one). Query latency is p50 60.7ms and p95 67.9ms over 48 samples; ingestion runs at roughly 7.7 to 13.5 emails/sec; because the model runs locally, marginal cost is $0.

<div class="emailai-viz">
<div class="ea-split">
  <div class="ea-panel">
    <h4>Where it fails <span class="badge warn">documented</span></h4>
    <ul>
      <li>Most low scores come from a <b>benchmark ceiling</b>: 12 relevant emails per category and k = 5 caps recall at 0.417, so a perfect retriever still scores low here.</li>
      <li>One genuine slip: a date-bound deadline reminder ranked alongside event invites because the query asked for "a specific date".</li>
      <li>BM25's misses are real: literal keyword mismatch, plus a stray "or" match on short off-topic newsletters.</li>
    </ul>
  </div>
  <div class="ea-panel">
    <h4>Security review <span class="badge good">3 checks</span></h4>
    <ul>
      <li><b>OAuth scope read-only</b>, confirmed by test. Cannot modify, send, or delete.</li>
      <li><b>At-rest storage unencrypted</b> in ChromaDB. An accepted tradeoff for a local single-user tool, stated plainly.</li>
      <li><b>Output-injection gap, open:</b> snippets reach the Markdown renderer unescaped. Documented with a recommended fix.</li>
    </ul>
  </div>
</div>
</div>

## Key design decisions

**Whole-email embedding, no chunking.** The earlier version chunked emails into overlapping windows. For an inbox of mostly short messages that hurt more than it helped, splitting a coherent email across vectors and pulling rankings toward fragments. Embedding each email as one document keeps the unit of retrieval equal to the unit a person actually wants back.

**Drop replies, keep the thread opener.** Reply chains quote the original under a different sender, which duplicates content across vectors and biases similarity toward repeated quoted text. Indexing only originals keeps one clean vector per thread.

**Message-ID as the ChromaDB key.** Keying on the Gmail message ID makes ingestion idempotent: re-run on an updated inbox and only genuinely new messages are added.

**A BM25 baseline, on purpose.** "Semantic search is better" is only worth anything against a baseline. Benchmarking against BM25 on a ground-truth corpus turns the claim into a number, and shows the specific vocabulary-gap cases where the embedding model wins and the bag-of-words matcher can't.

## Try it

<div class="emailai-viz">
<div class="ea-try">
  <div class="txt"><b>One command, no Gmail account</b><span>Zero setup, runs against a bundled synthetic corpus.</span></div>
  <div class="code">./demo.sh</div>
</div>
</div>

## What this became

This project established the core pattern that the Agentic Process Discovery system later extended. APD kept the same ingestion approach, BGE embeddings, and ChromaDB storage, then added WhatsApp as a second source, multi-stage query expansion, and an LLM synthesis step that turns retrieved messages into structured process narratives rather than just returning results.

**Technologies:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Python</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">ChromaDB</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">BGE-base-en-v1.5</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">sentence-transformers</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Gmail API</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">BeautifulSoup</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Gradio</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">rank-bm25</span>

**Concepts / Algorithms:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">RAG</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Vector embeddings</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Semantic search</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Whole-document embedding</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">BM25 baseline</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Retrieval evaluation</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Email parsing</span>
