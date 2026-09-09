# Folder Ownership and Document Map

Status: active
Version: 1.0
Owner: Project governance
Approver: User
Canonical path: docs/governance/folder-ownership-and-document-map.md

## 1. Purpose

This map assigns one responsibility to each project folder. It prevents
governance rules, machine schemas, runtime code, tests, examples, and reusable
templates from becoming competing sources of truth.

## 2. Documentation folders

### docs/governance/

Contains project-wide authority, scope, safety, change, release, and document
lifecycle policies. It must not contain runtime implementation details.

Expected documents:

- Governance index
- Authority and decision rights
- Project charter
- Scope and priorities
- Safety and risk
- Change control
- Release policy
- Documentation lifecycle
- Document and contract registries

### docs/architecture/

Contains structural contracts for system boundaries, dependency direction,
bounded contexts, context maps, and architecture exceptions.

Expected documents:

- System boundaries
- Dependency rules
- Bounded contexts
- Context map
- Architecture exceptions

### docs/specifications/

Contains behavior and interoperability specifications, including protocol,
request, output, error, capability, compatibility, and negotiation rules.

### docs/decisions/

Contains decision records only. A decision record may explain a rule, but the
canonical rule belongs in its owning governance, architecture, or specification
folder.

### docs/quality/

Contains definition-of-done, testing, evidence, determinism, regression, and
quality-threshold contracts.

### docs/operations/

Contains observability, recovery, incident, performance, and operational
readiness contracts.

## 3. Machine-contract folders

### schemas/

Contains machine-readable validation contracts. Schemas must not redefine
meaning that is absent from the canonical human-readable specification.

### schemas/v1/

Contains shared primitives and version-specific schema components used by the
first published protocol version.

### schemas/requests/

Contains request-manifest schema families. Each family must be versioned and
refer to the applicable output contract.

### schemas/outputs/

Contains output envelope and output-contract schema families.

### schemas/gates/

Contains machine-readable gate definitions and gate-result schemas.

### schemas/compatibility/

Contains migration, compatibility, and version-transition rules.

## 4. Runtime source folders

### src/agent_interop/shared/

Contains framework-free primitives such as identifiers, results, errors, clocks,
and shared value objects. It must not become a general utility dumping ground.

### src/agent_interop/protocol/

Contains canonical wire models and serializers for requests, responses, errors,
capabilities, negotiation, and versioning.

### src/agent_interop/ports/

Contains abstract capabilities required by application logic. It contains
interfaces, not concrete infrastructure.

### src/agent_interop/contexts/

Contains bounded contexts. Each context owns its domain, application use cases,
and ports. Cross-context calls require explicit contracts or translation layers.

### src/agent_interop/observability/

Contains portable event, provenance, metric, and audit models plus their ports.

### src/agent_interop/delivery/

Contains inbound and outbound translation. preflight/ validates request
manifests before application execution; direct/, process/, and mcp/ translate
transport-specific representations.

### src/agent_interop/infrastructure/

Contains concrete adapters, configuration, persistence, serialization, time,
logging, and composition. Infrastructure must remain outside the portable core.

### src/agent_interop/host_adapters/

Contains agent- or host-specific lifecycle and format translation. Host behavior
must not shape portable domain logic.

## 5. Verification folders

### tests/architecture/

Verifies dependency direction, folder ownership, and prohibited imports.

### tests/unit/

Verifies domain and application behavior without external services.

### tests/integration/

Verifies infrastructure, composition, and delivery paths.

### tests/contract/

Verifies schemas, envelopes, request manifests, output contracts, and versions.

### tests/compatibility/

Verifies host mappings, protocol revisions, supported behavior, and unsupported
behavior.

### tests/fixtures/

Contains reusable, bounded test inputs and fake ports.

### tests/snapshots/

Contains deterministic expected outputs and golden artifacts.

## 6. Example, template, and tool folders

### examples/

Contains runnable demonstrations only. Examples may depend on public interfaces
but must not become production logic.

### templates/

Contains reusable, fillable instances of approved document and request types.
Templates must not silently create policy or authorization.

### tools/

Contains repository maintenance, validation, checking, helper, and scaffolding
utilities. Mutating tools must be explicit, previewable, and idempotent where
practical.

## 7. Folder placement decision rule

When a new file could belong in more than one folder:

1. Identify whether it defines meaning, validates data, executes behavior, or
   demonstrates behavior.
2. Place meaning in docs/.
3. Place machine validation in schemas/.
4. Place reusable shapes in templates/.
5. Place execution in src/.
6. Place verification in tests/.
7. Place maintenance and validation utilities in tools/.
8. Record an ADR when the placement changes an existing boundary.

## 8. Change history

| Version | Change | Authority |
|---|---|---|
| 1.0 | Established folder ownership and document routing | User request |
