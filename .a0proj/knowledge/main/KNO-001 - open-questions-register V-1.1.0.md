# Open questions register

> Status: **Binding convention**. One row per active `§GOV_OPEN§`.
> Authoritative: `.a0proj/instructions/INS-005 - decision-and-question-recording V-1.0.0.md`.
> Last full sweep: 2026-09-09.
Version: V-1.1.0 (bumped to reflect binding-layer relax-human-only-lanes update per ADR-0001).

---

## How this register is maintained

- **Add a row** when a new `§GOV_OPEN§` line is written anywhere in
  the project.
- **Remove a row** when the matching inline sentinel is replaced by
  `§GOV_RESOLVED§`. Move the resolution context to the ADR or to
  the inline resolved marker.
- **Sort** by `Sentinel first seen` (ascending). Date format is
  ISO-8601 `YYYY-MM-DD`.
- **One row per question.** A multi-part question is one row with a
  `§GOV_OPEN§` block referencing each sub-question separately.

## Active `§GOV_OPEN§` entries

| Sentinel first seen | Topic | Owner | Item ref |
|---|---|---|---|
| 2026-09-09 | Substrate map in `KNO-000 - substrate-index V-1.0.0.md` §2 lists *suggested* paths under `schemas/v<N>/`, `docs/specifications/`, `templates/`, `tests/`, and `examples/` per bounded context — actual file inventory has not been performed and the inventory will likely show many gaps. | binding-author | `knowledge/main/KNO-000 - substrate-index V-1.0.0.md` §2 |
| 2026-09-09 | No `pre-commit` hook is installed in the project yet, so the rule in `INS-004 - git-and-remote-discipline V-1.1.0.md` §7 has no enforcement — the framework only enforces what `tools/checks/pre-commit.sh` enforces, and that script does not yet exist. | infra-owner | `instructions/INS-004 - git-and-remote-discipline V-1.1.0.md` §7 |
| 2026-09-09 | No `ADR-NNNN-title.md` has ever been authored in this project (the `docs/decisions/` directory exists but is empty), so the convention in `INS-005 - decision-and-question-recording V-1.0.0.md` §2 has no worked examples yet. | governance-owner | `instructions/INS-005 - decision-and-question-recording V-1.0.0.md` §2 |
| 2026-09-09 | The bounded-context map in `KNO-000 - substrate-index V-1.0.0.md` §2 implies 10 services (`contracts`, `artifacts`, `workspace`, `health`, `context_packages`, `evaluation`, `policy`, `execution_state`, `memory`, `recovery`) — but it is unknown how many of these are already implemented under `src/agent_interop/contexts/`. The MVP scope in `universal-agent-systems.md` says delivery + protocol + ports + infrastructure + cross-cutting primitives; the 10-context list may be aspirational. | architecture-owner | `knowledge/main/KNO-000 - substrate-index V-1.0.0.md` §2, `universal-agent-systems.md` §"Target hierarchy" |
| 2026-09-09 | The `tools/checks/pre-commit.sh` script is referenced by `04 § 7` but does not exist in the project; defining its contents (which checks to run, in which order, exit codes, timeouts) is part of standing up the binding layer's enforcement. | infra-owner | `instructions/INS-004 - git-and-remote-discipline V-1.1.0.md` §7 |
| 2026-09-09 | The framework patch on `/a0/helpers/mcp_server.py` line 346 is currently in an unknown state per the runtime; the binding layer rule in `02 § 3` requires that the agent consult the framework-level record before any framework-core write, but the agent does not yet have a documented escalation path for the case where the consult itself fails (e.g. framework runtime unavailable). | runtime-owner | `instructions/INS-002 - governance-and-core-files V-1.0.0.md` §3 |

## Recently resolved (audit trail)

> Resolution markers remain inline at the question site. This section
> is for fast triage only.

(none yet)

## How to query this register

```
grep -n '§GOV_OPEN§' .a0proj/knowledge/main/KNO-001 - open-questions-register V-1.1.0.md
grep -rn '§GOV_OPEN§' .            # all sites, including resolvable inline markers
```

If the project has any active `§GOV_OPEN§` lines outside this
register, the register is out of date; bring it up to date before
adding the new row.

## Anti-pattern (binding)

- Removing a row without replacing the inline sentinel
  (`§GOV_OPEN§` → `§GOV_RESOLVED§`).
- Resolving an open question without recording `owner`, `on`, and
  `refs` (see instruction 05 § 3.5).
- Adding speculative rows for which no inline `§GOV_OPEN§` marker
  exists in the project.
- Grouping multiple unrelated questions into one row — keep one per row.
