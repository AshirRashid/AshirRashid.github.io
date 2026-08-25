---
title: "Gmail RAG Pipeline"
excerpt: "Local semantic search over Gmail using whole-email BGE embeddings and ChromaDB, with no email content leaving the device. Benchmarked against a BM25 baseline. Foundation for the Agentic Process Discovery system.<br/>"
collection: portfolio
---

[![View Code on GitHub](https://img.shields.io/badge/View%20Code-GitHub-black?logo=github)](https://github.com/AshirRashid/schedule-ai)

## The problem

My inbox had become a useful but unsearchable archive. Investment newsletters, job threads, guitar deals, company research I'd saved for later. All there, but keyword search only returns what you already know to search for. And sending email content to an external API wasn't an option. Too much context in there that I didn't want leaving the machine.

The fix was local semantic search: BGE embeddings, ChromaDB, no external API calls, nothing leaving the device.

## What was built

A local ingestion-and-query pipeline that pulls inbox email over the Gmail API, embeds each message with a locally-run model, stores the vectors in ChromaDB, and answers natural-language queries against them. You type a query ("job offer from last month", "guitar deals", "an email confirming a dentist appointment") and get back the most relevant emails with their source metadata: sender, subject, date.

The pipeline runs in four steps:

1. **Fetch.** Emails are pulled via the Gmail API (OAuth 2.0, read-only scope). The MIME tree is walked recursively to extract the plain-text part, falling back to BeautifulSoup HTML-to-text conversion when no plain-text part exists. Individual message failures are skipped so one bad message doesn't abort the run.

2. **Clean.** EmailReplyParser strips quoted reply chains, and unsubscribe / "view this email in your browser" footer noise is cut, before anything reaches the vector store. Replies (messages carrying an `In-Reply-To` header) are dropped entirely, so each thread is represented once by its opening message.

3. **Embed.** Each cleaned email is embedded **as a single whole-email document** with BGE-base-en-v1.5 (768-dimensional, L2-normalized, cosine space). No chunking. The messages in this inbox are short enough that splitting them adds retrieval noise without adding recall, so the whole email is the unit of retrieval.

4. **Store.** Documents are upserted into ChromaDB keyed by the Gmail message ID, so re-running on the same inbox updates in place rather than duplicating.

The model runs locally through `sentence-transformers`; there is no LLM generation step and no external API call anywhere in the pipeline.

## What it shows

The retrieval quality is measured, not asserted. An evidence pack in the repo (`eval/`) benchmarks the semantic pipeline against a BM25 bag-of-words baseline on a 60-email synthetic corpus with hand-verified ground truth, over 16 natural-language queries at k = 5:

| Method | Precision@k | Recall@k | MRR |
|---|---|---|---|
| semantic | 0.463 | 0.849 | 0.781 |
| bm25 | 0.350 | 0.688 | 0.682 |

The gap shows up exactly where semantic search is supposed to help. On a query like "an email about a grant or research funding deadline", the correct email ("Grant renewal, documents due next Monday") shares only the single word "grant" with the query and never says "research", "funding", or "deadline". BM25 scored 0 recall and surfaced unrelated newsletters that literally repeat the token "research"; the semantic pipeline scored a perfect 1.0.

**Latency and cost.** Query latency is p50 60.7ms / p95 67.9ms over 48 samples; ingestion runs at roughly 7.7 to 13.5 emails/sec. Because the embedding model runs locally, the marginal cost per query or per ingest run is $0, with no external API in the loop.

**Where it fails, stated honestly.** The failure analysis (`eval/failures.md`) traces the lowest-recall queries per method. Most of the semantic pipeline's low scores are a benchmark ceiling artifact (12 near-duplicate relevant emails per category evaluated at k = 5 caps recall at 5/12 = 0.417), not real misses; the one genuine slip is a date-bound deadline reminder ranking alongside event invites because the query asked for something "with a specific date". BM25's failures are real: literal keyword mismatch, plus a lack of stopword filtering that let short off-topic newsletters outrank correct emails on a single stray match to the word "or".

**Security, including an unresolved gap.** A focused review (`eval/security_review.md`) checked three things against the real code: the OAuth scope is read-only (confirmed by test, so the tool cannot modify, send, or delete anything), ChromaDB stores email text unencrypted at rest (an accepted tradeoff for a local single-user tool, stated plainly), and retrieved snippets reach the Gradio Markdown renderer unescaped. That last one is a real, documented output-injection gap rather than a theoretical one: a crafted email body survives intact to the render boundary. Blast radius is limited to the operator's own machine, and it is left open with a recommended fix rather than papered over.

## Key design decisions

**Whole-email embedding, no chunking.** The earlier version of this pipeline chunked emails into overlapping windows. For an inbox of mostly short messages that hurt more than it helped: it split a single coherent email across vectors and pulled rankings toward fragments. Embedding each email as one document keeps the unit of retrieval equal to the unit a person actually wants back.

**Drop replies, keep the thread opener.** Reply chains quote the original under a different sender. Indexing them duplicates content across vectors and biases similarity toward repeated quoted text instead of unique signal. Detecting the `In-Reply-To` header and indexing only originals keeps one clean vector per thread.

**Message-ID as the ChromaDB key.** Keying on the Gmail message ID makes ingestion idempotent: re-run on an updated inbox and only genuinely new messages are added, existing ones upsert in place.

**A BM25 baseline, on purpose.** Claiming "semantic search is better" is only worth anything against a baseline. Benchmarking against BM25 on a ground-truth corpus turns the claim into a number and, more usefully, shows the specific vocabulary-gap cases where the embedding model wins and the bag-of-words matcher can't.

## What this became

This project established the core pattern that the Agentic Process Discovery system later extended. APD kept the same ingestion approach, BGE embeddings, and ChromaDB storage, then added WhatsApp as a second source, multi-stage query expansion, and an LLM synthesis step that turns retrieved messages into structured process narratives rather than just returning results.

## Try it

No Gmail account and zero setup, against a bundled synthetic corpus:

```bash
./demo.sh
```

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
