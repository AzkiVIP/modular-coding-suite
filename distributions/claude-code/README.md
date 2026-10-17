# Modular Coding Suite — Claude Code Plugin v1.0.0

This directory is the Claude Code plugin distribution of Modular Coding Suite. It includes `.claude-plugin/plugin.json`, the ten skill directories under `skills/`, and the project's Apache-2.0 license.

The root repository `skills/` directory is the canonical portable source. This directory is a distribution copy and should be synchronized whenever the canonical skills are updated.

## Packaging

Create the plugin ZIP from the **contents of this directory**, so the archive root contains `.claude-plugin/`, `skills/`, `README.md`, and `LICENSE`. Do not nest the full repository inside the plugin ZIP.

## Included skills

- `coding-orchestrator`
- `architecture`
- `ui-ux-design`
- `frontend`
- `backend-data`
- `debugging`
- `performance`
- `security`
- `testing-quality`
- `technical-research`

## License

Apache License 2.0. See `LICENSE`.
