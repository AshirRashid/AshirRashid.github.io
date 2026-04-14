---
title: "Agentic Process Discovery (v2)"
excerpt: "Reconstructs hidden administrative workflows from unstructured communications using local embeddings and LLMs.<br/>"
collection: portfolio
---

[![View Code on GitHub](https://img.shields.io/badge/View%20Code-GitHub-black?logo=github)](https://github.com/AshirRashid/process_discovery)


## Architecture Diagram

### Indexing Flow
<img src="/images/apd_indexing_flow.png" alt="Indexing Flow Architecture" style="width:100%">

### Query / Inference Flow
<img src="/images/apd_query_flow.png" alt="Query Flow Architecture" style="width:100%">

## Introduction
This system implements **Agentic Process Discovery (APD)** — a technique for reverse-engineering recurring administrative processes (e.g., "expense approvals", "meeting scheduling", "project handoffs") from unstructured communication data like Gmail and WhatsApp. The user provides a loose natural-language topic, and the system leverages a local LLM and vector database to synthesize a structured process narrative decomposed into ordered, discrete subtasks. PII (people names, phone numbers) is anonymised at display time using spaCy NER and regex.

## Workflow
The workflow has two main phases:

1. **Indexing Phase**:
    - **Data Ingestion**: Fetch emails from Gmail using the Gmail API (filtering replies) and parse WhatsApp chat exports into conversation windows.
    - **Vector Embedding**: Create L2-normalised embeddings using the `BAAI/bge-base-en-v1.5` model.
    - **Vector Database**: Store documents in ChromaDB in separate collections (`emails` and `whatsapp`).

2. **Query & Inference Phase**:
    - **Query Generation**: Use a local LLM (`llama3.2` via Ollama) to generate 4-5 stage-targeted retrieval queries representing different parts of a workflow based on the user's topic.
    - **Evidence Retrieval**: Search ChromaDB per query to retrieve top-N results and deduplicate them by subject and sender.
    - **Narrative Generation**: Use the LLM to synthesize a structured process narrative with ordered subtasks, triggers, owners, outputs, and friction points.
    - **PII Anonymization**: Apply spaCy NER and regex to mask names and phone numbers before displaying the narrative and evidence in the Gradio UI.

The system strictly utilizes local inference and vector storage components (aside from Gmail ingestion) ensuring that sensitive administrative workflows and correspondence remain private.

**Technologies:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Python</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">ChromaDB</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Ollama</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Llama3.2</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Gradio</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Hugging Face Transformers</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">spaCy</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Gmail API</span>

**Concepts / Algorithms:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Agentic Process Discovery</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Vector Embeddings</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">RAG</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Named Entity Recognition (NER)</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Semantic Search</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">LLM Inference</span>
