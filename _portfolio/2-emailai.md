---
title: "Gmail RAG Pipeline"
excerpt: "Local semantic search over Gmail using BGE embeddings and ChromaDB, with no email content leaving the device. Foundation for the Agentic Process Discovery system.<br/>"
collection: portfolio
---

[![View Code on GitHub](https://img.shields.io/badge/View%20Code-GitHub-black?logo=github)](https://github.com/AshirRashid/schedule-ai)

## The problem

My inbox had become a useful but unsearchable archive. Investment newsletters, job threads, guitar deals, company research I'd saved for later. All there, but keyword search only returns what you already know to search for. And sending email content to an external API wasn't an option. Too much context in there that I didn't want leaving the machine.

The fix was local semantic search: BGE embeddings, ChromaDB, no external API calls.

## What it does

You type a query ("job offer from last month", "guitar deals", "Sequoia portfolio companies") and get back the most relevant email chunks with source metadata: sender, subject, date. Everything runs on the local machine.

## Architecture

### Ingestion pipeline

<img src="/images/gmail_rag_architecture.png" alt="RAG Architecture" style="width:100%">

Five stages, each implemented as a LangChain Runnable:

1. **Gmail fetch.** Emails are pulled via the Gmail API (OAuth 2.0, read-only scope). The MIME tree is walked to extract plain text, falling back to BeautifulSoup HTML parsing if no plain-text part exists.

2. **Cleaning.** EmailReplyParser strips quoted reply chains before anything hits the vector store. Unsubscribe footers and signature markers are cut. Only the original message content gets indexed.

3. **Chunking.** Cleaned text is split into 1,500-character chunks with 200-character overlap using RecursiveCharacterTextSplitter. Each chunk is prefixed with the email's headers (From, Subject, Date) so the retriever has source context when a chunk comes back in isolation.

4. **Embedding.** BGE-base-en-v1.5 (768-dimensional, L2-normalized). The same model carries over into the APD system that came out of this work.

5. **Storage.** Chunks go to ChromaDB with stable IDs: `{source_id}-{chunk_index}`. Re-running on the same inbox doesn't add duplicates.

## Key design decisions

**Reply stripping before embedding.** Reply chains quote the original message under a different sender. Without stripping them, the same content shows up in multiple vectors and pulls similarity rankings toward repeated text instead of unique signal.

**Header prepended to each chunk.** A retrieved chunk rarely comes with the full email around it. Prepending sender, subject, and date to every chunk means results are readable without a separate metadata lookup.

**Stable chunk IDs.** IDs built from source message ID plus chunk index make the pipeline idempotent. Run it again on an updated inbox and only new emails get added.

## What this became

This project established the core pattern that the Agentic Process Discovery system later extended. APD kept the same ingestion approach, BGE embeddings, and ChromaDB storage, then added WhatsApp as a second source, multi-stage query expansion, and an LLM synthesis step that turns retrieved messages into structured process narratives rather than just returning chunks.

**Technologies:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Python</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">LangChain</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">ChromaDB</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">BGE-base-en-v1.5</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Gmail API</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">BeautifulSoup</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Docker</span>

**Concepts / Algorithms:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">RAG</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Vector embeddings</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Semantic search</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Text chunking</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Email parsing</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Embedding-based retrieval</span>
