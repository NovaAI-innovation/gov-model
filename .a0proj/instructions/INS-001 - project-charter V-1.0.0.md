# 01 — Project Charter (governance_model)

> Status: **Binding**. The agent treats this file as authoritative when
> this project is the active context (see `Projects Manifest`).
> Source of truth: [`universal-agent-systems.md`](../../universal-agent-systems.md)
> (repository control document).
> Last review: 2026-09-09

---

## 1. One-sentence purpose

Provide **portable agent services and a canonical interchange protocol**
that can be invoked by agents with different architectures, transports,
and formats.

## 2. Architectural stance (modular monolith)

- Domain logic must not import transport, framework, or host-specific
  code. Services **coordinate**, they do **not perform I/O**.
- **Ports** abstract external capabilities; **infrastructure** implements
  the ports; **delivery** translates between them.
- Host-specific behaviour stays at the edge. The portable core only
  declares inputs, outputs, effects, errors, and capabilities.
- Unsupported behaviour must not silently degrade. Either a capability
  is supported and the contract holds, or it is not and the caller is
  informed.

## 3. Delivery phases

| Phase | Primary result | Cost | Risk | Status |
|---|---|---|---|---|
| MVP | Stable protocol direction | Low | Low | Required baseline |
| Phase 1 | Tested portable services | Medium | Medium | Highest-practical |
| Phase 2 | Scalable interoperability foundation | High | Medium-high | Conditional on Phase 1 evidence |
| Future adapters | Host-specific event translation | Variable | Variable | Out of MVP scope |

Active phase specification: [`phase-1-portable-foundations.md`](../../phase-1-portable-foundations.md),
[`phase-2-scalable-interop.md`](../../phase-2-scalable-interop.md).

## 4. Bounded contexts (canonical set)

| Context | Responsibility |
|---|---|
| `contracts/` | Invariant rules, contracts, port interfaces |
| `artifacts/` | Deterministic ASCII / Mermaid / manifest generation |
| `workspace/` | Inventory, diffing, ownership, path rules |
| `health/` | Health-check states, findings, policies |
| `context_packages/` (evaluation) | Scores, findings, report generation |
| `policy/` | Risk classification, decision logic |
| `execution_state/` | State transitions, replay, completion |
| `memory/` | Records, ranking, retention, conflicts |
| `recovery/` | Recovery classification and recommendations |
| `observability/` | Events, provenance, metrics, manifests |

These map one-to-one with `src/agent_interop/<context>/`.

## 5. What this project is NOT

- Not a plugin framework. Future host adapters translate events **only**;
  the core never bolts on host-specific lifecycles.
- Not a microservice fleet. Initial deployment is a single modular
  monolith; services are not independently deployable in MVP.
- Not a universal lifecycle-hook framework. Phase 1 narrows the scope
  to **service execution portability**, not lifecycle automation.

## 6. Definition of done (MVP-level)

- Protocol spec is published under `docs/specifications/` and the
  matching wire schema under `schemas/v1/`.
- A second agent (different host / transport) can invoke the canonical
  service end-to-end without code changes to the core.
- `tests/architecture/` verifies dependency-direction rules.
- README, ADRs, and decision records are up to date.

## 7. Authority and overrides

- The charter is overridden only by an explicit ADR recorded in
  `docs/decisions/` numbered `ADR-XXXX` with a written rationale.
- A draft ADR is binding only when approved; otherwise the **last
  approved** charter and ADR set stand.
- See [`INS-005 - decision-and-question-recording V-1.0.0.md`](INS-005 - decision-and-question-recording V-1.0.0.md)
  for the recording convention and the open-question sentinel.
