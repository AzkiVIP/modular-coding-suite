# Upgrade Brief — `ui-ux-design`

## Goal

Improve the existing UI/UX design specialist so it consistently produces product- and brand-aligned design decisions, uses design references deliberately, and reviews visual quality without treating popular patterns as automatic failures.

## Scope

- Revise the existing canonical `skills/ui-ux-design/SKILL.md` and synchronize it into `distributions/claude-code/skills/ui-ux-design/`.
- Preserve the skill name and its responsibility as a design decision/handoff specialist.
- Keep all ten skill boundaries, all ten canonical skills, and all ten Claude Code distribution copies.
- Keep project/plugin version identifiers at `1.0.0`, per the repository owner's explicit direction. This prepared package is for updating repository files; it does not rewrite the existing public `v1.0.0` tag or release assets.
- Do not create a new design skill or require new UI libraries/frameworks.

## Research requirements

1. Inspect the current skill and relevant suite coordination guidance.
2. Inspect Anthropic Frontend Design: https://github.com/anthropics/skills/tree/main/skills/frontend-design
3. Inspect Impeccable: https://github.com/pbakaus/impeccable
4. Open and examine relevant entries in the Slop Catalog: https://impeccable.style/slop/
5. Inspect the official shadcn/ui Components index and component-specific pages when relevant: https://ui.shadcn.com/docs/components
6. Check applicable source licenses when adapting materials. Use original wording and do not reproduce large passages or code.
7. State source-access limits and never claim research that did not happen.

## Design requirements

- Make brand/product alignment central: purpose, audience, task, established identity, actual assets, existing screens and design system.
- Distinguish confirmed facts, reasonable inferences, and creative proposals.
- Establish a positive, product-specific direction for substantial work; do not rely only on anti-pattern avoidance.
- Use Slop Catalog examples as contextual diagnostics for substantial new design, major redesign, and relevant design reviews when available.
- Inspect the shadcn/ui index and relevant component pages for substantial UI/component decisions when available, without imposing React/Tailwind/shadcn or extra dependencies.
- Preserve task-scaled behavior for small mechanical changes.
- Keep handoffs actionable for `frontend`, and keep research, architecture, implementation, debugging, and testing within their existing specialist boundaries.
- Review rendered UI when tools permit; distinguish visual judgement from automated checks and disclose unverified aspects.

## Acceptance

See `evals/evals.json` for representative behavior probes. Structural checks cover file/frontmatter identity, required research references, task-scaled workflow, examples, coordination, and synchronization. Model behavior still needs a paired old-versus-new evaluation in the same model/environment plus human visual review.
