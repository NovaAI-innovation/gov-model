# Schema Registry

Status: active
Version: 1.0

## Purpose

This folder contains machine-readable contracts used to validate requests,
outputs, gates, versions, and compatibility.

## Folder rules

- v1/ contains shared version-one definitions.
- requests/ contains request-manifest schemas.
- outputs/ contains output and response schemas.
- gates/ contains gate and gate-result schemas.
- compatibility/ contains migration and compatibility schemas.

## Synchronization

Every schema must identify its human-readable source specification and contract
version. Schema changes require contract tests and documentation review.
