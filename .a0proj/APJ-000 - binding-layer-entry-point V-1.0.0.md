# Agent binding layer — `.a0proj/`

> Status: **Entry point**. Read this first when entering the project.
> This layer is **META**: it governs how the agent behaves in this
> project. The project's *substance* lives in `docs/`, `schemas/`,
> `templates/`, `tests/`, `tools/`, `src/agent_interop/`, `examples/`,
> plus the top-level control documents (`universal-agent-systems.md`,
> `implementation-plan.md`, the phase files, and
> `standard-operating-procedure.md`).

---

## 1. Layout

```
.a0proj/
├── APJ-000 - binding-layer-entry-point V-1.0.0.md                 # this file
├── instructions/             # project-scoped behavioural contract (binding)
├── knowledge/                # index + open-questions register (mirror layout)
│   ├── main/                 # entry-point artefacts
│   ├── fragments/            # atomic recall shards (placeholder)
│   └── solutions/            # retrospective case studies (placeholder)
├── skills/                   # loadable expertise
│   └── governance-authoring/ # the primary authoring skill
├── project.json              # agent framework binding metadata
├── mcp_servers.json          # per-project MCP server list
├── secrets.env               # local-only secret bindings (NEVER commit)
└── variables.env             # local-only variable bindings (NEVER commit)
```

## 2. Instructions (read these in order when entering the project)

| # | File | Purpose |
|---|---|---|
| 01 | `instructions/INS-001 - project-charter V-1.0.0.md` | One-sentence purpose, architectural stance, delivery phases, definition of done |
| 02 | `instructions/INS-002 - governance-and-core-files V-1.0.0.md` | What the agent MUST NOT modify in framework core; how to handle deliberate framework patches |
| 03 | `instructions/INS-003 - taxonomy V-1.0.0.md` | Where each artefact kind lives; modular-monolith invariant |
| 04 | `instructions/INS-004 - git-and-remote-discipline V-1.0.0.md` | Branch, commit, push, tag, and secret-hygiene rules |
| 05 | `instructions/INS-005 - decision-and-question-recording V-1.0.0.md` | ADR convention, `§GOV_OPEN§` sentinel, resolution lifecycle |

These five files are the **behavioural contract** for any agent,
sub-agent, or Ralph-loop iteration in this project. They are
**non-overridable**; if a task asks for behaviour that conflicts with
these files, refuse the task, document with a `§GOV_OPEN§`, and
escalate.

## 3. Knowledge (entry-point + audit trail)

| Path | Role |
|---|---|
| `knowledge/main/KNO-000 - substrate-index V-1.0.0.md` | Substrate map: bounded contexts → specs, schemas, templates, tests, examples |
| `knowledge/main/KNO-001 - open-questions-register V-1.0.0.md` | Active `§GOV_OPEN§` register (one row per open question) |

`fragments/` and `solutions/` mirror the canonical layout for atomic
recall and retrospective studies. They start as placeholders; populate
them as the project accumulates knowledge.

## 4. Skills (loaded on demand)

| Skill | Trigger | File |
|---|---|---|
| `governance-authoring` | User asks to author or review governance artefacts (schemas, ADRs, contracts, templates, phases, evidence, …) | `skills/governance-authoring/SKL-000 - governance-authoring V-1.0.0.md` |

Add new skills in their own subdirectory with a `SKL-000 - governance-authoring V-1.0.0.md` at the
root. Keep the skill body self-contained.

## 5. Iteration discipline (binding summary)

When the agent is asked to work in this project, the canonical sequence is:

1. Read this APJ-000.
2. Read `instructions/INS-001 - project-charter V-1.0.0.md`.
3. Read `knowledge/main/KNO-000 - substrate-index V-1.0.0.md`.
4. Read `instructions/INS-003 - taxonomy V-1.0.0.md` for the destination folder.
5. If authoring: load `skills/governance-authoring` and follow the
   8-step decision tree.
6. If a decision binds future work: raise an ADR via
   `instructions/INS-005 - decision-and-question-recording V-1.0.0.md`.
7. If anything is unclear: tag a `§GOV_OPEN§` (per instruction 05 § 3.4)
   and add a row to `knowledge/main/KNO-001 - open-questions-register V-1.0.0.md`.
8. Commit on a feature branch per
   `instructions/INS-004 - git-and-remote-discipline V-1.0.0.md`.

## 6. What this layer is NOT

- Not a substitute for `docs/specifications/`, `schemas/`, or any
  substance-bearing artefact.
- Not a place to commit secrets, generated artefacts, or runtime
  state.
- Not a copy of any framework prompt — it is the *project's* binding
  layer, distinct from the framework's agent profiles and
  prompt-includes.

## 7. Authority and overrides

- The five instruction files (01–05) are binding and non-overridable
  in their scope. Within their scope they take precedence over any
  external context the agent brings.
- The substrate index (`knowledge/main/KNO-000 - substrate-index V-1.0.0.md`)
  organises the project substance but is not itself authority; it
  always defers to the substance files it points at.
- A binding-layer change is itself an architectural change and
  requires an ADR.
