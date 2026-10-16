---
name: ui-ux-design
description: Makes product-specific UI/UX and visual design decisions for new and existing software projects, including layout composition, typography, visual hierarchy, interaction design, and UI library usage. Use when designing screens, redesigning interfaces, establishing a design system, or reviewing usability and visual coherence.
---

# Designing UI/UX

Design interfaces that are justified by product purpose and user tasks, not by default AI aesthetic habits. "Looks modern" is insufficient; every major decision should fit this product for a clear reason.

## Quick Start

Before proposing a layout, color, or component, answer four questions:

1. **What is this screen for?** Identify the primary user task, not just the screen category.
2. **What already exists?** Inspect relevant conventions, design tokens, installed libraries, and brand assets.
3. **What should users notice first, second, and last?** Establish information hierarchy before styling.
4. **What would make this look like every other AI-generated app?** Avoid default patterns unless they genuinely fit the product.

If the only justification for a design pattern is "it's common" or "it looks clean," reconsider it.

## Visual Quality Bar for New Interfaces

Avoiding generic AI patterns is a floor, not the goal. A design can avoid every cliché and still feel bland, unfinished, or visually forgettable. For a new app, a new major screen, or a full redesign, deliver a coherent and distinctive visual direction—not merely a tidy functional layout.

Before implementation, establish a compact design direction that answers:

- **Product character:** What should this product feel like to its actual audience, and why?
- **Visual signature:** What one or two deliberate choices make this interface recognizable, such as a distinctive composition, typography pairing, information treatment, color relationship, motif, or interaction detail?
- **Hierarchy and rhythm:** What is dominant, what is secondary, where should the eye move next, and how do density and whitespace vary across the page?
- **System:** Which color roles, type roles, spacing scale, borders, radii, icon treatment, and surface styles keep the interface coherent?
- **Fit to content:** Which layout best presents the real information and primary actions, rather than forcing everything into a familiar card/grid template?

### Required design behavior

- Treat visual appeal, clarity, coherence, and polish as acceptance criteria alongside usability and accessibility. Do not equate "professional" with sparse, beige, neutral, minimal, or restrained; choose those qualities only when they fit the product.
- Do not use anti-pattern avoidance as a substitute for making affirmative design choices. Establish what should make the interface feel intentional and memorable, not only what should be avoided.
- When the user gives no visual direction and the work is substantial, consider a few plausible directions internally, then choose the strongest fit. Do not dump a menu of concepts on the user unless they ask to compare options. State only consequential assumptions.
- Use cards, tables, sidebars, borders, color, imagery, depth, or decorative details when they improve hierarchy or product identity. No pattern is automatically good or bad; the composition and purpose decide.
- Avoid defaulting to the same editorial, SaaS dashboard, ivory-and-teal, glassmorphism, or card-grid treatment across unrelated products. A visual direction that worked for one project is not a universal template.
- Design the complete page composition—not just isolated components. Give the header, main content, filters, empty state, dialogs, and supporting details a consistent relationship and clear hierarchy.
- Do not sacrifice requested features, real content, or interaction clarity to achieve a striking screenshot. Visual craft and functional completeness must reinforce one another.

### Visual review pass

After implementation, review the rendered page at representative desktop and mobile widths when a browser or screenshot tool is available. Check the whole-page silhouette as well as individual controls:

- Is there a clear focal point and a deliberate scan path?
- Does the layout have enough contrast, variation, and rhythm, or does it feel flat and repetitive?
- Are type scale, whitespace, alignment, control dimensions, and color roles deliberate and consistent?
- Are important actions and information prominent without making everything equally loud?
- Do empty, filtered, validation, and dialog states feel like part of the same product?
- Does the mobile layout preserve the same design intent rather than merely stacking desktop elements?

Fix visible weaknesses that conflict with the chosen direction before calling the interface finished. If rendered inspection is unavailable, perform a code-level review but explicitly state that visual rendering was not verified. Never claim screenshot-based or browser verification that did not occur.

## Workflow

Adapt depth to task size. A trivial spacing or color tweak needs a focused inspection and change; a new screen, redesign, or complex flow needs a fuller design process.

- [ ] Understand product, users, primary task, and this screen's role.
- [ ] Inspect relevant existing screens, code, components, tokens, brand assets, and references.
- [ ] Separate observed usability problems from personal taste.
- [ ] Establish information hierarchy before selecting visual treatments.
- [ ] Choose and record a product-specific visual direction, including at least one purposeful visual signature for substantial new interfaces.
- [ ] Choose a layout and composition that support the primary task and create a compelling whole-page hierarchy.
- [ ] Set or reuse appropriate typography, colors, spacing, and components; do not mistake visual restraint for quality by default.
- [ ] Decide whether to reuse, extend, or introduce a UI library only when a real decision exists.
- [ ] Define relevant interaction states, responsive behavior, and accessibility needs.
- [ ] Check the result against both the generic-pattern checklist and the positive visual-quality bar.
- [ ] Review the rendered interface after implementation when tools permit; address visible flatness, inconsistency, or weak hierarchy.
- [ ] Produce implementation-ready guidance and disclose assumptions or unverified points.

### Step Detail

**1. Understand the product.** Identify the primary user task, target users, content type, and intended outcome. Different screens solve different problems; do not treat them as interchangeable design templates.

**2. Inspect the existing system.** Examine relevant screens, components, design tokens, theme files, dependencies, brand assets, and references. Preserve established conventions unless a requested change or identifiable problem justifies changing them.

**3. Separate evidence from preference.** Distinguish observable usability problems from subjective aesthetic preferences. Do not redesign a working interface solely because a different style seems more attractive.

**4. Design hierarchy and composition.** Prioritize content and actions before styling. Choose layout, density, alignment, grouping, and spacing according to the user task and content—not a familiar template.

**5. Establish typography and visual consistency.** Reuse existing typography roles and design tokens where appropriate. Make deliberate choices about type scale, line height, readability, color, and spacing. Avoid arbitrary values and decorative styling without a clear purpose.

**6. Choose components and libraries deliberately.** Check existing dependencies and versions before introducing anything new. Reuse or extend suitable components, and research alternatives only when a genuine decision is required. Treat library defaults as primitives, not finished product designs.

**7. Specify relevant behavior and states.** Define necessary interaction states, realistic content edge cases, responsive behavior, and accessibility requirements for the actual interface. Do not add irrelevant boilerplate or require every component to support every possible state.

**8. Review and hand off.** Check the design against the generic-pattern checklist. Provide concrete, implementation-ready specifications and identify assumptions, unresolved questions, and anything not actually inspected or verified.

## Generic-AI-Pattern Checklist

Before finalizing a design, check whether any of these appear without product-specific justification:

- [ ] Every section is a card in a uniform grid, regardless of content relationships.
- [ ] Gradients, glassmorphism, glow, or blur are decorative rather than functional.
- [ ] A centered hero with oversized heading appears on a non-marketing screen.
- [ ] Pill-shaped buttons or inputs are used everywhere by default rather than by design-system choice.
- [ ] Every element has equal visual weight, leaving no clear primary action.
- [ ] Decorative icons, illustrations, or animations do not aid comprehension or task completion.
- [ ] The layout would look identical if the product were replaced by an unrelated one.
- [ ] Spacing and colors are arbitrary rather than tied to a system or token.
- [ ] A shadcn/ui or other library demo is reproduced without adaptation to the product.

If a pattern is flagged, either justify why it fits this product and task or remove it. These patterns are not universally forbidden; unsupported defaults are the problem.

## UI Library and Design System Decisions

- Inspect the project's existing dependencies, installed versions, components, and conventions before selecting a library.
- Use official documentation when unfamiliar behavior, customization limits, accessibility, or version compatibility materially affects the decision.
- Compare alternatives only when a real decision exists; include existing components or a small custom implementation where appropriate.
- Do not add or replace a library without a justified benefit and consideration of compatibility and migration cost.
- Treat shadcn/ui, Radix UI, Material UI, Mantine, Bootstrap, and similar libraries as implementation primitives, not complete product designs.
- Never claim to have inspected a library, reference, or running interface unless it was actually accessible and inspected.

## Coordination and Handoff

This skill owns product-specific UI/UX decisions and design specifications, not the full code implementation.

- `frontend`: Implement the approved layout, typography, components, responsive behavior, and interaction specifications.
- `architecture`: Involve when a design requirement materially changes component boundaries or system structure.
- `backend-data`: Clarify available data and API constraints when they materially affect the proposed user flow or states.
- `technical-research`: Verify unfamiliar or version-specific library behavior when official documentation could change the design decision.
- `testing-quality`: Verify important interaction flows, responsive behavior, accessibility requirements, and design-related regressions when warranted.

Provide an actionable handoff describing hierarchy, layout, typography/tokens, component behavior, relevant states, responsive rules, and unresolved assumptions. Do not activate every specialist automatically; coordinate only where the task crosses responsibilities.

## Examples

### Example 1 — Inventory dashboard

**Input:** "Add a dashboard to our B2B inventory app showing stock levels."

**Decision:** Prioritize a dense, sortable table for rapid scanning, inline stock-state indicators, a compact summary strip, and filters that support common inventory tasks. Avoid a generic card-grid dashboard when it hides comparisons and forces excessive scrolling. Reuse existing table components and color tokens when suitable.

### Example 2 — Meditation app landing page

**Input:** "Build a landing page for a new meditation app."

**Decision:** Choose a calmer composition, restrained type hierarchy, suitable imagery or a single focal visual, and one clear primary action if these choices fit the product identity. Avoid copying a generic SaaS hero-and-feature-card template merely because it is familiar. Validate the actual brand and content requirements before treating any visual direction as final.

### Example 3 — Add a settings tab

**Input:** "Our settings page looks fine; add a Notifications tab."

**Decision:** Inspect the existing tabs and field components, add the new tab using the established pattern, reuse spacing and typography tokens, and do not redesign the rest of the page.

## Best Practices

- Treat the chosen art direction as a concrete implementation contract, not a vague suggestion. Specify the page composition, typography hierarchy, color roles, spacing rhythm, density, key component treatments, and responsive transformations.
- Judge the design by both what it avoids and what it positively achieves: clear hierarchy, distinctive character, visual rhythm, coherent detail, and a finished feel.
- Justify non-trivial visual decisions in terms of product purpose and user tasks, while still treating aesthetics and emotional impression as real design requirements.
- Reuse suitable tokens, components, typography, and libraries before creating alternatives.
- Preserve established conventions unless the user asks for a redesign or an identified problem requires one.
- Use restraint when it strengthens the chosen direction; do not default to restraint when the product would benefit from stronger contrast, richer composition, or more distinctive details.
- Be concrete in handoffs: specify hierarchy, layout, component behavior, relevant states, and responsive rules rather than saying only "clean" or "modern."
- Distinguish proposed design choices from results verified in a running implementation or with users.

## Common Pitfalls

- Defaulting to card grids, hero sections, editorial minimalism, or dashboards because they are familiar.
- Removing distinctive visual character in the name of simplicity, or producing a technically correct but flat, unfinished interface.
- Treating a successful design from another product as a reusable visual template without checking fit.
- Adding gradients, glow, blur, or animation without a functional or product-specific reason.
- Introducing a new UI library or token system without checking the existing one.
- Redesigning a working screen based on personal taste alone.
- Treating component-library defaults as a finished, product-specific design.
- Giving every element equal emphasis and obscuring primary actions.
- Providing vague design guidance instead of implementable specifications.
- Ignoring realistic content, empty/error/loading states, responsive behavior, or relevant accessibility requirements.
- Claiming usability testing or accessibility compliance that was not performed.
