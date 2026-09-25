# Jira verification comment template

## Verification summary

**Scope:** `<issue / PR / branch / commit>`  
**Environment:** `<IDE or command, SDK/JDK, device/emulator, configuration>`  
**Overall result:** `<READY | NOT READY | PARTIALLY VERIFIED>`

### Acceptance criteria

| Requirement | Result | Evidence |
|---|---|---|
| `<AC or behavior>` | `<VERIFIED / NOT TESTED / BLOCKED / FAILED>` | `<test, command, or observed scenario>` |

### Existing workflows

| Workflow | Result | Evidence |
|---|---|---|
| `<preserved behavior>` | `<status>` | `<test or observation>` |

### Automated verification

- `<exact command>` — `<result and count>`
- `<analysis/check>` — `<result>`

### Manual verification

- `<scenario and setup>` — `<observed result>`

### Gaps and risks

- `<NOT TESTED, BLOCKED, FAILED, unresolved failure, or “None identified”>`

**Readiness:** `<concise conclusion supported by the matrix>`

Keep the final comment factual. Do not include hidden reasoning, unsupported confidence, credentials, or local secrets.
