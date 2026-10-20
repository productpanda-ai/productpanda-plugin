---
name: reading-nightly-changes
description: >-
  Reads what changed on the default branch from get_nightly_changes. Use when
  the user asks what shipped, what landed, or what changed on a day or over
  the last few days, and after that tool returns. Product Panda records this
  overnight. The tool does not start a new analysis.
---

# Reading overnight changes

This is the stored record of what Product Panda noticed after code merged to the default branch. It does not start a new analysis. A night with no merges is still a result.

When you answer, say what changed for users of the product, what changed in the engineering picture, and which files shipped. Say overnight changes. Use nightly run, product truth, product audit, or tech audit only if the user already did.

## Call

`get_nightly_changes` needs exactly one window:

- `day`: a calendar day as `YYYY-MM-DD`, in the organization timezone.
- `period_days`: an integer from 1 to 31. `7` means today and the six days before it, in the organization timezone.

Pass optional `product_id` only when you already have that UUID. Omit it to include every product in the organization.

These tool texts are errors: `Provide either day or period_days.`, `Provide either day or period_days, not both.`, `day must be YYYY-MM-DD.`, `day must be a real calendar date.`, `period_days must be an integer from 1 to 31.`, `product_id must be a UUID.`

## Response

The header is:

```text
Nightly changes for this organization.
window: <day or start to end> (<timezone>)
product_id: <uuid or all>
runs: <count>
Each run covers merges on the default branch since the previous snapshot. A run on one day can include commits from earlier days when a previous night did not finish.
```

`runs: 0` is followed by `No nightly changes are available for this organization in that window.` Say that nothing was recorded for that window.

Otherwise each product is a `##` group with the product name and `product_id`. Each run under it is a `###` timestamp with:

- `status`: `done` means finished, `noop` means no source changes, `failed` means it did not finish. Any other status is still in progress.
- `run`: the run id.
- `Failed: ...` when status is `failed`.
- `Analysis is not finished.` when the run is still in progress.
- `No source changes.` when status is `noop`.
- Otherwise a range: short base and head commits, a commit count, and `PRs #id` when those were recorded.

Then three blocks, when the run has anything to show:

- **Product**: lines shaped `- <operation>: <name>. <reason>`. These are changes to what the product is and how users experience it. `created` is new, `updated` changed, `deprecated` is no longer current, `finalized` is settled, `needs_review` still needs a person. Unchanged items are left out. Empty text is `No product changes.`
- **Tech**: the same shape, for engineering findings. Empty text is `No tech changes.`
- **Source**: files shaped `- <path> (<change>, <impact>, <classification>)`. `change` is `added`, `modified`, `deleted`, or `renamed`. `impact` is `none`, `low`, `medium`, `high`, or `unknown`. `classification` is the kind of change, such as `feature`, `workflow`, `ui_ux`, `api_backend`, `data_model`, or `integration`. Test and docs only files (`test_docs_only`) and infra only files (`infra_only`) are left out when any other file exists. Empty text is `No source file changes.` A noop run says `No source changes.` instead.

The list is capped at 40 runs, 25 product lines, 25 tech lines, and 30 files. When a cap is hit the text says `More runs exist in this window.`, `N more findings omitted.`, or `N more files omitted.` Tell the user the list was cut.

A run dated on one day can include commits from earlier days, when a previous night did not finish. Say that when the user asks why an older commit appears.
