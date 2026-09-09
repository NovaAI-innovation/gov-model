# Governance Document Contract

Status: active
Version: 1.0
Owner: Project governance
Approver: User
Canonical path: docs/governance/document-contract.md

## 1. Purpose

This contract defines the required shape of governance documents and contracts.
Stable headings allow deterministic parsing, generation, review, and context
injection.

## 2. Applicability

This contract applies to governance policies, architecture contracts,
specifications, output contracts, request manifests, decision records, phase
plans, evidence records, release records, and incident records.

## 3. Required metadata

Every document must declare:

- 'document_id'
- 'document_type'
- 'status'
- 'version'
- 'owner'
- 'approver'
- 'canonical_path'
- 'effective_date' when active
- 'supersedes' or 'none'
- 'dependencies'

## 4. Required section order

Unless a document type profile specifies a stricter order, use:

1. Purpose
2. Authority and ownership
3. Scope
4. Non-scope
5. Definitions
6. Inputs
7. Rules or requirements
8. Expected outputs
9. Gates and validation
10. Failure conditions
11. Side effects
12. Rollback or recovery
13. Exceptions
14. Evidence
15. Dependencies
16. Change history

## 5. Section rules

- Required sections must appear even when their value is 'Not applicable'.
- A section may not be silently omitted.
- Each requirement must use MUST, MUST NOT, SHOULD, SHOULD NOT, or MAY.
- Each gate must state its pass condition and evidence.
- Each output must identify its format and canonical location.
- Each side effect must identify authorization and rollback expectations.
- Each exception must identify its approver and expiration.

## 6. Input rules

Inputs must be classified as:

- Required
- Optional
- Conditional
- Derived
- Prohibited
- Unknown

Unknown required inputs block submission. Defaults may not conceal missing
scope, authorization, safety, acceptance, or rollback information.

## 7. Output rules

An output contract must identify:

- Primary artifact
- Supporting artifacts
- Metadata
- Status
- Evidence bundle
- Side-effect report
- Acceptance criteria
- Canonical location

## 8. Parsing rules

- Use one H1 title.
- Use numbered H2 headings for contract sections.
- Keep heading names stable after activation.
- Use tables for repeated typed fields.
- Use explicit empty arrays or 'Not applicable' instead of omitted fields.
- Keep normative rules separate from explanatory notes.
- Use ISO 8601 dates and semantic versions.

## 9. Approval rules

Changing a required section, field, gate, or meaning is a contract change and
requires versioning and recorded approval.

## 10. Change history

| Version | Change | Authority |
|---|---|---|
| 1.0 | Established common document shape and parsing rules | User request |
