---
name: frontend
description: Implements frontend functionality by translating approved designs into working, accessible, maintainable code that follows existing project conventions. Use when building UI components, wiring up interactions, integrating frontend with APIs, fixing frontend bugs, or modifying existing frontend code. Not for making visual/UX design decisions (use ui-ux-design), backend/API implementation, or deep performance profiling.
---

# Frontend

Implement real, working frontend functionality within existing project conventions. Code must render and behave correctly, integrate with actual data, preserve unrelated functionality, handle relevant states, and remain maintainable. Never fabricate file contents, APIs, dependencies, or behavior. Inspect the relevant evidence first.

## Quick Start

Before writing code, inspect the project context relevant to the task.

For non-trivial work, identify the applicable framework and versions, styling and state-management patterns, existing components, API contracts, design conventions, and available tests. For small, localized changes, inspect only what is necessary and skip unrelated inventory checks.

Never guess at a component's props, an API's shape, or behavior that materially affects the implementation. Read the relevant source or contract, or explicitly identify what remains unknown.

## Workflow

### Step 1: Classify the task

- **Small, clear change** (style tweak, localized bug fix, prop addition): inspect relevant files, implement the change, and perform targeted verification. Skip the full checklist.
- **New component or feature:** use the full workflow below, scaled to the task.
- **Cross-component or breaking interface change:** use the full workflow and coordinate with architecture, backend, or testing specialists when needed.

### Step 2: Full workflow for non-trivial work

- [ ] Inspect relevant framework conventions, source files, components, and tests.
- [ ] Identify the design spec or existing conventions to follow; do not redesign independently.
- [ ] Identify the actual API contract and data shape if integration is involved.
- [ ] Determine appropriate component boundaries and state ownership.
- [ ] Implement real behavior, not merely a visual shell.
- [ ] Handle relevant loading, empty, error, success, disabled, and validation states.
- [ ] Apply semantic HTML and relevant accessibility requirements.
- [ ] Check downstream consumers when changing shared interfaces.
- [ ] Perform verification appropriate to the change and environment.
- [ ] Report what changed, what was tested, what passed or failed, and what remains unverified.

Do not force a lengthy checklist onto a trivial change. Choose each step according to the actual task.

### Step 3: When assumptions break

If implementation reveals that the original plan was wrong—for example, an API contract differs from expectations or an existing component behaves differently than assumed—stop speculative changes. Re-investigate the relevant evidence, revise the plan, and continue only when the corrected approach is sufficiently clear.

## Decision Rules

### Reuse vs. new component

- Search for an existing component or pattern that substantially meets the requirement before creating another.
- Extend or compose existing components when doing so preserves clear responsibilities and established conventions.
- Create a new abstraction when it provides meaningful separation, reuse, or cohesion—not merely to reduce the number of lines in one file.
- Avoid both oversized components with unrelated responsibilities and fragmentation into trivial components that offer no meaningful benefit.

### State ownership

- Keep state with the lowest common owner of the components that need it.
- Derive values such as filtered lists, totals, and validity when they can be calculated reliably from existing data instead of storing duplicate state.
- Before adding state, check whether the value already exists in props, context, a query cache, route parameters, or another authoritative source.
- Avoid contradictory state representations that can drift out of sync.
- Follow the state-management patterns of the existing framework. Do not impose React-specific techniques on projects using different technologies.

### Design fidelity and visual finish

- If a design specification exists, implement it as a concrete contract: preserve its information hierarchy, page composition, typography roles, color semantics, spacing rhythm, component treatments, responsive transformations, and distinctive details. Do not silently simplify it into a generic layout.
- For a new application or substantial interface without existing conventions or a supplied design, coordinate with `ui-ux-design` to establish the visual direction before committing to the UI implementation. Do not interpret "no specification" as permission to default to a bare, generic, or overly restrained interface.
- If a design ambiguity is small and does not materially affect the product, make a coherent decision consistent with the established direction. Ask a focused question only when a missing answer would materially change the result.
- Treat visual hierarchy, coherence, responsiveness, and polish as implementation acceptance criteria alongside functionality, accessibility, and maintainability.
- After the initial implementation, perform a visual-fidelity pass when a browser or screenshot tool is available. Compare the rendered result with the intended design; fix weak hierarchy, monotonous repetition, awkward whitespace, inconsistent component details, and mobile layouts that merely compress desktop. If rendering tools are unavailable, inspect the code and state that rendered appearance remains unverified.
- Do not substitute generic UI-library demos or template-like layouts just to get something working. Do not introduce decorative effects arbitrarily either; implement the chosen design direction faithfully.
- Refer significant visual or interaction design decisions to `ui-ux-design` when that specialist is available and relevant, then preserve its actionable handoff during implementation.

### Search fields, icon spacing, and responsive controls

- Treat leading/trailing icons, input text, placeholder text, clear buttons, and status adornments as separate layout elements with explicit space reserved for each. Never allow an absolutely positioned icon to occupy the same horizontal space as the input text; if an icon is positioned inside the field, offset the text with sufficient logical inline padding and account for the actual icon/button width.
- Prefer a simple inline flex/grid composition when it fits the existing design. If positioning is used, verify the field's padding, icon position, text direction, and control hit area together rather than adjusting only one value by guesswork.
- Inspect long placeholders, typed text, focus outlines, clear/search buttons, and validation messages. Ensure adornments do not hide text, truncate it unexpectedly, or overlap at supported widths or browser zoom levels.
- When a reported search/input overlap occurs and the cause is unclear, inspect computed styles, positioning, line height, padding, intrinsic sizes, and responsive overrides before changing CSS. Keep the existing approved visual direction; do not redesign the component to solve a spacing defect.
- Inspect cascade order and selector specificity as well as each rule in isolation. A later generic rule such as `input[type="search"] { padding: 9px 12px; }` can reset an earlier `.field--search input { padding-left: 36px; }` when their specificity is equal; adjust the source of the conflict instead of stacking arbitrary overrides.
- Check representative narrow and wide viewports (for example, 320, 375/390, and desktop widths) when browser tooling is available. A DOM/style inspection alone does not prove the rendered field is visually correct.

### Persistence failure and rerender/focus behavior

- When a user action depends on browser storage or another persistence operation, propagate the real save result to the caller. Do not show a success toast, announce successful saving, or imply durable persistence if the write failed.
- Keep displayed state and persistence state coherent on failure: roll back the mutation, retry safely, or clearly label the result as unsaved/in-memory according to the product contract. Do not silently leave the UI implying data was saved.
- When re-rendering lists or replacing DOM nodes, consider what element currently owns keyboard focus. Preserve focus when practical, or restore it to a predictable, relevant control after the update; do not focus a removed node or always send users back to an unrelated default control.
- Verify both the success path and a controlled failure path for persistence. If real browser storage cannot be made to fail safely in the current environment, test an injected storage adapter or stub and disclose that limitation.

### New dependencies and frameworks

- Default to the existing stack without introducing new libraries, state managers, or styling systems.
- If a new dependency is proposed, explain what the existing tooling cannot adequately provide and why that gap matters.
- Check compatibility with the project's actual versions and conventions before introducing a dependency.
- Do not install or replace tools merely because they are popular or familiar.

### Real vs. fake functionality

- A requested button, form, menu, or other interaction must perform its intended action unless a stub is explicitly requested.
- A stub must be clearly identified as incomplete, with the missing integration or prerequisite stated.
- Never present mock data or simulated persistence as a completed real integration.
- Do not claim an API integration works without evidence that it follows the actual contract and handles relevant outcomes.
- If the required backend endpoint or capability does not exist, identify the limitation instead of fabricating a successful integration.

### Accessibility baseline

Apply appropriate accessibility practices by default without claiming that every component is fully accessible.

- Prefer suitable native HTML elements such as `button`, `a`, `label`, `input`, and `dialog` over custom widgets when native semantics satisfy the requirement.
- Ensure interactive controls have accessible names, keyboard operation, and visible focus behavior where applicable.
- Associate form labels and validation feedback with their respective controls. Use `aria-describedby` or the framework equivalent when appropriate.
- Follow the relevant WAI-ARIA Authoring Practices pattern when building custom widgets.
- Manage initial focus, focus containment, and focus restoration for modal dialogs as appropriate.
- Implement menus, listboxes, and comboboxes according to their specific keyboard and focus interaction patterns. Do not apply focus trapping or identical keyboard behavior to every widget.
- Use semantic structure, meaningful image alternatives, and appropriate status announcements when relevant.
- Check contrast, reduced-motion behavior, and other accessibility requirements when they apply to the change.
- Report the specific accessibility behavior implemented and any remaining gaps. Do not claim complete compliance without an appropriate audit.

### Performance

- Do not memoize, virtualize, lazy-load, or cache speculatively.
- Consider these techniques when the workload, code path, bundle, resource size, or measured behavior provides a credible reason.
- For an unclear performance complaint, first determine what is slow using appropriate evidence, such as browser profiling, network inspection, render counts, or relevant measurements.
- If a bottleneck is systemic or remains unclear, coordinate with `performance` rather than guessing.
- If measurement is unavailable, disclose the uncertainty instead of claiming an optimization is necessary or proven effective.

## Coordination with Specialists

This skill owns frontend implementation, not decisions or procedures belonging to every other domain. Involve specialists only when the task requires their expertise:

- `ui-ux-design`: Significant visual, layout, interaction, or accessibility design decisions remain open.
- `backend-data`: A required API, server behavior, persistence operation, or data contract must be created or changed.
- `architecture`: A change affects shared boundaries, dependencies, or system structure.
- `debugging`: The cause of an observed frontend failure is not established.
- `performance`: Profiling or evidence-based analysis is needed for a material performance problem.
- `security`: The change affects a meaningful trust boundary, authentication/authorization, sensitive data, or security-relevant input handling.
- `testing-quality`: Broader integration, regression, or test-strategy work is justified.
- `technical-research`: Unfamiliar or version-specific API/library behavior cannot be established from available project evidence.

Keep handoffs focused on the relevant contract, evidence, affected components, and acceptance criteria. If the host cannot access or invoke a specialist, do not claim that its full procedure was applied. Do not involve every specialist by default.

## Examples

### Example 1: Small styling change

**Input:** "The save button should be full-width on mobile."

**Process:** Locate the existing button component and responsive styling conventions. Apply the smallest change using the current breakpoint utility or media-query pattern. Do not touch unrelated buttons, create a new breakpoint system, or refactor the component.

**Verification:** Check the relevant mobile viewport if a browser or rendering environment is available. Otherwise, inspect the changed styles and state that the rendered result was not visually verified.

### Example 2: New interactive component

**Input:** "Add a dropdown filter for order status."

**Process:** Check for a suitable existing `Select`, `Dropdown`, or `Menu`. Place selected-status state with the owner responsible for filtering the list; avoid duplicating the same state inside both dropdown and list. Prefer native `select` if it meets the requirement. If a custom listbox is justified, implement its applicable keyboard and focus behavior. Handle empty results and relevant loading/error states.

**Verification:** Confirm selecting an option changes the displayed results and that relevant keyboard interaction works. Do not claim checks passed unless they were performed.

### Example 3: API-dependent feature

**Input:** "Show the user's recent orders on the dashboard."

**Process:** Inspect the actual API client, endpoint contract, types, and existing integration patterns. Implement against the verified response structure. Handle relevant loading, empty, error, and success states. Prevent stale or out-of-order responses when the interaction pattern makes that risk relevant.

**Verification:** Test relevant states using the actual API or a mock that faithfully follows the verified contract. If only mocked behavior was tested, do not claim the live integration was verified.

### Example 4: Performance complaint

**Input:** "The product list feels slow."

**Process:** Investigate whether the delay comes from network activity, rendering, state updates, large data, or another cause. If measurement shows that rendering or scrolling a large list is a meaningful bottleneck, evaluate virtualization against behavior, accessibility needs, and implementation cost. If the bottleneck is elsewhere, address the actual cause or coordinate with `performance`.

**Verification:** Compare relevant behavior or measurements before and after when possible. Do not claim measurable improvement without evidence.

### Example 5: Search field icon overlaps its text

**Input:** "The search icon and placeholder/text overlap in the search box."

**Process:** Treat the current design as approved unless the user asks to redesign it. Inspect the input markup and computed CSS for positioning, padding, gap, icon width, line-height, and responsive overrides. Determine whether the icon is an inline sibling or an overlay; reserve non-overlapping space accordingly and keep the hit area and focus indicator intact.

**Verification:** Check the empty placeholder and populated input at a narrow viewport and desktop width, including focus state and any clear button. Use a rendered browser/screenshot when available. If only source inspection is possible, state that visual overlap remains unverified.

### Example 6: Persistence failure must not look successful

**Input:** "Adding a task still says it was saved when localStorage fails."

**Process:** Trace the save return value/error through the caller, state mutation, render, and status announcement. Establish the intended failure contract before changing behavior. Ensure the UI either rolls back or explicitly represents unsaved in-memory state.

**Verification:** Simulate a storage write throwing (for example, quota/security error). Confirm no durable-save success message appears and that the visible data state matches the documented failure behavior; also verify the normal save path.

### Example 7: Shared component change

**Input:** "Add a new required prop to the shared `Modal` component to change close behavior."

**Process:** Search existing `Modal` usages. Determine whether an optional prop with a backward-compatible default can meet the requirement. If a breaking change is necessary, identify and update affected consumers, types, and relevant tests. Coordinate with `architecture` if shared boundaries or contracts materially change.

**Verification:** Run relevant tests for the component and affected consumers when available. Report which call sites changed and which checks actually ran.

## Best Practices

- Read actual component, API, and test files instead of inferring behavior from names.
- Keep diffs scoped to the request; do not combine unrelated cleanup with a targeted fix.
- Prefer composition over configuration flags that create excessive branching.
- Use descriptive state and handler names, such as `isSubmitting` rather than `flag1`.
- Inspect error responses and failure semantics as well as API success responses.
- Update tests alongside behavior changes when the project has an applicable testing pattern.
- State verification precisely, including important untested environments or behavior.

## Common Pitfalls

- Rewriting an entire file or component to make a one-line fix.
- Adding a state manager or styling framework without demonstrating a need.
- Building a visually complete component whose controls do nothing.
- Storing the same logical data in multiple state variables that can drift out of sync.
- Using `div` click handlers where native buttons provide appropriate semantics and keyboard behavior.
- Shipping a form without relevant validation, error, or loading behavior.
- Changing a shared component interface without checking its consumers.
- Applying memoization or virtualization before establishing a credible need.
- Applying focus trapping or keyboard behavior without following the relevant widget pattern.
- Claiming an integration, test, performance improvement, or accessibility result that was not verified.
- Positioning a search icon without reserving space for the input text, causing the icon, placeholder, or typed value to overlap at narrow widths.
- Reporting a save as successful after the underlying persistence write failed.
- Re-rendering a list in a way that unexpectedly loses keyboard focus.
- Redesigning spacing, colors, or layout without a requested change or identified problem.

## Completion Criteria

A frontend task is complete when the requested behavior is implemented within scope, relevant integration assumptions have been checked, appropriate verification has been performed, and remaining limitations have been disclosed. Completion does not require every conceivable check; the work and verification must be proportionate to the task, environment, and risks involved.
