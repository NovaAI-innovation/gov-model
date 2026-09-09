# Output Contract: <name>

document_type: output-contract
contract_version: "1.0"
document_id: OUT-<name>
status: draft
owner: <owner>
approver: <approver>
canonical_path: <path>
supersedes: none
dependencies: []

## 1. Purpose

<What this output is for.>

## 2. Authority and ownership

<Who owns the contract, who approves it, and who may execute it.>

## 3. Scope

<Included work and output boundaries.>

## 4. Non-scope

<Explicit exclusions.>

## 5. Required inputs

| Name | Type | Source | Validation | Freshness | Provenance |
|---|---|---|---|---|---|
| <name> | <type> | <source> | <rule> | <requirement> | <requirement> |

## 6. Optional and conditional inputs

<List optional and conditional inputs and the condition for each.>

## 7. Prior artifacts and context

<Required documents, artifacts, context fields, versions, hashes, and budgets.>

## 8. Expected output

Primary artifact: <format and canonical location>

Supporting artifacts: <metadata, manifests, reports>

## 9. Acceptance criteria

- <criterion>

## 10. Required gates

### Pre-submission

- <gate and pass condition>

### Execution

- <gate and pass condition>

### Post-execution

- <gate and pass condition>

### Closure

- <gate and evidence>

## 11. Failure conditions

- <condition that stops or fails the task>

## 12. Side effects and authorization

Expected side effects: <list or none>

Required authorization: <authority and approval>

Prohibited side effects: <list or none>

## 13. Idempotency

Idempotency key: <key rule>

Safe rerun behavior: <behavior>

Duplicate behavior: <behavior>

## 14. Rollback or recovery

<Rollback steps, recovery path, and verification.>

## 15. Evidence

<Evidence required to prove completion.>

## 16. Canonical location

<Authoritative output location.>

## 17. Change history

| Version | Change | Authority |
|---|---|---|
| 1.0 | Initial contract | <approver> |
