---
name: reading-tech-audit
description: >-
  Reads a Product Panda tech audit from get_latest_tech_audit. Use after that
  tool returns, or when the user asks about technical risk, security,
  reliability, performance, tech debt, the stack, architecture, or the CTO audit.
---

# Reading the tech audit

`get_latest_tech_audit` returns the merged CTO document when one exists, otherwise the latest finished CTO analysis. The body is markdown, usually titled `# CTO Technical Audit`. Severity in the narrative is `CRITICAL`, `HIGH`, `MEDIUM`, or `LOW`. File paths, function names, and routes in the text are the evidence. Quote them.

## Sections

| Heading | Use it for |
| --- | --- |
| Executive Summary | Health verdict and the one immediate focus |
| Top Risks (Ranked) | The 5 to 10 risks to lead with |
| Architecture Assessment | Boundaries, coupling, request flow |
| Security Assessment | Auth, secrets, input validation, tenant isolation |
| Reliability Assessment | Failures, retries, queues, likely incidents |
| Performance Assessment | Bottlenecks and where load breaks |
| Maintainability Assessment | Duplication, dead code, change safety |
| Testing and Release Confidence | Coverage gaps and deploy confidence |
| Developer Productivity Issues | CI, local dev, typing, conventions |
| Data Layer Assessment | Schema, indexes, migrations, integrity |
| Observability Assessment | Logs, traces, alerts |
| Technical Debt Map | Debt by business impact, with effort |
| Quick Wins | Hours or days |
| Strategic Refactors | Multi week changes and the cost of waiting |
| Prioritized Action Plan (Immediate / Short-term / Medium-term) | Ordered plan |
| Technology Stack | Layer, technology, version table |
| AI/LLM Usage | SDKs, prompt injection, pinning, cost, validation. Absence is stated in one line |
| Project Structure | Top level layout and entry points |
| Core Components | Modules and their roles |
| Data Model | Tables and relationships |
| API Surface | Endpoints by domain, and auth |
| Data Flow | Request to database and back |
| Dependencies & Module Coupling | Coupling narrative and mermaid graph |
| Testing Strategy | Frameworks and what is covered |
| Infrastructure & Deployment | Hosting, CI, environments |
| Key Patterns & Conventions | Patterns used across the code |
| Areas of Note | Legacy, migrations in progress, odd risks |
| Unknowns / Assumptions / Blind Spots | What the audit could not verify |

The action plan heading includes the parenthetical. Match it exactly when you search.

## Trailing JSON

A fenced `json` block may follow the markdown. Use it for scores and structured lists. Use the `##` section for the writeup. Top level keys:

- `system_overview`: `system_stability`, `security_alerts`, `tech_debt_index`, `infrastructure_risk`, `delivery_risk` (scores 0 to 100, plus a short `description`).
- `architecture_components`: `name`, `type`, `technology`, `dependency_count`, `risk`.
- `dependency_graph`: `from`, `protocol`, `to`.
- `stress_points`: `component`, `severity`, `issue`, `recommendation`.
- `security_risks`: `vulnerability_clusters`, `dependency_risks`, `code_pattern_risks`.
- `tech_debt`: `hotspots`, `refactor_candidates`.
- `scalability`: `bottlenecks`, `infrastructure_risks`, `stress_scenarios`.
- `ai_llm_usage`: `uses_ai`, `sdks`, `capabilities`, `risks`.

If `uses_ai` is false, do not describe an AI stack. If the JSON block is missing, answer from the markdown.
