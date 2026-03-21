# Homelab HA Blueprints Agent Guide

## Scope

This repo is the Git-backed source of truth for reusable Home Assistant blueprints.

## Operating Rules

- Keep blueprint inputs stable and backward compatible unless a breaking change is explicitly requested.
- Prefer clear input names, descriptions, and defaults over clever logic.
- Do not embed environment-specific secrets or host-specific assumptions in blueprints.
- Favor reuse and maintainability over feature sprawl.

## Maintenance Expectations

- Update `README.md` when a blueprint interface or behavior changes materially.
- Keep examples and documentation aligned with actual blueprint fields.
- If a blueprint becomes too environment-specific, move that logic into `ha-config` instead of overfitting the shared blueprint.

## Change Standard

- Make minimal YAML edits.
- Preserve import compatibility for existing Home Assistant installs whenever practical.