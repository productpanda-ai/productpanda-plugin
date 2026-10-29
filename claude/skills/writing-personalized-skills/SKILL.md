---
name: writing-personalized-skills
description: >-
  Writes a product skill set into this repository after
  generate-personalized-skills returns. Use when the user asks for skills
  for this product, a personalized skill set, or to generate skills from
  the stored product analysis.
---

# Writing personalized skills

Call `generate-personalized-skills`. Pass `product_id` only when you already have that UUID. Omit it to use the latest documents in the organization.

The tool reads the stored product map, the user experience review, and the engineering review. When several repositories belong to one product, those documents are already merged. The tool does not save the skills.

## Write the files

Each skill is wrapped like this:

```text
<<<FILE skills/<name>/SKILL.md>>>
...
<<<END FILE>>>
```

Write that file to `.claude/skills/<name>/SKILL.md`. Create the directory if it is missing. If the file already exists, replace it. Keep the body as the tool wrote it.

Then tell the user the paths you wrote, and which source lines say `not available`.

## When not to write

If the tool says no documents are available, say so and do not invent skills.

If the tool says the documents are not for the same product, stop and ask for a `product_id`. Do not write a mixed skill set.

`product_id must be a UUID.` is a tool error. Retry only with a UUID from an earlier response, or omit the argument.
