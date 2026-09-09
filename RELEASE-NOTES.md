# RELEASE-NOTES — v1.1.0

> Release branch: `release/v1.1.0`
> Cut from: `feat/bind-update-v1.1.0` @ `f55ae8a`
> Awaiting merge of PR #1 (<https://github.com/NovaAI-innovation/gov-model/pull/1>) onto `master` before the tag is created.
> Supersedes: V-1.0.0 binding-layer baseline (`feat/binding-v1` @ `e4512dd`).

## Summary

This release captures the **first relaxation of the binding-layer's push-policy** per `ADR-0001 - relax-human-only-lanes`.

The most material change: "human only" is no longer a categorical carve-out. Previously-blocked actions remain blocked **until** the principal provides an explicit request + clear confirmation of approval. The new procedure is captured in `INS-004 § 8.1` on `feat/bind-update-v1.1.0`.

## Files in this release

9 files changed vs the V-1.0.0 baseline, 304 insertions(+), 162 deletions(-).

| Status | Path | Note |
|---|---|---|
| deleted | `.a0proj/instructions/INS-004 - git-and-remote-discipline V-1.0.0.md` | superseded |
| created | `.a0proj/instructions/INS-004 - git-and-remote-discipline V-1.1.0.md` | new § 8 gate |
| renamed (94%) | `.a0proj/knowledge/main/KNO-001 - open-questions-register V-1.0.0.md` → `V-1.1.0.md` | +1 register row |
| created | `docs/decisions/ADR-0001-relax-human-only-lanes.md` | new ADR |
| modified | `.a0proj/APJ-000 - binding-layer-entry-point V-1.0.0.md` | cross-ref |
| modified | `.a0proj/instructions/INS-003 - taxonomy V-1.0.0.md` | cross-ref |
| modified | `.a0proj/instructions/INS-005 - decision-and-question-recording V-1.0.0.md` | cross-ref |
| modified | `.a0proj/knowledge/main/KNO-000 - substrate-index V-1.0.0.md` | cross-ref |
| modified | `.a0proj/skills/governance-authoring/SKL-000 - governance-authoring V-1.0.0.md` | cross-ref |

## ADR-0001 at a glance

- **Status**: accepted (time-boxed, subject to sunset review per § 6).
- **What it does**: replaces `INS-004 § 8`'s categorical `human only` framing with a procedural gate.
- **Gate elements**: explicit request + clear confirmation + resource identifier.
- **Affected section**: `INS-004 § 8.1 - § 8.3` and `INS-004 § 10` anti-pattern list.
- **Audit hook**: future push commits should record the principal-confirmation phrase in the commit footer per `INS-004 § 8.3`.

## Reviewer-facing notes

- The single substantive behavioural change is in `INS-004 § 8`. Read it before commenting on push policy.
- `ADR-0001` is the source of truth for the policy rationale; `INS-004 § 8.1` is the binding operational rule.
- The 6 active rows in `KNO-001` (now V-1.1.0) remain open; no row is closed by this release. The 7th row added by `ADR-0001` is itself time-boxed — see `ADR-0001 § 6` sunset triggers.

## Operational impact

- Other instructions (`INS-001`, `INS-002`, `INS-003`, `INS-005`) are unchanged in substance; they receive only the cross-reference update driven by the `INS-004` rename.
- The active branch in this workspace remains `feat/bind-update-v1.1.0` until the user checks out elsewhere.
- `master` is unchanged at scaffold baseline `b6f659b`; this release will land on `master` only after PR #1 is merged.
- `feat/binding-v1` (V-1.0.0) is to be closed as superseded by this release per the recommendation in PR #2's body.

## Test

| Check | Result |
|---|---|
| Binding-layer file inventory | `find .a0proj/ -type f` → 11 Markdown + 2 `.gitkeep` |
| Cross-reference scan | `grep -E 'INS-004 V-1\.0\.0\.md\|KNO-001 V-1\.0\.0\.md' .a0proj/ -rn` → 0 hits |
| Sentinel consistency | `grep -h -o '§GOV_OPEN§' .a0proj/**/*.{md,example} 2>/dev/null \| wc -l` → 39 occurrences |
| Workspace decoupling | `grep -rln '/a0/usr/workdir' .a0proj/` → 0 matches |
| Stale-narrative scan | `grep -rE 'INS-004 § [0-9]' .a0proj/` → still references V-1.1.0 sections |

## Approver trail

Principal approval phrase (verbatim) carries through this release via the commit footer of `f55ae8a`:

```
Refs: ADR-0001; approved "2 approved route a 2 approved" on 2026-09-09
```

## Out of scope (deferred to a later release)

- Pre-commit hook (`tools/checks/pre-commit.sh`) — see `KNO-001` row in `INS-004 § 7`.
- Reconciliation of the substrate map (`KNO-000 § 2`) with the actual file inventory under `templates/`, `schemas/`, `docs/`.
- Authoring the first `ADR-0002` (when the next relaxation or refinement is needed).
- Implementing agents / skills referenced in the substrate but not yet authored (e.g. `evaluation/` bounded context).
