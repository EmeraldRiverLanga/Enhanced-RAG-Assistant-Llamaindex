# Enhanced RAG Assistant with LlamaIndex

## Overview

An advanced Retrieval-Augmented Generation (RAG) assistant built with LlamaIndex. The system loads a collection of documents, indexes them as vectors, and answers questions using three query types — fact, comparison, and summary — each with a dedicated prompt template and optimized retrieval settings.

## Technologies Used

- **Python** — core language
- **LlamaIndex** — RAG framework for document indexing and retrieval
- **OpenAILike (OpenRouter / GPT-3.5-turbo)** — LLM backend
- **OpenAI Embeddings (text-embedding-ada-002)** — document vectorization
- **pypdf** — PDF document loading
- **Google Colab** — development environment

## Setup

### Install dependencies

```bash
pip install llama-index llama-index-llms-openai-like llama-index-embeddings-openai llama-index-readers-file pypdf
```

### Run in Google Colab

1. Open the notebook in Google Colab
2. Add your OpenRouter API key to Colab Secrets as `OPENROUTER_API_KEY`
3. Upload your documents to `/content/`
4. Run all cells in order

## Project Structure

The notebook is divided into 7 stages:

### Stage 1 — Library Installation
- Install LlamaIndex and all required dependencies

### Stage 2 — API Connection
- Connect LLM (`gpt-3.5-turbo`) and embedding model (`text-embedding-ada-002`) via OpenRouter
- Configure global LlamaIndex settings

### Stage 3 — Document Loading & Indexing
- Load PDF and TXT documents using `SimpleDirectoryReader`
- Build `VectorStoreIndex` from loaded documents

### Stage 4 — `smart_query()` Function
- `detect_query_type()` — detects query type from keywords
- `answer_found()` — checks if response contains "not found" phrases
- `smart_query()` — selects the appropriate prompt and `top_k` value, queries the index, and returns a structured response with sources

### Stage 5 — Test Queries
- Fact question: Canadian budget deficit
- Comparison question: Canadian budget vs AutoGen Studio document
- Summary question: Paul Graham essay

### Stage 6 — `top_k` Experiment
- Tests `top_k` values of 1, 3, 5, and 7 on the same question
- Measures retrieved fragments and unique documents per setting

### Stage 7 — System Description
- Documents the data flow, query types, error handling, and experiment conclusions

## Query Types

| Type | Trigger keywords | top_k | Description |
|---|---|---|---|
| `fact` | *(default)* | 3 | Precise, concise factual answer |
| `comparison` | compare, difference, contrast, versus, vs | 6 | Compares information across documents |
| `summary` | summarize, summary, main points, overview | 3 | Structured summary of main points |

## Documents Used

| File | Description |
|---|---|
| `2023_canadian_budget.pdf` | Canadian federal budget 2023 |
| `ag-studio.pdf` | AutoGen Studio research paper |
| `paul_graham_essay.txt` | Paul Graham essay on programming and startups |

## top_k Experiment Results

| top_k | Fragments retrieved | Unique documents |
|---|---|---|
| 1 | 1 | 1 |
| 3 | 3 | 1 |
| 5 | 5 | 2 |
| 7 | 7 | 2 |

Conclusion: `top_k=3` is sufficient for fact questions. Comparison questions require `top_k=6` to retrieve fragments from multiple documents.

## Key Concepts Demonstrated

- **RAG pipeline** — document loading, vectorization, indexing, and retrieval
- **Query type detection** — automatic prompt selection based on question keywords
- **Prompt engineering** — separate prompt templates for fact, comparison, and summary queries
- **Retrieval tuning** — `top_k` parameter optimization for different query types
- **Error handling** — detecting unanswered queries and suggesting rephrasing
