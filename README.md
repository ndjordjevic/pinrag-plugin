# PinRAG plugin (Cursor, Claude Code, VS Code Copilot, Amp, Goose)

A **plugin bundle** for editors that support the Open Plugins / Agent Skills
model: ships the [PinRAG](https://github.com/ndjordjevic/pinrag) MCP server
configuration plus a `use-pinrag` skill that teaches the agent how to call
PinRAG's tools.

The Python package (`pinrag` on PyPI) lives in
[ndjordjevic/pinrag](https://github.com/ndjordjevic/pinrag). This repo adds
only manifests, skill files, and a Goose-ready subtree.

## Contents

| Path | Purpose |
|------|---------|
| [`.cursor-plugin/plugin.json`](.cursor-plugin/plugin.json) | Cursor + VS Code Copilot manifest |
| [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json) | Claude Code manifest |
| [`.mcp.json`](.mcp.json) | MCP server: `uvx --from pinrag pinrag-mcp` |
| [`skills/use-pinrag/SKILL.md`](skills/use-pinrag/SKILL.md) | Agent skill (Cursor, Claude Code, VS Code Copilot) |
| [`skills/use-pinrag/mcp.json`](skills/use-pinrag/mcp.json) | Amp-compatible MCP config (co-located with the skill) |
| [`pinrag/SKILL.md`](pinrag/SKILL.md) | Skill copy for [block/agent-skills](https://github.com/block/agent-skills) (Goose marketplace) |
| [`assets/pinrag-icon.svg`](assets/pinrag-icon.svg) | Plugin logo |

## Prerequisites

- **[uv](https://docs.astral.sh/uv/)** on your PATH (used by `uvx` to run `pinrag-mcp`).
- **API keys:** After install, set `ANTHROPIC_API_KEY` and/or `OPENAI_API_KEY`
  in the MCP `env` block.
  - Default: Anthropic LLM + OpenAI embeddings → set both keys.
  - OpenAI only: set `PINRAG_LLM_PROVIDER=openai` and only `OPENAI_API_KEY`.
- **Stable index:** Set `PINRAG_PERSIST_DIR` to an absolute path (e.g.
  `~/.pinrag/chroma_db`) so indexes are not tied to the editor's working
  directory.

## Editor install

### Cursor

Submit or browse this plugin on [Cursor Directory](https://cursor.directory).
For manual install, point Cursor Directory's Auto-scan at this repo URL; it
picks up `.cursor-plugin/plugin.json`, `.mcp.json`, and `skills/`.

### Claude Code

```bash
claude --plugin-dir ./pinrag-plugin
```

Or install via a marketplace that references this repo.

### VS Code Copilot (agent plugins preview)

*Chat: Install Plugin From Source* → enter this repo URL, or add it via the
agent plugins marketplace per the
[VS Code agent plugins docs](https://code.visualstudio.com/docs/copilot/customization/agent-plugins).

### Amp

```bash
amp skill add ndjordjevic/pinrag-plugin/skills/use-pinrag
```

The `skills/use-pinrag/mcp.json` at the skill root is picked up automatically.

## Goose (block/agent-skills)

The `pinrag/` subtree matches the
[block/agent-skills](https://github.com/block/agent-skills) layout.

**Submitted:** [block/agent-skills#18](https://github.com/block/agent-skills/pull/18)
(pending maintainer review).

To submit manually: fork block/agent-skills, copy the `pinrag/` directory to
the fork root (alongside `code-review/`, etc.), and open a PR using the
"Submit a Skill" template.

## License

MIT — same as [PinRAG](https://github.com/ndjordjevic/pinrag/blob/main/LICENSE).
