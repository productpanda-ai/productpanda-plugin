# Product Panda for Claude Code

Claude Code plugin. The marketplace entry at the repository root points here with `"source": "./claude"`.

The server is read only. A key can see every product in its organization.

## Layout

This matches the Claude Code plugin directory:

```text
claude/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── <skill>/
│       └── SKILL.md
├── .mcp.json
├── assets/
│   └── panda.png
└── README.md
```

Claude Code discovers `.mcp.json` and `skills/` from those default locations. `plugin.json` declares the MCP URL and MCP key as `userConfig`. Claude Code substitutes `${user_config.PRODUCTPANDA_MCP_URL}` and `${user_config.PRODUCTPANDA_MCP_KEY}` into `.mcp.json`. The key is sensitive and is not stored in this repository.

Only `plugin.json` belongs in `.claude-plugin/`. The marketplace file is at the repository root, not in this directory.

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

In `/mcp` the server is `plugin:productpanda:productpanda`. A tool such as `connection_status` is invoked as `mcp__plugin_productpanda_productpanda__connection_status`.

## Skills

Skills are prefixed with the plugin name, so setup is `/productpanda:setup`.

- `setup` guides connection setup and verifies the organization with `connection_status`.
- `using-product-panda` answers what the plugin can do, in everyday language, then chooses the tool.
- `reading-product-truth` explains the stored map of the product.
- `reading-product-audit` explains the stored review of how users experience the product.
- `reading-tech-audit` explains the stored engineering review.
- `reading-competitive-analysis` explains the stored competitor comparison.
- `reading-nightly-changes` explains overnight changes on the default branch.
- `writing-personalized-skills` writes the skill set from `generate-personalized-skills` into `.claude/skills/`.

## Configure

1. In Product Panda, open Settings, then MCP, and create a key. Copy it then. It starts with `mcp_`.
2. Add the marketplace and install the plugin:

```bash
claude plugin marketplace add productpanda-ai/productpanda-plugin
claude plugin install productpanda@productpanda
```

On Claude Code v2.1.292 or later, one command does both: `claude plugin install productpanda --marketplace productpanda-ai/productpanda-plugin`.

3. Open the configuration dialog and set **MCP URL** to `https://coding-agent-api.fly.dev/mcp` and **MCP key** to that key. For a local API, set the URL to `http://localhost:3000/mcp`. From a session, run `/plugin configure productpanda`. The shell install command does not prompt. To set values from the shell instead, pass `--config PRODUCTPANDA_MCP_KEY=...` on install. Do not commit the key.

Start a new session. Run `connection_status` and confirm it returns the expected organization id.

## Try it locally

From this repository, load the plugin for one session:

```bash
claude --plugin-dir ./claude
```

`--plugin-dir` does not open the configuration dialog. In that session, run `/plugin configure productpanda`, then `/reload-plugins`.

Validate the plugin and the marketplace before sharing:

```bash
claude plugin validate --strict ./claude
claude plugin validate --strict .
```
