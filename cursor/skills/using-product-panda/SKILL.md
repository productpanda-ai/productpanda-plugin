---
name: using-product-panda
description: >-
  Explains what someone can ask the Product Panda plugin, then calls the
  matching read only tool. Use when the user asks what they can do with
  Product Panda or this plugin, or asks what the product is, who it is for,
  how it works, where users get stuck, engineering risks, competitors, or
  what changed on the default branch, or asks for a skill set for this
  product. Also use before answering those questions from memory or from
  the repo.
---

# Using Product Panda

The tools only read analysis Product Panda already stored for the organization that owns the key. They do not search the web, edit the product, start a new analysis, or list every product. `generate-personalized-skills` writes nothing on the server. It returns skill files for you to save in this repository.

## What you can ask

When the user asks what this plugin can do, answer with these six things. Describe them in everyday language. Do not mention tool names. Do not say product truth, product audit, tech audit, CPO, or CTO unless the user already used those words.

- What the product is, who it is for, how people use it, what is built, and what is missing.
- Where users get stuck, how the product makes money, and what to fix first.
- Engineering risks: security, reliability, performance, debt, the stack, and how the system is built.
- How this product compares with the competitors already analyzed: features, pricing, and gaps.
- What changed on the default branch on a given day, or over the last few days: product changes, engineering changes, and the files that shipped.
- A skill set for this product, written into this repository, from the stored product map, the user experience review, and the engineering review.

## Which tool

| The user wants | Tool | Then read |
| --- | --- | --- |
| Is the key connected, and which organization is it? | `connection_status` | |
| What the product is, who it is for, how it works, what is built, what is missing | `get_latest_product_truth` | `reading-product-truth` |
| Where users get stuck, journeys, time to value, billing, what to fix first | `get_latest_product_audit` | `reading-product-audit` |
| Security, reliability, performance, debt, the stack, architecture | `get_latest_tech_audit` | `reading-tech-audit` |
| Competitors, feature comparison, pricing, gaps | `get_latest_competitive_analysis` | `reading-competitive-analysis` |
| What shipped on a day, or over the last few days | `get_nightly_changes` | `reading-nightly-changes` |
| A skill set for this product, written into this repository | `generate-personalized-skills` | `writing-personalized-skills` |

Call one tool. Call a second only when the question needs a second document. `connection_status` takes no arguments and returns no product data. Use it when a later call fails auth, or when you need the organization id.

## Arguments

The four latest document tools accept one optional argument, `product_id`, and it must be a UUID.

- Omit `product_id` to load the latest finished document in the organization. That document can belong to any product. There is no tool that lists products or looks them up by name.
- Pass `product_id` only when you already have that UUID from an earlier response header, or the user pasted it.
- A non UUID returns a tool error: `product_id must be a UUID.`

`get_nightly_changes` also needs a window. Pass exactly one of `day` (`YYYY-MM-DD`) or `period_days` (an integer from 1 to 31). `period_days: 7` means today and the six days before it, in the organization timezone. The same `product_id` rule applies. Read `reading-nightly-changes` before you summarize that call.

`generate-personalized-skills` takes the same optional `product_id`. Read `writing-personalized-skills` and write every returned file into this repository. Do not summarize the skills instead of writing them.

## Response

A latest document hit starts with four lines, then the document:

```text
Latest <title> for this organization.
id: <document id>
product_id: <uuid or none>
updated: <ISO timestamp or unknown>

<body>
```

The title is an internal label: `product truth`, `product audit`, `tech audit`, or `competitive analysis`. Use it only to choose the reading skill. When you answer, use the wording in "What you can ask".

A miss is not a failure. The text is `No <title> is available for this organization.` or `No <title> is available for product <uuid> in this organization.` Say that this part of the stored analysis is not available. Do not invent a stand in document.

`product_id: none` means the row is not tied to a product. Repeat the header `product_id` and `updated` when you answer, so the user can see which product and which version you used.

## After the call

Read the matching skill before you summarize. Answer from the returned text. Cite names, ids, file paths, and scores that appear there. If a section is missing, say it is missing. `Rate limit exceeded` means stop and tell the user to retry later.
