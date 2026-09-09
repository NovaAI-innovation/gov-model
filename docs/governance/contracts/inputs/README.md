# Input Contract Catalog

Status: active
Version: 1.0

## Purpose

This folder defines input classes and completeness requirements for requests.
Reusable request instances belong in templates/requests/; machine validation
belongs in schemas/requests/.

## Required input classes

- Basic variables
- Structured fields
- Complex context
- Prior artifacts
- Constraints
- Authorization
- Tools and environment
- Temporal requirements
- Evidence and baselines

## Required field behavior

Each input must declare its type, source, requiredness, validation, freshness,
provenance, and sensitivity. Missing required inputs block submission.
