# Governance Documentation Index

Status: active for document placement; individual policy contracts may remain draft until approved
Version: 1.0
Owner: Project governance
Approver: User
Canonical path: docs/governance/index.md

## 1. Purpose

This index routes governance documents to one canonical folder and gives each
document type a stable parsing and review boundary.

## 2. Authority

This index supplements, and does not replace:

- standard-operating-procedure.md for task lifecycle and execution gates
- universal-agent-systems.md for the architecture baseline
- implementation-plan.md for sequencing, dependencies, risks, and phase gates

When a placement rule conflicts with an existing control document, work stops
until the conflict is recorded and resolved.

## 3. Document families

| Family | Canonical folder | Responsibility |
|---|---|---|
| Governance policies | docs/governance/ | Authority, scope, safety, change, and lifecycle |
| Architecture contracts | docs/architecture/ | Boundaries, dependencies, contexts, and exceptions |
| Behavioral specifications | docs/specifications/ | Protocols, capabilities, errors, and compatibility |
| Decisions | docs/decisions/ | Approved or rejected decisions and their consequences |
| Quality contracts | docs/quality/ | Definition of done, testing, evidence, and determinism |
| Operations contracts | docs/operations/ | Observability, recovery, incidents, and performance |
| Machine schemas | schemas/ | Versioned validation contracts |
| Reusable templates | templates/ | Fillable instances of approved document types |
| Verification tools | tools/ | Validators, checks, helpers, and scaffolding utilities |

## 4. Required navigation

Each canonical folder must contain an index or README that identifies:

- What belongs in the folder
- What does not belong in the folder
- The document types it owns
- Required sections for those document types
- Source-of-truth rules
- Validation and review expectations

## 5. Source-of-truth rule

Human-readable policy and specification documents are authoritative for meaning.
Machine schemas are authoritative for machine validation. Templates are
authoritative only as reusable document shapes; they must not introduce policy
that is absent from the governing document.

## 6. Request rule

Every substantive request must reference an output contract, input manifest,
applicable gates, and evidence requirements before execution begins.

## 7. Change history

| Version | Change | Authority |
|---|---|---|
| 1.0 | Established canonical governance documentation routing | User request |
