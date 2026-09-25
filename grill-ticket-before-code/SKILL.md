---
name: grill-ticket-before-code
description: Grill a software ticket against its codebase before implementation. Use when a Jira issue, bug, feature request, or existing pull request must be checked for missing acceptance criteria, current behavior, regressions, edge cases, test coverage, or implementation readiness before any code changes.
---

# Grill the ticket before coding

## Establish facts

1. Read the ticket, acceptance criteria, linked designs, dependencies, and discussion.
2. If a pull request exists, read its description, diff, review comments, unresolved threads, checks, and target branch.
3. Inspect the relevant production paths and existing tests. Trace data from source through state to UI and side effects.
4. Verify current behavior from code, local fixtures, tests, logs, or a runnable app. Do not infer behavior from names alone.
5. Separate every conclusion into:
   - **Evidence**: directly supported by a source, file, test, log, or observed run.
   - **Inference**: plausible but not yet proven.
   - **Unknown**: requires a product decision, missing access, or runtime verification.

Record exact source links, file paths, line references, commands, and observed results where useful.

Do not quote, summarize, name, or cite a local artifact in a Jira recommendation unless ticket readers will receive it as an attachment or access-controlled link. If it will remain unavailable, ask the user how to handle it and use only a minimal sanitized illustrative value pattern when needed. Never include user, account, transaction, request, or credential data.

## Save tokens with bounded delegation

Delegate only independent, read-only investigations that can run in parallel. Use the lowest-cost capable subagent for mechanical extraction and inventory; reserve stronger reasoning for contradictions and final synthesis. Give each subagent one narrow question, named sources, a strict output shape, and permission to report unknowns. Good assignments include:

- Extract ticket and PR requirements without proposing code.
- Trace the current production behavior and state transitions.
- Inventory existing tests and identify uncovered boundaries.
- Audit analytics, navigation, accessibility, and error handling.

Do not delegate the final judgment. Do not give subagents expected answers or conclusions. Require concise evidence with locations, risks, and unknowns. Synthesize results yourself, resolve contradictions against primary evidence, and discard duplicated speculation. Do not spawn agents when the task is too small or the work would overlap.

Run independent lanes in parallel and use the lowest-cost capable model when supported. Stop research once evidence is sufficient for the requested decision; do not repeat extraction already completed by another lane.

## Staged judgment contract

When this skill is used by a staged run, it is the judgment lane for `security-audit`,
`migration-audit`, or `adversarial-challenge`. Return evidence, contradictions, risks,
and unknowns with source locations. Stay read-only, do not invent a repair, and do not
repeat the `change-map` inventory or the planner's synthesis.

## Aim for the minimum safe change

Start with the user's stated outcome and identify the smallest coherent production change that achieves it. Do not expand the ticket merely because the audit finds adjacent defects or possible improvements.

- Separate **required for this fix**, **sanity/regression only**, and **separate follow-up**.
- Prefer existing event names, property keys, lifecycle points, and control paths.
- For analytics, explicitly distinguish event name, property key, property value, source, and emission point.
- Ask the user in chat before adding attributes, retry redesign, navigation changes, new events, or unrelated parser work that is not required for the stated outcome.
- Put the human-readable behavior and impact before code, payload, or contract details.

## Build the behavior matrix

Cover at least:

- Primary happy path.
- New requested behavior.
- Existing behavior that must remain unchanged.
- Empty, partial, stale, duplicate, and malformed data.
- Initial, intermediate, terminal, and failed pagination where applicable.
- Query/filter state, reset behavior, and empty scoped results where applicable.
- Loading, shimmer/skeleton, initial failure, retry, and recovery.
- Back, close, deep-link, and other relevant navigation.
- Analytics success, failure, empty, duplicate-event, and timing semantics.
- Accessibility labels, focus, announcements, contrast, and dynamic content.
- Relevant configuration, caching, concurrency, lifecycle, and offline behavior.

For each row state: precondition, action, expected result, unchanged behavior, evidence, test level, and status.
Keep the matrix to at most 20 relevant rows. Summarize non-applicable categories once instead of creating empty rows.

Translate the final matrix into two acceptance-criteria groups:

1. **New change**: only the requested behavior, with one independently testable result per criterion.
2. **Sanity / existing flows**: applicable happy path plus touched failure, retry, navigation, and compatibility behavior that must remain unchanged.

Exclude non-applicable cases and do not promote sanity checks into new product behavior.

## Maintain an ambiguity register

List contradictions, missing decisions, unverified assumptions, and sources that disagree. Rank each as:

- **Blocking**: different answers would materially change the implementation.
- **Non-blocking decision**: does not affect behavior and may be deferred, but must remain explicit; do not supply an authoritative answer.
- **Verification-only**: implementation can proceed, but manual or integration testing is required.

Ask only questions that cannot be answered from the ticket, linked authoritative documents, PR, codebase, existing tests, logs, or safe runtime evidence.

## Enforce the question gate

Never fill an evidence gap with a likely answer. This is especially important for producer or backend contracts, field names and types, lifecycle ownership, retry rules, security policy, compatibility behavior, and analytics semantics.

For every blocking unknown:

1. State exactly what was searched and what was not found or could not be accessed.
2. Ask one direct question whose answer would resolve the ambiguity.
3. Name the expected decision owner or authoritative source, such as the API owner, product owner, analytics owner, security policy, or versioned contract.
4. Explain in one sentence which implementation or acceptance criterion changes depending on the answer.
5. Ask it in chat. Add it to a Jira open-questions section only when the user requests that handoff or the question cannot be resolved before the ticket is shared.

Do not offer a guessed default for a blocking question. Do not treat conceptual guidance, code enums, frontend models, old tickets, or observed downstream data as proof of a current producer-to-consumer contract. If access to the authoritative source is missing, request the contract, sanitized sample, or owner decision and keep the item **Blocking**.

Group related questions and ask the smallest useful set in chat, ordered by implementation impact. Defer non-blocking decisions explicitly; do not manufacture an authoritative answer or enlarge the ticket with speculative detail.

## Challenge the proposed change

Before approving implementation, ask:

1. Is the requirement evaluated at the correct lifecycle point?
2. Does it distinguish global empty state from filtered or intermediate empty state?
3. Is business behavior owned by the correct layer?
4. Can failure or missing secondary data be mistaken for a valid empty result?
5. Does pagination defer terminal decisions until completion?
6. Are existing success, alternate-outcome, failure, retry, and navigation paths preserved?
7. Can analytics fire early, twice, or replace existing events?
8. Do tests observe public behavior rather than private implementation details?
9. Is the proposed diff the smallest coherent change?

## Enforce the stop/go gate

Do not edit code until all of the following are true:

- Ticket, codebase, and—when one exists—PR facts have been reconciled.
- The behavior matrix covers new, happy, edge, and preserved flows.
- Blocking ambiguities are resolved or explicitly returned to the user.
- Every unresolved blocking item has a targeted question, expected owner/source, and implementation impact; no answer is assumed.
- Current tests and missing tests are identified.
- The intended ownership, terminal condition, and analytics timing are clear.
- A verification plan names automated and manual checks.

Return **STOP** when any blocking item remains. Return **GO** only with a coherent implementation plan, test plan, preserved-behavior list, risks, and evidence. After GO, implement only if the user asked for implementation.

## Offer a Jira update

Always finish the grill with a Jira update recommendation. Use the Atlassian/Jira MCP
to read the current issue before drafting. Compare the issue with the verified behavior
matrix and identify missing or contradictory acceptance criteria, edge cases,
dependencies, design links, test expectations, and unresolved product decisions.

- If no update is useful, state `No Jira update recommended` and why.
- Otherwise show the exact proposed Jira comment or field edits, identify the target
  issue and fields, and ask permission to apply them.
- Include an `Open questions / decisions required` section whenever authoritative answers remain missing. For each item record the question, owner/source, why it matters, and whether it blocks ticket creation, implementation, or verification. Remove or resolve an item only after recording the authoritative answer and source.
- Do not write to Jira from an instruction to grill, analyze, or implement. Update only
  after explicit approval of the exact draft and fields.
- After approval, use the Jira MCP, re-read the issue, and confirm the resulting content
  and available identifier or timestamp.

## Propose skill improvements

After using this skill, inspect observed friction from the current conversation. If a
reusable skill improvement is supported by evidence, show the user the exact addition,
its target file/section, and its expected reliability or token-saving benefit. Ask for
explicit permission before changing the shared skill, script, eval, reference, or asset.
Do not silently edit the skill, and do not generalize task-specific facts into rules.

For forward-testing this skill, use [evals/evals.json](evals/evals.json).
