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

| Tool | What the user can ask |
| --- | --- |
| `connection_status` | Is the key connected, and which organization is it? No product data. |
| `get_latest_product_truth` | What the product is, who it is for, how people use it, what is built, and what is missing. |
| `get_latest_product_audit` | Where users get stuck, how the product makes money, and what to fix first. |
| `get_latest_tech_audit` | Engineering risks: security, reliability, performance, debt, the stack, and how the system is built. |
| `get_latest_competitive_analysis` | How this product compares with competitors already analyzed: features, pricing, and gaps. |
| `get_nightly_changes` | What changed on the default branch on a day, or over the last few days. |
| `generate-personalized-skills` | A skill set for this product, written into this repository. Nothing is stored on the server. |

The four latest document tools and `generate-personalized-skills` take an optional `product_id` (UUID). Omit it for the latest document in the organization. `get_nightly_changes` needs exactly one of `day` (`YYYY-MM-DD`) or `period_days` (1 to 31), and the same optional `product_id`.

## Skills

- `setup` guides connection setup and verifies the organization with `connection_status`.
- `using-product-panda` answers what the plugin can do, in everyday language, then chooses the tool.
- `reading-product-truth` explains the stored map of the product.
- `reading-product-audit` explains the stored review of how users experience the product.
- `reading-tech-audit` explains the stored engineering review.
- `reading-competitive-analysis` explains the stored competitor comparison.
- `reading-nightly-changes` explains overnight changes on the default branch.
- `writing-personalized-skills` writes the skill set from `generate-personalized-skills` into `.cursor/skills/`.

## Configure

1. In Product Panda, open Settings, then MCP, and create a key. Copy it then. It starts with `mcp_`.
2. Install this plugin from the repository root.
3. Open Plugins, then Configure. Set **MCP URL** to `https://coding-agent-api.fly.dev/mcp` and **MCP key** to that key. For a local API, set the URL to `http://localhost:3000/mcp`.

Reload Cursor and start a new chat. Run `connection_status` and confirm it returns the expected organization id.

## Try it locally

Copy this directory to `~/.cursor/plugins/local/productpanda`. A symlink that points outside that folder is ignored. Restart Cursor or run Developer: Reload Window.

Local plugin imports must be allowed. On Enterprise that control is off until an admin enables Allow Local Plugin Imports.
