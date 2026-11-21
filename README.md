# Cross-Lingual RAG Assistant

A retrieval-augmented generation (RAG) system that answers **English questions from a corpus of French football news articles**, built step by step from a simple baseline to a multi-query pipeline with chat memory.

Assignment for Text Retrieval and Mining (University of Amsterdam), in collaboration with Xanthos Kekkos, Ayush Mudunuru, Jack Martin, and Pranav Krishnan.

## What it does

Ask a question in English, for example *"How did Neymar do at Paris?"*. The system searches the French documents, reads the most relevant passages, and answers in English, with links to its sources.

## How it was built

The notebook contains nine versions in four stages. Each one adds an idea to the last.

| Stage | Versions | Main idea |
|---|---|---|
| Naive RAG | 1.0, 2.0 | Translate the question into French, retrieve, answer. 1.0 answers in English directly; 2.0 answers in French and translates back. |
| Rewrite-Retrieve-Read | 1.0, 2.0, plus compression | The model rewrites the question into a better French search query. The compression version also splits text into chunks, removes near-duplicates, and drops chunks below a 0.65 similarity threshold. |
| Conversational RAG | chain, chain with compression, agent | Retrieval becomes a tool the model chooses to call, with memory across turns. The agent version can retrieve several times in one turn. |
| Most advanced RAG | final | Multiple queries, query verification, hypothetical-answer search (HyDE), adaptive chunking, and filtering with a small local embedding model. |

### Final pipeline

1. The model decides whether the question needs retrieval.
2. It writes three related queries.
3. A yes/no check keeps only relevant, clear queries.
4. Each query is translated into French, and a hypothetical French answer is generated (HyDE).
5. Results from both are merged and ranked.
6. Documents are split into chunks (smaller for longer documents), near-duplicates are removed, and low-similarity chunks are dropped.
7. GPT-4o answers in English and says so when the context is insufficient or it is not confident.

## Tech stack

Python, LangChain, LangGraph, GPT-4o and `text-embedding-3-small` (hosted), Milvus (vector database), `sentence-transformers/all-MiniLM-L6-v2` (local filtering).


## Notes

- The notebook needs the course's infrastructure, so it **cannot be run without access to it**:
- The French document collection is not included.
- The environment setup and endpoint tests at the start were provided by the course; the RAG implementations are our own.
