---
title: "Agentic Process Discovery"
excerpt: "Reverse-engineers hidden administrative workflows from email and chat history using a two-stage local LLM pipeline with no data leaving the device.<br/>"
collection: portfolio
---

<style>
.apd-viz { --accent: #6a4fd0; --apd-ink: #1a1c20; --apd-muted: #6b7280; --apd-line: #e6e8ec; --apd-card: #f7f8fa; --apd-good: #17a673; --apd-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; margin: 1.2em 0; }
.apd-viz * { box-sizing: border-box; }
.apd-viz .apd-steps { display: grid; grid-template-columns: repeat(4, 1fr); gap: 9px; }
.apd-viz .apd-phase-label { font-family: var(--apd-mono); font-size: 11px; font-weight: 700; letter-spacing: .06em; text-transform: uppercase; color: var(--accent); margin: 0 0 8px; }
.apd-viz .apd-step { position: relative; background: var(--apd-card); border: 1px solid var(--apd-line); border-radius: 10px; padding: 13px; }
.apd-viz .apd-step .sn { font-family: var(--apd-mono); font-size: 11px; color: var(--accent); font-weight: 700; }
.apd-viz .apd-step .sh { font-weight: 700; font-size: 14px; margin: 5px 0 3px; color: var(--apd-ink); }
.apd-viz .apd-step .sd { font-size: 12px; color: var(--apd-muted); line-height: 1.45; }
.apd-viz .apd-phase + .apd-phase { margin-top: 16px; }
.apd-viz .apd-fan { display: grid; grid-template-columns: 200px 1fr; gap: 18px; align-items: center; background: var(--apd-card); border: 1px solid var(--apd-line); border-radius: 10px; padding: 16px; }
.apd-viz .apd-fan .apd-topic { font-family: var(--apd-mono); font-size: 14px; font-weight: 700; color: #fff; background: var(--accent); border-radius: 8px; padding: 12px 14px; text-align: center; }
.apd-viz .apd-fan .apd-topic small { display: block; font-family: inherit; font-weight: 600; font-size: 10.5px; opacity: .85; margin-top: 4px; letter-spacing: .04em; text-transform: uppercase; }
.apd-viz .apd-chips { display: flex; flex-direction: column; gap: 7px; }
.apd-viz .apd-chip { display: flex; align-items: center; gap: 10px; background: #fff; border: 1px solid var(--apd-line); border-left: 3px solid var(--accent); border-radius: 7px; padding: 8px 12px; }
.apd-viz .apd-chip .stage { font-family: var(--apd-mono); font-size: 10.5px; font-weight: 700; letter-spacing: .04em; text-transform: uppercase; color: var(--accent); min-width: 82px; }
.apd-viz .apd-chip .desc { font-size: 12.5px; color: var(--apd-ink); }
.apd-viz .apd-privacy { background: #0f1115; border: 1px solid #23262d; border-radius: 12px; padding: 18px 20px; }
.apd-viz .apd-privacy .head { display: flex; align-items: center; gap: 10px; margin-bottom: 12px; }
.apd-viz .apd-privacy .lock { font-size: 18px; }
.apd-viz .apd-privacy h4 { margin: 0; font-size: 15px; color: #e8ebf0; }
.apd-viz .apd-privacy .badge { font-size: 10px; font-weight: 700; letter-spacing: .05em; text-transform: uppercase; padding: 2px 8px; border-radius: 999px; background: rgba(23,166,115,.16); color: #2fd39a; border: 1px solid rgba(23,166,115,.35); }
.apd-viz .apd-privacy ul { margin: 0; padding: 0; list-style: none; display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
.apd-viz .apd-privacy li { display: flex; align-items: flex-start; gap: 8px; font-size: 12.5px; color: #9aa4b2; line-height: 1.45; }
.apd-viz .apd-privacy li .tick { color: #2fd39a; font-weight: 700; }
.apd-viz .apd-privacy li b { color: #e8ebf0; font-weight: 600; }
.apd-viz .apd-figure { border: 1px solid var(--apd-line); border-radius: 10px; overflow: hidden; background: #fff; }
.apd-viz .apd-figure .apd-fig-title { font-family: var(--apd-mono); font-size: 11px; font-weight: 700; letter-spacing: .05em; text-transform: uppercase; color: var(--accent); padding: 11px 14px; border-bottom: 1px solid var(--apd-line); background: var(--apd-card); }
.apd-viz .apd-figure .apd-fig-body { padding: 14px; }
.apd-viz .apd-figure img { width: 100%; display: block; border-radius: 6px; }
.apd-viz .apd-figure .apd-cap { font-size: 12px; color: var(--apd-muted); font-style: italic; margin-top: 10px; }
.apd-viz .apd-figure + .apd-figure { margin-top: 12px; }
.apd-viz .apd-split { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
.apd-viz .apd-panel { border: 1px solid var(--apd-line); border-radius: 10px; padding: 15px; }
.apd-viz .apd-panel h4 { margin: 0 0 4px; font-size: 14px; color: var(--apd-ink); }
.apd-viz .apd-panel .tag { font-size: 10.5px; font-weight: 700; letter-spacing: .04em; text-transform: uppercase; padding: 2px 7px; border-radius: 999px; background: rgba(106,79,208,.12); color: var(--accent); }
.apd-viz .apd-panel .rule { font-family: var(--apd-mono); font-size: 12.5px; color: var(--apd-ink); margin: 10px 0 4px; font-weight: 700; }
.apd-viz .apd-panel p { font-size: 12.5px; color: var(--apd-muted); margin: 0; line-height: 1.5; }
@media (max-width: 620px) {
  .apd-viz .apd-steps { grid-template-columns: repeat(2,1fr); }
  .apd-viz .apd-fan { grid-template-columns: 1fr; }
  .apd-viz .apd-privacy ul { grid-template-columns: 1fr; }
  .apd-viz .apd-split { grid-template-columns: 1fr; }
}
</style>

[![View Code on GitHub](https://img.shields.io/badge/View%20Code-GitHub-black?logo=github)](https://github.com/AshirRashid/process_discovery)

## The problem

Most recurring workflows inside an organization are never written down. Expense approvals, project handoffs, scheduling chains exist as informal patterns spread across email threads and chat histories. Traditional process mining tools need structured event logs. They can't read an inbox.

So when someone asks "how does our expense approval process actually work?", the answer is usually "check with whoever's been doing it the longest."

## What it does

You type a process topic in plain language (e.g. "expense approvals", "meeting scheduling", "project handoffs") and the system retrieves relevant communications and produces a structured breakdown: ordered steps, what triggers each one, who owns it, and where things tend to get stuck.

The design is shaped around the assumption that the communications being analyzed are sensitive. Everything is run locally. No email content, message history, or generated output is sent to an external API.

<div class="apd-viz">
<div class="apd-phase">
  <p class="apd-phase-label">Indexing phase</p>
  <div class="apd-steps">
    <div class="apd-step"><div class="sn">01</div><div class="sh">Fetch</div><div class="sd">Pull Gmail over the Gmail API, read-only OAuth scope.</div></div>
    <div class="apd-step"><div class="sn">02</div><div class="sh">Filter</div><div class="sd">Drop reply chains, index the original messages only.</div></div>
    <div class="apd-step"><div class="sn">03</div><div class="sh">Window</div><div class="sd">Parse WhatsApp into 30-minute conversation windows.</div></div>
    <div class="apd-step"><div class="sn">04</div><div class="sh">Store</div><div class="sd">Load into separate ChromaDB collections, BGE-base-en-v1.5, 768-dim cosine.</div></div>
  </div>
</div>
<div class="apd-phase">
  <p class="apd-phase-label">Query and inference phase</p>
  <div class="apd-steps">
    <div class="apd-step"><div class="sn">01</div><div class="sh">Query</div><div class="sd">A single natural-language process topic from the user.</div></div>
    <div class="apd-step"><div class="sn">02</div><div class="sh">Expand</div><div class="sd">Llama 3.2 via Ollama rewrites it into 4 to 5 stage-targeted queries.</div></div>
    <div class="apd-step"><div class="sn">03</div><div class="sh">Retrieve</div><div class="sd">Pull from ChromaDB, then dedupe by subject and sender.</div></div>
    <div class="apd-step"><div class="sn">04</div><div class="sh">Synthesize</div><div class="sd">A second LLM call turns the evidence into a structured narrative.</div></div>
    <div class="apd-step"><div class="sn">05</div><div class="sh">Anonymize</div><div class="sd">PII anonymization, spaCy NER plus regex, before output.</div></div>
    <div class="apd-step"><div class="sn">06</div><div class="sh">Present</div><div class="sd">Render the result in the Gradio UI.</div></div>
  </div>
</div>
</div>

## Architecture

<div class="apd-viz">
<div class="apd-figure">
  <div class="apd-fig-title">Indexing flow</div>
  <div class="apd-fig-body">
    <img src="/images/apd_indexing_flow.png" alt="Indexing Flow Architecture">
    <div class="apd-cap">How email and WhatsApp are fetched, filtered, and loaded into ChromaDB collections.</div>
  </div>
</div>
<div class="apd-figure">
  <div class="apd-fig-title">Query and inference flow</div>
  <div class="apd-fig-body">
    <img src="/images/apd_query_flow.png" alt="Query Flow Architecture">
    <div class="apd-cap">How one topic expands into stage-targeted queries, retrieves evidence, and becomes a narrative.</div>
  </div>
</div>
</div>

## Key design decisions

**Two-stage retrieval instead of a single query.** One query against a process topic only surfaces messages that literally match the topic phrase. Query expansion instead rewrites the topic into stage-targeted searches, each phrased as a description of the kind of message that would represent that stage, so retrieval covers the full lifecycle even when those stages are never worded like the topic itself.

<div class="apd-viz">
<div class="apd-fan">
  <div class="apd-topic">"expense approvals"<small>one topic</small></div>
  <div class="apd-chips">
    <div class="apd-chip"><span class="stage">Initiation</span><span class="desc">the message that first requests an expense sign-off</span></div>
    <div class="apd-chip"><span class="stage">Delegation</span><span class="desc">handing the approval to the person who owns it</span></div>
    <div class="apd-chip"><span class="stage">Tracking</span><span class="desc">chasing where an in-flight approval currently sits</span></div>
    <div class="apd-chip"><span class="stage">Stalling</span><span class="desc">the point where an approval goes quiet or gets stuck</span></div>
    <div class="apd-chip"><span class="stage">Resolution</span><span class="desc">the message that closes the loop and confirms approval</span></div>
  </div>
</div>
</div>

**Reply filtering before embedding.** Reply chains repeat the same content under multiple senders and pull cosine similarity rankings toward noise. Only original messages are indexed; replies are dropped at ingestion.

**Source-aware chunking.** The index unit differs by source because email and chat carry context differently.

<div class="apd-viz">
<div class="apd-split">
  <div class="apd-panel">
    <h4>Email <span class="tag">per message</span></h4>
    <div class="rule">index unit = 1 message</div>
    <p>Each email is indexed on its own. A message is already a self-contained unit, so one vector maps to one message.</p>
  </div>
  <div class="apd-panel">
    <h4>WhatsApp <span class="tag">30-min window</span></h4>
    <div class="rule">index unit = 30-min window</div>
    <p>Chats are windowed on a configurable gap threshold (default 30 minutes), because a single chat message out of context often means nothing.</p>
  </div>
</div>
</div>

**Local by design.** Everything runs on the machine: Ollama and Llama 3.2 for generation, ChromaDB for storage, spaCy NER plus regex for PII masking before anything is shown. The tradeoff is deliberate, since local setup takes longer than an API call, but sensitive workflow data never leaves the device.

<div class="apd-viz">
<div class="apd-privacy">
  <div class="head">
    <span class="lock">🔒</span>
    <h4>Local by design, nothing leaves the device</h4>
    <span class="badge">Private</span>
  </div>
  <ul>
    <li><span class="tick">✓</span><span><b>Read-only OAuth.</b> Gmail access cannot modify, send, or delete.</span></li>
    <li><span class="tick">✓</span><span><b>On-device inference.</b> Llama 3.2 runs locally via Ollama, no external API call.</span></li>
    <li><span class="tick">✓</span><span><b>Local vector store.</b> ChromaDB collections stay on the machine.</span></li>
    <li><span class="tick">✓</span><span><b>PII anonymization.</b> spaCy NER plus regex masks names and phone numbers before output.</span></li>
  </ul>
</div>
</div>

## What this means for clients

The same architecture works for any organization with undocumented processes buried in communication data: Slack exports, customer email histories, support threads, vendor correspondence. The system maps process structure without requiring any prior instrumentation or schema definition.

If a workflow currently lives only in people's heads and inboxes, this reconstructs it from the evidence that's already there.

**Technologies:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Python</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">ChromaDB</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Ollama</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Llama 3.2</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">BGE-base-en-v1.5</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Gradio</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">spaCy</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Gmail API</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Docker</span>

**Concepts / Algorithms:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Agentic Process Discovery</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">RAG</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Query Expansion</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Vector Embeddings</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Semantic Search</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Named Entity Recognition (NER)</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">LLM Inference</span>
