# Product Panda for Cursor

Cursor plugin. The marketplace entry at the repository root points here with `"source": "cursor"`.

The server is read only. A key can see every product in its organization.

## Layout

This matches the Cursor plugin directory:

```text
cursor/
├── .cursor-plugin/
│   └── plugin.json
├── skills/
│   └── <skill>/
│       └── SKILL.md
├── mcp.json
├── assets/
│   └── panda.png
└── README.md
```

Cursor discovers `mcp.json` and `skills/` from those default locations. `plugin.json` declares the MCP URL and MCP key. Cursor substitutes them into `mcp.json`. The key is not stored in this repository.

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
2. Install this plugin from the repository root.
3. Open Plugins, then Configure. Set **MCP URL** to `https://coding-agent-api.fly.dev/mcp` and **MCP key** to that key. For a local API, set the URL to `http://localhost:3000/mcp`.

Reload Cursor and start a new chat. Run `connection_status` and confirm it returns the expected organization id.

## Try it locally

Copy this directory to `~/.cursor/plugins/local/productpanda`. A symlink that points outside that folder is ignored. Restart Cursor or run Developer: Reload Window.

Local plugin imports must be allowed. On Enterprise that control is off until an admin enables Allow Local Plugin Imports.
