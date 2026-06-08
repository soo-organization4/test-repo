# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A Streamlit-based RAG (Retrieval-Augmented Generation) demo app that uses MariaDB as a vector store and OpenAI for embeddings and LLM responses. The vector store is implemented from scratch (not using `langchain-mariadb`) — cosine similarity is computed in Python after fetching all rows.

## Running the App

```bash
streamlit run app/main.py
```

## Environment Variables 설정파일 절대 수정 금지
- ../.env  ( 프로젝트 상위 폴더에 존재함 )

```
OPENAI_API_KEY=...
DB_HOST=...
DB_PORT=...
DB_USER=...
DB_PASS=...
DB_NAME=...
DB_TABLE=ai_vector_store   # optional, defaults to ai_vector_store
```

## Architecture

```
app/
  config.py             — loads .env into a Settings singleton
  mariadb_vector_store.py — custom vector store: stores embeddings as JSON in MariaDB,
                            computes cosine similarity in Python (no native vector type)
  service.py            — VectorService wraps the store; exposes search1, search2,
                          search_with_scores, native_query
  rag_service.py        — RagService: retrieves top-k docs then calls gpt-4o-mini
  sample_docs.py        — hardcoded Korean sci-fi sample documents for seeding
  main.py               — Streamlit UI wiring all services together
```

`MariaDBVectorStore` is **not** a LangChain `VectorStore` subclass — it implements the same interface (`add_documents`, `similarity_search`, `similarity_search_with_score`) but manually. On every search it fetches all rows and ranks by cosine similarity in Python; this is intentional for educational simplicity.

The DB table schema (`ai_vector_store`):
- `id` BIGINT AUTO_INCREMENT
- `text` TEXT
- `embedding` JSON  (OpenAI `text-embedding-3-small` vector stored as a JSON array)
- `metadata` JSON
- `created_at` TIMESTAMP

`search2` hard-codes a metadata filter for `author=john` AND `article_type=blog`.
