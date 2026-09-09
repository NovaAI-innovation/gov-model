# 05 — Decisions, questions, and the §GOV_OPEN§ sentinel

> Status: **Binding**. Mandatory for every agent, sub-agent, and
> Ralph-loop iteration in this project. Companion:
> `INS-001 - project-charter V-1.0.0.md` § 7.

---

## 1. Two record kinds, two locations

| Record kind | Tag / format | Default location |
|---|---|---|
| **Decision** | ADR (see §2) | `docs/decisions/ADR-NNNN-title.md` |
| **Open question** | `§GOV_OPEN§` line (see §3) | inline next to the artefact being questioned, plus a top-level `§GOV_OPEN§` register in `.a0proj/knowledge/main/KNO-001 - open-questions-register V-1.1.0.md` |

The presence of either record is part of the project substance. The
absence of decisions is itself a defect.

## 2. Architecture Decision Records (ADRs)

### 2.1 File shape

- Path: `docs/decisions/ADR-NNNN-title.md`
- Numbering: monotonically increasing 4-digit zero-padded decimal.
- Title: lower-case kebab; describes the *outcome*, not the topic.
- Authoring: copy from `templates/decisions/` first; fill every field.

### 2.2 What is and isn't an ADR

| It is an ADR when… | It is NOT an ADR when… |
|---|---|
| The decision binds future work in this project | It is a session-level choice (Ralph-loop runtime setting) |
| Reverting requires a new ADR (no silent reversals) | It is a one-off codebase fix |
| It states the alternatives considered | It is only a goal statement |
| It states the consequence of the choice | It is only a wish-list |
| A bounded context is affected or several are constrained | A single file changes |

### 2.3 ADR lifecycle

```
DRAFT → ACCEPTED → (DEPRECATED | SUPERSEDED by ADR-NNNN)
```

A draft ADR is binding only when the human owner accepts it in the
PR that introduces it. Until then, the **previous** ADR (or
charter default) holds. State MUST be visible in the ADR header:

```yaml
---
adr: ADR-0042
title: agent-binding-layer-skill-format-v2
status: accepted          # draft | accepted | deprecated | superseded
date: 2026-09-09
supersedes: ADR-0036      # if any
superseded_by: null       # set if a later ADR replaces this
---
```

## 3. The `§GOV_OPEN§` sentinel

### 3.1 Purpose

A single grep-able token that flags any unresolved question. The
sentinel exists so that a one-line `grep -rn '§GOV_OPEN§' .` surfaces
**every** open item at once. There MUST be exactly one sentinel
string and it MUST appear unchanged.

### 3.2 Where the sentinel may appear

- This file (canonical definition site).
- Any instruction file under `.a0proj/instructions/`.
- Any PRD story (`prd.json` → `notes` field) produced by a Ralph loop.
- Any skill file (`.a0proj/skills/<skill>/SKL-000 - governance-authoring V-1.0.0.md` body).
- Any template under `templates/` as a `§GOV_OPEN§ <reason>` placeholder.
- Any decision draft in `docs/decisions/` whose status is `draft` AND
  whose body contains an unresolved sub-question.

### 3.3 Where the sentinel MUST NOT appear

- Generated artefacts (output of a build, snapshots, fixtures).
- Wire-schemas (`schemas/v<N>/*.json`) — schemas resolve unknowns with
  explicit `enum` or `oneOf`, never with a sentinel.
- Sealed ADR headers (status: accepted/deprecated/superseded).
- Production code (`src/agent_interop/**`).

If a sentinel appears in any of those locations, treat it as a bug
in the place that produced the artefact (not in the artefact itself).

### 3.4 Format

A sentinel line MUST follow this exact shape:

```
§GOV_OPEN§ <one-line summary> — owner: <name-or-team>, since: <YYYY-MM-DD>
```

Examples:

```
§GOV_OPEN§ which actor identity is used for codex-transport auth? — owner: delivery-team, since: 2026-09-09
§GOV_OPEN§ should the memory context ranker expire after 30 days of inactivity? — owner: context-packages-team, since: 2026-09-04
```

No variation in casing or spacing is permitted; one broken sentinel
breaks grep, and broken grep means answers are lost.

### 3.5 Resolution lifecycle

When an open question is answered:

1. Replace the sentinel line with:

   ```
   §GOV_RESOLVED§ <decision summary> — owner: <name>, on: <YYYY-MM-DD>, refs: <ADR-NNNN-or-link>
   ```

2. If the resolution is itself an architectural decision, raise an ADR
   FIRST and link it (`refs: ADR-0042`).
3. If the resolution is a workaround or a comment-only decision, just
   replace the sentinel inline. Always include the `owner`, `on` date,
   and `refs`.

The resolved marker (`§GOV_RESOLVED§`) is NOT a sentinel that needs
to be cleaned up; it is permanent audit trail. Grep for it too,
historically, when answering similar future questions.

## 4. The open-questions register

A single canonical register at
`.a0proj/knowledge/main/KNO-001 - open-questions-register V-1.1.0.md` lists every active
`§GOV_OPEN§` in the project, with owner and "since" date:

```markdown
# Open questions register

| Sentinel first seen | Topic | Owner | Item ref |
|---|---|---|---|
| 2026-09-09 | identity for codex transport | delivery-team | `instruction-04 § 3` |
```

This register is updated:
- When a new `§GOV_OPEN§` is created (add row).
- When an existing one is `§GOV_RESOLVED§` (remove row, log to ADR if
  the resolution is architectural).

Without the register, the sentinel is just a token; with it, the
sentinel becomes a queryable project asset.

## 5. Anti-pattern (binding)

- Variants of the sentinel (`GOV-OPEN`, `gov_open`, `§§GOV§§`, etc.) —
  forbidden; grep breaks.
- Open questions recorded only in chat or in scratch files — they
  vanish at session end; use the sentinel inline plus the register.
- Resolution markers without an owner, on-date, or `refs` link — the
  decision context is lost.
- ADRs without a status header — drift to "accepted by default".
- Deprecating an ADR without a `supersedes` line in the new ADR —
  history breaks.