---
adr: ADR-0001
title: relax-human-only-lanes
status: accepted
date: 2026-09-09
supersedes: null
superseded_by: null
owners:
  - binding-author
  - infrastructure-owner
---

# ADR-0001 — Relax "human only" carve-out

## 1. Context

Prior binding-layer text (specifically `INS-004 - git-and-remote-discipline
V-1.0.0.md` § 8 and § 10) encoded push to remote as a categorical
"human only" lane. The principle was: the agent MUST NOT auto-push;
all pushes were framed as human gestures. Equivalent blocking framing
existed for any action deemed irreversible or remote-affecting.

On 2026-09-09 the principal issued a refinement:

> For the time being no actions should be considered "human only
> lanes" as long as previously blocked actions are still gated by
> the requirement of an explicit request as well as clear and
> concise confirmation of approval.

This ADR records the change and supersedes the categorical framing
in the affected instruction files.

## 2. Decision

Replace the categorical "human only" framing with a procedural
gate, applied uniformly to any previously-blocked action:

**The gate.** An action that was previously blocked (push, force-
push, framework-core write, gate deletion, etc.) MAY be performed
when, in the same turn or the same short chain of turns, the
principal has provided:

1. **An explicit request** naming the action (verb + target),
2. **Clear and concise confirmation of approval** (e.g. "approved",
   "go", "yes, do it"),
3. (where the action affects remote state) **a clear identifier of
   the affected resource** (branch, remote, file, ADR, etc.).

The agent MUST refuse the action if any one of those three elements
is absent, ambiguous, or contradicted by an earlier statement in
the same chain.

## 3. Scope

This gate applies project-wide. Concretely, it edits:

| File | Section | Change |
|---|---|---|
| `INS-004 - git-and-remote-discipline V-1.1.0.md` | § 8 | Categorical "MUST NOT auto-push; all pushes are human gestures" → procedural gate with the three elements above. |
| `INS-004 - git-and-remote-discipline V-1.1.0.md` | § 10 anti-pattern | "Do not auto-push" → "Do not push to remote without an explicit request + clear confirmation of approval." |

No other instruction file encodes a human-only lane today. Other
blocking rules remain as written (e.g. `INS-002 § 1` framework-core
never-touch list, `INS-005 § 2.3` ADR lifecycle, `INS-004 § 6`
secret hygiene).

## 4. Consequences

### Positive
- Faster turnaround on remote-affecting operations when the
  principal knows what they want.
- Consistent procedural shape across action kinds (no class of
  action is categorically forbidden, but every previously-blocked
  action gets the same gate).
- Auditability improved: every previously-blocked action leaves a
  traceable approval in the conversation transcript.

### Negative / risks
- Higher risk of accidental execution if the principal reads
  "approved" generously. The "explicit + clear" reading in § 2
  mitigates this but does not eliminate it.
- The gate depends on conversation state, not on a durable artefact;
  accidental approval from a stray keyword is harder to detect
  than an explicit categorical rule.

### Mitigations (binding)
- Refusal script is mandatory before any previously-blocked action:
  the agent re-quotes the three elements back to the principal
  and waits a beat before executing.
- All push-affecting actions log the approval chain (`Refs:
  ADR-0001, approved <turn-id>") in the commit footer.

## 5. Alternatives considered

- **A1 — Keep categorical human-only.** Rejected: principal has now
  articulated a procedural preference.
- **A2 — Move the gate to a dedicated file (e.g. `INS-006 - approval
  gates`)** instead of editing `INS-004`. Deferred: current scope
  is small enough (one rule) to live in `INS-004`. If a second
  action class becomes gated, this ADR should be supplemented by
  `INS-006`.

## 6. Sunset / review

This relaxation is `"for the time being"`. Review triggers:

1. The principal articulates a re-tightening of the gate.
2. A mis-execution occurs that this gate's "explicit + clear"
   reading did not catch.
3. The principal requests sunset in chat.

A subsequent ADR (ADR-####-reinstate-human-only-lane or ADR-####-
refine-gate) MUST be raised before the policy is changed again.

Status: this is `accepted` for the time being; sunset or refinement
requires a new ADR.
