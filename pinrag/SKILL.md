---
name: pinrag
description: >-
  Use when the PinRAG MCP server is enabled. Query indexed PDFs, YouTube
  videos, GitHub repos, and Discord exports with citations; add or remove
  documents; list the index. Map user requests to the correct MCP tool.
author: ndjordjevic
version: "1.0"
tags:
  - rag
  - mcp
  - pdf
  - citations
  - langchain
  - chromadb
  - youtube
  - github
---

# PinRAG MCP tools

Use this skill whenever the **PinRAG** MCP server is running. The server
process is named `pinrag-mcp`; it is launched automatically from your editor's
MCP configuration via `uvx --from pinrag pinrag-mcp` (or from PATH after
`pipx install pinrag` / `uv tool install pinrag`).

## Prerequisites

- **API keys:** Set `ANTHROPIC_API_KEY` and/or `OPENAI_API_KEY` in the MCP
  `env` block. Default setup: Anthropic LLM + OpenAI embeddings (both keys
  required). OpenAI-only: set `PINRAG_LLM_PROVIDER=openai` and only
  `OPENAI_API_KEY`. See the [PinRAG README](https://github.com/ndjordjevic/pinrag#installation).
- **uv:** Required when using `uvx`. Install from [docs.astral.sh/uv](https://docs.astral.sh/uv/).
- **Stable index path:** Set `PINRAG_PERSIST_DIR` to an absolute path (e.g.
  `~/.pinrag/chroma_db`) so the vector store is not tied to the editor's
  working directory.

## Tools

| Tool | When to use | Key parameters |
|------|-------------|----------------|
| `query_tool` | Answer questions over the index; returns answer with citations | Required: `query`. Optional: `document_id`, `tag`, `document_type`, `page_min`, `page_max` (PDF), `file_path` (GitHub), `response_style` (`thorough` or `concise`) |
| `add_document_tool` | Index local files, directories, or any supported URL in one call | Required: `paths` (list of strings — file paths, dirs, YouTube/playlist URLs, GitHub URLs). Optional: `tags` (list, one per path) |
| `add_url_tool` | Index a single YouTube video/playlist or GitHub repo URL | Required: `url` |
| `list_documents_tool` | List indexed documents and chunk counts | Optional: `tag` to filter |
| `remove_document_tool` | Remove a document from the index | Required: `document_id` (from `list_documents_tool`) |

`add_document_tool` accepts both local paths **and** URLs in a single call;
use `add_url_tool` only when the user provides a single URL and no local paths.

## MCP resources and prompt

- `pinrag://documents` — indexed documents (read in the MCP resources panel).
- `pinrag://server-config` — effective config: LLM, embeddings, API key
  status, chunking settings.
- Prompt `use_pinrag` (parameter: `request`) — for combined flows when the
  host supports MCP prompts.

## Workflow

1. Identify what the user wants:
   - Question / search → `query_tool`
   - "Index / add …" → `add_document_tool` (files, dirs, or mixed paths+URLs);
     single URL only → `add_url_tool`
   - "List / show documents" → `list_documents_tool`
   - "Remove / delete …" → `remove_document_tool` (obtain `document_id` first
     with `list_documents_tool` if not given)
2. Call the tool with required parameters; add optional filters as appropriate.
3. Return the result verbatim — citations (page numbers, timestamps, file paths)
   are embedded in the server response.

## Examples

- "What does the amiga-book PDF say about AGA?" →
  `query_tool` with `query` and optional `tag`
- "Index ~/papers with tag ml" →
  `add_document_tool` with `paths: ["~/papers"]` and `tags: ["ml"]`
- "Add this YouTube video https://youtu.be/abc" →
  `add_url_tool` with `url: "https://youtu.be/abc"`
- "Index the LangChain repo" →
  `add_document_tool` with `paths: ["https://github.com/langchain-ai/langchain"]`
- "List documents" →
  `list_documents_tool` with no parameters
- "Remove document intro.pdf" →
  `remove_document_tool` with `document_id` obtained from `list_documents_tool`
- "Search only in intro.pdf for chunking" →
  `query_tool` with `query` and `document_id: "intro.pdf"`

If PinRAG is not configured, direct the user to
[PinRAG installation](https://github.com/ndjordjevic/pinrag).
