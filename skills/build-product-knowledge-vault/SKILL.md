---
name: build-product-knowledge-vault
description: Build or substantially refactor a source-linked product-and-code knowledge vault with durable PRD-to-code traceability, evidence authority, freshness boundaries, and curated navigation. Use for multi-source feature knowledge bases or major vault restructuring.
---

# Build Product Knowledge Vault

Build a durable explanation and navigation layer for a feature or product. The vault helps humans and AI understand the product, but it never replaces primary sources such as code, approved product documents, tickets, designs, analytics, tests, or runtime evidence.

## Principles

- Separate **approved product intent**, **current implementation**, **solutioning context**, **delivery state**, **analytics evidence**, and **open questions**.
- Keep one canonical note per meaningful concept.
- Link claims to their sources and record the source date, revision, commit, or snapshot when available.
- Preserve disagreements between sources instead of blending them into one confident answer.
- Do not copy secrets, credentials, customer data, private payloads, or unrelated internal material into the vault.
- Do not change product code, tickets, documents, repositories, or production systems unless the user separately authorizes those writes.

## Establish the evidence boundary

Before building, inventory what is available:

| Source | What it can prove |
| --- | --- |
| Product brief / PRD | Approved intent, user outcome, scope, non-goals |
| Design | Intended interaction and visual behavior |
| Repository and tests | Current implementation and executable behavior |
| Architecture / API docs | System boundaries and documented contracts |
| Jira / issue tracker | Delivery scope and current workflow state |
| Pull requests | Proposed or merged code changes |
| Analytics | Measured behavior when metric definition, denominator, window, and ownership are known |
| Runtime evidence | Observed behavior for the captured environment and revision |

Label unsupported claims as **Needs verification**. Label future ideas as **Proposed** or **Hypothesis**.

## Build one end-to-end slice first

Start with the smallest useful chain:

`product intent → user journey → product rule → implementation entry → state/API boundary → event/analytics contract → relevant test → delivery state`

Make that slice internally consistent before expanding into adjacent modules.

A useful default structure is:

```text
Home.md
00 Product/
  00 Index.md
  User journeys.md
  Product rules.md
01 Architecture/
  00 Index.md
  System boundaries.md
  API contracts.md
02 Code/
  00 Index.md
  Entry points.md
  State and persistence.md
03 Events and Analytics/
  00 Index.md
  Event contracts.md
  Metrics.md
04 Delivery/
  00 Index.md
  Issues and PRs.md
05 Decisions and Gaps/
  00 Index.md
  Decisions.md
  Open questions.md
Sources/
  Source register.md
  Freshness and authority.md
```

Create only the layers supported by evidence. Do not create empty folders just to make the structure look complete.

## Author for meaning

For each important note:

1. State what the concept is and why it matters.
2. Describe the user or system behavior.
3. Add source-backed rules and constraints.
4. Link to implementation entry points when known.
5. Record relevant events, tests, and delivery items.
6. Add explicit unknowns, conflicts, and freshness limits.

Prefer repository-relative paths and durable URLs over machine-specific absolute paths.

## Negative paths are first-class

When relevant, distinguish:

- no data;
- ineligible or filtered-empty data;
- initial loading;
- partial and paginated data;
- fetch or parse failure;
- retry and recovery;
- decline or cancellation;
- dismissal, back navigation, route removal, backgrounding, and unknown outcome.

Code that contains a route guard or retry button does not by itself prove process-death recovery, request cancellation, backend idempotency, or safe replay.

## Navigation

Use Markdown as the durable knowledge layer. Use graph or canvas views only when they answer a specific question, such as:

- product journey;
- frontend-to-backend boundary;
- event lifecycle;
- delivery dependency;
- decision history.

Avoid all-to-all linking. A link should help a reader move to the next useful piece of evidence.

## Refresh strategy

Separate generated inventories from curated explanations.

Generated content may include repository file inventories, issue or PR snapshots, event catalogs, and source registers.

Curated content includes product meaning, architectural explanation, decisions, root-cause analysis, conflict resolution, and manually arranged diagrams.

Automation may refresh generated sections but must not silently rewrite curated conclusions.

## Validation

Before handoff, check:

- all internal links resolve;
- referenced repository paths exist for the captured revision;
- source links are accessible to the intended audience;
- conflicts and unknowns remain labelled;
- generated tools do not overwrite curated content;
- private data and secrets are absent;
- the vault is isolated from production build/package paths unless deliberately included;
- unrelated repositories and external systems were not changed.

## Handoff

Report the vault path, sections changed, sources used or skipped, authority decisions, important unknowns, refresh behavior, validation performed, and refresh commands.
