# PinRAG plugin (Cursor, Claude Code, VS Code Copilot)

This repository is a **plugin bundle** for editors that support the Open Plugins / Agent Skills model: it ships the [PinRAG](https://github.com/ndjordjevic/pinrag) MCP server plus a **`use-pinrag`** skill that explains how to call PinRAG’s tools.

## Contents

| Path | Purpose |
|------|---------|
| [`.cursor-plugin/plugin.json`](.cursor-plugin/plugin.json) | Cursor + VS Code Copilot manifest |
| [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json) | Claude Code manifest |
| [`.mcp.json`](.mcp.json) | MCP server: `uvx --from pinrag pinrag-mcp` |
| [`skills/use-pinrag/`](skills/use-pinrag/) | Skill + Amp-compatible `mcp.json` |
| [`pinrag/SKILL.md`](pinrag/SKILL.md) | Copy of the skill formatted for [block/agent-skills](https://github.com/block/agent-skills) (Goose marketplace) |

## Install (PyPI server)

The MCP server itself is published as the **`pinrag`** package on PyPI (`pinrag-mcp` entry point). This repo only adds manifests and skills.

## Editor-specific install

- **Cursor / Cursor Directory:** Point the listing at this repo; Auto-scan picks up `.cursor-plugin/` and `.mcp.json`.
- **Claude Code:** `claude --plugin-dir ./pinrag-plugin` or install from a marketplace that references this repo.
- **VS Code Copilot:** *Chat: Install Plugin From Source* → this repo URL, or add the repo as a marketplace per [Agent plugins docs](https://code.visualstudio.com/docs/copilot/customization/agent-plugins).
- **Amp:** `amp skill add ndjordjevic/pinrag-plugin/skills/use-pinrag` (see skill-level `mcp.json`).

## Goose (block/agent-skills)

The **`pinrag/`** subtree matches [block/agent-skills](https://github.com/block/agent-skills) layout; open a PR with that folder per [CONTRIBUTING.md](https://github.com/block/agent-skills/blob/main/CONTRIBUTING.md).

**Submitted:** [block/agent-skills#18](https://github.com/block/agent-skills/pull/18) (pending maintainer review).

## License

MIT (same as PinRAG).
