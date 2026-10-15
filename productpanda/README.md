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

- `using-product-panda` chooses the tool and reads the response header.
- `reading-product-truth` explains overview, modules, flows, features, findings, and relationships.
- `reading-product-audit` explains the product audit sections and the trailing scores.
- `reading-tech-audit` explains the tech audit sections and the trailing scores.
- `reading-competitive-analysis` explains the comparison report.

## Configure

1. In Product Panda, open Settings, then MCP, and create a key. Copy it then. It starts with `mcp_`.
2. Install this plugin.
3. In Cursor, open Plugins, then Configure. Set **MCP URL** to `https://coding-agent-api.fly.dev/mcp` and **MCP key** to that key. For a local API, set the URL to `http://localhost:3000/mcp`.
4. In Codex, replace `PASTE_YOUR_MCP_KEY` in `productpanda/.mcp.json` with that key. The server is registered the same way as a direct Codex config:

```toml
[mcp_servers.productpanda]
url = "https://coding-agent-api.fly.dev/mcp"
http_headers = { Authorization = "Bearer PASTE_YOUR_MCP_KEY" }
```

Reload Cursor. In Customize, confirm the `productpanda` server and the skills.

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
