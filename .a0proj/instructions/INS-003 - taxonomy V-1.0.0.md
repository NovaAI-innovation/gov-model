# 03 — Taxonomy: where each artefact type lives

> Status: **Binding**. Authoring rule for any new file in this project.
> Companion: `INS-001 - project-charter V-1.0.0.md`, `INS-004 - git-and-remote-discipline V-1.1.0.md`.

---

## 1. Top-level layout (the project's substance)

| Folder | Purpose | Authoritative spec |
|---|---|---|
| `universal-agent-systems.md` | Repository control document (rascii) | MVP definition |
| `implementation-plan.md` | Three-phase delivery plan | Phases 0–2 |
| `phase-1-portable-foundations.md` | Phase 1 specification | Phase 1 |
| `phase-2-scalable-interop.md` | Phase 2 specification | Phase 2 |
| `standard-operating-procedure.md` | Canonical operating procedure | (substance-bearing) |
| `docs/` | Human-maintained project documentation | §2 |
| `schemas/` | Versioned wire contracts | §3 |
| `templates/` | Reusable authoring scaffolds | §4 |
| `tests/` | Test infrastructure, contract + architecture | §5 |
| `tools/` | Validation helpers and scaffolders | §6 |
| `src/agent_interop/` | Production Python implementation | bounded contexts in §7 |
| `examples/` | Reference invocations per transport | §8 |

`/a0/usr/projects/governance_model/.a0proj/` is the **agent binding**
layer (this directory), separate from the substance above.

## 2. `docs/` taxonomy

| Subfolder | Stores |
|---|---|
| `architecture/` | Layering, dependency direction, hexagonal ports/adapters |
| `decisions/` | Architecture Decision Records (ADR-NNNN-title.md) |
| `governance/` | Process rules, review cadences, sign-off matrices |
| `operations/` | Runbooks, alerting, incident response |
| `quality/` | Test policy, lint policy, coverage thresholds |
| `specifications/` | Protocol specifications: envelope, errors, capability declaration |

When authoring a new doc, place it under the subfolder that matches
its **purpose**, not its audience. Audience-indifferent docs (e.g.
"this explains a decision") still go to the most specific subfolder.

## 3. `schemas/` taxonomy

| Subfolder | Stores |
|---|---|
| `v1/` | First stable wire-schema version (the public surface) |
| `compatibility/` | Migration rules between schema versions |
| `gates/` | Request gates (input validation, capability matching) |
| `outputs/` | Output envelopes and standard response forms |
| `requests/` | Canonical request envelopes |
| `APJ-000 - binding-layer-entry-point V-1.0.0.md` | Index of schema versions and their status |

Authoring rule: any wire contract MUST land in `v<N>/` first; cross-
version migration rules land in `compatibility/` only after the
target version is stable.

## 4. `templates/` taxonomy

`templates/` is the authoring scaffold layer. The convention is **one
template per document kind**; each template enumerates the fields a
finished artefact must include.

| Subfolder | Authoring scaffold for |
|---|---|
| `architecture/` | Architecture documents |
| `decisions/` | ADR files (decision records) |
| `evidence/` | Evidence records (test runs, audits, attestations) |
| `governance/` | Governance documents (charter, policy, runbook) |
| `incidents/` | Incident reports |
| `outputs/` | Output templates (messaging, manifests) |
| `phases/` | Phase specifications |
| `architecture/`, `decisions/`, `evidence/`, `governance/`, `incidents/`, `outputs/`, `phases/` | (as above) |
| `APJ-000 - binding-layer-entry-point V-1.0.0.md` | Index of templates and what each generates |

When generating a new artefact, copy from the matching template
FIRST, then fill it in. Do not author from scratch.

## 5. `tests/` taxonomy

| Subfolder | Test kind |
|---|---|
| `architecture/` | Dependency-direction, layer-boundary, port-implementation conformance |
| `compatibility/` | Cross-version wire-schema compatibility matrices |
| `contract/` | Service contract conformance (inputs, outputs, errors) |
| `fixtures/` | Test data (do not commit secrets) |
| `integration/` | End-to-end service invocation across transport(s) |
| `snapshots/` | Stored artefact snapshots for regression diffing |
| `unit/` | Bounded-context unit tests |

Authoring rule: every public service MUST have a contract test under
`tests/contract/` and at least one integration test under
`tests/integration/`. Unit tests are required for any non-trivial
domain logic.

## 6. `tools/` taxonomy

| Subfolder | Tool kind |
|---|---|
| `APJ-000 - binding-layer-entry-point V-1.0.0.md` | Index of project tools |
| `checks/` | Stateless validators (run before commit; must exit 0) |
| `helpers/` | Convenience scripts used during dev and CI |
| `scaffolding/` | Codegen for new files (template-driven) |
| `validators/` | Stateful validators; may invoke the live runtime |

Authoring rule: any check that should block a commit MUST live in
`tools/checks/` and exit non-zero on failure. Wire it into the
project's pre-commit hook.

## 7. `src/agent_interop/` taxonomy (bounded contexts)

```
agent_interop/
├── shared/           cross-cutting primitives (no business ownership)
├── protocol/         canonical request/response/envelope/error/version
├── ports/            abstract external capabilities
├── delivery/         direct, process(stdin/stdout), mcp transports
├── infrastructure/   ports implementations + DI composition
├── contexts/
│   ├── contracts/        invariants + port interfaces (domain|application|ports)
│   ├── artifacts/        ASCII/Mermaid/manifest generation
│   ├── workspace/        inventory, hashing, diff, ownership
│   ├── health/           health-check state + providers
│   ├── context_packages/ candidate collection + ranking + assembly
│   ├── evaluation/       output validation + scoring + regression
│   ├── policy/           risk classification + decision rules
│   ├── execution_state/  framework-neutral state transitions
│   ├── memory/           portable records + ranking + retention
│   └── recovery/         failure classification + retry/recovery
└── observability/    events + provenance + metrics + manifests
```

**Authoring rule (the modular monolith invariant):**

- `domain/` code depends only on `shared/` and the same-context local code.
- `application/` code depends on `domain/` and abstract `ports/`.
- `infrastructure/` implements `ports/` and is assembled at the outer edge.
- `delivery/` translates between transport wire formats and `application/`.
- Domain code MUST NOT import transport, framework, or host-specific code.

`tests/architecture/` enforces these rules; do not bypass.

## 8. `examples/` taxonomy

One reference invocation per transport. New transports MUST add an
example here.

| Subfolder | Transport |
|---|---|
| `compatibility/` | Cross-version compatibility examples |
| `direct_python/` | Direct Python API invocation |
| `mcp/` | MCP transport examples |
| `stdin_stdout/` | Generic process stdin/stdout JSON examples |

## 9. Naming and casing

- Files: kebab-case with optional numeric prefix (`00-overview.md`,
  `15-decision-record.md`).
- ADR files: `ADR-NNNN-title.md`, zero-padded to 4 digits, in
  `docs/decisions/`.
- Bounded-context folders: snake_case (`context_packages/`,
  `execution_state/`).
- Module names: snake_case Python, PascalCase only for dataclasses /
  Pydantic models that follow framework norms.

## 10. Anti-pattern (binding)

- Do not author documents at the project root. The only allowed root
  files are the canonical control documents (charter, plans, SOP) and
  the binding-layer entry point (`.a0proj/`). New top-level files
  require an ADR.
- Do not author schemas without first publishing a corresponding
  specification under `docs/specifications/`.
- Do not author code under `src/agent_interop/` without first
  matching an existing or new bounded-context entry. Random
  sub-packages are forbidden.