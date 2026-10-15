---
name: reading-product-audit
description: >-
  Reads a Product Panda product audit (CPO audit) from get_latest_product_audit.
  Use after that tool returns, or when the user asks about product health,
  personas, journeys, friction, activation, monetization, instrumentation, or
  product recommendations.
---

# Reading the product audit

`get_latest_product_audit` returns the latest finished product audit whose document is present. The body is a markdown report, usually titled `# CPO Product Audit`. Section headings are the contract. Look up the heading that matches the question. Do not treat a heading that is absent as an empty finding.

## Sections

| Heading | Use it for |
| --- | --- |
| Executive Summary | Verdict, top product risks, strengths, the one immediate focus |
| Product Reality Map | What the product actually does, and maturity (production ready, partial, prototype, hardcoded) |
| Product Domains and Persona Map | Personas, views, artifacts, workflows, gaps, handoffs |
| Features and Modules Inventory | Features by module, completeness, boundaries, overlap |
| Architecture Overview (Product Lens) | Topology, integrations, and constraints on product change |
| Core User Journeys | Onboarding, activation, core work, repeat use, admin. Steps and drop off |
| Business Logic and Rule Assessment | Permissions, gating, plans, edge cases, fallbacks |
| Activation and Time-to-Value Analysis | Signup to first value, setup, where users stop |
| Friction and Drop-off Map | Ranked friction |
| Product Maturity and Scalability Assessment | Self serve vs services, hardcoded limits |
| Instrumentation and Analytics Readiness | What is measured and what is blind |
| Monetization and Packaging Assessment | Billing, plan gating, upgrade surfaces |
| Trust and Quality Assessment | Stale data, confirmation, privacy, tenant isolation |
| UI Patterns Assessment | Consistency, design system, accessibility |
| Ownership and Dependencies | Ownership, coupling, external dependency risk |
| Product Sprawl vs Strategic Coherence | Focus vs one off features |
| Quick Wins | Hours or days, with why and effort |
| Strategic Product Recommendations | Larger bets, business case, risk of waiting |
| Prioritized Action Plan | Immediate, short term, medium term |
| Unknowns / Assumptions / Blind Spots | What the audit could not verify |

## Trailing JSON

A fenced `json` block may follow the markdown. Prefer it for scores and row lists, and the matching `##` section for the narrative.

- `product_overview`: scores 0 to 100 for `product_maturity`, `activation_friction`, `user_experience_quality`, `monetization_readiness`, `analytics_coverage`, `product_coherence`. Each has `score` and `description`.
- `feature_maturity_map`: `feature`, `maturity` (`production`, `partial`, `prototype`, `hardcoded`), `user_impact`.
- `user_journeys`: `journey`, `steps`, `friction_points`, `drop_off_risk`, `quality`.
- `friction_points`: `location`, `severity`, `issue`, `user_impact`, `recommendation`.
- `monetization_signals`: billing, plan gating, usage limits, upgrade surfaces, packaging issues.
- `instrumentation_gaps`: flow, what is tracked, what is missing.
- `product_risks`: `risk`, `severity`, `category` (`trust`, `activation`, `retention`, `monetization`, `scale`, `coherence`), `recommendation`.
- `product_domains`: persona, views, artifacts, core flows, gaps.
- `features_and_modules`: module, features, completeness, boundary quality.
- `architecture_overview`: topology, layers, integrations, constraints.
- `ui_patterns`: design system adoption, component reuse, consistency score, accessibility.
- `ownership_dependencies`: module ownership, coupling, external risks.

If the JSON block is missing, answer from the markdown. Do not invent scores.
