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
2. In Codex, the server is `productpanda` at `https://coding-agent-api.fly.dev/mcp`, with `http_headers.Authorization` set to `Bearer` plus the key. That matches:

```toml
[mcp_servers.productpanda]
url = "https://coding-agent-api.fly.dev/mcp"
http_headers = { Authorization = "Bearer PASTE_YOUR_MCP_KEY" }
```

Replace `PASTE_YOUR_MCP_KEY` in the plugin `.mcp.json` or in `~/.codex/config.toml`. Do not ask the user to paste the key into the chat. In Cursor, the key goes in Plugins, then Configure.
3. After they confirm the connection is saved, call `connection_status`. A successful call returns the organization id and no product data.
