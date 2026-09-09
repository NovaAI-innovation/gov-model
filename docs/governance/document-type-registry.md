# Document Type Registry

Status: active
Version: 1.0
Owner: Project governance
Approver: User
Canonical path: docs/governance/document-type-registry.md

## 1. Purpose

This registry defines the stable sections required by each document and contract
type. A document may add type-specific sections, but it may not remove required
sections without an approved contract change.

## 2. Section profiles

### 2.1 Governance policy

Required sections:

1. Purpose
2. Authority and ownership
3. Scope
4. Non-scope
5. Definitions
6. Governing principles
7. Rules
8. Decision rights
9. Inputs
10. Gates and validation
11. Failure conditions
12. Exceptions
13. Evidence
14. Dependencies
15. Change history

### 2.2 Architecture contract

Required sections:

1. Purpose
2. Authority and ownership
3. Scope
4. Non-scope
5. Context and boundaries
6. Allowed dependencies
7. Prohibited dependencies
8. Ports and adapters
9. Invariants
10. Verification
11. Exceptions
12. Change history

### 2.3 Protocol or behavioral specification

Required sections:

1. Purpose
2. Authority and ownership
3. Scope
4. Non-scope
5. Terms and versions
6. Inputs
7. Outputs
8. Errors
9. Capabilities and unsupported behavior
10. Side effects
11. Compatibility
12. Validation
13. Failure conditions
14. Evidence
15. Change history

### 2.4 Output contract

Required sections:

1. Purpose
2. Authority and ownership
3. Scope
4. Non-scope
5. Required inputs
6. Optional and conditional inputs
7. Prior artifacts and context
8. Expected output
9. Acceptance criteria
10. Required gates
11. Failure conditions
12. Side effects and authorization
13. Idempotency
14. Rollback or recovery
15. Evidence
16. Canonical location
17. Change history

### 2.5 Request manifest

Required fields:

- Request identifier
- Objective
- Output contract and version
- Scope and exclusions
- Required inputs
- Optional and conditional inputs
- Context and prior artifacts
- Expected outputs
- Acceptance criteria
- Authorization
- Risk classification
- Side effects
- Idempotency
- Rollback or recovery
- Pre-submission gates
- Execution gates
- Post-execution gates
- Evidence requirements

### 2.6 Decision record

Required sections:

1. Decision metadata
2. Context
3. Problem
4. Constraints
5. Options considered
6. Decision
7. Consequences
8. Risks
9. Reversal strategy
10. Evidence
11. Approval
12. Change history

### 2.7 Phase plan

Required sections:

1. Phase objective
2. Authority and ownership
3. Scope
4. Non-scope
5. Dependencies
6. Ordered work
7. Required inputs
8. Expected outputs
9. Risks
10. Validation
11. Exit gate
12. Deferred work
13. Change history

### 2.8 Evidence record

Required sections:

1. Evidence metadata
2. Claim being verified
3. Baseline
4. Inputs inspected
5. Checks performed
6. Results
7. Side-effect inspection
8. Limitations
9. Reviewer or approver
10. Change history

### 2.9 Release record

Required sections:

1. Release metadata
2. Scope
3. Included changes
4. Contract and compatibility impact
5. Validation results
6. Documentation status
7. Migration
8. Rollback
9. Known limitations
10. Approval
11. Change history

### 2.10 Incident record

Required sections:

1. Incident metadata
2. Detection
3. Impact
4. Timeline
5. Affected contracts or components
6. Containment
7. Recovery
8. Root cause
9. Corrective actions
10. Evidence
11. Closure
12. Change history

## 3. Extension rule

Type-specific sections may be added after the required sections. An extension
must state its purpose, input, output, and validation behavior.

## 4. Change history

| Version | Change | Authority |
|---|---|---|
| 1.0 | Established document-type section profiles | User request |
