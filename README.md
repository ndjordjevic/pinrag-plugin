# PinRAG — Claude Code plugin

A [Claude Code](https://code.claude.com/docs/en/plugins.md) plugin for [PinRAG](https://github.com/ndjordjevic/pinrag): local RAG over PDFs, YouTube, GitHub repos, and Discord exports, with citations.

The Python package (`pinrag` on PyPI) lives in [ndjordjevic/pinrag](https://github.com/ndjordjevic/pinrag). This repo ships only the Claude Code plugin layout: manifest (`.claude-plugin/plugin.json`), bundled MCP (`.mcp.json`), and the `use-pinrag` skill.

## Contents

| Path | Purpose |
|------|---------|
| [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json) | Plugin manifest ([schema](https://code.claude.com/docs/en/plugins-reference.md#plugin-manifest-schema)) |
| [`.mcp.json`](.mcp.json) | MCP server: `uvx pinrag` |
| [`skills/use-pinrag/SKILL.md`](skills/use-pinrag/SKILL.md) | Agent skill — maps user requests to PinRAG MCP tools |
| [`assets/pinrag-icon.svg`](assets/pinrag-icon.svg) | Icon for marketplace / docs |
| [`LICENSE`](LICENSE) | MIT (matches `plugin.json` `license`) |

## Prerequisites

- **[uv](https://docs.astral.sh/uv/)** on your PATH (used by `uvx` to run `pinrag`).
- **API keys:** Set `OPENAI_API_KEY` in the MCP `env` block (default: OpenAI embeddings and chat). For Anthropic as the query LLM, set `PINRAG_LLM_PROVIDER=anthropic` and `ANTHROPIC_API_KEY` — see the [PinRAG README](https://github.com/ndjordjevic/pinrag#quick-start).
- **Stable index:** Set `PINRAG_PERSIST_DIR` to an absolute path (e.g. `~/.pinrag/chroma_db`) so the vector store is not tied to the working directory.

## Install in Claude Code

### From a local clone (development)

```bash
git clone https://github.com/ndjordjevic/pinrag-plugin.git
cd pinrag-plugin
claude --plugin-dir .
```

After changes, run `/reload-plugins` in Claude Code to pick up updates.

### From the official marketplace

Submit or list plugins via:

- [claude.ai/settings/plugins/submit](https://claude.ai/settings/plugins/submit)
- [platform.claude.com/plugins/submit](https://platform.claude.com/plugins/submit)

See [Create plugins](https://code.claude.com/docs/en/plugins.md) and [Discover and install plugins](https://code.claude.com/docs/en/discover-plugins.md).

## Usage

- With the plugin enabled, the MCP server starts from `.mcp.json` and the skill is available as **`/pinrag:use-pinrag`** (namespace matches the `name` field in `plugin.json`).
- Invoke `/help` to confirm the skill appears under the plugin namespace.

## License

MIT — see [LICENSE](LICENSE); aligns with [PinRAG](https://github.com/ndjordjevic/pinrag/blob/main/LICENSE).
