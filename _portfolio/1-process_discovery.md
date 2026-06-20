---
title: "Agentic Process Discovery"
excerpt: "Reverse-engineers hidden administrative workflows from email and chat history using a two-stage local LLM pipeline with no data leaving the device.<br/>"
collection: portfolio
---

[![View Code on GitHub](https://img.shields.io/badge/View%20Code-GitHub-black?logo=github)](https://github.com/AshirRashid/process_discovery)

## The problem

Most recurring workflows inside an organization are never written down. Expense approvals, project handoffs, scheduling chains exist as informal patterns spread across email threads and chat histories. Traditional process mining tools need structured event logs. They can't read an inbox.

So when someone asks "how does our expense approval process actually work?", the answer is usually "check with whoever's been doing it the longest."

## What it does

You type a process topic in plain language (e.g. "expense approvals", "meeting scheduling", "project handoffs") and the system retrieves relevant communications and produces a structured breakdown: ordered steps, what triggers each one, who owns it, and where things tend to get stuck.

The design is shaped around the assumption that the communications being analyzed are sensitive. Everything is run locally. No email content, message history, or generated output is sent to an external API.

## Architecture

### Indexing flow

<img src="/images/apd_indexing_flow.png" alt="Indexing Flow Architecture" style="width:100%">

### Query and inference flow

<img src="/images/apd_query_flow.png" alt="Query Flow Architecture" style="width:100%">

**Indexing phase:**

Gmail is fetched via the Gmail API (OAuth 2.0, read-only scope). Reply chains are filtered before embedding and only original messages are indexed, which keeps the vector space from filling up with repeated content under different senders. WhatsApp exports are parsed into conversation windows using a 30-minute gap threshold so retrieved chunks preserve context rather than isolating single messages. Both sources are then loaded into separate ChromaDB collections using BGE-base-en-v1.5 embeddings (768-dimensional, cosine-normalized).

**Query and inference phase:**

A single user query triggers two LLM calls:

1. **Query expansion.** Llama 3.2 (via Ollama, running on-device) rewrites the topic into 4-5 stage-targeted retrieval queries. Each targets a different part of the process: initiation, delegation, tracking, stalling, resolution. A topic like "expense approvals" becomes five distinct searches, each phrased as a description of the kind of message that would represent that stage.

2. **Narrative synthesis.** Results from all queries are pulled from ChromaDB and deduplicated by subject and sender. The full evidence set goes to a second LLM call that produces a structured process narrative: numbered steps, triggers, owners, expected outputs, and friction points.

PII anonymization (spaCy NER + regex) runs on everything before it reaches the Gradio UI. Names and phone numbers get replaced with consistent labels throughout the session.

## Key design decisions

**Two-stage retrieval instead of a single query.** One query against a process topic only surfaces messages that literally match the topic phrase. The query expansion step retrieves evidence across the full lifecycle from the initial request to the approval, the follow-up, and the resolution. These separate steps are often not phrased like the topic itself.

**Reply filtering before embedding.** Reply chains repeat the same content under multiple senders and pull cosine similarity rankings toward noise. Only original messages are indexed; replies are dropped at ingestion.

**Source-aware chunking.** Emails are indexed as individual messages. WhatsApp conversations are windowed with a configurable gap threshold (default 30 minutes) because a single message out of context often means nothing.

**Local inference.** Ollama + Llama 3.2 for generation, ChromaDB for storage. The setup takes longer than an API call, but sensitive workflow data stays on the machine.

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
