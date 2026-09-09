# Phase 1 — Portable Foundations rascii

Status: planned

Phase 1 turns the MVP structure into a small, testable set of genuinely portable
services. The objective is to prove that the canonical protocol is useful before
adding stateful memory, lifecycle semantics, or host-specific adapters.

## Phase objective

Build services that can run through:

1. Direct Python imports
2. A generic stdin/stdout JSON process
3. A test harness with no agent runtime present

## In scope

- Canonical request and response envelopes
- Versioned JSON schemas
- Contract validation
- Deterministic artifact generation
- Read-only workspace inspection
- Health checks
- Deterministic output evaluation
- Architecture and protocol tests

## Out of scope

- Agent lifecycle hooks
- MCP registration
- Model calls
- Network services
- Long-term memory
- Automatic retries
- Permission enforcement
- Host-specific adapters

## Phase 1 structure

```text
Phase 1
├── shared primitives
├── protocol v1
├── contracts context
├── artifacts context
├── workspace read-only capabilities
├── health context
├── evaluation context
├── process delivery
├── schemas and fixtures
└── architecture and contract tests
```

## Exit criteria

- A request can be executed without an agent framework.
- Every supported operation has a versioned request and response schema.
- Invalid requests fail with stable error codes.
- Deterministic outputs are covered by golden tests.
- Workspace operations default to read-only behavior.
- No domain or application module imports a host adapter or transport library.
- The same normalized input produces the same result across direct and process invocation.

