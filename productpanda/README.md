# Product Panda

This plugin connects Cursor, ChatGPT, and Codex to a Product Panda organization. It installs the MCP server and skills that tell the agent which tool to call and how each returned document is structured.

The server is read only. A key can see every product in its organization.

## Tools

| Tool | Returns |
| --- | --- |
| `connection_status` | Organization id for this key. No product data. |
| `get_latest_product_truth` | Latest product truth (merged truth when a finished analysis has one). |
| `get_latest_product_audit` | Latest finished product audit. |
| `get_latest_tech_audit` | Merged CTO document when one exists, otherwise the latest finished CTO analysis. |
| `get_latest_competitive_analysis` | Latest finished competitive comparison. |

Each document tool takes an optional `product_id` (UUID). Omit it for the latest document in the organization.

## Skills

- `setup` guides connection setup and verifies the organization with `connection_status`.
- `using-product-panda` chooses the tool and reads the response header.
- `reading-product-truth` explains overview, modules, flows, features, findings, and relationships.
- `reading-product-audit` explains the product audit sections and the trailing scores.
- `reading-tech-audit` explains the tech audit sections and the trailing scores.
- `reading-competitive-analysis` explains the comparison report.

## Configure

1. In Product Panda, open Settings, then MCP, and create a key. Copy it then. It starts with `mcp_`.
2. Install this plugin.
3. In Cursor, open Plugins, then Configure. Set **MCP URL** to `https://coding-agent-api.fly.dev/mcp` and **MCP key** to that key. For a local API, set the URL to `http://localhost:3000/mcp`.
4. In Codex, add the key in `~/.codex/config.toml`. Codex installs the plugin server, then drops `Authorization` from `mcp.json` because that header is client owned. The handshake returns 401 and the tools never load. This user server is the one Codex sends:

```toml
[plugins."productpanda@productpanda".mcp_servers.productpanda]
enabled = false

[mcp_servers.productpanda]
url = "https://coding-agent-api.fly.dev/mcp"
http_headers = { Authorization = "Bearer PASTE_YOUR_MCP_KEY" }
```

Do not paste the key into chat, and do not put it in `mcp.json`.

Restart the relevant host and start a new chat after changing connection settings. Run `connection_status` and confirm it returns the expected organization id. In Cursor, confirm the server and skills in Customize.

## Package formats

- `plugin.json`, `mcp.json`, and `skills/` are the portable OpenAI package. OpenAI presentation and onboarding settings live under `extensions.com.openai` in `plugin.json`.
- `.codex-plugin/plugin.json` and `.mcp.json` provide compatibility for older Codex clients. Keep their identity, presentation, and endpoint synchronized with the portable files; the configurations are not merged.
- `.cursor-plugin/plugin.json` explicitly selects `mcp.cursor.json` for Cursor's configuration variables.

### Public OpenAI submission

Codex strips `Authorization` from a plugin `mcp.json`, so a local Codex install needs the key in `~/.codex/config.toml`. A public listing still needs an authentication flow the submission portal accepts.

See the official [packaging guide](https://developers.openai.com/plugins/build/plugins), [authentication limitations](https://developers.openai.com/plugins/deploy/submission#authentication-limitations), and [local Codex MCP configuration](https://learn.chatgpt.com/docs/extend/mcp).

## Add the marketplace

In Cursor, add plugins from the repository root (the folder that contains `.cursor-plugin/marketplace.json`). Selecting `productpanda/` alone fails, because that folder has no marketplace manifest.

In Codex:

```bash
codex plugin marketplace add productpanda-ai/productpanda-plugin
```

In the ChatGPT desktop app, restart after pulling this repo, open the Plugins directory, choose the Product Panda marketplace, and install the plugin.

## Try it locally

Copy this directory to `~/.cursor/plugins/local/productpanda`. A symlink that points outside that folder is ignored. Restart Cursor or run Developer: Reload Window.

Local plugin imports must be allowed. On Enterprise that control is off until an admin enables Allow Local Plugin Imports.
