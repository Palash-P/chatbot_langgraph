# LangGraph Streamlit Chatbot

A collection of Streamlit demos that build up a LangGraph chatbot step by step: basic chat, streaming responses, SQLite-backed conversation history, tool calling, PDF RAG, and MCP tool integration.

## Features

- Streamlit chat UI with session-based message history
- LangGraph state graph using `ChatOpenAI`
- Streaming assistant responses
- SQLite checkpointing for persistent chat threads
- Tool-calling examples:
  - DuckDuckGo web search
  - Calculator
  - Alpha Vantage stock quote lookup
- PDF upload and RAG over uploaded documents with FAISS
- Experimental MCP tool loading through `MultiServerMCPClient`

## Project Structure

| File | Purpose |
| --- | --- |
| `1_streamlit_frontend.py` | Basic Streamlit chat UI using the in-memory backend. |
| `2_streamlit_frontend_streaming.py` | Basic chat UI with streamed LangGraph messages. |
| `3_streamlit_frontend_database.py` | Chat UI with SQLite-backed thread history. |
| `4_streamlit_frontend_threading.py` | Threaded chat UI using the tool backend. |
| `5_streamlit_frontend_tool.py` | Tool-calling chatbot UI with visible tool status updates. |
| `6_streamlit_rag_frontend.py` | PDF upload chatbot with per-thread document retrieval. |
| `7_streamlit_frontend_mcp.py` | MCP-enabled chatbot UI using async streaming. |
| `langgraph_backend.py` | Minimal LangGraph chatbot with in-memory checkpointing. |
| `langgraph_database_backend.py` | LangGraph chatbot with SQLite checkpointing. |
| `langgraph_tool_backend.py` | LangGraph chatbot with web search, calculator, and stock tools. |
| `langraph_rag_backend.py` | RAG backend for PDF ingestion, FAISS retrieval, and tools. |
| `langgraph_mcp_backend.py` | Async LangGraph backend with MCP tool loading. |
| `.gitignore` | Keeps secrets, virtual environments, caches, and local databases out of Git. |

## Requirements

- Python 3.10+
- OpenAI API key
- Optional LangSmith API key if you want tracing

Install the main dependencies:

```bash
pip install streamlit python-dotenv requests
pip install langgraph langgraph-checkpoint-sqlite langchain langchain-openai langchain-core langchain-community
pip install duckduckgo-search
pip install pypdf faiss-cpu langchain-text-splitters
pip install aiosqlite langchain-mcp-adapters
```

If you only run the earlier demos, you may not need the PDF/RAG or MCP packages.

## Environment Variables

Create a `.env` file in the project root:

```env
OPENAI_API_KEY=your-openai-api-key
LANGCHAIN_TRACING_V2=true
LANGCHAIN_ENDPOINT=https://api.smith.langchain.com
LANGCHAIN_API_KEY=your-langsmith-api-key
LANGCHAIN_PROJECT=chatbot-langgraph
```

LangSmith variables are optional. The app needs `OPENAI_API_KEY` for OpenAI chat and embedding models.

## How to Run

Run any frontend with Streamlit:

```bash
streamlit run 1_streamlit_frontend.py
```

Other useful entry points:

```bash
streamlit run 2_streamlit_frontend_streaming.py
streamlit run 3_streamlit_frontend_database.py
streamlit run 5_streamlit_frontend_tool.py
streamlit run 6_streamlit_rag_frontend.py
streamlit run 7_streamlit_frontend_mcp.py
```

For the PDF chatbot, start `6_streamlit_rag_frontend.py`, upload a PDF from the sidebar, then ask questions about the uploaded document.

## Notes Before Pushing to GitHub

- Do not commit `.env`, `myenv/`, `__pycache__/`, or `chatbot.db`. These are ignored by `.gitignore`.
- `chatbot.db`, `chatbot.db-shm`, and `chatbot.db-wal` are local SQLite runtime files created by the checkpoint saver.
- The stock price tool currently contains a hardcoded Alpha Vantage API key in `langgraph_tool_backend.py`, `langraph_rag_backend.py`, and `langgraph_mcp_backend.py`. Move that key into `.env` before making the repository public.
- `langgraph_mcp_backend.py` includes a local MCP server path (`/Users/nitish/Desktop/mcp-math-server/main.py`). Update that path for your machine before using the MCP demo.
- The RAG backend filename is currently `langraph_rag_backend.py` without the second `g` in `langgraph`; the frontend imports that exact name.

## Suggested Git Commands

```bash
git status
git add .gitignore README.md *.py
git commit -m "Add LangGraph chatbot README"
git push
```

