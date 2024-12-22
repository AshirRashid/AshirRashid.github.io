---
title: "Faster RAG"
excerpt: "Making RAG Faster Using Information Extraction<br/>"
collection: portfolio
---

RAG systems rely heavily on comparing documents. If we can find a way to highlight the relevant information, we can make a RAG system more accurate and precise. This is exactly what I tried to do with this project. I used information extraction techniques (NER and Relation Extraction) to extract and highlight relevant information from documents.

For the implementation, I used Elasticsearch's flexible querying mechanism and BM25 retriever. The query mechanism allowed me to integrate my custom queries and features in the RAG system seamlessly.

I based my research on this codebase: [GitHub](https://github.com/starsuzi/Adaptive-RAG)


Technologies: Python, Elasticsearch, NLTK, spaCy, Hugging Face Transformers, PyTorch, NumPy, Pandas, Jsonnet, Docker

Concepts/Algorithms: LLM, RAG, bag-of-words embedding, BM25 ranking, Named Entity Recognition (NER), Relation Extraction, Entity Linking, Information Retrieval (IR), Contextual Embedding, Transformer Fine-tuning
