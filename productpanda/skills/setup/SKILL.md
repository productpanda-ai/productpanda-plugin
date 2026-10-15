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
   - In Codex, the bundled server comes from `mcp.json`. `headers.Authorization` must be `Bearer` plus the key. Replace `PASTE_YOUR_MCP_KEY` there. Codex rejects an `extensions` field on that server and then does not install the MCP. Do not ask the user to paste the key into chat.

The same header in `~/.codex/config.toml` is:

```toml
[mcp_servers.productpanda]
url = "https://coding-agent-api.fly.dev/mcp"
http_headers = { Authorization = "Bearer PASTE_YOUR_MCP_KEY" }
```

## Verify

After the user saves the connection, have them restart the relevant host and start a new chat if tools have not refreshed. Call `connection_status` once the tool is available. A successful call returns the organization id and no product data. If it fails or is unavailable, explain that the connection remains unverified.

OpenAI's public submission portal currently lacks an API-key connection flow. If this is a public submission, explain that the server needs supported OAuth authentication or guidance from OpenAI; do not disable authentication or invent OAuth endpoints.
