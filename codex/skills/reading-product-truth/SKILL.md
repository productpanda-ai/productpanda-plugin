---
name: reading-product-truth
description: >-
  Reads a Product Panda product truth from get_latest_product_truth. Use after
  that tool returns, or when interpreting overview, modules, flows, features,
  findings, relationships, evidence, status, or confidence in a product truth.
---

# Reading product truth

The body is markdown. Each `##` heading is one JSON value: `overview`, `modules`, `flows`, `features`, `findings`, `relationships`, `sources`. Parse the JSON under the heading you need. Extra keys can appear. Ignore them unless the user asked about that key.

`sources` is generation metadata. It is not the product.

## Overview

`description` is what the product does. `target_users` and `core_value_proposition` are objects with `value` and `status`, not plain strings. `confidence` is `high`, `medium`, or `low`. `evidence` is an array of source references.

## Shared item fields

Modules, flows, features, and findings use:

- `status`: `inferred`, `confirmed`, `user_corrected`, `disputed`, `deprecated`. Prefer confirmed and user corrected over inferred. Mention disputed and deprecated instead of treating them as current fact.
- `confidence`: `high`, `medium`, `low`.
- `source_origin`: `code`, `doc`, `mixed`, `user`.
- `evidence`: repo relative file paths, HTTP routes, or `symbol()` references. Quote these when you claim something exists.

## Collections

- **modules**: `name`, `description`, `completeness` (`complete`, `partial`, `unclear`), `entry_points`, `key_components`, `related_features`, `related_flows`.
- **flows**: `name`, `entry_point`, `exit_point`, `steps` (`step_id`, `description`, `related_components`, `evidence`), `gaps`.
- **features**: `name`, `description`, `implementation_status` (`complete`, `partial`, `unclear`), `related_modules`, `related_flows`.
- **findings**: `description`, `type` (`gap`, `inconsistency`, `missing_flow`, `partial_implementation`), `severity` (`critical`, `high`, `medium`, `low`), `why_it_matters`, `related_modules`, `related_flows`.
- **relationships**: `from_id`, `to_id`, `type` (`enables`, `depends_on`, `related_to`). Ids point at modules, flows, or features.

## How to answer

- What it is and who it is for: `overview` only.
- How something works: the matching `flows` item, then its `steps`.
- What is built: `features.implementation_status` and `modules.completeness`.
- What is wrong: `findings`, highest severity first, with `why_it_matters`.
- How two areas connect: `relationships`, then `related_*` arrays.

Do not flatten every array into the answer. Name the items that match the question and say how many others exist.
