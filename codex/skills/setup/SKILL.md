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
2. Codex removes `Authorization` from a plugin `mcp.json`, so the bundled server answers 401 and its tools never load. Disable that plugin server and add the key in `~/.codex/config.toml`. Do not ask the user to paste the key into chat, and do not write the key into this repository.

```toml
[plugins."productpanda@productpanda".mcp_servers.productpanda]
enabled = false

[mcp_servers.productpanda]
url = "https://coding-agent-api.fly.dev/mcp"
http_headers = { Authorization = "Bearer PASTE_YOUR_MCP_KEY" }
```

## Verify

After the user saves the connection, have them restart Codex and start a new chat if tools have not refreshed. Call `connection_status` once the tool is available. A successful call returns the organization id and no product data. If it fails or is unavailable, explain that the connection remains unverified.

OpenAI's public submission portal currently lacks an API-key connection flow. If this is a public submission, explain that the server needs supported OAuth authentication or guidance from OpenAI; do not disable authentication or invent OAuth endpoints.
