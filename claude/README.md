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

| Tool | Returns |
| --- | --- |
| `connection_status` | Organization id for this key. No product data. |
| `get_latest_product_truth` | Latest product truth (merged truth when a finished analysis has one). |
| `get_latest_product_audit` | Latest finished product audit. |
| `get_latest_tech_audit` | Merged CTO document when one exists, otherwise the latest finished CTO analysis. |
| `get_latest_competitive_analysis` | Latest finished competitive comparison. |

Each document tool takes an optional `product_id` (UUID). Omit it for the latest document in the organization.

In `/mcp` the server is `plugin:productpanda:productpanda`. A tool such as `connection_status` is invoked as `mcp__plugin_productpanda_productpanda__connection_status`.

## Skills

Skills are prefixed with the plugin name, so setup is `/productpanda:setup`.

- `setup` guides connection setup and verifies the organization with `connection_status`.
- `using-product-panda` chooses the tool and reads the response header.
- `reading-product-truth` explains overview, modules, flows, features, findings, and relationships.
- `reading-product-audit` explains the product audit sections and the trailing scores.
- `reading-tech-audit` explains the tech audit sections and the trailing scores.
- `reading-competitive-analysis` explains the comparison report.

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
