---
name: create-jira-ticket
description: Investigate, draft, create, and verify high-quality Jira bugs, stories, or tasks using Jira metadata, related issues, repository evidence, runtime artifacts, and explicit user authorization. Use when asked to create or raise a Jira ticket, convert a reported problem or requirement into a Jira issue, place work under an epic, reproduce the structure of an existing ticket, check for duplicates, or populate project-specific required fields.
---

# Create a Jira ticket

## Respect the write boundary

Treat Jira creation as an external write.

- Create only when the user explicitly asks to create, raise, file, or submit a ticket.
- Treat that explicit request as authorization to create the verified ticket within the stated scope; do not ask for redundant approval after drafting.
- For requests to investigate, grill, review, or draft only, show the exact proposal and request approval before writing.
- Do not create a second issue after a partial or ambiguous tool response. Search by the exact summary and recent creation time first.
- Never transition, assign, link, comment, or edit beyond the requested ticket unless required fields or explicit instructions authorize it.

## Establish Jira facts

1. Resolve the accessible Jira site and cloud identifier.
2. Read every issue named by the user, including the requested parent or epic.
3. Confirm the exact project and parent keys. Do not infer a parent from a similar name or from the same issue number in another project.
4. Inspect one or two recent, comparable issues in the same project to learn local conventions for issue type, labels, priority, product story, assignee, and description structure.
5. Query create metadata for the intended project and issue type immediately before creation. Record every required field, allowed option, and default.
6. Search for duplicates using the product area, symptom, event/API/screen name, and key error terms. Read plausible matches rather than judging by summary alone.

Do not reuse custom-field IDs or option IDs across projects without live metadata verification.

## Staged triage contract

When assigned the `+triage` stage, classify the request read-only before any Jira write:
scope, priority, owner, duplicate signal, blocking unknowns, and the next safe action.
Do not create, edit, transition, assign, or link an issue from triage alone.

Store hierarchy only in Jira hierarchy fields. Never repeat the parent or epic in the description unless the user explicitly asks for a narrative cross-reference that is not represented by Jira fields.

## Keep Jira/MCP reads small

Jira responses can be large and MCP calls can be expensive. Use the smallest read set that still supports the decision. Keep this optimization generic: preserve the full evidence gates when the task is high-risk, disputed, or contract-sensitive.

- If the user supplies a parent/epic or reference tickets, use those keys directly and skip discovery searches for them.
- For an existing-issue update, read the target issue once with only the fields being changed plus summary, type, parent, status, priority, and required custom fields. Do not fetch comments, changelog, rendered fields, attachments, or remote links unless they can change the decision.
- For a new issue, prefer one targeted duplicate search, one comparable issue, one create-metadata call, one create call, and one minimal verification read. Do not fan out across a whole project.
- Make searches specific: combine the project key with two to four high-signal terms such as the product area, event/API/screen, and failure phrase. Cap duplicate searches at a small result count and read only plausible matches.
- If the parent or reference ticket is material and cannot be resolved with one specific search, ask the user for the epic or one or two reference keys instead of issuing broad Jira searches.
- Request narrow fields from Jira and reduce tool output to a compact evidence packet: key, summary, type, status, priority, parent, relevant field values, duplicate signal, and unresolved questions. Never echo raw MCP payloads when a short extraction is enough.
- Cache facts already established in the current turn. Reuse the same cloud ID, project, issue type, parent, and allowed options rather than fetching them again.

## Investigate the source claim

When a repository, log, dashboard, API response, design, or document is in scope, inspect it before drafting.

- Trace the behavior across relevant boundaries: source system, intermediary service, versioned consumer-facing contract, client parser, normalization or mapping, application state, user-visible behavior, side effects, observability, and tests.
- Treat internal source data and the versioned consumer-facing contract as separate evidence. Do not assign data loss to a consumer when the published contract never exposes the value; record a producer-contract dependency.
- Prefer the latest authoritative, versioned contract over conceptual diagrams or design guidance. Use design material as intent, not proof of a runtime payload.
- Record exact paths and concise code references.
- Separate **Evidence**, **Inference**, and **Unknown**.
- Confirm current behavior from primary sources. Do not turn a spoken hypothesis into a Jira fact.
- Preserve unknown contract details as verification requirements. Never invent response fields, event values, reproduction data, customer identifiers, or production impact.
- Sanitize tokens, credentials, full account identifiers, payloads, and personal data.
- Do not quote, summarize, name, or cite a local artifact as ticket evidence unless it will be attached to Jira or linked through an access-controlled source available to ticket readers. If the artifact cannot be attached, ask the user how to handle it. Use only the minimum sanitized value pattern or illustrative example needed to explain the behavior, clearly labeled as an example and stripped of user, account, transaction, and request data.

### Apply the cross-boundary ownership gate

Classify each disputed value or behavior before assigning ownership:

| Evidence | Ticket implication |
| --- | --- |
| Published by the authoritative contract but rejected, required incorrectly, transformed, or discarded by the consumer | Record a consumer parsing or propagation defect. |
| Present only inside a producer or intermediary | Record a producer-contract dependency before consumer work. |
| Preserved through the data path but acted on or emitted at the wrong lifecycle point | Record a lifecycle or integration defect and define one canonical outcome. |
| Unsupported by an authoritative source | Record a decision or verification requirement; do not invent a field, value, format, or owner. |

For failure and observability work, consider the smallest valid input, an extended input, malformed or partial input, null optional values, and unknown future values. Cover raw-value preservation, safe user-facing fallback, alternate entry points, retry behavior, non-blocking side effects, duplicate prevention, and sensitive-data handling when relevant.

## Resolve unknowns without assuming

Search the ticket, linked authoritative documents, related issues, repository, tests, logs, and safe runtime evidence before asking the user. If an answer still cannot be verified, do not infer it from names, enums, consumer models, conceptual diagrams, old tickets, or internal producer data.

Use this question protocol:

| Step | Required action |
| --- | --- |
| Classify | Mark the item as blocking ticket creation, blocking implementation only, or verification-only. |
| Ask | Write one direct question that can produce a concrete answer. |
| Route | Name the expected owner or authoritative source, such as the API owner, product owner, analytics owner, security policy, or versioned contract. |
| Explain | State which ticket field, acceptance criterion, or implementation decision depends on the answer. |
| Record | Keep the question and answer in chat. Add it to Jira only when the user requests it or it is necessary for a developer handoff. |

If an unknown changes the project, parent, issue type, duplicate decision, required field, scope, or core meaning, ask in chat before creating or expanding the ticket. Keep the Jira concise; do not add a large open-questions section when one direct chat question can resolve the issue. Never turn an unanswered question into an asserted contract, field name, default value, owner, impact, or acceptance criterion.

If the request involves a complex implementation-readiness audit, use the sibling `grill-ticket-before-code` skill first when available.

## Use bounded read-only delegation

Use subagents when the user requests them, an applicable skill requires them, or host policy permits them. Delegation is for independent evidence gathering, not external writes.

For a complex grill, use up to three lanes:

| Lane | Read-only task |
| --- | --- |
| Jira | Read the parent, comparable issues, duplicate candidates, and create metadata. |
| Implementation | Trace the runtime path and inventory focused tests and gaps. |
| Contract and risk | Compare authoritative contracts, lifecycle semantics, compatibility, security, and unresolved decisions. |

Give every agent a bounded question and require a compact result containing findings, direct evidence, confidence, risks, and unknowns. Agents may recommend interpretations, but the coordinator must reconcile conflicts and owns the final evidence classification, ticket scope, STOP/GO judgment, duplicate decision, and every Jira write. If delegation is unavailable, run the same lanes sequentially.

## Write for humans

Lead with the observable problem and why it matters. Keep implementation detail available without making readers decode a code audit.

- Use short sentences and familiar product language.
- Use descriptive Markdown links such as `[API contract](https://example.test/contract)` and `[APP-123](https://example.test/browse/APP-123)`. Do not leave important sources as naked URLs.
- Use a table when three or more items share the same fields, especially evidence, ownership, affected scenarios, or test coverage.
- Keep reproduction, actual result, expected result, and acceptance criteria as prose or numbered steps when sequence matters.
- Put filenames and implementation findings under a concise `Technical evidence` section.
- Explain what a technical finding changes for the user, system, or measurement.
- Avoid repeating the same finding across the problem statement, evidence, behavior matrix, and acceptance criteria.
- Prefer a small set of grouped, observable acceptance criteria over a long inventory of implementation statements.
- Lead with product behavior, impact, current result, and expected result. Put contracts, code paths, payload shapes, and implementation notes afterward under `Technical details`.
- For analytics work, distinguish the event name from property keys and property values. Use an event-contract table with `Event name`, `Key`, `Value/source`, and `When emitted`; never call a property value an event.
- Ask in chat before adding adjacent defects, extra attributes, lifecycle redesign, retry changes, or broad regression work that is not necessary to solve the stated problem.

## Keep scope minimal

Write the smallest independently executable ticket that solves the stated problem. Do not turn code-review findings into extra requirements unless they are necessary for the fix or the user approves the scope expansion.

- State the single monitoring, user, or system outcome first.
- Prefer existing event names, keys, flows, and control paths.
- Do not require unrelated attributes merely because the event supports them.
- Treat retries and terminal outcomes as sanity checks when the requested change does not alter them.
- If a nearby issue is real but not required, mention it to the user in chat and ask whether it should be a separate ticket.

## Build the ticket draft

Use the smallest structure that makes the work independently actionable.

For a bug, normally include:

1. Issue or problem statement.
2. Environment and scope, only when verified.
3. Steps to reproduce or deterministic trigger.
4. Actual result.
5. Expected result.
6. User, operational, or analytics impact without unsupported severity claims.
7. Verified implementation evidence and related issues.
8. Numbered, testable acceptance criteria covering the new behavior and preserved behavior.
9. Automated and manual verification expectations.
10. Explicit unknowns or contract-verification notes.

For a story or task, replace reproduction sections with user outcome, scope, non-goals, dependencies, acceptance criteria, and verification.

Split acceptance criteria into:

1. **New change**: only directly requested, independently testable behavior. Each criterion must have one setup, action, and observable result.
2. **Sanity / existing flows**: applicable happy path and touched failure, retry, navigation, or compatibility behavior that must remain unchanged.

Do not pad either section with non-applicable cases.

Read [references/examples.md](references/examples.md) when choosing a ticket structure or translating evidence into acceptance criteria.

## Apply quality gates

Before creation, ensure:

- The summary states the affected area and observable problem or outcome.
- The issue type matches the work.
- The parent exists and is the requested epic or hierarchy node.
- No open issue already covers the same behavior and acceptance criteria.
- Actual and expected results do not prescribe an unverified implementation.
- Acceptance criteria are observable, numbered, and include relevant null, failure, retry, duplicate-event, navigation, compatibility, and preserved-success cases.
- Ownership is assigned at the correct contract boundary: backend exposure, frontend parsing/propagation, and analytics naming or lifecycle decisions are separated.
- Error-path criteria cover minimal, extended, malformed, and unknown inputs rather than assuming one ideal payload.
- Analytics criteria define the canonical lifecycle point, duplicate-event behavior, retry-count semantics, entry-point parity, and approved sanitization/length rules.
- Code references support the claim and do not include stale line numbers when the branch may drift.
- Priority, environment, platform, ownership, and financial impact are evidence-based or follow an explicitly verified local template.
- All required fields use allowed values from live metadata.
- Every unresolved authoritative answer has been asked in chat before it expands the ticket.

Return `STOP for ticket creation` when ambiguity would materially change the target, hierarchy, issue type, duplicate decision, required field, claim, or meaning. Proceed only with verified facts and explicitly unresolved questions; never fill authoritative gaps with conservative assumptions.

A ticket may be created or corrected while implementation remains blocked. Return `GO for ticket, STOP for implementation` when code would otherwise have to guess about a contract, lifecycle owner, retry rule, compatibility requirement, or sensitive-data policy. Ask the minimum unanswered questions directly in chat; add them to Jira only when necessary for handoff or requested by the user.

## Create and verify

1. Call Jira creation once with the verified project, issue type, summary, description, assignee if authorized, parent/epic fields, and all required custom fields.
2. Capture the returned key and URL.
3. Re-read the created issue once with only summary, issue type, parent, status, assignee, priority, labels, description, and project-required fields.
4. If a field is missing or incorrect, edit only that field, then re-read once more.
5. Report the key, link, parent, status, priority, assignee, and any remaining unknown or unverified runtime requirement.

Do not claim success from the create response alone; the re-read is the completion gate.

## Handle failures safely

- On a validation error, refresh create metadata and correct only rejected fields.
- On permission failure, return the exact blocked action and a complete copyable draft.
- On timeout or uncertain creation, search recent issues by exact summary before retrying.
- When a required choice has no authoritative value, stop the applicable gate and ask one concise, targeted question; do not infer a value from convention.
- Do not silently drop required fields or place the issue under a different parent.

## Preserve Atlassian failure markers

When Atlassian or the Jira MCP rejects, marks, or omits something, keep a compact failure record: tool/action, issue key if known, exact marker or field-level message, and the next safe action. Never hide a rejected field or treat a partial response as success.

| Atlassian marker | Safe response |
| --- | --- |
| `INVALID_ARGUMENT`, HTTP 400, or an invalid field value | Refresh live metadata; correct only the named field or option; do not broaden the write. |
| `not valid Atlassian Document Format (ADF) content` | Convert only the rejected rich-text field to minimal ADF (`doc` → `paragraph` → `text`) and retry that write once; do not recreate the issue. |
| Required field or missing allowed option | Preserve the exact field name and marker, re-read metadata once, then ask the user if no authoritative value exists. |
| HTTP 401/403 or permission error | Stop the write and return the exact blocked action plus a copyable draft. |
| HTTP 404 for issue, project, parent, or field | Re-resolve once using the supplied key; ask the user rather than inferring a different target. |
| Timeout, 5xx, or an ambiguous create response | Do not retry creation blindly. Search by the exact summary and recent creation time, then verify before deciding. |
| Create returned an error or re-read omits a requested field | Treat creation as unverified; edit only the missing field after the re-read and verify again. |

For forward-testing this skill, use [evals/evals.json](evals/evals.json).
