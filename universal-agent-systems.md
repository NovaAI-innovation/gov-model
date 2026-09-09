# Universal Agent Systems — MVP rascii

Status: MVP architecture baseline

This document is the repository ASCII control document. It defines the intended
repository shape and keeps implementation aligned with the project objective:

> Provide portable agent services and a canonical interchange protocol that can
> be invoked by agents with different architectures, transports, and formats.

The MVP is a modular monolith. It is not a collection of host plugins, not a
microservice fleet, and not a universal lifecycle-hook framework.

## Design rules

- Domain logic does not import transport, framework, or host-specific code.
- Application services coordinate use cases but do not perform direct I/O.
- Ports define external capabilities; infrastructure implements them.
- Delivery translates requests into application calls and results back into wire data.
- Host-specific adapters remain outside the portable core.
- Every service declares inputs, outputs, side effects, errors, and capabilities.
- Unsupported behavior must be explicit; silent degradation is prohibited.
- Deterministic services must produce stable output for normalized identical input.

## Target hierarchy

```text
agent-interop/                                      # Root repository for the compatibility layer
├── docs/                                           # Human-maintained project and specification documentation
│   ├── architecture/                              # Structural, layering, and dependency documentation
│   ├── specifications/                             # Formal protocol and behavior specifications
│   └── decisions/                                  # Architecture Decision Records and rejected alternatives
│
├── schemas/                                        # Versioned machine-readable wire contracts
│   ├── v1/                                         # First stable protocol schema version
│   └── compatibility/                              # Migration rules between schema versions
│
├── src/                                            # Production Python source
│   └── agent_interop/                              # Main Python package
│       ├── shared/                                 # Cross-cutting primitives with no business ownership
│       ├── protocol/                               # Canonical request, response, capability, and error protocol
│       ├── ports/                                  # Abstract interfaces required by application logic
│       │
│       ├── contexts/                               # Bounded reusable capabilities
│       │   ├── contracts/                          # Contract definitions and invariant validation
│       │   │   ├── domain/                          # Contract entities, rules, and violations
│       │   │   ├── application/                     # Contract validation use cases
│       │   │   └── ports/                           # Contract persistence or retrieval interfaces
│       │   ├── artifacts/                          # Deterministic ASCII, Mermaid, and manifest generation
│       │   │   ├── domain/                          # Artifact models, graphs, and canonicalization rules
│       │   │   ├── application/                     # Artifact generation and validation use cases
│       │   │   └── ports/                           # Renderer and artifact-storage interfaces
│       │   ├── workspace/                          # Inventory, hashing, diffing, and safe file operations
│       │   │   ├── domain/                          # Path, file, ownership, and workspace rules
│       │   │   ├── application/                     # Workspace inspection and comparison use cases
│       │   │   └── ports/                           # Filesystem and workspace access interfaces
│       │   ├── health/                             # Environment and dependency health evaluation
│       │   │   ├── domain/                          # Health statuses, findings, and policies
│       │   │   ├── application/                     # Health-check orchestration use cases
│       │   │   └── ports/                           # Health-check provider interfaces
│       │   ├── context_packages/                   # Context assembly and deterministic selection
│       │   │   ├── domain/                          # Context items, budgets, priorities, and provenance
│       │   │   ├── application/                     # Candidate collection, ranking, and package construction
│       │   │   └── ports/                           # Context-source interfaces
│       │   ├── evaluation/                         # Output validation, scoring, and regression comparison
│       │   │   ├── domain/                          # Criteria, scores, findings, and evaluation models
│       │   │   ├── application/                     # Evaluation and report-generation use cases
│       │   │   └── ports/                           # Evaluator and baseline interfaces
│       │   ├── policy/                             # Risk classification and policy decision logic
│       │   │   ├── domain/                          # Actions, risks, decisions, and policy rules
│       │   │   ├── application/                     # Policy evaluation use cases
│       │   │   └── ports/                           # Policy source and configuration interfaces
│       │   ├── execution_state/                    # Framework-neutral execution state transitions
│       │   │   ├── domain/                          # States, transitions, guards, and events
│       │   │   ├── application/                     # State transition, replay, and completion use cases
│       │   │   └── ports/                           # Snapshot and state persistence interfaces
│       │   ├── memory/                             # Portable records, ranking, retention, and conflicts
│       │   │   ├── domain/                          # Memory records, namespaces, and retention rules
│       │   │   ├── application/                     # Memory creation, retrieval, and consolidation
│       │   │   └── ports/                           # Memory storage interfaces
│       │   └── recovery/                           # Failure classification and recovery recommendations
│       │       ├── domain/                          # Failure types, retry rules, and decisions
│       │       ├── application/                     # Recovery classification and backoff use cases
│       │       └── ports/                           # Checkpoint and state-storage interfaces
│       │
│       ├── observability/                          # Events, provenance, metrics, and manifests
│       │   ├── domain/                              # Event and metric models
│       │   ├── application/                         # Event processing and aggregation use cases
│       │   └── ports/                               # Event sink interfaces
│       │
│       ├── delivery/                               # Inbound invocation mechanisms
│       │   ├── direct/                              # Direct Python API invocation
│       │   ├── process/                             # Generic stdin/stdout JSON invocation
│       │   └── mcp/                                 # Optional MCP transport adapter
│       │
│       ├── infrastructure/                         # Concrete implementations and composition
│       │   ├── composition/                         # Dependency injection and application assembly
│       │   ├── configuration/                       # Configuration loading and validation
│       │   ├── filesystem/                          # Local filesystem implementations
│       │   ├── persistence/                         # In-memory and file-backed stores
│       │   ├── serialization/                       # Concrete JSON and other serializers
│       │   ├── time/                                # System and test clock implementations
│       │   └── logging/                             # Structured logging implementations
│       │
│       └── host_adapters/                           # Agent-specific lifecycle and format translation
│           ├── generic_process/                     # Minimal adapter for process-capable agents
│           ├── claude_code/                         # Claude Code event and hook translations
│           ├── codex/                               # Codex event and hook translations
│           ├── hermes/                              # Hermes event and hook translations
│           └── agent_zero/                          # Agent Zero extension and tool translations
│
├── tests/                                           # Automated verification
│   ├── architecture/                               # Enforces dependency and layering rules
│   ├── unit/                                       # Tests domain and application logic in isolation
│   ├── integration/                                # Tests infrastructure and delivery paths
│   ├── contract/                                   # Tests schemas, envelopes, and versions
│   ├── compatibility/                              # Tests host-specific mappings
│   ├── fixtures/                                   # Reusable test inputs and fake dependencies
│   └── snapshots/                                  # Stable expected outputs
│
├── examples/                                       # Runnable usage examples, not production logic
│   ├── direct_python/                              # Direct library usage
│   ├── stdin_stdout/                               # Generic process-protocol usage
│   ├── mcp/                                        # MCP transport usage
│   └── compatibility/                              # Adapter mapping examples
│
└── tools/                                          # Development and repository-maintenance utilities
```

## Explicitly deferred

Host-specific hooks, MCP registration, model calls, remote services, vector
databases, and agent-specific permission enforcement are not MVP domain logic.
They may use the portable protocol later, but they must not shape the core.

