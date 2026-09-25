---
name: verify-and-report-jira
description: Verify a software change against Jira acceptance criteria and report evidence back to the ticket. Use after implementation or an existing PR needs automated and manual validation across new, happy, regression, error, loading, pagination, retry, filter, navigation, and analytics paths, especially when a Jira comment must be drafted, approved by the user, posted, and confirmed.
---

# Verify and report Jira

## Reconstruct the verification contract

1. Read the Jira issue, acceptance criteria, descriptions, comments, linked designs, dependencies, and related issues.
2. Read the current PR description, diff, review threads, checks, target branch, and latest commit when a PR exists.
3. Inspect the relevant production code, tests, fixtures, and local test harness.
4. Reconcile Jira, PR, and code facts. Label each claim as **evidence**, **inference**, or **unknown**.
5. Do not treat implementation intent, a review comment, or a passing test as proof of behavior it does not directly exercise.

When an installed repository-specific testing skill matches the current code and task, use its reusable scenarios only after verifying asset versions and paths against the checkout.

## Delegate bounded evidence collection

Use subagents only for independent, read-only work that saves context. Use the lowest-cost capable subagent for extraction, inventory, and log summarization; reserve stronger reasoning for contradictions and the final readiness judgment.

- Extract acceptance criteria and preserved behaviors from Jira.
- Map automated tests and their exact coverage to acceptance criteria.
- Build a manual VS Code/Android Studio checklist from available scenarios.

Give each subagent one narrow task, named inputs, a strict concise output, and permission to report unknowns. Do not give expected conclusions. Do not delegate external posting, approval decisions, or the final verification judgment.

Synthesize the results yourself. Resolve contradictions using primary evidence, deduplicate claims, and reject any result without a source or observed output. Do not spawn agents for overlapping or trivial work.

## Staged verification contract

When assigned `verify-N`, act as an independent judgment lane after repair. Verify the
changed behavior and preserved workflows from direct test or runtime evidence, label
each result `VERIFIED`, `NOT TESTED`, `BLOCKED`, or `FAILED`, and keep residual gaps
explicit. Do not treat the worker's claim or a passing unrelated test as proof.

## Build the verification matrix

Create one row per acceptance criterion and material regression path. Include:

- Happy and primary success paths.
- Every newly requested behavior.
- Existing workflows explicitly or implicitly required to remain unchanged.
- Initial loading and shimmer/skeleton behavior.
- Initial API failure, retry, recovery, and repeated failure.
- First, intermediate, terminal, empty, and failed pagination.
- Filters applied, filters removed, and empty filtered results.
- Back, close, Help Centre, deep-link, and other relevant navigation.
- Analytics event identity, timing, multiplicity, success, failure, and empty outcomes.
- Accessibility behavior where the UI changes.

Record: requirement, setup, action, expected result, test level, evidence, result, and notes.

Use only these result labels:

- **VERIFIED**: directly observed by an automated test or manual run with matching evidence.
- **NOT TESTED**: no execution evidence exists.
- **BLOCKED**: execution was attempted but an environmental, access, data, or product-decision blocker prevented a result.
- **FAILED**: observed behavior does not meet the requirement.

Never convert NOT TESTED or BLOCKED into VERIFIED through inference.

## Run automated verification

1. Confirm branch, diff scope, dependencies, SDK/tool versions, and test target.
2. Run the smallest relevant tests first, then the appropriate broader suite.
3. Run static analysis, formatting/diff checks, and relevant PR checks.
4. Capture exact commands, working directory, environment versions, result counts, failures, and timestamps when useful.
5. Classify broader-suite failures as related, unrelated, or unresolved using evidence; do not dismiss failures solely because they predate the change.

Do not change production code merely to make verification pass unless the user separately asks for fixes.

## Run manual verification

1. Prefer deterministic local scenarios, fixtures, or debug-only configuration.
2. Record the IDE or command used, app target, SDK/JDK versions, device/emulator, branch/commit, scenario flag, and setup steps.
3. Fully restart when compile-time configuration changes; do not rely on hot reload for a new scenario.
4. Execute every applicable matrix row in VS Code, Android Studio, or the project’s supported runtime.
5. Record the observed result and any screenshots, logs, analytics traces, or navigation outcomes.
6. Keep local-only harness or environment changes separate from production and PR scope.

If the user performs the manual checks, record their stated results as user-observed evidence and identify any missing environment details without invalidating clear observations.

## Draft before posting

Read [references/comment-template.md](references/comment-template.md) and draft a concise Jira comment containing:

- Scope and tested revision.
- Acceptance-criteria results.
- Preserved-workflow results.
- Automated and manual evidence.
- NOT TESTED, BLOCKED, FAILED, and residual risks.
- Relevant PR or file references.
- Final readiness statement.

Show the complete draft to the user. Obtain explicit confirmation to post that exact draft or a user-approved revision. A prior request to test, implement, or “update Jira” is not approval to publish the verification comment.

## Post and confirm

After explicit approval:

1. Use the Atlassian connector to add the comment to the exact Jira issue.
2. Do not edit acceptance criteria, status, assignee, or other ticket fields unless separately authorized.
3. Re-read the issue or returned comment after posting.
4. Confirm the comment text, issue key, author if available, and timestamp/comment identifier.
5. Report a posting failure as BLOCKED; never claim success from a submitted request alone.

## Propose skill improvements

After using this skill, inspect observed friction from the current conversation. If a
reusable skill improvement is supported by evidence, show the user the exact addition,
its target file/section, and its expected reliability or token-saving benefit. Ask for
explicit permission before changing the shared skill, script, eval, reference, or asset.
Do not silently edit the skill, and do not generalize task-specific facts into rules.

For forward-testing this skill, use [evals/evals.json](evals/evals.json).
