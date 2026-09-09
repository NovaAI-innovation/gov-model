# 04 — Git and remote discipline

> Status: **Binding**. Mandatory rule for every commit and every push.
> Authoritative project remote is recorded in `.a0proj/project.json`
> under the `git_url` field; this file governs **how** that remote is
> used. Companion: `INS-001 - project-charter V-1.0.0.md`, `INS-002 - governance-and-core-files V-1.0.0.md`.

---

## 1. Project remote (read-only canonical)

| Field | Value |
|---|---|
| Remote name | `origin` |
| Repository URL | see `.a0proj/project.json` → `git_url` |
| Fetch + push | both enabled |
| Default branch | `master` (current) |

**Rule:** the project's only remote is `origin`. Do **not** add
secondary remotes (`upstream`, `fork`, etc.) without an explicit ADR
recorded in `docs/decisions/`. If a fork is genuinely needed, the ADR
must state the purpose and the deletion-on-merge plan.

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
`INS-003 - taxonomy V-1.0.0.md` §7 (e.g. `feat/contracts-port-validator`,
`fix/memory-namespace-conflict`, `chore/poetry-bump`).

## 3. Commit conventions

Conventional Commits 1.0 (https://www.conventionalcommits.org/), with
the additive notes:

- A scope is required when the project has 3 or more bounded
  contexts: `<type>(<scope>): <subject>`. Scope matches a bounded
  context name when applicable.
- Body MUST reference:
  - Any spec or template the change derives from (e.g. "Implements
    `docs/specifications/protocol-v1.md` § 4.2").
  - Any ADR it implements (e.g. "Implements `ADR-0042`").
  - Any `§GOV_OPEN§` item it closes (see
    `INS-005 - decision-and-question-recording V-1.0.0.md`).
- Footer MUST include:
  - `Co-authored-by:` lines for any sub-agent that materially
    contributed (Ralph-loop iterations, planner sub-agents).
  - `Refs:` line listing the issue or PR.
  - `Test:` line listing the backpressure commands run (e.g. `Test:
    pnpm typecheck; pnpm test; tools/checks/pre-commit.sh`).

## 4. Worktree use (optional, encouraged for long tasks)

For multi-day or multi-PR work, run the work in a git worktree:

```
git worktree add ../gov-model.<branch> -b <branch>
```

This isolates editor state, build artefacts, and review state from
the main checkout. Worktrees MUST be deleted once the branch is
merged.

## 5. Sub-agent commit hygiene

When a sub-agent (Ralph loop iteration, planner sub-agent, etc.)
makes a commit, the commit message MUST:

- Identify the sub-agent or iteration in the footer:
  `Sub-agent: ralph-iter-NN` or `Sub-agent: <name>`.
- Reference the PRD story ID, plan item, or iteration index.
- Include `Co-authored-by: <sub-agent-as-Co-authored-trailer>`.

Human reviewers MUST see immediately whether a commit was produced
by an autonomous loop versus a human edit. Treat the absence of this
marker as a soft block on review.

## 6. Secret hygiene

| Location | Status | Rule |
|---|---|---|
| `.a0proj/secrets.env` | local-only | Never committed. Never pushed. |
| `.a0proj/variables.env` | local-only | Same. |
| Anything matching `.env*` | local-only | Same. Add to `.gitignore` if absent. |
| `examples/` | public | Never place real credentials; use fixtures. |
| `tests/fixtures/` | public | Never place real credentials. |

**Authoring rule:** if a test or example needs a credential, design
the example to read from an env var (e.g. `EXAMPLE_TOKEN`) and document
that requirement in the README of that example. Do not stub secrets
in committed files.

## 7. Pre-commit hook requirement

The project SHOULD have a `pre-commit` hook that:

1. Runs `tools/checks/pre-commit.sh` (or equivalent) — must exit 0.
2. Runs secret scanning (gitleaks, trufflehog, or detect-secrets) —
   must exit 0.
3. Runs `git diff --check` (whitespace + conflict markers) — exit 0.
4. Warns on large file additions (>1 MB unless expected).

If any check fails, the commit MUST be aborted. The agent should
treat a passed pre-commit as part of "the commit succeeded"; if the
hook was bypassed, the commit is suspect and should be reauthored.

## 8. Push policy (default)

| Branch kind | Push policy |
|---|---|
| `master` | Never push directly. PR review is mandatory. |
| `feat/*`, `fix/*`, `chore/*`, `docs/*` | Push freely during development; squash-merge on PR. |
| `release/*` | Push only after release notes are drafted; tag at the release commit. |
| `*` personal scratch | Local only; never push. |

The agent MUST NOT auto-push to remote. All pushes are human
gestures. Ralph-loop runs configured to push each iteration are
**forbidden**; configure them to commit locally only.

## 9. Tag and release discipline

- Tags follow semver: `vMAJOR.MINOR.PATCH`.
- Tags are GPG-signed by the human release owner.
- Each tag is preceded by a `release/<version>` branch with a
  `RELEASE-NOTES.md` summarising the diff since the previous tag.
- Tags MUST NOT be moved after publication. If a release is
  withdrawn, cut a new minor or patch; never re-tag.

## 10. Anti-pattern (binding)

- Do not commit to `master`.
- Do not force-push any branch on remote.
- Do not commit secrets, even in fixtures.
- Do not bypass pre-commit hooks.
- Do not auto-push.
- Do not amend commits already pushed to remote (squash locally
  instead; force-push is forbidden).