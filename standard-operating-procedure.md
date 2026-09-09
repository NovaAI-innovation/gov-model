# Universal Agent Systems — Standard Operating Procedure

Status: governing project convention

This document defines the required practices for planning, designing, documenting,
implementing, reviewing, and maintaining the Universal Agent Systems project.

The purpose is to keep the project consistent while it grows across multiple
systems, protocol versions, transports, and agent integrations.

## 1. Governing principles

The following rules are mandatory unless a documented decision explicitly overrides
them:

1. The canonical protocol is more important than any individual agent integration.
2. Portable service logic must remain independent of agent vendors and frameworks.
3. Every public behavior must have a defined input, output, error, and side-effect model.
4. Unsupported behavior must be reported explicitly.
5. Documentation and implementation must not silently diverge.
6. Decisions must be reversible where practical and recorded when they affect structure.
7. Deterministic behavior must be preferred over convenience or hidden automation.
8. No implementation begins without a clear scope and acceptance criteria.
9. No destructive operation occurs without a rollback strategy and explicit authorization.
10. The project should solve the smallest useful problem before expanding its abstraction.

## 2. Normative language

The words below have specific meanings:

- **MUST**: required for acceptance.
- **MUST NOT**: prohibited.
- **SHOULD**: expected unless there is a documented reason not to follow it.
- **SHOULD NOT**: discouraged unless there is a documented reason.
- **MAY**: optional.

## 3. Project control documents

The project uses three control documents:

| Document | Responsibility |
|---|---|
| `universal-agent-systems.md` | Defines the target repository architecture and MVP boundary |
| `phase-1-portable-foundations.md` | Defines Phase 1 scope and exit criteria |
| `phase-2-scalable-interop.md` | Defines Phase 2 scope and exit criteria |
| `implementation-plan.md` | Defines sequencing, dependencies, costs, risks, and rationale |
| `standard-operating-procedure.md` | Defines how the project is planned and maintained |

When two documents conflict, the conflict MUST be resolved before implementation.
The implementation plan SHOULD be updated when the resolution changes phase scope,
dependencies, or acceptance criteria.

## 4. Task lifecycle

Every meaningful piece of work follows this lifecycle:

```text
proposed
   ↓
scoped
   ↓
planned
   ↓
drafting / researching
   ↓
generating
   ↓
validating / verifying
   ↓
canonicalizing
   ↓
accepted
```

Exceptional states:

```text
planned ─────────────→ blocked
any active state ─────→ superseded
accepted ─────────────→ deprecated
```

### 4.1 Proposed

The task has a desired outcome but is not yet authorized for implementation.

Required information:

- Objective
- Reason it matters
- Expected deliverable
- Known constraints
- Explicit exclusions
- Whether the task changes files, code, or external systems

### 4.2 Scoped

The task boundary is clear enough to prevent uncontrolled expansion.

The scope MUST identify:

- What is included
- What is excluded
- Relevant existing documents
- Dependencies
- Risks and assumptions
- Completion criteria

No task should proceed while a missing decision could materially change the result.

### 4.3 Planned

The work has an ordered approach and an intended validation method.

A plan SHOULD identify:

- Files or systems affected
- Dependency order
- Expected side effects
- Rollback strategy
- Verification commands or tests
- Exit criteria

### 4.4 Active work

The task is in drafting, researching, generating, or validating. Progress updates
MUST distinguish completed work from proposed work.

### 4.5 Accepted

The deliverable satisfies its acceptance criteria, has been validated, and has
been canonicalized into the project’s current source of truth.

### 4.6 Blocked

A task is blocked only when progress requires an unresolved decision, missing
authority, unavailable external state, or a repeated technical failure that cannot
be safely worked around.

The blocker record MUST state:

- Exact blocking condition
- Checks already performed
- Safe alternatives considered
- Required decision or external change

## 5. Phased document-generation process

Documents and specifications MUST move through these phases in order:

```text
drafting / ideating
        ↓
researching
        ↓
generating
        ↓
validating / verifying
        ↓
canonicalization
```

The phases may be repeated, but they MUST NOT be silently skipped when the
document changes project direction or defines a public contract.

### 5.1 Drafting and ideating

Purpose: explore the problem without presenting ideas as settled decisions.

Required practices:

- Label proposals as proposed.
- Separate goals from implementation ideas.
- Identify competing approaches.
- Record known unknowns.
- Avoid prematurely naming files or APIs unless that helps comparison.
- State which assumptions require confirmation.

Output:

- Problem statement
- Candidate approaches
- Constraints
- Open questions
- Initial recommendation, if one exists

### 5.2 Researching

Purpose: replace uncertain assumptions with evidence.

Research is required when information is current, external, version-dependent,
security-sensitive, or unfamiliar.

Required practices:

- Prefer primary or authoritative sources.
- Record source, date, and the claim supported by the source.
- Distinguish facts from inferences.
- Identify version-specific behavior.
- Do not turn a single example into a universal rule.
- Update the design when research disproves an assumption.

Output:

- Evidence-backed constraints
- Compatibility findings
- Rejected assumptions
- Remaining uncertainties

### 5.3 Generating

Purpose: create or modify the requested artifact or implementation.

Required practices:

- Confirm the scope before writing.
- Use the project’s established naming and directory conventions.
- Preserve unrelated user work.
- Keep generated content deterministic where possible.
- Keep framework-specific wiring outside portable service logic.
- Record new dependencies and side effects.

Generated files MUST include enough surrounding context to remain understandable
when viewed independently.

### 5.4 Validating and verifying

Purpose: determine whether the result is correct, coherent, and safe.

Validation MUST cover the applicable categories:

- Structural: files exist in the intended locations.
- Syntactic: formats parse and code imports.
- Contract: inputs, outputs, and errors match schemas.
- Behavioral: stated capabilities work as described.
- Deterministic: repeated normalized input produces stable output.
- Architectural: dependency direction is preserved.
- Safety: paths, writes, secrets, and destructive operations are controlled.
- Documentation: claims match the implementation.

Verification evidence MUST be reported. “Looks correct” is not sufficient for
public contracts or destructive behavior.

### 5.5 Canonicalization

Purpose: convert exploratory or duplicated material into the authoritative form.

Canonicalization occurs after validation, not before it.

Required practices:

- Remove superseded statements.
- Resolve contradictions.
- Use one authoritative name for each concept.
- Move examples out of normative rules when they create ambiguity.
- Normalize headings, terminology, ordering, and formatting.
- Record unresolved issues instead of hiding them.
- Update cross-references.
- Mark the document status accurately.

## 6. Canonicalization standards

### 6.1 Source of truth

Each rule, contract, or decision MUST have one authoritative location.

Other documents MAY summarize it, but they MUST link to or clearly identify the
source of truth. Duplicated normative text is a maintenance defect.

### 6.2 Naming

Names MUST be:

- Lowercase for Python modules and directories
- `snake_case` for Python identifiers
- Stable once referenced by another document or service
- Specific enough to distinguish domain concepts
- Free of vendor names in portable layers

Renaming a public concept requires a migration note or compatibility alias.

### 6.3 Ordering

Canonical lists SHOULD use a stable ordering:

1. Conceptual dependencies first
2. Public interfaces before implementations
3. Required items before optional items
4. Alphabetical ordering where no semantic order exists

### 6.4 Status labels

Documents and major sections SHOULD use one of these statuses:

```text
proposed
draft
planned
in-progress
validated
accepted
deprecated
superseded
blocked
```

Do not label work accepted when it has only been drafted.

## 7. Document lifecycle

Every project document follows this lifecycle:

```text
created → drafted → reviewed → validated → canonical → maintained → archived
```

### 7.1 Creation

Before creating a document, determine whether an existing document should be
updated instead. New documents require a distinct responsibility or audience.

### 7.2 Review

Review MUST check:

- Scope and audience
- Terminology
- Contradictions with control documents
- Missing dependencies
- Unsupported claims
- Unclear ownership
- Implementation consequences

### 7.3 Maintenance

When implementation changes a documented contract, update the document in the
same task or explicitly record the documentation debt.

### 7.4 Archiving

Archive a document when it is no longer authoritative but remains useful for
historical context. Mark it `deprecated` or `superseded` and link to the replacement.

Do not delete historical material merely to make the repository look cleaner.
Deletion requires explicit authorization and a recoverable path.

## 8. Request formatting standard

Requests SHOULD use this structure:

```text
Objective:
<The result that is wanted.>

Scope:
<Systems, files, or decisions included.>

Constraints:
<Technical, safety, timing, or compatibility constraints.>

Existing context:
<Relevant documents, decisions, or prior work.>

Deliverables:
<Files, recommendations, implementation, or analysis expected.>

Acceptance criteria:
<How completion will be judged.>

Exclusions:
<Actions that are not authorized or not wanted.>
```

Requests MUST distinguish between:

- Discussion and implementation
- Suggestions and decisions
- Diagnosis and remediation
- Read-only inspection and state-changing action
- Portable core work and host-specific integration

If the request is ambiguous in a way that changes the deliverable, ask before
proceeding. Do not silently choose a materially different interpretation.

## 9. Response formatting standard

Responses SHOULD use this order:

```text
Status:
<completed | partially completed | blocked | needs decision>

Outcome:
<What happened or what was determined.>

Changes:
<Files or external state changed, if any.>

Evidence:
<Tests, inspections, sources, or validation performed.>

Risks and limitations:
<Known gaps, incompatibilities, or assumptions.>

Open decisions:
<Questions that require direction.>

Next action:
<The smallest useful next step.>
```

Responses MUST NOT imply that a proposal was implemented. They MUST identify
partial completion and failed validation clearly.

## 10. Universal service conventions

Every portable service MUST define:

- Service identifier
- Operation identifier
- Protocol version
- Request schema
- Response schema
- Error codes
- Required capabilities
- Side effects
- Determinism behavior
- Idempotency behavior
- Resource limits
- Test strategy

The preferred execution forms are:

```text
Direct Python API
Generic stdin/stdout JSON process
Optional transport adapter
```

Portable code MUST NOT require:

- Agent lifecycle hooks
- A particular agent SDK
- A model provider
- Implicit network access
- An unspecified database
- An unbounded current working directory
- Hidden environment variables

## 11. Safety and change control

Before changing files or external state:

1. Identify the exact target.
2. Confirm that the action is within scope.
3. Determine whether the action is destructive or difficult to reverse.
4. Define a rollback strategy.
5. Obtain explicit authorization when required.
6. Perform the smallest change that satisfies the request.
7. Verify the result.

For generated or overwritten files:

- Compare before replacing when practical.
- Preserve user-owned work.
- Use atomic writes for implementation code.
- Keep backups or version-control recovery available.
- Report what changed.

## 12. Definition of done

A task is complete only when all applicable items are true:

- Scope was understood and respected.
- Required files or decisions exist.
- Naming and structure match the canonical documents.
- Dependencies are documented.
- Tests or validation were performed.
- Known limitations are reported.
- Cross-references are current.
- No unresolved contradiction was hidden.
- The result is ready for the next person or task to use.

## 13. Nonconformance handling

When a convention cannot be followed:

1. State which convention is being violated.
2. Explain why it cannot be followed.
3. Describe the impact.
4. Define the temporary workaround.
5. Record the corrective action or decision owner.

Do not normalize an exception by silently repeating it.

## 14. Minimum auditable task record

Every task MUST have an auditable record, regardless of size. A minute task may
use a compact record, but it may not omit the record.

The minimum record MUST contain:

| Field | Required content |
|---|---|
| Task ID | Unique identifier or parent task reference |
| Objective | One concrete outcome |
| Scope | Exact files, decisions, or systems affected |
| Authority | The request or decision authorizing the work |
| Inputs | Documents, data, or prior state used |
| Plan | Ordered actions, including the validation action |
| Dependencies | Preconditions and required capabilities |
| Failure conditions | Conditions that stop or fail the task |
| Rollback | How changes will be reversed, if applicable |
| Completion gates | Conditions required before closure |
| Evidence | Tests, inspections, hashes, or other verification |
| Final status | Completed, partially completed, blocked, or failed |

### 14.1 Minute-task format

For small tasks, the following compact record is sufficient:

```text
Task: <one-sentence objective>
Scope: <exact target>
Authority: <request or decision reference>
Plan: <action> → <validation> → <close>
Fail if: <condition 1>; <condition 2>
Rollback: <reversal or not applicable>
Gate: <completion condition>
Evidence: <validation result>
Status: <completed | partial | blocked | failed>
```

The record may appear in a task response, commit description, change log, or
other approved project record. It MUST be recoverable by someone who did not
perform the task.

## 15. Failing conditions

A task MUST stop and be marked failed or blocked when any of the following occurs:

- The requested target cannot be identified exactly.
- The action exceeds the authorized scope.
- A required input, dependency, or permission is missing.
- The proposed change would contradict a control document.
- A safety or policy check rejects the action.
- Validation produces an unexpected result.
- The output is incomplete, malformed, or non-deterministic when determinism is required.
- A destructive action lacks a rollback path.
- The implementation and documentation disagree after the change.
- The task cannot produce sufficient evidence for its completion claim.

Failure handling MUST include:

1. Stop the affected task or isolate the failing step.
2. Preserve the relevant state and evidence.
3. Identify the exact failure condition.
4. Roll back when a safe rollback exists.
5. Report whether the task is failed, blocked, or partially completed.
6. Do not continue by silently weakening the acceptance criteria.

### 15.1 Failure classifications

| Classification | Meaning | Required response |
|---|---|---|
| `invalid_scope` | The requested action is ambiguous or exceeds authority | Stop and clarify |
| `precondition_failed` | Required input or dependency is unavailable | Stop and report the missing prerequisite |
| `validation_failed` | The result does not meet its criteria | Roll back or mark incomplete |
| `safety_rejected` | Policy or safety rules disallow the action | Do not bypass; escalate if needed |
| `infrastructure_failed` | Tool, filesystem, or runtime failure | Preserve evidence and retry only if authorized |
| `documentation_drift` | Implementation and documentation disagree | Update the correct source of truth before closure |
| `evidence_insufficient` | Completion cannot be proven | Keep the task open or mark it incomplete |

## 16. Completion gates

Every task MUST pass the applicable gates in order. A minute task may satisfy
multiple gates in one compact record, but it may not omit the decisions or
evidence represented by those gates.

The project uses three levels of gates:

```text
Micro-gates  →  applied to individual actions
Task gates   →  applied to a complete task or change
Phase gates  →  applied to MVP, Phase 1, and Phase 2 transitions
```

### 16.1 Universal micro-gates

The following checkpoints apply to every task:

```text
G00  Request received
G01  Objective extracted
G02  Scope and exclusions confirmed
G03  Authority confirmed
G04  Target identified
G05  Current state inspected
G06  Dependencies checked
G07  Failure conditions defined
G08  Rollback defined
G09  Validation method defined
G10  Plan recorded
G11  Pre-change baseline captured
G12  Action executed
G13  Immediate result inspected
G14  Output and schema validated
G15  Behavioral result verified
G16  Side effects inspected
G17  Documentation synchronized
G18  Canonical location confirmed
G19  Evidence recorded
G20  Task closed
```

The gate record MAY be compact. It MUST still answer:

- What was requested?
- What exact target was changed or inspected?
- What was expected to happen?
- What would have caused the task to stop?
- What evidence proves the result?
- What is the final status?

### 16.2 Task-level gates

Micro-gates are grouped into the following task-level checkpoints:

| Gate | Required decision |
|---|---|
| Intake gate | The request is understood well enough to act |
| Scope gate | The exact boundaries and exclusions are known |
| Authority gate | The action is authorized |
| Baseline gate | The starting state is captured or confirmed |
| Dependency gate | Required inputs and capabilities are available |
| Risk gate | Failure conditions and rollback are defined |
| Plan gate | Actions and validation steps are ordered |
| Execution gate | Only approved actions are performed |
| Inspection gate | Immediate results are checked before proceeding |
| Contract gate | Structure, schemas, and formats are valid |
| Behavior gate | The result performs the intended function |
| Side-effect gate | No unintended changes occurred |
| Documentation gate | Affected documentation matches the result |
| Canonicalization gate | One authoritative version remains |
| Evidence gate | Completion can be independently checked |
| Closure gate | Final status and next action are explicit |

### 16.3 Risk-adjusted gate policy

The gate model is universal, but the amount of evidence scales with risk.

#### Minute task

The minimum record is:

```text
request → target → plan → action → validation → evidence → closure
```

#### Standard task

The task MUST additionally record:

- Baseline state
- Dependencies
- Failure conditions
- Rollback
- Contract validation
- Behavioral verification
- Documentation impact

#### High-risk task

The task MUST additionally include, where applicable:

- Explicit approval
- Dry run or preview
- Backup verification
- Intermediate execution checkpoint
- Regression check
- Side-effect audit
- Recovery or rollback test
- Closure review

Small size does not reduce a task's risk classification. A one-line change to a
security rule or public protocol may require the high-risk gate policy.

### 16.4 Phase gates

Phase transitions require evidence beyond individual task completion.

#### MVP-to-Phase-1 gate

- Architecture and dependency rules are agreed.
- Canonical protocol boundaries are defined.
- Scope excludes host-specific lifecycle behavior.
- Initial schemas and naming conventions are stable enough to implement.
- Implementation risks and deferred work are documented.

#### Phase-1-to-Phase-2 gate

- At least three services run through direct Python and stdin/stdout.
- Contract failures are stable and documented.
- Deterministic outputs have golden tests.
- Read-only workspace operations work in isolated fixtures.
- Architecture tests reject invalid imports.
- At least one protocol revision has been simulated.
- Phase 1 limitations are recorded rather than carried implicitly.

#### Phase-2 acceptance gate

- Capability negotiation distinguishes supported, unsupported, and degraded states.
- Protocol evolution has migration tests.
- Context packages include provenance, priority, and budget information.
- State can be serialized, restored, and replayed.
- Policy results distinguish advisory from enforceable decisions.
- Persistence implementations remain replaceable through ports.
- Performance and determinism regressions are measured.

No task or phase may be reported as complete without passing its applicable
closure gate and recording the evidence.

## 17. Periodic review

Review this SOP when:

- A new phase begins.
- The canonical protocol changes.
- A host adapter reveals a structural incompatibility.
- A repeated process failure occurs.
- The repository gains a new bounded context.
- A convention is repeatedly ignored or misunderstood.

The SOP should remain short enough to follow and specific enough to enforce.
If a rule cannot be checked, it should be rewritten as a clearer practice or
removed from the mandatory section.
