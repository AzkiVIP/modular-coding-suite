# UI/UX Design — Manual Smoke-Test Notes

These are single-pass examples produced by applying the revised instruction set to the test prompts. They check whether the written rules are internally actionable. They are **not** independent model runs, a baseline comparison, or evidence that another model will follow the skill the same way.

## Case 1 — Brand-aligned bicycle repair website

**Prompt focus:** existing orange/charcoal brand; service comparison, turnaround expectations, booking; no invented claims.

**Sample handoff:**
- Confirmed: repair-shop domain, orange/charcoal brand, users comparing repair services, turnaround and booking goals.
- Still unknown: actual service names, prices, turnarounds, logo assets, booking capabilities. Do not fabricate them.
- Direction: practical workshop-oriented information design, led by clear service comparisons and a visible booking path. Use genuine shop/work imagery only if available. Let orange distinguish primary actions and meaningful status; use charcoal/neutral surfaces for readable information. Avoid generic SaaS metrics and fake testimonial sections.
- Layout: concise service comparison with turnaround information adjacent to each service; booking action placed near the service details and repeated only where it helps completion. On narrow screens, preserve the service/turnaround relationship and a reachable booking action.
- Handoff: exact content dependencies, type/color roles using existing tokens when present, booking state requirements, responsive behavior, and open facts to confirm.

**Smoke-test assessment:** Meets brand alignment, no-invented-content, task hierarchy, responsive, and actionable-handoff expectations.

## Case 2 — Clinic appointment cards, glow, and nested filter cards

**Prompt focus:** distinguish useful patterns from unsupported decoration.

**Sample critique:**
- Keep cards for distinct appointment slots if they help users compare date/time/provider availability; the content relationship justifies a discrete container.
- Keep an overlay or contrast treatment only if it improves legibility or communicates a meaningful state. Remove the glow behind every section if it provides no status, hierarchy, or brand purpose.
- Simplify nested filter surfaces to one coherent group with explicit labels. Redundant card framing adds visual weight and consumes room without helping users choose a time.
- Check status text and colors together, selected/disabled slots, no availability, loading, and mobile scrolling/focus behavior.
- If the Slop Catalog is accessible, inspect the specific entries about card layering, decorative effects, and contrast; describe any finding as a review prompt rather than an automatic verdict.

**Smoke-test assessment:** Distinguishes task-serving cards from unneeded decoration and explicitly makes catalog access honest.

## Case 3 — Vanilla-JS inventory table

**Prompt focus:** use shadcn/ui documentation as a reference without forcing its stack.

**Sample handoff:**
- Inspect existing markup, CSS tokens, table semantics, data shape, and current sorting/filtering behavior first.
- For the substantial component decision, inspect the official component index and relevant Table/Data Table docs if web access is available. Use them to think through headers, sort affordances, filtering, and meaningful table features, then implement with the project's existing vanilla stack.
- Give part name/SKU and on-hand quantity clear hierarchy; show low-stock status with text plus a meaningful visual cue instead of color alone. Only include sorting/filtering that staff need now. Do not add row selection or pagination when the brief says they are unnecessary.
- Use a real HTML table with accessible headers/controls. On narrow screens, choose a deliberate pattern (e.g. a scrollable table with cues, or compact row treatment) based on which fields must be compared; do not silently hide the low-stock value.
- Do not install React, Tailwind, shadcn/ui, or table libraries.

**Smoke-test assessment:** Covers reference inspection, adaptation to existing stack, scoped features, accessibility, and responsive data display.

## Case 4 — Fully specified button-token change

**Prompt focus:** avoid unnecessary process.

**Sample action:** Inspect the existing component and the defined `--color-primary-strong` token, apply that token to the save button, and inspect the final diff plus basic contrast/nearby-style impact. Do not do a full brand audit or open external references because the visual decision is already specified. State exactly which local check was performed.

**Smoke-test assessment:** Scales the workflow down and preserves scope.

## Case 5 — Explicit brutalist youth music festival direction

**Prompt focus:** honor an intentional visual brief even where a technique resembles a common anti-pattern.

**Sample direction:** Preserve the requested red/black palette, dense brutalist type, and oversized graphics as the core identity. Use deliberate alignment and strong spacing breaks to prevent the density from making event name, date, venue, lineup, and ticket action hard to scan. Review text contrast, narrow-screen line breaks, hit targets, keyboard focus, and motion; revise only where an observable readability or usability issue appears. The style itself is not a defect merely because it may resemble a known design pattern.

**Smoke-test assessment:** Follows the explicit style brief and uses quality/accessibility checks rather than taste-based rejection.

## Result

All five cases can be addressed coherently using the revised written instructions. This validates basic clarity and cross-section consistency only. It does **not** establish a behavioral improvement versus the old skill or prove rendered visual quality. For that, run the same prompts against the old snapshot and the new skill in the same model/environment, save both outputs, then perform qualitative and quantitative grading; visual tasks should also receive human review of rendered outputs where possible.
