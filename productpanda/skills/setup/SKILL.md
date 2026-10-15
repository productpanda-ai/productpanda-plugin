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
2. The host stores that key as the bearer token for the `productpanda` server. Do not ask the user to paste the key into the chat.
3. After they confirm the connection is saved, call `connection_status`. A successful call returns the organization id and no product data.
