---
name: setup
description: >-
  Connect Product Panda after the plugin is installed. Use when the user just
  installed the plugin, needs to add an MCP key, or Product Panda tools fail
  authentication.
---

# Set up Product Panda

The server is read only. The key belongs to one organization and can see every product in that organization.

1. Ask the user to open Product Panda, then Settings, then MCP, and create a key. It starts with `mcp_` and is shown once.
2. Guide them through the connection method supported by their host:

   - In Cursor, open Plugins, then Configure. Set MCP URL to `https://coding-agent-api.fly.dev/mcp` and MCP key to the raw key.
   - In an OpenAI host with API-key plugin connection setup, enter the raw key there. The portable `mcp.json` declares `extensions.com.openai.auth` with `type: api_key` and `headerScheme: bearer`; the host supplies the Authorization header. Do not assume every host exposes this connection flow.
   - On a local Codex host without that flow, use the direct MCP fallback below. Older clients that load the compatibility `.mcp.json` already reference `PRODUCTPANDA_MCP_KEY` and only need that environment variable set.

## Local Codex fallback

If no working plugin connection exists, configure a separate direct MCP connection in the user's `~/.codex/config.toml`:

```toml
[mcp_servers.productpanda]
url = "https://coding-agent-api.fly.dev/mcp"
bearer_token_env_var = "PRODUCTPANDA_MCP_KEY"
```

The user must set `PRODUCTPANDA_MCP_KEY` to the raw key, without `Bearer `, in the environment of the process launching Codex. A desktop app launched outside that terminal may not inherit it. This direct connection does not override plugin credentials; do not create a duplicate if the plugin connection already works.

Do not ask the user to paste the key into chat, print it, or write it to repository files or the installed plugin cache. Portable hosts discover `mcp.json`; editing `.mcp.json` does not configure them. Do not promise that a local environment variable configures hosted ChatGPT tools.

## Verify

After the user saves the connection, have them restart the relevant host and start a new chat if tools have not refreshed. Call `connection_status` once the tool is available. A successful call returns the organization id and no product data. If it fails or is unavailable, explain that the connection remains unverified.

OpenAI's public submission portal currently lacks an API-key connection flow. If this is a public submission, explain that the server needs supported OAuth authentication or guidance from OpenAI; do not disable authentication or invent OAuth endpoints.
