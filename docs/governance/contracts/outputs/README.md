# Output Contract Catalog

Status: active
Version: 1.0

## Purpose

This folder defines the meaning and required sections of output contracts.
Reusable filled templates belong in templates/outputs/; machine validation
belongs in schemas/outputs/.

## Required contract sections

Every output contract must define:

- Purpose
- Scope and non-scope
- Required, optional, and conditional inputs
- Prior artifacts and context
- Expected output and format
- Acceptance criteria
- Gates
- Failure conditions
- Side effects and authorization
- Idempotency
- Rollback or recovery
- Evidence
- Canonical location

## Change rule

Changing an output shape, required input, gate, or acceptance criterion requires
versioning and a compatibility review.
