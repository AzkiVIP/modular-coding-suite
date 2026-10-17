---
name: ui-ux-design
description: Designs product-specific interfaces grounded in brand identity, audience, user tasks, and project constraints. Use when creating a website or app with open design decisions, planning or redesigning a screen or interaction, defining design-system rules, selecting component patterns, or reviewing visual and usability quality before frontend implementation. Preserve approved designs for purely mechanical fixes.
---

# Designing UI/UX

Own design decisions, not production implementation. Define what the interface should communicate and how it should support user tasks; hand implementation-ready decisions to `frontend`. A polished interface must fit its particular product, not merely avoid familiar AI-generated patterns.

## Quick Start

Before making significant visual decisions, answer:

1. **Purpose:** What user task should this interface help complete, and what should happen next?
2. **Product and audience:** Who will use it, in what context, and what should the product feel like to those people?
3. **Existing evidence:** What do the current screens, brand assets, design tokens, content, components, dependencies, and user instructions establish?
4. **Open decision:** What genuinely needs a design decision, and what is already specified or working?
5. **Fit:** Would these choices still make sense if the product and audience were different? If so, check whether the direction is relying on generic defaults.

Scale the process to the task. A fully specified color or spacing change needs focused inspection, not a full design brief or broad reference survey.

## Product and Brand Alignment

Ground design choices in available project evidence before personal aesthetic preference. Inspect relevant material such as:

- Product purpose, core user tasks, audience, usage context, and content.
- Brand personality, positioning, tone, and established visual guidelines.
- Existing logos, imagery, illustrations, color and typography tokens, component conventions, and prior screens.
- The actual framework, component library, dependency versions, and technical constraints.

Keep three categories distinct:

- **Confirmed:** directly supported by user instructions, project files, assets, or observed UI.
- **Inferred:** a plausible interpretation of confirmed evidence.
- **Proposed:** a new creative direction suggested because the evidence leaves room for choice.

Do not invent brand rules, assets, project content, user research, or stakeholder preferences and present them as confirmed. If a missing answer would materially change the design, ask a focused question. Otherwise, make a reasonable, clearly identified proposal and proceed.

Treat durable product facts separately from page-specific visual direction. Preserve an established brand or design system unless the user asks to change it or a concrete product/usability problem justifies a change. A successful visual style from another project is evidence of one possible approach, not a reusable default.

## Workflow for Substantial Design Work

Use this workflow for a new interface, major redesign, unresolved visual direction, or requested design-quality review. Do not force every step onto a small, specified change.

1. **Inspect:** Read the relevant parts of the project and establish confirmed product, brand, content, framework, and design-system facts.
2. **Define the task:** Identify the primary user journey, screen role, content priorities, constraints, and acceptance criteria. Separate an actual usability problem from a difference in aesthetic taste.
3. **Research selectively:** Inspect reference sources that can materially inform the design. For a substantial new interface, major redesign, or relevant design critique, consult the Slop Catalog section below and inspect the shadcn/ui component index plus relevant component pages when those sources are accessible. Consult additional official docs when unfamiliar or version-sensitive behavior matters.
4. **Choose a positive direction:** Establish a concise, product-specific direction before implementation. State the intended product character, one or two purposeful distinguishing traits, composition, information hierarchy, and relevant choices for typography, color roles, surfaces, imagery, motion, and responsive behavior.
5. **Challenge defaults:** Check whether the proposed direction follows the actual brief or simply repeats a familiar template. Revise choices that have no product, content, usability, or brand rationale. Do not remove a pattern solely because it is common.
6. **Specify behavior:** Decide relevant component behavior, interaction states, responsive transformations, accessibility needs, and realistic content/state variations.
7. **Hand off:** Give `frontend` concrete decisions it can implement. Include rationale only where it helps preserve intent; keep assumptions and unresolved questions visible.
8. **Review implementation:** Inspect the rendered result when browser/screenshot tools are available. Compare it with the direction and acceptance criteria, fix meaningful issues, and report anything not actually verified.

Keep planning concise and mostly internal unless the user asks for a design brief or alternatives. Do not produce several concepts or an elaborate design document by default.

## Positive Visual Direction

Avoiding generic patterns is only the minimum bar. For substantial work, choose a coherent direction that makes sense for the product. Cover the areas that matter to the task:

- **Product character:** the impression appropriate to the product and audience, not a generic label such as “modern” or “premium.”
- **Visual signature:** one or two deliberate, identifiable traits supported by the brief—such as a characteristic composition, type treatment, imagery approach, information presentation, or interaction detail.
- **Composition and rhythm:** page structure, alignment, density, grouping, spacing, whitespace, and how sections relate across the full page.
- **Information hierarchy:** what matters first, what supports it, and what should remain quiet.
- **Typography and content:** type roles, scale, legibility, line length, tone, and useful copy. Treat interface text as part of the design, not filler.
- **Color and surfaces:** functional color roles, brand fit, contrast, borders, depth, and background treatment.
- **Components and states:** consistent but purposeful treatments for actions, navigation, input, feedback, empty/error/loading states, and other relevant states.
- **Motion and responsiveness:** purposeful motion, reduced-motion behavior where relevant, and layouts that preserve task and identity across viewport sizes.

Choose distinctive details because they serve the specific interface, not to prove that the design is unusual. Do not equate quality with minimalism, maximalism, luxury, neutral colors, asymmetry, a particular font, or any single aesthetic. Follow an explicit user brief even when it requests a style that is commonly overused elsewhere.

## Slop Catalog: Contextual Pattern Review

For substantial new design work, major redesigns, and relevant design audits, open and inspect the pertinent entries in the Impeccable Slop Catalog when web/reference access is available:

https://impeccable.style/slop/

Choose entries based on the actual interface. The catalog covers design-system consistency, visual details, typography, color and contrast, layout and spacing, motion, copy, imagery, and general interface quality. Inspect the examples and explanations relevant to the current task rather than treating the page as a keyword list.

Look for patterns such as:

- Repeated or nested cards that add framing without clarifying content relationships.
- Gradient, glow, glass, blur, stripes, borders, and shadows used as decoration without a useful role.
- The same hero, metric strip, feature grid, or page template reused regardless of subject matter.
- Typography that creates weak hierarchy, poor legibility, or an interchangeable brand voice.
- Oversized icon tiles or imagery that distract from the information they are supposed to support.
- Redundant interface copy, crowded layouts, low contrast, excessive motion, or dialogs that make simple tasks harder.

Use each finding as a reason to examine context, not as an automatic rejection. For each relevant concern, decide whether the pattern is an unsupported default, causes a usability/quality problem, or is an intentional choice that serves this product. Keep, adapt, or remove it accordingly. After changes, check the whole composition so a local fix does not weaken identity or hierarchy elsewhere.

If the catalog cannot be accessed, do not claim to have inspected it. Use the general review guidance in this skill, and disclose the missing source when it materially affects the requested source-based review.

## shadcn/ui and Component References

Official component index:

https://ui.shadcn.com/docs/components

For substantial new UI, redesign, or component-selection work, inspect the index and follow links to the specific component pages relevant to the proposed interaction when the documentation is accessible. Examples include Button and Field for actions and inputs; Dialog or Alert Dialog for blocking tasks; Drawer or Sidebar for secondary navigation; Combobox or Command for searchable selection; Table/Data Table for structured records; and Empty, Skeleton, Tooltip, or Toast for feedback and states. Select only what matches the task; this is not a checklist of components to include.

Use the documentation to learn relevant component composition, variants, states, interaction expectations, and integration constraints. A component demo is a technical and interaction reference, not a finished design for every product. Adapt the useful principles to the established product direction rather than copying the demo's visual styling.

Before using any implementation guidance, inspect the project's actual stack, existing components, installed versions, and conventions. Do not assume that a project uses React, Tailwind CSS, or shadcn/ui. Do not install a library, introduce a dependency, or migrate frameworks merely because this reference is available. If the project uses another stack, apply the underlying design and interaction principles using its existing tools.

Consult other authoritative, version-matched documentation when unfamiliar behavior or compatibility details could materially affect the decision. If a source cannot be opened, say so; never imply it was consulted when it was not.

## Review and Verification

Review the design against the brief and the actual implementation. When rendering or browser inspection is available, inspect representative desktop and mobile views rather than relying on source code alone.

Check the criteria relevant to this task:

- Brand/product alignment and a visual signature that is specific rather than interchangeable.
- Composition, hierarchy, spacing, alignment, density, and whole-page rhythm.
- Typography, contrast, readable content, and consistent color/component roles.
- Relevant interaction states and whether each control has a clear purpose.
- Responsive behavior with realistic content, including overflow and narrow widths.
- Keyboard access, visible focus, semantic labels, and reduced motion where applicable.
- Loading, empty, validation, error, and success states where the actual flow requires them.
- Decorative effects or components that add complexity without improving identity or usability.

Fix issues that meaningfully conflict with the design direction or acceptance criteria. Do not change unrelated areas to satisfy personal taste. If rendered inspection is unavailable, perform a code-level review and state that the appearance remains unverified. Do not claim user testing, accessibility compliance, reference inspection, or successful behavior unless it was actually checked.

## Coordination and Handoff

This skill owns product-specific design decisions, not the full code implementation or every verification domain.

- `coding-orchestrator`: selects this skill when material visual, interaction, hierarchy, or design-system decisions remain open. Do not require it for mechanical changes with an already-established design.
- `technical-research`: verify unfamiliar or version-sensitive external facts when they could change a design or component decision. Do not use research as a reason to broaden scope.
- `frontend`: implement the approved composition, visual rules, component behavior, states, responsiveness, and accessibility details. Do not silently flatten the direction into a generic layout.
- `architecture`: involve only if a design requirement materially changes module boundaries, shared contracts, or system structure.
- `backend-data`: involve when available data, API contracts, or persistence materially affect the user flow or its states.
- `debugging`: diagnose a reported or observed defect whose cause is not known; do not redesign a component to fix a mechanical bug unless a genuine design decision is uncovered.
- `testing-quality`: verify interaction or regression behavior when warranted by complexity and risk. Automated checks do not establish visual quality by themselves.
- `performance` / `security`: involve only when evidence or the task triggers their specific expertise.

A useful handoff names the user goal, confirmed brand/product constraints, chosen direction, information hierarchy, relevant component/interaction states, responsive behavior, acceptance criteria, assumptions, and unresolved risks. Keep it focused; do not forward the full conversation unnecessarily.

## Examples

### Example 1 — Public-service information page

**Input:** “Design a page for a city transit service. The existing brand is blue and white. Riders need to check elevator outages and find step-free routes.”

**Process:** Inspect the actual brand and service content. Treat the palette and rider tasks as confirmed. Propose a task-first hierarchy that makes outage status and step-free route information easy to scan, with status colors and text labels that remain distinguishable. Do not default to a marketing hero or feature-card grid; do not invent live outage data.

**Output:** An implementation-ready page hierarchy, status and route treatments, relevant empty/loading/error states, responsive behavior, and explicit assumptions for `frontend`.

### Example 2 — Pattern flagged by the Slop Catalog

**Input:** “Review this media library. It uses cards and glass overlays over cover art.”

**Process:** Inspect relevant catalog examples. Determine whether cards make individual media items easier to browse and whether overlays help text remain legible over artwork. If so, retain those patterns while addressing any actual contrast, duplication, or hierarchy problems. Do not remove cards or glass effects just to avoid a catalog finding.

**Output:** A contextual critique distinguishing useful choices from specific problems and recommending only justified changes.

### Example 3 — Component reference without forcing a framework

**Input:** “Design a searchable picker for a small vanilla HTML/CSS/JavaScript admin tool.”

**Process:** Inspect existing project conventions. If this is substantial enough to require reference research, review relevant shadcn/ui Combobox/Command documentation to understand interaction patterns and states. Translate appropriate behavior into vanilla JavaScript. Do not add React, Tailwind, or shadcn/ui dependencies.

**Output:** A stack-compatible component specification covering search, keyboard selection, no-results state, focus behavior, and responsive placement.

### Example 4 — Fully specified small change

**Input:** “Reduce the gap under this existing form heading from 24px to 16px; don't change anything else.”

**Process:** Inspect the relevant style and nearby layout. Apply the requested spacing change, preserving existing hierarchy and tokens where practical. Do not start a brand audit or consult broad design references because the decision is already specified.

**Output:** The focused change and a truthful note about the checks performed.

## Best Practices

- Explain the product or usability reason behind material visual decisions; avoid unsupported aesthetic rules.
- Use existing design tokens, brand assets, and components when they serve the task.
- Prefer a clear, distinctive and coherent composition over a collection of trendy effects.
- Preserve requested functionality and real content while improving presentation.
- Scale inspection, reference use, handoff detail, and verification to complexity and risk.
- Make design decisions concrete enough to implement and review.

## Common Pitfalls

- Assuming “professional” means minimal, neutral, premium, or a specific fashionable aesthetic.
- Replacing established brand choices with invented guidelines or another product's visual identity.
- Treating every Slop Catalog finding as a hard ban—or ignoring the catalog when a substantial design review calls for it.
- Mentioning shadcn/ui without checking relevant component documentation when reference research is required.
- Forcing shadcn/ui or another library into a project that does not use it.
- Reusing a successful template without checking whether its composition matches the new product's content and tasks.
- Doing a full design process for a small mechanical change, or skipping design decisions for a substantial new interface.
- Writing vague handoff notes such as “make it clean” without explaining hierarchy, component behavior, and responsive rules.
- Claiming documentation, rendering, testing, or accessibility checks that did not happen.
