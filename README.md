# AutoRAG — Fully Local Version (Ollama, No API Key)

A modified version of the AutoRAG tutorial from awesome-llm-apps that runs
100% on your own machine — no OpenAI key, no Docker, no PostgreSQL, and
nothing sent over the internet except optional DuckDuckGo web search.

## What changed from the original tutorial
| Original                        | This version                         |
|----------------------------------|----------------------------------------|
| GPT-4o-mini (OpenAI, needs a key)| Llama 3.1 or Mistral, via local Ollama |
| OpenAI embeddings (needs a key)  | nomic-embed-text, via local Ollama     |
| PgVector (Postgres, via Docker)  | ChromaDB (local folder on disk)        |
| PostgresAgentStorage             | SqliteAgentStorage (local file)        |

This lines up with the "LlamaIndex + open-source LLM (Llama/Mistral)" stack
from your project plan — everything here runs the actual open-source models
on your own hardware.

## One-time setup

1. **Install Ollama** (the local model runner): https://ollama.com/download
   — available for Mac, Windows, and Linux.

2. **Pull the models you need** (run these once in a terminal):
   ```bash
   ollama pull llama3.1          # or: ollama pull mistral
   ollama pull nomic-embed-text  # used for embeddings
   ```
   Each is a few GB, so this may take a while depending on your connection.

3. **Make sure Ollama is running.** It usually starts automatically after
   install; if not, run `ollama serve` in a terminal and leave it running.

## Running the app

1. Put `autorag.py` and `requirements.txt` in a folder.
2. Install Python dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Run it:
   ```bash
   streamlit run autorag.py
   ```
4. It opens in your browser (usually http://localhost:8501). In the
   sidebar, pick your chat model (llama3.1 or mistral) and confirm the
   Ollama host (defaults to `http://localhost:11434`, correct for a
   standard local install). Upload a PDF, then ask questions.

## Notes
- No API key, no billing, no internet required after the models are
  downloaded (except the optional DuckDuckGo fallback search).
- Chat history is saved to `tmp/agent_storage.db` (SQLite) and document
  embeddings to `tmp/chromadb/` — both created automatically next to the
  script on first run.
- `agno` and `chromadb` are pinned (`agno==1.1.0`, `chromadb==0.6.3`)
  because they need to match each other's internal API — don't upgrade
  one without the other without checking compatibility.
- Response quality and speed depend on your machine's CPU/GPU/RAM — an 8B
  model like llama3.1 runs comfortably on most modern laptops, but will be
  slower than a cloud API.
- If you'd rather not run models locally, I can also set this up to use a
  free cloud API (e.g. Groq) instead — just ask.
