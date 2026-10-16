# Modular Coding Suite v1.0.0

A modular, portable set of ten `SKILL.md` guides for AI coding assistants. Use the orchestrator first to select the smallest relevant set of specialists, then apply and verify their guidance.

This is the **first public release**. Earlier numbered archives were internal development snapshots; their numbering is not part of the public release sequence.

## Included skills

- `coding-orchestrator` — selects and coordinates the smallest relevant set of specialists.
- `architecture` — system boundaries, structure, and design trade-offs.
- `ui-ux-design` — product-specific visual direction and interface quality.
- `frontend` — frontend implementation, fidelity, interaction, and accessibility.
- `backend-data` — APIs, business logic, databases, and data integrity.
- `debugging` — root-cause diagnosis and minimal corrective changes.
- `performance` — measurement-driven performance investigation.
- `security` — risk assessment, safe data handling, and remediation.
- `testing-quality` — tests, regression coverage, and evidence-based verification.
- `technical-research` — documentation, APIs, libraries, and compatibility research.

## Portable usage

1. Read [`skills/coding-orchestrator/SKILL.md`](skills/coding-orchestrator/SKILL.md) first.
2. Follow its selection rules to identify relevant specialists.
3. Read the selected `SKILL.md` files before applying their workflows.
4. Verify the actual result and report what was and was not tested.

The suite is a set of instruction documents, not a runnable application. Whether skills are automatically loaded or coordinated depends on the host AI tool. If automatic skill invocation is unavailable, the model or user must explicitly provide the relevant files and follow their instructions. Do not claim an automatic invocation that did not happen.

## Claude Code plugin distribution

A Claude Code plugin package is included under [`distributions/claude-code/`](distributions/claude-code/). Its plugin manifest and skill directory follow Claude Code's plugin layout. When making a plugin release ZIP, package the **contents of that plugin directory** (including `.claude-plugin/` and `skills/`) rather than uploading the whole repository as a plugin.

The root `skills/` directory is the canonical portable source. The plugin's `skills/` directory is a distribution copy; when updating skills, sync it from the canonical source before packaging a new plugin release.

## License

MIT. See [`LICENSE`](LICENSE).

## Credits

Created by AzkiVIP  <img src="https://i.imgur.com/vMTgvCV.png" width="16">.
