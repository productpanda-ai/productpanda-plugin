---
name: using-product-panda
description: >-
  Calls the Product Panda MCP (connection_status, get_latest_product_truth,
  get_latest_product_audit, get_latest_tech_audit,
  get_latest_competitive_analysis) and reads the response header. Use when the
  user asks about their product, what it does, a user flow, product risks,
  technical risks, architecture, competitors, positioning, or Product Panda
  documents, and before answering those questions from memory or from the repo.
---

# Using Product Panda

Product Panda tools are read only. They return documents already stored for the organization that owns the MCP key. They do not search the web, edit the product, or list every product.

## Which tool

| Question | Tool |
| --- | --- |
| Is the key connected, and which organization is it? | `connection_status` |
| What the product is, who it is for, modules, flows, features, gaps | `get_latest_product_truth` |
| Product health, journeys, friction, monetization, personas | `get_latest_product_audit` |
| Engineering risks, security, reliability, stack, debt | `get_latest_tech_audit` |
| Competitors, feature matrix, pricing, gaps vs the market | `get_latest_competitive_analysis` |

Call one tool. Call a second only when the question needs a second document. `connection_status` takes no arguments and returns no product data. Use it when a later call fails auth, or when you need the organization id.

## Arguments

Document tools accept one optional argument, `product_id`, and it must be a UUID.

- Omit `product_id` to load the latest finished document in the organization. That document can belong to any product. There is no tool that lists products or looks them up by name.
- Pass `product_id` only when you already have that UUID from an earlier response header, or the user pasted it.
- A non UUID returns a tool error: `product_id must be a UUID.`

## Response

A hit starts with four lines, then the document:

```text
Latest <title> for this organization.
id: <document id>
product_id: <uuid or none>
updated: <ISO timestamp or unknown>

<body>
```

Titles are `product truth`, `product audit`, `tech audit`, and `competitive analysis`.

A miss is not a failure. The text is `No <title> is available for this organization.` or `No <title> is available for product <uuid> in this organization.` Say that plainly. Do not invent a stand in document.

`product_id: none` means the row is not tied to a product. Repeat the header `product_id` and `updated` when you answer, so the user can see which product and which version you used.

## After the call

Read the matching skill before you summarize:

- product truth → `reading-product-truth`
- product audit → `reading-product-audit`
- tech audit → `reading-tech-audit`
- competitive analysis → `reading-competitive-analysis`

Answer from the returned text. Cite names, ids, file paths, and scores that appear there. If a section is missing, say it is missing. `Rate limit exceeded` means stop and tell the user to retry later.
