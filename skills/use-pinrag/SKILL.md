---
name: use-pinrag
description: >-
  Use when the PinRAG MCP server is enabled. Query indexed PDFs, YouTube, GitHub repos,
  and Discord exports; add or remove documents; list the index. Call MCP tools by name.
---

# PinRAG MCP tools

Use this skill when the **PinRAG** MCP server is running (stdio process from `pinrag-mcp`). The server is typically started with **`uvx --from pinrag pinrag-mcp`** or after `pipx install pinrag` / `uv tool install pinrag` so `pinrag-mcp` is on `PATH`.

## Prerequisites

- **API keys:** Set `ANTHROPIC_API_KEY` and/or `OPENAI_API_KEY` in the MCP `env` block (or your environment). See the [PinRAG README](https://github.com/ndjordjevic/pinrag#installation).
- **uv:** If using `uvx`, install [uv](https://docs.astral.sh/uv/) and ensure it is on `PATH`.

## Tools (invoke via your host’s MCP integration)

| Tool | When to use | Parameters |
|------|-------------|------------|
| `query_tool` | Answer questions over the index with citations | Required: `query`. Optional: `document_id`, `page_min`, `page_max`, `tag`, `document_type`, `response_style`, GitHub `file_path`, etc. |
| `add_document_tool` | Index local files, directories, or URLs (YouTube, GitHub) in one call | Required: `paths` (list of strings). Optional: `tags`. |
| `add_url_tool` | Index a **single** YouTube or GitHub URL | Required: `url`. |
| `list_documents_tool` | List indexed documents and chunk counts | Optional: `tag` to filter. |
| `remove_document_tool` | Remove a document from the index | Required: `document_id`. |

## Resources and prompt

- **Resources:** `pinrag://documents` (indexed docs), `pinrag://server-config` (effective config), if your client lists MCP resources.
- **Prompt:** `use_pinrag` with parameter `request` for combined indexing/query/list/remove flows when supported.

## Workflow

1. Map the user’s request to a tool (questions → `query_tool`; “add/index …” → `add_document_tool` or `add_url_tool`; “list docs” → `list_documents_tool`; “remove …” → `remove_document_tool`).
2. Call the tool through the host’s MCP layer using the exact tool names above.
3. Return answers with citations as provided by the server.

## Examples

- “What does the PDF say about X?” → `query_tool` with `{"query": "What does the PDF say about X?"}`
- “Index `~/papers` with tag `amiga`” → `add_document_tool` with `{"paths": ["~/papers"], "tags": ["amiga"]}`
- “Add this YouTube video” (single URL) → `add_url_tool` with `{"url": "<url>"}` or `add_document_tool` with `{"paths": ["<url>"]}`
- “List indexed documents” → `list_documents_tool` with `{}`
- “Search only in document X” → `query_tool` with `query` plus `document_id` set to that document’s id

If the user has not configured the PinRAG MCP server, point them to [PinRAG installation](https://github.com/ndjordjevic/pinrag) or this plugin’s `.mcp.json`.
