---
title: "Gmail RAG Pipeline"
excerpt: "Building a Retrieval-Augmented Generation Pipeline from Gmail Data<br/>"
collection: portfolio
---

[![View Code on GitHub](https://img.shields.io/badge/View%20Code-GitHub-black?logo=github)](https://github.com/AshirRashid/schedule-ai)


## Architecture Diagram
<img src="/images/gmail_rag_architecture.png" alt="RAG Architecture" style="width:100%">

## Introduction
This project creates an end-to-end Retrieval-Augmented Generation (RAG) pipeline for Gmail data, enabling semantic search and retrieval over email content. The pipeline fetches and cleans emails, chunks them into searchable segments, embeds the chunks, and stores them in a vector database for efficient querying.

## Workflow
The workflow has six main steps:
1. **Data Ingestion**: Fetch emails from Gmail using the Gmail API with OAuth 2.0 authentication and Base64 decoding.
2. **Data Cleaning**: Process email text to remove HTML and CSS, normalize content, and keep original metadata intact.
3. **Text Chunking**: Split cleaned content into overlapping character chunks to improve retrieval context.
4. **Vector Embedding and Storage**: Create embeddings using the BGE-base-en model for high-quality 768-dimensional embeddings.
5. **Vector Database**: Store embeddings in ChromaDB (a vector database) for fast and accurate similarity search.
6. **Query Interface**: Search stored embeddings to find the most relevant email chunks according to a given prompt.

The system automates the flow from Gmail inbox to vectorized semantic search and is designed to be modular and easy to extend.

**Technologies:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Python</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Gmail API</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">OAuth 2.0</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">LangChain</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Ollama</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Llama3</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">ChromaDB</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Hugging Face Transformers</span>  

**Concepts / Algorithms:**  
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">RAG</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Vector embeddings</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Semantic search</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Text chunking</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Email parsing</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">LLM inference</span>
<span style="background:#f2f2f2; padding:4px 8px; border-radius:6px; margin:2px; display:inline-block;">Embedding-based retrieval</span>
