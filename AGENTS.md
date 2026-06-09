# Repository Guidelines

## Project Structure & Module Organization

This is a small Python Streamlit RAG demo. Application code lives in `app/`. Key modules are `app/main.py` for the Streamlit UI, `app/config.py` for environment-based settings, `app/mariadb_vector_store.py` for the custom MariaDB-backed vector store, `app/service.py` for vector search operations, `app/rag_service.py` for answer generation, and `app/sample_docs.py` for seed documents. The repository currently has no dedicated `tests/` directory. Ignore generated `__pycache__/` folders in reviews and commits.

## Build, Test, and Development Commands

Create and activate a virtual environment before installing dependencies:

```bash
python -m venv .venv
pip install -r requirements.txt
streamlit run app/main.py
```

`pip install -r requirements.txt` installs Streamlit, OpenAI, LangChain, and MariaDB client dependencies. `streamlit run app/main.py` starts the local UI. There is no configured build step or test runner yet; add one with the first test suite.

## Coding Style & Naming Conventions

Use Python 3 with 4-space indentation. Keep module and file names in `snake_case`, classes in `PascalCase`, and constants in `UPPER_SNAKE_CASE` as seen in `app/service.py`. Prefer explicit imports from `app.*` modules. Keep Streamlit UI code in `main.py`; place database, retrieval, and LLM logic in service modules. When editing Korean UI labels or comments, preserve UTF-8 encoding.

## Testing Guidelines

No tests are present today. For new behavior, add `pytest` tests under `tests/` using names like `test_rag_service.py` and `test_mariadb_vector_store.py`. Mock OpenAI calls and MariaDB connections in unit tests; reserve live database checks for clearly named integration tests. Once tests exist, document and use a single command such as `pytest`.

## Commit & Pull Request Guidelines

Recent history uses short Korean-language commit subjects and GitHub merge commits, for example `Merge pull request #4 ...`. Keep commit messages concise and outcome-focused. Pull requests should describe the user-visible change, list any environment or database setup needed, link related issues, and include screenshots for Streamlit UI changes.

## Security & Configuration Tips

Do not commit secrets. Configure runtime values in `.env` outside this repository when possible, including `OPENAI_API_KEY`, `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASS`, `DB_NAME`, and optional `DB_TABLE`. Treat `native_query()` and table-name changes carefully because they directly affect MariaDB access.
