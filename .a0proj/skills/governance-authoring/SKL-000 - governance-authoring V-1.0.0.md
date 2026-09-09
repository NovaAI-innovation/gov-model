# Skill: governance-authoring

> Status: **Loadable skill**. Loaded on demand when the agent is asked
> to author or review governance artefacts.
> Trigger phrases: "write a schema", "draft an ADR", "author a contract",
> "create a phase spec", "review a governance document", "where does
> this go?", "create a template", "validate this artefact".
> Companion: every file under `.a0proj/instructions/` and
> `.a0proj/knowledge/`.

---

## 1. What this skill does

This skill is the **decision tree** for producing or reviewing any of:

- wire-schemas (`schemas/v<N>/*.json`)
- specifications (`docs/specifications/*.md`)
- architecture documents (`docs/architecture/*.md`)
- architecture decision records (`docs/decisions/ADR-NNNN-*.md`)
- governance documents (`docs/governance/*.md`)
- runbooks / operations docs (`docs/operations/*.md`)
- quality / test policy docs (`docs/quality/*.md`)
- phase specifications (`phase-*-*.md`)
- templates (`templates/<kind>/*`)
- evidence records (`tests/evidence/*`, audit notes)
- incident reports (`docs/operations/incidents/*` per project convention)
- output scaffolds (`templates/outputs/*`)

The skill DOES NOT author production code under `src/agent_interop/`;
that has its own bounded contexts and tests; coordinate via ADRs,
not via this skill.

## 2. The 8-step decision tree

When the agent loads this skill:

### Step 1 — Read the binding-layer contract

1. Read `.a0proj/instructions/INS-001 - project-charter V-1.0.0.md` for project stance.
2. Read `.a0proj/knowledge/main/KNO-000 - substrate-index V-1.0.0.md` for the substrate
   map; pay attention to § 2 (bounded-context map) and § 6 (open-
   questions register).
3. Read `.a0proj/instructions/INS-003 - taxonomy V-1.0.0.md` to confirm the correct
   destination folder for the artefact kind.
4. Read `.a0proj/instructions/INS-005 - decision-and-question-recording V-1.0.0.md` for
   the `§GOV_OPEN§` and ADR conventions.

### Step 2 — Pick the bounded context (if applicable)

If the artefact is bounded to a single context, choose the row from
`knowledge/main/KNO-000 - substrate-index V-1.0.0.md` § 2 that the task belongs to. If
no context fits, raise a `§GOV_OPEN§` before authoring.

### Step 3 — Pick the template kind

Match the artefact to the table in
`knowledge/main/KNO-000 - substrate-index V-1.0.0.md` § 4. If no template exists yet,
follow instruction 03 § 4 and co-author the template + first artefact
in the same change.

### Step 4 — Copy the template, fill every field

NEVER author from scratch when a template exists. Templates make gaps
explicit; authoring from scratch hides them.

If the template requires fields you cannot answer:

- Author what you can.
- Tag each unanswered field with `§GOV_OPEN§ <field-name>` followed by
  the format defined in instruction 05 § 3.4.
- Add a row to `.a0proj/knowledge/main/KNO-001 - open-questions-register V-1.0.0.md`.

### Step 5 — Validate against the project tooling

If `tools/checks/<kind>.py` or `tools/validators/<kind>.py` exists for
this artefact kind, run it before declaring done. Read
`tools/APJ-000 - binding-layer-entry-point V-1.0.0.md` first to confirm the canonical check names.

If the tooling does NOT exist yet:

- The artefact is not yet enforceable.
- Raise `§GOV_OPEN§ <missing-validator>` so the gap is visible.
- Do not silently skip.

### Step 6 — Decide if a decision record is required

Cross-reference with instruction 05 § 2.2 ("It is an ADR when …"). If
the artefact binds future work, reverts require an ADR, several
contexts are affected, or the alternatives considered need to survive
in git history: open an ADR alongside the artefact.

### Step 7 — Cross-link from related artefacts

Update `knowledge/main/KNO-000 - substrate-index V-1.0.0.md` § 2 if the cell was
previously empty (i.e. an artefact now exists where the index listed
a gap). Update cross-references in:

- the new artefact (link BACK to the index)
- any related ADR
- any related spec
- the templates README if a template was co-authored

### Step 8 — Commit on a feature branch

Per `.a0proj/instructions/INS-004 - git-and-remote-discipline V-1.0.0.md`:

- `feat/<context>-<short>` for new artefacts
- `fix/<context>-<short>` for fixes
- `chore/<context>-<short>` for refactors
- `docs/<context>-<short>` for documentation-only

Commit message body MUST reference the spec, ADR (if any), and any
`§GOV_OPEN§` items this commit closes. Sub-agent Co-authored-by
footer is required if a sub-agent materially authored the change.

## 3. Common pitfalls (binding)

| Pitfall | Why it fails | Instead |
|---|---|---|
| Authoring under a wrong folder | Breaks taxonomy conventions; rejected by reviewers | Use `INS-003 - taxonomy V-1.0.0.md` § 1 |
| Authoring from scratch when template exists | Hides required fields | Copy template first |
| Marking a story `passes:true` in the Ralph-loop PRD without running checks | Violates instruction 04 § 6 implicit rule | Run all checks before pass |
| Forcing a story through when 3 attempts have failed | Wastes tokens, hides real issues | Append to `fix_plan.md` and emit `<promise>BLOCKED</promise>` |
| Bypassing `pre-commit` (when installed) | Violates instruction 04 § 7 | Either fix the issue or do not commit |
| Recording a decision in chat only | Lost at session end | Raise an ADR; update the open-questions register |
| Adding an enum item or runtime secret to a schema for one test | Pollutes public schema | Use `tests/fixtures/` with env-var indirection |
| Cross-bounded-context imports of domain code | Violates `INS-003 - taxonomy V-1.0.0.md` § 7 invariant | Add a port in `ports/` |

## 4. Output convention

When this skill produces output, the agent's reply MUST end with a
short audit line. Replace placeholders accordingly.

```
[AUTHORED] <artefact-kind>: <path> — bounded-context: <context-or-n/a> — opened: <ADR-NNNN-or-n/a> — closed: §GOV_OPEN§ items <list-or-none>
```

Examples:

```
[AUTHORED] schema: schemas/v1/contracts.json — bounded-context: contracts — opened: n/a — closed: §GOV_OPEN§ schema-lifecycle-events
[AUTHORED] ADR: docs/decisions/ADR-0001-identity-federation.md — bounded-context: n/a — opened: ADR-0001 — closed: §GOV_OPEN§ identity-federation-protocol
[AUTHORED] template: templates/decisions/contract-record.md — bounded-context: contracts — opened: ADR-0002 — closed: none
```

## 5. Hand-off back to base loop

When the artefact is complete:

- Return to whatever loop was driving the task. If a Ralph loop is
  active, the runner will pick the next story.
- Do NOT emit `<promise>COMPLETE</promise>` for this skill's own work;
  only BUILDING-mode Ralph loops emit that tag.
- If the task is "review X" and X is found in violation, do not
  auto-fix; raise a `§GOV_OPEN§` and emit a `[REVIEW_FLAGGED]` line
  with the violation list.

## 6. Self-test (verify before declaring done)

Run through this 5-question check before every artefact produces an
`[AUTHORED]` line:

1. Did you read `INS-001 - project-charter V-1.0.0.md` and `KNO-000 - substrate-index V-1.0.0.md`?
2. Did you use a template (or co-author one + the artefact)?
3. Did you run every available check under `tools/checks/` /
   `tools/validators/`?
4. Did you update the index § 2 cell if a gap was filled?
5. Did you record every `§GOV_OPEN§` you wrote in the register?

If any answer is "no", the artefact is not done.

## 7. Anti-pattern (binding)

- This skill is bound to *this project's* binding layer. Do not
  re-export its decision tree to other projects as-is; each project
  has its own binding layer.
- Do not extend the instruction files from inside this skill; the
  instructions are governed separately (see `01 § 7` authority
  rules).
- Do not invoke this skill on production code edits under
  `src/agent_interop/`; that path is governed by the modular
  monolith invariant (`INS-003 - taxonomy V-1.0.0.md` § 7) and the standard
  architectural review process, not by governance authoring alone.