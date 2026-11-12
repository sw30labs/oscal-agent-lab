# 0005. OpenAI through langchain-openai, configured from env/.env, temperature 0, lazy clients

## Status
Accepted, retrospective, inferred 2026-11-12

## Context
Every agent needs an LLM (answers, diff summaries, profile deltas, error explanations) and the RAG needs embeddings.

## Decision
Use `ChatOpenAI(model=OPENAI_MODEL, temperature=0)` (default gpt-4o-mini) and `OpenAIEmbeddings`. Configuration comes from environment variables, with `python-dotenv` loading `.env` at import of `config.py`. Each agent module lazily creates its own LLM client in a module-level `_get_llm()` so imports don't need a key. `.env` is gitignored and `.env.example` documents the variables.

## Consequences
Positive:
- Deterministic-leaning output (temperature 0), and tests can import modules without credentials.

Negative:
- Single provider in code: `.env.example` mentions Anthropic and Ollama alternatives, but nothing reads `ANTHROPIC_API_KEY`, `OLLAMA_BASE_URL` or `OLLAMA_MODEL` (A-004).
- Four duplicate `_get_llm()` singletons.

## Alternatives considered
Anthropic and local Ollama appear as commented options in .env.example but were not implemented.

## Evidence
- src/oscal_agent_lab/config.py (`load_dotenv`, `OPENAI_MODEL`, `OPENAI_EMBED_MODEL`)
- src/oscal_agent_lab/agents/explainer.py, diff.py, profile_builder.py, validator.py (`_get_llm`)
- src/oscal_agent_lab/vectorstore.py (`get_embeddings`)
- .env.example, .gitignore
