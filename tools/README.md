# Repository Tools

Status: active
Version: 1.0

## Purpose

This folder contains reusable repository-maintenance and governance-validation
tools.

## Subfolders

- helpers/ — deterministic, general-purpose helpers
- validators/ — request, schema, and document validators
- checks/ — architecture, determinism, drift, and side-effect checks
- scaffolding/ — safe structure-generation utilities

## Mutation rules

Mutating tools must identify exact targets, support preview where practical, use
idempotency keys when rerun behavior matters, and report their side effects.
