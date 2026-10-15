# Product Panda for Codex and ChatGPT

Codex and ChatGPT plugin. The repo marketplace at `.agents/plugins/marketplace.json` points here with `source.path` `./codex`.

The server is read only. A key can see every product in its organization.

## Layout

Current Codex reads the portable package:

```text
codex/
├── plugin.json
├── mcp.json
├── skills/
│   └── <skill>/
│       └── SKILL.md
└── assets/
    └── panda.png
```

`plugin.json` uses the Agent Plugins schema. Presentation and the setup skill are under `extensions.com.openai`. When that object is present, Codex uses this manifest and does not merge `.codex-plugin/plugin.json`.

`.codex-plugin/plugin.json` and `.mcp.json` remain for older Codex clients. They point at the same skills and the same endpoint.

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
2. Install this plugin from the repository marketplace.
3. Codex strips `Authorization` from plugin `mcp.json`, so the bundled server connects without the key and its tools do not load. Turn that server off and add the key in `~/.codex/config.toml`:

```toml
[plugins."productpanda@productpanda".mcp_servers.productpanda]
enabled = false

[mcp_servers.productpanda]
url = "https://coding-agent-api.fly.dev/mcp"
http_headers = { Authorization = "Bearer PASTE_YOUR_MCP_KEY" }
```

Do not paste the key into chat, and do not commit it.

Restart Codex and start a new chat. Run `connection_status` and confirm it returns the expected organization id.

Add the marketplace with:

```bash
codex plugin marketplace add productpanda-ai/productpanda-plugin
```

A public directory listing still needs an authentication flow the submission portal accepts. See the [packaging guide](https://developers.openai.com/plugins/build/plugins) and [local Codex MCP configuration](https://learn.chatgpt.com/docs/extend/mcp).
