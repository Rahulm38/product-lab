---
name: raise-pr
description: Prepare, validate, create, and verify a pull request from a local change while keeping issue-tracker updates and other external writes explicit and evidence-backed.
---

# Raise PR

Create a reviewable pull request, not just a branch with code on it.

## Establish repository state

Before changing Git state, inspect:

- repository and remote;
- current branch and target branch;
- working-tree status;
- staged, unstaged, and untracked files;
- commits ahead/behind the target;
- merge-base diff;
- linked issue or ticket when one exists;
- required checks and repository contribution guidance.

Preserve unrelated user changes. Do not stage or commit files merely because they are present.

## Verify the change

Run the smallest relevant checks first, then proportionate broader checks.

Depending on the repository, this may include:

- focused unit/integration tests;
- lint and formatting;
- type checking;
- build or compile checks;
- generated-file consistency;
- security or dependency checks;
- manual validation for UI or runtime behavior.

Record exact commands and results. If a broader suite fails, distinguish failures caused by the change, pre-existing failures, and unresolved failures with evidence.

Do not claim “all tests pass” when only a focused subset ran.

## Shape the commit

Create the smallest coherent commit set that tells a reviewer what changed.

Good commit messages describe the outcome:

```text
Fix empty state after terminal pagination
Add retry coverage for failed initial load
```

Avoid noisy commits such as `updates`, `fix`, or `changes`.

If the repository requires a specific branch or commit convention, follow the repository evidence rather than inventing one.

## Draft the PR body

A useful PR body usually contains:

### What changed
A short description of the behavior or technical outcome.

### Why
The user, system, operational, or developer problem being solved.

### How it works
Only the implementation details a reviewer needs to understand the approach.

### Verification
Commands, tests, manual scenarios, screenshots, or other evidence.

### Risks / gaps
Known limitations, follow-up work, migration concerns, rollout constraints, or unresolved checks.

### Related work
Issue, ticket, design, document, or dependency links that the reviewer can actually access.

Keep the PR body factual. Do not copy secrets, personal data, private payloads, or inaccessible local file paths.

## Review the final diff

Before opening the PR:

1. Compare the final branch to the target branch.
2. Check that no unrelated file was included.
3. Look for debug code, temporary logs, credentials, generated junk, or machine-specific files.
4. Verify tests cover the behavior being changed.
5. Confirm the PR title matches the actual scope.
6. Confirm the body does not overclaim verification.

If the diff differs materially from the user's requested scope, stop and explain it before publishing.

## Create the PR

When the user asked to create/raise/open a PR, creation is authorized within the verified scope.

Create one PR against the correct target branch. Capture the PR number and URL.

Do not also merge, approve, request reviewers, change issue status, or post external comments unless the user requested those actions or they are required by an established repository workflow and separately authorized.

## Issue-tracker updates

A PR request does not automatically authorize editing Jira or another issue tracker.

If an update would be useful, show the exact proposed update first. After explicit approval, make only that update and verify it by re-reading the issue.

## After creation

Re-read the PR or inspect its returned metadata and confirm:

- title;
- base and head branches;
- body;
- changed-file scope when available;
- current checks/status;
- draft versus ready state.

Report the PR link, verification performed, any remaining failing or unrun checks, and any follow-up that is intentionally outside the PR.

## Failure handling

- If push or PR creation fails, report the exact failure and preserve local work.
- If a PR may already have been created after a timeout, search the branch or exact title before retrying.
- If the target branch is ambiguous, ask rather than guessing.
- If required checks cannot run locally, mark them as not run rather than implying success.
- Never force-push or rewrite shared history unless the user explicitly authorizes it.
