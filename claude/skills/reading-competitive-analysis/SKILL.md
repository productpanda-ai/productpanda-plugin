---
name: reading-competitive-analysis
description: >-
  Reads a Product Panda competitive analysis from
  get_latest_competitive_analysis. Use after that tool returns, or when the
  user asks about competitors, positioning, a feature matrix, pricing
  comparison, market gaps, or what to build next relative to competitors.
---

# Reading the competitive analysis

`get_latest_competitive_analysis` returns the latest finished comparison report. The body is markdown, usually titled `# Competitive Comparison Report`. It compares this product with competitors that were analyzed for that run. It is not a live market search.

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
- Keep this document separate from product truth. Product truth describes this product. The comparison describes it against competitors.
