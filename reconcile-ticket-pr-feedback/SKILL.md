---
name: reconcile-ticket-pr-feedback
description: Reconcile a Jira ticket, acceptance criteria, codebase behavior, pull-request diff, review comments, CI results, local changes, and tests into one evidence-backed implementation plan. Use before changing an existing PR, responding to review feedback, deciding whether a ticket is fully covered, resolving contradictions between the ticket, code, tests, and reviewer claims, or coordinating a staged multi-agent run.
---

# Reconcile Ticket and PR Feedback

Produce one coherent, evidence-backed plan before changing code or external systems.

## Compose the existing skills

1. Run `$grill-ticket-before-code` first to establish ticket readiness, the behavior matrix, preserved workflows, and the STOP/GO decision.
2. When GitHub review feedback exists, run `$github:gh-address-comments` for thread-aware reads and comment clustering. Keep it read-only until the combined synthesis is complete; do not enter its edit/reply/resolve steps yet.
3. Merge both outputs into the claim-evidence matrix below. Recheck any duplicated or conflicting conclusion against primary sources.
4. If either named skill is unavailable, execute its equivalent embedded workflow in this skill and record that fallback.
5. Permit implementation only when the ticket grill returns GO and every selected actionable PR thread maps to a verified claim, plan item, or drafted explanation.

## Stage the run

When this work is executed as a staged run, assign each lane one responsibility and one
bounded handoff. The table records the requested allocation; use the runtime's actual
model and report it from evidence rather than claiming a model from this document alone.

| Stage | Model | Thinking | Role | Required output |
|---|---|---|---|---|
| `change-map` | `gpt-5.6-luna` | max | research workhorse | Scope, artifacts, dependencies, and evidence map. |
| `security-audit` | `gpt-5.6-sol` | max | judgment | Security risks, controls, and unknowns. |
| `migration-audit` | `gpt-5.6-sol` | max | judgment | Data, schema, compatibility, and migration risks. |
| `deployment-plan` | `gpt-5.6-terra` | max | planner | Ordered implementation and release plan. |
| `user-impact` | `gpt-5.6-terra` | max | planner | Observable user and operational impact. |
| `key-lifecycle` | `gpt-5.6-terra` | max | planner | Ownership, lifecycle, and terminal-state plan. |
| `synthesis` | `gpt-5.6-terra` | max | planner | One reconciled plan with evidence and open decisions. |
| `adversarial-challenge` | `gpt-5.6-sol` | max | judgment | Counterexamples, contradictions, and failure modes. |
| `+triage` | `gpt-5.6-sol` | max | judgment | Priority, scope, owner, and blocking decision. |
| `repair-N` | `gpt-5.6-luna` | max | worker | Smallest authorized repair for the selected finding. |
| `verify-N` | `gpt-5.6-sol` | max | judgment | Evidence-backed verification result and residual gaps. |
| `deployment-runbook` | `gpt-5.6-terra` | max | planner | Safe release, rollback, monitoring, and handoff steps. |

Use `change-map` once, run independent audit and planning lanes in parallel when their
inputs are ready, and pass only compact evidence to later stages. Keep `repair-N` behind
the synthesis and authorization gates. Keep `verify-N` independent of the worker lane.

## Rules

- Treat the ticket, PR description, review comments, implementation, tests, and current behavior as separate claims until verified.
- Inspect the actual target branch, current branch, merge-base diff, working tree, and test results. Do not infer local state from the PR UI.
- Do not edit code, update Jira, reply to comments, resolve threads, commit, push, or rerun mutating workflows until the synthesis is complete and the user has authorized the applicable action.
- Preserve existing workflows unless the ticket explicitly changes them.
- Distinguish missing requirements from missing implementation and missing verification.
- Prefer primary evidence: source, tests, command output, API models, local storage behavior, Jira text, PR threads, and CI logs.
- Label uncertainty. Never convert an assumption into an acceptance criterion.

## Delegate to save context

When subagents are available and the task is large enough, delegate bounded read-only collection in parallel. Use the lowest-cost capable subagent for extraction and inventory; reserve stronger reasoning for contradictions and final synthesis. Do not create overlapping lanes merely to fill the assignment table.

1. **Ticket analyst**: extract requirements, acceptance criteria, ambiguities, happy paths, edge cases, and explicitly preserved behavior.
2. **PR analyst**: inspect the PR description, diff, review threads, and CI; classify each comment without proposing edits.
3. **Code/test analyst**: trace the current behavior from entry point through state, persistence, API models, pagination, filters, analytics, and tests.

Give each subagent only its artifact scope and a strict output limit: at most 12 findings, each with a source locator and confidence. Ask for facts, not conclusions or patches. Do not let subagents edit files or external systems. The main agent must independently read the selected skill and synthesize all outputs. If evidence is small, collect it directly rather than spawning agents.

## Workflow

### 1. Establish scope and snapshots

Record:

- ticket key and URL;
- PR number, URL, base branch, and head branch;
- current local branch, HEAD, merge-base, and working-tree status;
- repository/module in scope;
- user-authorized actions.

If the PR cannot be identified from the ticket, branch, or repository, stop and ask for the missing identifier.

### 2. Collect ticket claims

Read the full ticket, including description, acceptance criteria, comments, attachments, linked issues, and recent edits. Extract:

- intended user outcome;
- happy path;
- empty, error, retry, pagination, filter, navigation, and analytics behavior;
- existing workflows that must remain unchanged;
- test expectations;
- unresolved ambiguity or contradiction.

Separate explicit requirements from reasonable test hypotheses. Do not silently add requirements.

### 3. Collect PR and local evidence

Inspect:

- PR description and linked ticket;
- merge-base diff, including deleted and renamed files;
- all review threads, including resolved and outdated threads;
- latest CI checks and failure logs;
- current local diff and untracked files;
- commits added after each comment;
- tests changed or added by the PR.

Compare the hosted PR head with local HEAD. State clearly if they differ.

### 4. Trace current behavior

Follow the implementation end to end. Include relevant:

- UI entry and rendering;
- events, state transitions, and terminal conditions;
- API/domain models and pagination contract;
- primary and alternate outcome partitions;
- local database, cache, and persistence behavior;
- filters and reset behavior;
- loading, error, retry, back, and help flows;
- analytics events and emission timing.

Verify whether a count or status comes from the server, local database, cache, or UI-derived state. Do not confuse these sources.

### 5. Build the claim-evidence matrix

Use one row per material claim:

| ID | Claim / AC / comment | Source | Code path | Test evidence | Runtime/CI evidence | Status | Gap |
|---|---|---|---|---|---|---|---|

Allowed statuses:

- `covered`
- `partially covered`
- `missing`
- `contradicted`
- `not applicable`
- `unknown`

Keep the matrix bounded to 30 rows. Combine duplicates while preserving source locators.

### 6. Classify every PR comment

Classify each review thread:

- **actionable**: still valid against current code and ticket;
- **stale**: valid for an older revision but already addressed;
- **misunderstanding**: contradicted by verified behavior or requirements;
- **out-of-scope**: valid concern not required for this ticket.

For every classification, cite the relevant current code/diff and ticket or test evidence. A resolved thread is not automatically stale, and an unresolved thread is not automatically actionable.

### 7. Grill the combined story

Challenge the proposed behavior before planning:

- What exact terminal condition triggers the new behavior?
- Can pagination, cache failure, or unknown counts produce a false terminal state?
- Could filters accidentally trigger a global empty state?
- Does the new branch displace an existing success, alternate-outcome, failure, retry, or navigation flow?
- Are analytics mutually exclusive, timed correctly, and compatible with existing success/failure events?
- Are Jira ACs testable and free of implementation-specific wording?
- Does each test exercise a public behavior or stable seam?
- Which claim is supported only by assumption?

Record contradictions explicitly and decide which source controls. If product intent cannot be derived, pause for user/product clarification.

### 8. Produce a coherent plan

Before mutations, present:

1. verified current behavior;
2. ticket/PR/comment contradictions;
3. missing acceptance criteria or missing tests;
4. comments to address, explain, or leave unchanged;
5. ordered implementation steps;
6. test matrix covering new behavior and unchanged workflows;
7. Jira/PR updates proposed after verification;
8. risks and open questions.

Keep the plan to 15 implementation/test items. Each item must map to a matrix row. Ask for confirmation when the plan changes product behavior or acceptance criteria.

### 9. Implement only after synthesis

After authorization:

- make the smallest coherent change;
- retain unrelated local changes;
- add regression tests at stable public seams;
- run focused tests first, then proportionate wider checks;
- distinguish failures caused by the change from pre-existing failures;
- inspect the final diff against the claim-evidence matrix.

Do not resolve or reply to PR threads until the implementation and evidence support the response.

### 10. Close the loop

Report:

- what changed and why;
- matrix rows now covered;
- focused and wider validation results;
- remaining unrelated failures or unknowns;
- exact Jira AC/comment updates proposed or completed;
- PR comments addressed, stale, misunderstood, or out of scope.

## Propose skill improvements

After using this skill, inspect observed friction from the current conversation. If a
reusable skill improvement is supported by evidence, show the user the exact addition,
its target file/section, and its expected reliability or token-saving benefit. Ask for
explicit permission before changing the shared skill, script, eval, reference, or asset.
Do not silently edit the skill, and do not generalize task-specific facts into rules.

For forward testing of this skill, use [evals/evals.json](evals/evals.json).
