# Ticket examples

Use these patterns as structure guidance. Replace every placeholder and discover project fields live.

## Cross-boundary observability bug

**Prompt:** Create a bug because diagnostic information visible in one system is absent from an operational event.

**Good investigation:**

- Read EPIC-42 and a comparable analytics bug.
- Identify the authoritative producer-to-consumer contract.
- Determine whether the disputed value is published, transformed, discarded, or never exposed.
- Compare the smallest documented response with parser requirements.
- Trace the value through normalization, application state, lifecycle handling, and observability.
- Check whether outcome handling and UI impressions produce duplicate measurements.
- Inspect null, malformed, retry, alternate-entry, unknown-value, and success behavior.
- Search for existing issues using the endpoint, event, and error-attribute names.
- Separate verified defects from contract or product decisions.

**Good summary:** `Checkout: Documented failure context is missing from operational events`

**Evidence pattern:**

| Boundary | Verified behavior | Ticket implication |
| --- | --- | --- |
| Versioned contract | State what is actually documented. | Establishes producer and consumer responsibility. |
| Consumer parser | State required and optional inputs. | Identifies parsing mismatches. |
| Lifecycle handling | State where outcomes and impressions occur. | Identifies missing or duplicate side effects. |
| Tests | State what is covered and absent. | Defines proportionate verification. |

Avoid inventing diagnostic fields or formats. Use `GO for ticket, STOP for implementation` when the defect is proven but implementation still needs an explicit contract or policy decision.

## User-facing state bug

**Prompt:** Raise a bug based on BUG-100's format: an empty paginated screen never leaves shimmer.

Distinguish initial loading, empty intermediate pages, terminal global empty, filtered empty, API failure, retry, and recovery. Acceptance criteria should say when the terminal decision occurs and which alternate states remain unchanged. Do not copy BUG-100's user identifiers or project fields blindly.

## Product story

**Prompt:** Create a story under EPIC-9 for adding saved filters.

Use sections for user outcome, background, in scope, non-goals, dependencies, analytics, accessibility, acceptance criteria, and verification. Do not use bug-only fields such as actual result unless the Jira project requires them; if required, use truthful values such as `Not applicable - new capability` only when allowed by local convention.

## Possible duplicate

**Prompt:** Create a bug for a missing failure-side effect that may already be covered by an older integration issue.

If an open issue already requires the same event, lifecycle point, attributes, and verification, return the matching issue and do not create a duplicate. If the existing issue defines the event but the new defect is a distinct data-loss path, create a focused issue and link or reference the related ticket in the description when authorized.

## Project-field discovery

Before creation, map semantic choices to live Jira metadata:

| Meaning | Live value source |
| --- | --- |
| Project and issue type | Project and issue-type metadata |
| Epic or parent | Re-read requested parent issue |
| Required custom fields | Create metadata for this issue type |
| Priority | User instruction, verified impact, or comparable local ticket |
| Assignee | Explicit instruction or verified local convention |
| Platform/environment | Evidence, not repository language alone |
| Labels/component | Comparable issues and current project taxonomy |

Never hardcode example IDs from another Jira project.
