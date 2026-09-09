# 04 — Git and remote discipline

> Status: **Binding**. Mandatory rule for every commit and every push.
> Authoritative project remote is recorded in `.a0proj/project.json`
> under the `git_url` field; this file governs **how** that remote is
> used. Companion: `INS-001 - project-charter V-1.0.0.md`,
> `INS-002 - governance-and-core-files V-1.0.0.md`.
> Version: V-1.1.0 — see `ADR-0001 - relax-human-only-lanes`.

---

## 1. Project remote (read-only canonical)

| Field | Value |
|---|---|
| Remote name | `origin` |
| Repository URL | see `.a0proj/project.json` -> `git_url` |
| Fetch + push | both enabled |
| Default branch | `master` (current) |

**Rule:** the project's only remote is `origin`. Do **not** add
secondary remotes (`upstream`, `fork`, etc.) without an explicit
request + clear confirmation of approval (see § 8 below) and an
ADR recorded in `docs/decisions/`.

## 2. Branching strategy

| Action | Allowed | Rule |
|---|---|---|
| Push to `master` directly | **NO** | All work lands on a feature branch first. |
| Force-push any branch | **NO** | Including `--force-with-lease`. History is append-only on shared branches. |
| Force-push a personal scratch branch | Local-only | Never to remote. |
| Delete a remote branch | Only after merge | Use the GitHub PR "delete branch" button, not `git push --delete`. |
| Create `feat/<scope>-<short>` | Yes | For new capabilities. |
| Create `fix/<scope>-<short>` | Yes | For bug fixes. |
| Create `chore/<scope>-<short>` | Yes | For chores (refactors, dependency bumps). |
| Create `docs/<scope>-<short>` | Yes | For documentation-only changes. |
| Create `release/<version>` | Yes | For release prep; cut from `master`. |

`<scope>` SHOULD match an entry from the bounded contexts in
`INS-003 - taxonomy V-1.0.0.md` § 7 (e.g. `feat/contracts-port-validator`,
`fix/memory-namespace-conflict`, `chore/poetry-bump`).

## 3. Commit conventions

Conventional Commits 1.0 (https://www.conventionalcommits.org/), with
the additive notes:

- A scope is required when the project has 3 or more bounded
  contexts: `<type>(<scope>): <subject>`. Scope matches a bounded
  context name when applicable.
- Body MUST reference:
  - Any spec or template the change derives from.
  - Any ADR it implements (e.g. "Implements `ADR-0042`").
  - Any `§GOV_OPEN§` item it closes (see
    `INS-005 - decision-and-question-recording V-1.0.0.md`).
- Footer MUST include:
  - `Co-authored-by:` lines for any sub-agent that materially
    contributed.
  - `Refs:` line listing the issue or PR.
  - `Test:` line listing the backpressure commands run.

## 4. Worktree use (optional, encouraged for long tasks)

For multi-day or multi-PR work, run the work in a git worktree:

```
git worktree add ../gov-model.<branch> -b <branch>
```

Worktrees MUST be deleted once the branch is merged.

## 5. Sub-agent commit hygiene

When a sub-agent (Ralph loop iteration, planner sub-agent, etc.)
makes a commit, the commit message MUST identify the sub-agent,
reference the PRD story ID / iteration index, and include
`Co-authored-by:`. Treat absence of this marker as a soft block
on review.

## 6. Secret hygiene

| Location | Status | Rule |
|---|---|---|
| `.a0proj/secrets.env` | local-only | Never committed. Never pushed. |
| `.a0proj/variables.env` | local-only | Same. |
| Anything matching `.env*` | local-only | Same. |
| `examples/` | public | Never place real credentials; use fixtures. |
| `tests/fixtures/` | public | Never place real credentials. |

## 7. Pre-commit hook requirement

The project SHOULD have a `pre-commit` hook that runs the project's
backpressure stack and aborts on failure. If the hook was bypassed,
the commit is suspect.

## 8. Push policy (controlled, never autonomous)

> **Authority: `ADR-0001 - relax-human-only-lanes`, 2026-09-09.**
> This section replaces the prior categorical "human gestures only"
> framing. The agent applies the **gate** below; per-branch kind
> is a specialisation of that gate.

### 8.1 The push gate

**No push to remote may be performed autonomously.** A push is
allowed only when the principal has provided, in the same turn or
the same short chain of turns, all of:

1. **An explicit request** naming the action (verb + target), e.g.
   "push `feat/x` to `origin`", "merge this to master".
2. **A clear and concise confirmation of approval** (e.g.
   "approved", "go", "yes, do it", "proceed").
3. **A clear identifier of the affected resource** (branch
   name and target remote; the SHA / PR / merge target when
   applicable).

If any of these three is absent, ambiguous, or contradicted by an
earlier statement in the same chain, the agent MUST refuse the
push. Refusal is the default.

### 8.2 Per branch kind

Once the gate is passed:

| Branch kind | Push policy |
|---|---|
| `master` | PR review still mandatory. Push is only after merge commit, via CI. |
| `feat/*`, `fix/*`, `chore/*`, `docs/*` | Push freely during development. Default merge via squash on PR. |
| `release/*` | Push only after release notes are drafted; tag the release commit. |
| `*` personal scratch | No push. Local only. |

**Ralph-loop posture:** a Ralph-loop run is forbidden from being
configured to push each iteration. Configure Ralph loops to commit
locally only; treat any push as a discrete action performed
manually (or approved through the § 8.1 gate).

### 8.3 Approval chain audit

When a push (or any previously-blocked action) is approved under
§ 8.1, the agent MUST record the approval chain in the next
relevant commit footer:

```
Refs: ADR-0001; approved <principal-confirmation-phrase> on <date>
```

The phrase is the exact wording the principal used to confirm.

## 9. Tag and release discipline

- Tags follow semver: `vMAJOR.MINOR.PATCH`.
- Tags are GPG-signed by the human release owner.
- Each tag is preceded by a `release/<version>` branch with a
  `RELEASE-NOTES.md` summarising the diff since the previous tag.
- Tags MUST NOT be moved after publication.

## 10. Anti-pattern (binding)

- Do not commit to `master`.
- Do not force-push any branch on remote.
- Do not commit secrets, even in fixtures.
- Do not bypass pre-commit hooks.
- **Do not push to remote without an explicit request + clear
  confirmation of approval.** (See § 8.1.)
- Do not amend commits already pushed to remote.

## 11. Authority and supersession

- This V-1.1.0 supersedes the V-1.0.0 form of this file. The change
  is `ADR-0001`.
- Any future change to the gate in § 8.1 MUST itself be recorded as
  a new ADR; an unscheduled edit to § 8 is forbidden.
