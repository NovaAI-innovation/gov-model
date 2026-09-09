# 00 — Substrate index (knowledge/main)

> Status: **Entry point**. Read this first when entering the project.
> Source of truth files are linked below; this index only organises
> them. Companion: every file under `.a0proj/instructions/` and
> `.a0proj/skills/`.

---

## 0. How to use this index

1. Read `INS-001 - project-charter V-1.0.0.md` to know the project's stance.
2. Read this file to locate the artefact relevant to your task.
3. Open the artefact; do not paraphrase from this index — always
   follow the link.
4. If you discover that an artefact is missing, follow
   `INS-005 - decision-and-question-recording V-1.0.0.md` and tag the gap with
   `§GOV_OPEN§`. Do not invent a substitute artefact.

The agent MUST NOT cite this index as authority; the index points
to authority, it does not embody it.

## 1. Top-level control documents

| Path | Role |
|---|---|
| `universal-agent-systems.md` | Repository control document (rascii) — MVP definition |
| `implementation-plan.md` | Three-phase delivery plan |
| `phase-1-portable-foundations.md` | Phase 1 specification |
| `phase-2-scalable-interop.md` | Phase 2 specification |
| `standard-operating-procedure.md` | Canonical operating procedure |
| `.a0proj/APJ-000 - binding-layer-entry-point V-1.0.0.md` | Binding-layer entry point |
| `.a0proj/instructions/` | Project-scoped behavioural contract for the agent |
| `.a0proj/knowledge/` | Knowledge mirror (this file is here) |
| `.a0proj/skills/` | Loadable expertise |

## 2. Bounded context map

The project's portable-core contexts live under `src/agent_interop/contexts/`.
Use this map to find the documentation, schemas, templates, tests,
and examples that belong with each context.

| Context | Domain entry | Spec | Schema | Template | Tests | Examples |
|---|---|---|---|---|---|---|
| `contracts` | `src/agent_interop/contexts/contracts/` | `docs/specifications/contracts-v1.md` | `schemas/v1/contracts.json` | `templates/decisions/contract-record.md` | `tests/contract/contracts_*` | `examples/stdin_stdout/contracts_*.json` |
| `artifacts` | `src/agent_interop/contexts/artifacts/` | `docs/specifications/artifacts-v1.md` | `schemas/v1/artifacts.json` | `templates/outputs/artifact.md` | `tests/unit/artifacts_*` | `examples/direct_python/artifact_*.py` |
| `workspace` | `src/agent_interop/contexts/workspace/` | `docs/specifications/workspace-v1.md` | `schemas/v1/workspace.json` | `templates/outputs/workspace-manifest.md` | `tests/contract/workspace_*` | `examples/stdin_stdout/workspace_*.json` |
| `health` | `src/agent_interop/contexts/health/` | `docs/specifications/health-v1.md` | `schemas/v1/health.json` | `templates/outputs/health-report.md` | `tests/integration/health_*` | `examples/mcp/health_*.json` |
| `context_packages` | `src/agent_interop/contexts/context_packages/` | `docs/specifications/context-packages-v1.md` | `schemas/v1/context-packages.json` | `templates/outputs/context-package.md` | `tests/unit/context_packages_*` | `examples/direct_python/context_package_*.py` |
| `evaluation` | `src/agent_interop/contexts/evaluation/` | `docs/specifications/evaluation-v1.md` | `schemas/v1/evaluation.json` | `templates/evidence/evaluation-report.md` | `tests/integration/evaluation_*` | `examples/stdin_stdout/evaluation_*.json` |
| `policy` | `src/agent_interop/contexts/policy/` | `docs/specifications/policy-v1.md` | `schemas/v1/policy.json` | `templates/governance/policy-decision.md` | `tests/contract/policy_*` | `examples/direct_python/policy_*.py` |
| `execution_state` | `src/agent_interop/contexts/execution_state/` | `docs/specifications/execution-state-v1.md` | `schemas/v1/execution-state.json` | `templates/outputs/state-snapshot.md` | `tests/contract/execution_state_*` | `examples/stdin_stdout/state_*.json` |
| `memory` | `src/agent_interop/contexts/memory/` | `docs/specifications/memory-v1.md` | `schemas/v1/memory.json` | `templates/outputs/memory-record.md` | `tests/unit/memory_*` | `examples/direct_python/memory_*.py` |
| `recovery` | `src/agent_interop/contexts/recovery/` | `docs/specifications/recovery-v1.md` | `schemas/v1/recovery.json` | `templates/incidents/recovery-report.md` | `tests/contract/recovery_*` | `examples/stdin_stdout/recovery_*.json` |

If a cell is empty, it is a known gap; consult the open-questions
register rather than fabricating the artefact.

## 3. Layered responsibilities (from the control document)

```
delivery/        (transports: direct / process / mcp)
   │
   ▼
infrastructure/ (DI composition, filesystem, persistence)
   │
   ▼
application/     (use cases within each bounded context)
   │
   ▼
domain/          (entities, invariants, rules)
   │
   ▼
shared/          (cross-cutting primitives, no business ownership)
```

`ports/` abstract external capabilities. Domain code MUST NOT import
delivery, infrastructure, framework, or host-specific code.

`tests/architecture/` enforces this dependency direction.

## 4. Authoring scaffolds (templates)

| Kind | Template location |
|---|---|
| Architecture document | `templates/architecture/` |
| Architecture Decision Record | `templates/decisions/` (use `ADR-NNNN-title.md`) |
| Evidence record (test run, audit, attestation) | `templates/evidence/` |
| Governance document (charter, policy, runbook) | `templates/governance/` |
| Incident report | `templates/incidents/` |
| Output (messaging, manifest, snapshot) | `templates/outputs/` |
| Phase specification | `templates/phases/` |

Always copy from a template before authoring from scratch. If a
template does not yet exist for an artefact kind, raise a
`§GOV_OPEN§` and create the template together with the first
artefact (commit them in the same PR).

## 5. Validation tooling

| Folder | Role |
|---|---|
| `tools/checks/` | Stateless validators that MUST pass before commit |
| `tools/validators/` | Stateful validators that may invoke the runtime |
| `tools/helpers/` | Convenience scripts for development and CI |
| `tools/scaffolding/` | Codegen scaffolding (template-driven) |

Read `tools/APJ-000 - binding-layer-entry-point V-1.0.0.md` for the canonical index.

## 6. Open-questions register

Every active `§GOV_OPEN§` is logged at:

- `.a0proj/knowledge/main/KNO-001 - open-questions-register V-1.0.0.md` — register

Format and resolution rules: see
`.a0proj/instructions/INS-005 - decision-and-question-recording V-1.0.0.md`.

## 7. Skills available

| Skill | Trigger | File |
|---|---|---|
| `governance-authoring` | Agent is asked to author or review governance artefacts | `.a0proj/skills/governance-authoring/SKL-000 - governance-authoring V-1.0.0.md` |

When the user asks the agent to "write a schema", "draft an ADR",
"review a contract", "create a phase spec", or asks "where does this
go?", the agent SHOULD load `governance-authoring` and follow its
decision tree.

## 8. Anti-pattern (binding)

- Reading this index as authority rather than as a navigation aid.
- Authoring artefacts in folders that don't have a row in §2.
- Generating artefacts under `examples/` that depend on real
  credentials (use env vars; see `INS-004 - git-and-remote-discipline V-1.0.0.md` §6).
- Treating `.a0proj/knowledge/` as a substitute for the actual
  artefacts in §1–§5.