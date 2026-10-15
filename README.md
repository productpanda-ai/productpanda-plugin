# Product Panda plugins

This repository is two plugins that share a Git remote. Each host only reads its own marketplace file at the repository root, then loads one plugin directory.

| Host | Marketplace file | Plugin directory |
| --- | --- | --- |
| Cursor | `.cursor-plugin/marketplace.json` | `cursor/` |
| ChatGPT and Codex | `.agents/plugins/marketplace.json` | `codex/` |

Import this repository root. Cursor resolves `"source": "cursor"` to `cursor/.cursor-plugin/plugin.json`. Codex resolves `./codex` from the repository root.

`cursor/` is a Cursor plugin: `.cursor-plugin/plugin.json`, `mcp.json`, and `skills/`. The MCP key is a Cursor variable, set in Plugins, then Configure.

`codex/` is an Agent Plugins package: `plugin.json`, `mcp.json`, and `skills/`. `.codex-plugin/plugin.json` and `.mcp.json` are the older Codex fallback. Codex does not send an `Authorization` header from a plugin file, so the key goes in `~/.codex/config.toml`.

See `cursor/README.md` and `codex/README.md`.
