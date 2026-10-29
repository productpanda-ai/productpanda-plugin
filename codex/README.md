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
- `writing-personalized-skills` writes the skill set from `generate-personalized-skills` into `.agents/skills/`.

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
