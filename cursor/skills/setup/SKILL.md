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
2. In Cursor, open Plugins, then Configure. Set MCP URL to `https://coding-agent-api.fly.dev/mcp` and MCP key to the raw key. For a local API, set the URL to `http://localhost:3000/mcp`.
3. Do not ask the user to paste the key into chat.

## Verify

After the user saves the connection, have them reload Cursor and start a new chat if tools have not refreshed. Call `connection_status` once the tool is available. A successful call returns the organization id and no product data. If it fails or is unavailable, explain that the connection remains unverified.
