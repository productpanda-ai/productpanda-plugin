---
name: reading-competitive-analysis
description: >-
  Reads the stored comparison of this product with competitors already
  analyzed, from get_latest_competitive_analysis. Use after that tool returns,
  or when the user asks about competitors, positioning, features, pricing, or
  what competitors offer that this product does not. This is not a live market
  search.
---

# Reading the competitor comparison

This document compares the product with competitors Product Panda already analyzed. It is a stored report, not a live market search. The tool response title is `competitive analysis`. When you answer, talk about competitors, features, and pricing.

`get_latest_competitive_analysis` returns the latest finished comparison report. The body is markdown, usually titled `# Competitive Comparison Report`. It compares this product with competitors that were analyzed for that run.

## Sections

| Heading | Use it for |
| --- | --- |
| Executive Summary | Where the product stands, in a few sentences |
| Feature Comparison Matrix | Table of features across this product and each competitor. Cells are checkmarks, X marks, or short notes |
| Pricing Comparison | Tiers, numbers, and value for money. Incomplete pricing is called out |
| Our Advantages | Where this product leads, with concrete features |
| Gaps & Opportunities | What competitors offer that this product does not, ordered by customer impact |
| Strategic Recommendations | What to build, improve, or emphasize |

A report can include an extra focus section when that run was asked to go deep on one topic. Read it when the user question matches that topic. Do not drop the standard sections.

## How to answer

- Name products, features, and prices that appear in the report.
- If a competitor is marked incomplete, say the comparison for that competitor is partial.
- A missing section means that part was not written. Do not fill it from general knowledge.
- Keep this comparison separate from the product map (`get_latest_product_truth`). The product map describes this product. The comparison describes it against competitors.
