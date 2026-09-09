# Phase 2 — Scalable Interoperability rascii

Status: planned

Phase 2 expands the portable core into a durable compatibility foundation. It
adds state models, context packaging, policy evaluation, provenance, recovery
recommendations, and memory interfaces without making any of those systems
dependent on a particular agent runtime or storage provider.

## Phase objective

Make the system useful as a stable abstraction layer between different agents,
while keeping host adapters and enforcement outside the core.

## In scope

- Capability negotiation
- Protocol version negotiation
- Context package construction
- Execution state modeling
- Structured observability and provenance
- Policy and risk evaluation
- Recovery recommendations
- Explicit memory interfaces
- File-backed and in-memory persistence
- Performance, architecture, and compatibility regression tests

## Out of scope

- Claiming universal lifecycle behavior
- Treating an MCP tool as a security boundary
- Embedding model-dependent decisions in the core
- Selecting a permanent database or vector store
- Building full Codex, Claude Code, Hermes, or Agent Zero plugins
- Automatic retries that the host did not request

## Phase 2 structure

```text
Phase 2
├── protocol negotiation
├── capability profiles
├── context packages
├── execution state
├── observability and provenance
├── policy evaluation
├── recovery recommendations
├── portable memory interfaces
├── explicit persistence implementations
└── compatibility and performance tests
```

## Exit criteria

- A caller can discover what a service supports before invoking it.
- Protocol evolution does not require rewriting domain logic.
- Context packages carry provenance, priority, and budget information.
- State transitions are replayable and testable without an agent runtime.
- Policy results distinguish advisory decisions from enforceable decisions.
- Recovery logic recommends actions without assuming that the host will apply them.
- Memory remains replaceable through ports.
- All persistence is explicit and testable.

