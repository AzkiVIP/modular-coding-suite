# UI/UX Design Skill Upgrade Notes

## Scope and repository baseline

- The uploaded `Modular_Coding_Suite_GitHub_Repository_v1.0.0_Design_Upgrade_Final(1).zip` was used as the authoritative source for the researched skill set in this package.
- The uploaded archive contains all ten canonical `SKILL.md` files, all ten Claude Code distribution copies, the plugin manifest, research notes, and evaluation files. All canonical/distribution skill pairs have been retained and verified byte-for-byte equal in the prepared package.
- The live `AzkiVIP/modular-coding-suite` `main` branch was also checked through the GitHub connector. Its UI/UX skill was still the pre-upgrade version and did not explicitly contain the Slop Catalog URL. The live repository uses Apache License 2.0, so the conflicting MIT license/manifest metadata in the uploaded archive was corrected here.
- No repository write succeeded and no changes were pushed to GitHub. This ZIP is a local prepared package for the owner to review and upload. The existing public `v1.0.0` tag and release assets have not been changed.
- Scope of skill changes: update the existing UI/UX specialist, then synchronize its Claude Code copy. Other specialist skills are retained in full; no specialist was removed or renamed.

## Research findings and design decisions

### Anthropic Frontend Design

Reviewed: https://github.com/anthropics/skills/tree/main/skills/frontend-design and its `SKILL.md` page. The source emphasizes grounding visual direction in the real subject and audience, making deliberate choices in typography/composition/motion, critiquing the proposed plan against the brief before implementation, and not confusing repeated model defaults with universal rules.

Applied as an original synthesis: ask for confirmed product/brand evidence, a deliberate visual direction, and a check against generic defaults while retaining a task-scaled workflow. No text or code was copied into the skill.

The source `LICENSE.txt` was inspected: Apache License 2.0. This revision does not incorporate source text or code directly. If source wording/code is introduced later, review applicable license obligations before distribution.

### Impeccable and its product context

Reviewed: https://github.com/pbakaus/impeccable and https://github.com/pbakaus/impeccable/blob/main/PRODUCT.md. The repository describes product setup that distinguishes durable product truth from page-specific visual direction, and separate source/style checks from judgment-based design critique.

Applied as an original synthesis: separate confirmed product context from page-level visual proposals, and keep design critique distinct from claims about code/browser verification. The skill does not depend on Impeccable commands, CLI, or runtime.

The repository license was inspected as Apache License 2.0. No Impeccable code or passages were copied into the skill.

### Impeccable Slop Catalog

Reviewed: https://impeccable.style/slop/. The catalog organizes examples across visual details, typography, color/contrast, layout/space, motion, copy, imagery, general quality, and design-system consistency. Findings are review prompts, not automatic proof that a design decision is wrong.

Applied as an operating requirement: for substantial new design, major redesign, and relevant design critique, inspect relevant catalog examples when source access is available, then keep/adapt/remove patterns based on product evidence and usability—not blanket bans.

### Official shadcn/ui component documentation

Reviewed: https://ui.shadcn.com/docs/components and relevant Button, Dialog, Sidebar, and Data Table pages. The docs provide reference material for component composition and interaction. Data-table needs differ in sorting, filtering, pagination, visibility, and selection requirements.

Applied as an operating requirement: for substantial new UI or relevant component-selection work, inspect the index and relevant component pages when available. Treat these as selective design/interaction references. Do not assume the project's framework, install shadcn/ui, or migrate technologies merely to match the docs.

## Suite integration analysis

- `coding-orchestrator`: existing routing already activates UI/UX for material unresolved visual/interaction decisions and excludes it for mechanical changes. No change made.
- `frontend`: owns implementation and already expects concrete design direction, fidelity, responsive behavior, and truthful visual review. The revised handoff makes design contracts more explicit. No change made.
- `technical-research`: verifies unfamiliar/version-specific external behavior; UI/UX directs targeted source inspection without duplicating the entire research workflow. No change made.
- `testing-quality`: owns risk-proportionate test strategy; UI/UX distinguishes visual judgment from automated interaction/regression tests. No change made.
- `architecture`, `backend-data`, `debugging`, `performance`, `security`: remain conditionally activated by their current routing criteria. No change made.

## Important limitation

This document records source inspection and the reasoning for the changes, not a claim that the revised skill produces better rendered UI in all models. Structural/package checks can be run locally, but a paired evaluation using the old and revised skill in the same model/environment and human review of rendered outputs remain necessary to evaluate behavioral improvement.
