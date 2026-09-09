# Universal Agent Systems — Three-Phase Implementation Plan

Status: planning baseline

This plan treats the existing `universal-agent-systems.md` as the MVP rascii.
Phase 1 and Phase 2 extend the MVP without changing the central rule: portable
service logic must not depend on agent-specific lifecycle behavior.

## Delivery strategy

Use a modular monolith first. Enforce boundaries with tests and import checks.
Do not split packages into deployable services until there is a demonstrated
need for independent scaling, release cadence, ownership, or fault isolation.

```text
MVP
  ↓ stabilizes the protocol and repository boundaries
Phase 1
  ↓ proves portable service execution
Phase 2
  ↓ adds state, negotiation, provenance, and scalable interoperability
Future adapters
  ↓ translate host-specific events without changing the core
```

## Cross-phase dependency rules

- Domain code depends only on shared primitives and domain-local code.
- Application code depends on domain code and abstract ports.
- Infrastructure implements ports and is assembled at the outer boundary.
- Delivery translates external requests and responses.
- Host adapters translate host semantics only.
- Wire schemas are versioned independently from internal models.
- Optional capabilities must be declared rather than inferred.
- All writes require explicit scope and a rollback strategy.

## Phase summary

| Phase | Primary result | Cost | Risk | Recommendation |
|---|---|---:|---:|---|
| MVP | Stable repository shape and protocol direction | Low | Low | Required first |
| Phase 1 | Tested portable services | Medium | Medium | Highest practical value |
| Phase 2 | Scalable interoperability foundation | High | Medium-high | Start only after Phase 1 evidence |

## MVP — existing baseline

### Purpose

Define the architecture, boundaries, naming, and universal execution assumptions.
The MVP should remain small enough to revise without migration pain.

### Files created or maintained

| File or directory | Dependencies | Capability or benefit | Cost and risk | Why it exists |
|---|---|---|---|---|
| `universal-agent-systems.md` | None | Repository control document | Low cost; risk of becoming stale | Keeps the intended structure visible and reviewable |
| `docs/` | None | Stores specifications and decisions | Low cost; documentation drift | Separates design decisions from implementation |
| `schemas/` | Protocol decisions | Defines external data contracts | Medium cost; premature schemas can fossilize bad decisions | Makes compatibility explicit |
| `src/agent_interop/shared/` | Python standard library | Stable primitives and result types | Low cost; can become a dumping ground | Provides a small shared kernel |
| `src/agent_interop/protocol/` | Shared primitives | Canonical envelopes, errors, versions | Medium cost; protocol changes require discipline | Creates the abstraction boundary between callers and services |
| `src/agent_interop/ports/` | Shared primitives | Abstract external capabilities | Low cost; overly broad ports become vague | Keeps application logic independent of infrastructure |
| `src/agent_interop/contexts/` | Shared primitives and ports | Organizes domain capabilities | Medium cost; poor boundaries create coupling | Provides scalable bounded contexts |
| `src/agent_interop/delivery/` | Protocol and application services | Direct, process, and future transport entry points | Low-medium cost; transport leakage is possible | Gives callers stable invocation paths |
| `src/agent_interop/infrastructure/` | Ports and application services | Concrete implementations and composition | Medium cost; can become a service locator | Keeps technical details at the edge |
| `tests/architecture/` | Source tree | Detects dependency violations | Low cost; tests require maintenance | Prevents gradual architectural erosion |

### MVP capability

The MVP is successful if the project can explain, in a stable way:

- What a service receives
- What a service returns
- Which capabilities it requires
- Which side effects it may perform
- What errors mean
- Which layer owns each responsibility

### MVP cost versus risk

The cost is low because no runtime integrations are required. The main risk is
false confidence: a clean directory tree does not prove that the protocol works.
The MVP must therefore remain a design baseline, not be treated as a completed
implementation.

## Phase 1 — portable foundations

### Phase goal

Prove that the protocol and architecture support useful services without an
agent runtime, network, database, or model call.

### Files created

| File or directory | Depends on | Capability or benefit | Cost versus risk | Why now |
|---|---|---|---|---|
| `schemas/v1/request-envelope.schema.json` | Protocol decisions | Validates incoming requests | Low cost; schema mistakes affect all callers | Establishes the wire boundary |
| `schemas/v1/response-envelope.schema.json` | Protocol decisions | Validates successful responses | Low cost; may need revisions | Makes results interoperable |
| `schemas/v1/error.schema.json` | Error model | Standardizes failures | Low cost; vague codes create confusion | Prevents host-specific error interpretation |
| `src/agent_interop/shared/result.py` | None | Consistent success and failure values | Low cost; can become over-abstracted | Every service needs a common result shape |
| `src/agent_interop/shared/errors.py` | None | Stable error categories and codes | Low cost; errors require governance | Errors are part of the compatibility contract |
| `src/agent_interop/protocol/envelope.py` | Shared result and errors | Request/response envelope models | Medium cost; versioning must be deliberate | Converts protocol documentation into executable types |
| `src/agent_interop/protocol/serialization.py` | Envelope models | JSON encoding and decoding | Low cost; malformed input risk | Enables process-level interoperability |
| `src/agent_interop/protocol/schema_validation.py` | JSON schemas | Runtime contract validation | Medium cost; extra validation overhead | Fails invalid data at the boundary |
| `src/agent_interop/contexts/contracts/` | Protocol and shared validation | Defines reusable contracts and invariants | Medium cost; rules can be overspecified | Contracts are the first practical service |
| `src/agent_interop/contexts/artifacts/` | Shared models | Generates stable ASCII, Mermaid, and manifests | Low-medium cost; formatting churn is possible | Demonstrates deterministic output |
| `src/agent_interop/contexts/workspace/` | Filesystem port | Inventory, hashing, and read-only diffs | Medium cost; platform differences | Provides useful local-state inspection |
| `src/agent_interop/contexts/health/` | Ports and workspace services | Reports environment readiness | Low-medium cost; checks can become host-specific | Enables predictable preflight diagnostics |
| `src/agent_interop/contexts/evaluation/` | Contracts and artifacts | Validates outputs and snapshots | Medium cost; evaluation criteria require judgment | Creates a feedback loop for quality |
| `src/agent_interop/delivery/process/` | Protocol and application services | Runs services through stdin/stdout JSON | Low cost; process startup overhead | This is the broadest common invocation surface |
| `src/agent_interop/infrastructure/filesystem/` | Filesystem port | Concrete local filesystem access | Medium cost; writes require safety controls | Keeps I/O outside application logic |
| `src/agent_interop/infrastructure/persistence/in_memory_store.py` | Storage ports | Testable state without external services | Low cost; not production persistence | Enables isolated tests |
| `tests/contract/` | Schemas and protocol | Protects wire compatibility | Low cost; requires updates with intentional changes | Prevents accidental protocol drift |
| `tests/snapshots/` | Artifact and reporting services | Detects formatting changes | Low cost; snapshots can be noisy | Supports deterministic artifact generation |
| `tests/architecture/` | Source tree | Enforces import boundaries | Low cost; rules need maintenance | Prevents clean architecture decay |

### Phase 1 capabilities

- Validate requests before application logic runs.
- Return stable, machine-readable responses.
- Generate deterministic ASCII and Mermaid output.
- Inspect a workspace without requiring an agent framework.
- Hash and compare files safely.
- Run health checks with explicit inputs.
- Evaluate outputs against deterministic criteria.
- Invoke all first-party services through stdin/stdout JSON.

### Phase 1 cost and risk assessment

The main cost is protocol discipline and test coverage. The main risks are:

- Designing schemas before real use cases expose their weaknesses
- Treating local filesystem behavior as identical across operating systems
- Allowing convenience utilities to bypass ports
- Letting artifact formatting become unstable
- Adding too many services before the request/response contract settles

The mitigation is to implement only a few vertical slices and require contract,
unit, integration, and determinism tests for each one.

### Phase 1 exit gate

Do not begin Phase 2 until the following are true:

- At least three services run through both Python and stdin/stdout.
- Contract failures are stable and documented.
- Golden outputs are deterministic.
- Read-only filesystem operations work in isolated fixtures.
- Architecture tests reject invalid imports.
- At least one protocol revision has been simulated in tests.

## Phase 2 — scalable interoperability

### Phase goal

Add the structures needed to support different agent capabilities without
pretending that their lifecycle or permission models are identical.

### Files created

| File or directory | Depends on | Capability or benefit | Cost versus risk | Why now |
|---|---|---|---|---|
| `src/agent_interop/protocol/capability.py` | Protocol v1 | Describes supported operations and limits | Medium cost; capability inflation is possible | Callers need to know what is actually supported |
| `src/agent_interop/protocol/negotiation.py` | Capability and version models | Handles compatible and unsupported requests | Medium cost; negotiation can become complex | Prevents silent degradation between agents |
| `schemas/v1/capability.schema.json` | Capability model | Makes capabilities machine-readable | Low cost; schema must stay conservative | Allows generic callers to inspect services |
| `src/agent_interop/contexts/context_packages/` | Workspace and protocol | Builds bounded, provenance-aware context packages | High cost; context relevance is hard to measure | Context is the main cross-agent data problem |
| `src/agent_interop/contexts/execution_state/` | Shared events and storage port | Models replayable state transitions | Medium-high cost; state semantics vary by host | Gives adapters a stable state vocabulary |
| `src/agent_interop/observability/` | Event and serialization ports | Records structured events and provenance | Medium cost; excessive logging creates noise | Debugging compatibility requires traceability |
| `src/agent_interop/contexts/policy/` | Contracts and workspace models | Produces risk and policy decisions | Medium cost; decisions may be mistaken for enforcement | Separates portable evaluation from host enforcement |
| `src/agent_interop/contexts/recovery/` | Error and execution models | Recommends retry and recovery actions | Medium cost; hosts may ignore recommendations | Makes failure behavior explicit without owning execution |
| `src/agent_interop/contexts/memory/` | Storage ports and context models | Provides portable memory record logic | High cost; persistence and relevance are difficult | Adds value only after core contracts are stable |
| `src/agent_interop/infrastructure/persistence/json_file_store.py` | Storage ports | Provides transparent local persistence | Medium cost; corruption and concurrency risks | Supplies a replaceable reference implementation |
| `src/agent_interop/infrastructure/persistence/jsonl_event_store.py` | Observability events | Stores append-only event history | Medium cost; rotation and recovery are needed | Supports replay and audit without a database |
| `src/agent_interop/infrastructure/composition/` | All application services and ports | Assembles a runnable application | Medium cost; composition can hide dependencies | Creates one controlled construction boundary |
| `tests/compatibility/` | Protocol and host-independent mappings | Tests supported and unsupported capabilities | Medium cost; matrices grow quickly | Compatibility claims require evidence |
| `tests/integration/` | Infrastructure and delivery | Verifies real process and persistence flows | Medium cost; platform variance | Unit tests alone will miss boundary failures |
| `tools/check_determinism.py` | Artifact and protocol outputs | Detects unstable serialization and rendering | Low-medium cost | Determinism is a stated product requirement |

### Phase 2 capabilities

- Capability discovery and negotiation.
- Version-aware request handling.
- Context packages with priorities, budgets, and provenance.
- Replayable execution state.
- Structured event and provenance history.
- Portable policy evaluation.
- Explicit recovery recommendations.
- Replaceable memory storage.
- Local persistence without committing to a database.

### Phase 2 cost and risk assessment

Phase 2 is substantially more expensive because it introduces state, versioning,
and cross-service behavior. The main risks are:

- Building a generic capability model that no caller understands
- Creating a memory abstraction that hides incompatible storage semantics
- Confusing policy recommendations with enforcement
- Allowing protocol negotiation to become a second programming language
- Adding persistence before recovery and corruption behavior are specified
- Expanding scope into agent orchestration

The mitigation is to keep every feature optional, versioned, and testable through
the generic process interface. No Phase 2 feature should require an agent hook.

### Phase 2 exit gate

- Capability negotiation has explicit unsupported and degraded states.
- Protocol version changes have migration tests.
- Context packages are bounded and provenance-aware.
- State can be serialized, restored, and replayed.
- Policy outputs clearly identify advisory versus enforceable decisions.
- Persistence implementations can be replaced through ports.
- Performance and determinism regressions are measured.

## File creation order

### Order 1 — protocol spine

1. Shared result and error models
2. Request and response envelopes
3. JSON serialization
4. Version and schema validation
5. Process dispatcher

### Order 2 — first vertical slices

1. Contract validation
2. Artifact generation
3. Workspace inventory
4. Health reporting
5. Output evaluation

### Order 3 — boundary protection

1. Architecture import tests
2. Contract tests
3. Deterministic snapshots
4. Process integration tests
5. Documentation and examples

### Order 4 — Phase 2 expansion

1. Capability model
2. Version negotiation
3. Context packages
4. Execution state
5. Observability
6. Policy evaluation
7. Recovery recommendations
8. Memory ports and reference stores

## Final project risks

| Risk | Impact | Mitigation |
|---|---|---|
| Universal scope becomes vague | High | Define compatibility as protocol-level portability |
| Host behavior leaks into core | High | Enforce import boundaries and separate host adapters |
| Protocol changes break callers | High | Version schemas and maintain migration tests |
| Security results are mistaken for enforcement | High | Mark decisions as advisory or enforceable explicitly |
| Context packages become unbounded | Medium-high | Require budgets, provenance, and omission reports |
| Persistence is introduced too early | Medium | Start with in-memory and JSON reference stores |
| Too many systems are built before validation | High | Require vertical-slice exit gates |
| Documentation drifts from code | Medium | Add schema, architecture, and snapshot checks to CI |

## What should not happen yet

- Do not build all host adapters during MVP.
- Do not add a database because memory is anticipated.
- Do not define lifecycle events that no current use case needs.
- Do not expose every internal class as a public protocol type.
- Do not call a policy result a security control until a host enforces it.
- Do not split the repository into microservices before the protocol survives real use.

