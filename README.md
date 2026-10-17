# Modular Coding Suite v1.0.0

A modular, portable collection of ten `SKILL.md` guides designed to instruct AI coding assistants. Start with the orchestrator to select the smallest relevant set of specialist skills, then apply and verify their guidance.

This is the **first public release**. Earlier numbered archives were internal development snapshots; their numbering is not part of the public release sequence.

## Included Skills

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

## Important: These Are Instructions for AI Assistants

**The contents of each `SKILL.md` file are instructions intended for an AI assistant to read and apply when performing relevant coding tasks.**

These files are not executable programs, standalone AI models, or merely reference articles. They provide task-specific guidance for how an AI assistant should plan, design, implement, investigate, and verify software.

Users can read, review, and modify these files. However, their primary purpose is to guide the behavior and workflow of an AI coding assistant.

The `coding-orchestrator` skill helps the AI determine which specialist skills are relevant to a task. The assistant should use only the necessary specialists instead of blindly applying every skill to every request.

**Important:** Having these files in a repository does not guarantee that an AI assistant will read or follow them. Automatic loading and skill invocation depend on the host AI tool and its configuration.

## Portable Usage

1. Read [`skills/coding-orchestrator/SKILL.md`](skills/coding-orchestrator/SKILL.md) first.
2. Follow its selection rules to identify the relevant specialist skills.
3. Read the selected `SKILL.md` files and apply their instructions to the task.
4. Verify the actual result and report what was and was not tested.

The suite is a collection of instruction documents, not a runnable application. If the host AI tool cannot automatically load or invoke skills, provide the relevant files to the AI explicitly and tell it to follow their instructions.

Do not claim that a skill was automatically loaded, invoked, or followed unless that actually happened.

### Optional: Manual Skill Usage Prompt

Use this prompt when your AI coding assistant can access the skill files but does not automatically select or apply them. If your platform already supports automatic skill loading and invocation, follow its native setup instead.

Copy the prompt below and provide access to the relevant `SKILL.md` files:

```text
Read and apply the provided Modular Coding Suite skill instructions when working on the requested task.

Start by reading `coding-orchestrator/SKILL.md` to identify the smallest relevant set of specialist skills. Read the selected skill files before proceeding, and apply their instructions where relevant.

Treat the skill files as instructions for your work, not as documents to summarize unless summarization is specifically requested. Do not blindly apply irrelevant skills or introduce unnecessary complexity.

Respect the task requirements, higher-priority instructions, and the actual capabilities and permissions of your environment.

Verify the result as thoroughly as the available tools allow. Clearly distinguish what was verified from what remains untested. Never claim to have read, applied, or tested something that you did not.
```

**Note:** This prompt does not provide access to skill files by itself. Ensure the AI can read the relevant files before using it.

## Claude Code Plugin Distribution

A Claude Code plugin package is included under [`distributions/claude-code/`](distributions/claude-code/). Its plugin manifest and skill directory follow Claude Code's plugin layout.

When creating a plugin release ZIP, package the **contents of the plugin directory**, including `.claude-plugin/` and `skills/`, rather than uploading the whole repository as a plugin.

The root `skills/` directory is the canonical portable source. The plugin's `skills/` directory is a distribution copy. When updating skills, synchronize the plugin copy with the canonical source before packaging a new plugin release.

## License

Apache 2.0. See [`LICENSE`](LICENSE).

## Credits

Created by AzkiVIP  <img src="https://i.imgur.com/vMTgvCV.png" width="16">.
