# UI/UX Design Evaluation Rubric

Use the same model, tools, project context, and prompt when comparing the old and revised skill. Evaluate skill-following behavior separately from the aesthetic quality of rendered output.

## Cross-case checks

- **Scope discipline:** Does the assistant avoid full design research for a tiny, fully specified change?
- **Evidence honesty:** Does it separate confirmed facts from inferences/proposals and avoid claiming inaccessible sources were read?
- **Brand alignment:** Are significant choices justified by the product, audience, task, and existing identity?
- **Positive visual direction:** Is there a concrete, coherent visual signature and page hierarchy rather than only a list of things to avoid?
- **Contextual anti-slop:** Does it inspect relevant Slop Catalog entries when accessible and keep patterns that serve the product while revising patterns that do not?
- **Component documentation:** Does it inspect shadcn/ui's index and relevant component pages for substantial relevant work when accessible, without forcing its framework/dependencies?
- **Handoff quality:** Could `frontend` implement the design decisions, states, and responsive behavior without guessing major intent?
- **Verification honesty:** Does it distinguish source review, code review, rendered inspection, automated checks, and human visual judgment?

## Prompt-specific indicators

1. **Brand-aligned service website:** Requirements and content are grounded; no fake factual claims; hierarchy supports comparing services and booking; responsive handoff is concrete.
2. **Clinic appointment page:** Appointment cards are judged by their browsing purpose; redundant nested surfaces and decoration are challenged for a reason; relevant Slop Catalog source is inspected if available.
3. **Vanilla inventory tool:** Relevant shadcn table docs are inspected if available; table features are scoped to actual needs; no stack migration or dependency addition; status and responsive behavior remain accessible.
4. **Specified token change:** Only the requested token/component is changed; no broad audit or reference research; verification is proportionate.
5. **Explicit brutalist brief:** The requested aesthetic is preserved unless a concrete usability/accessibility issue is found; common patterns are not rejected solely because they are familiar.

## Human visual review

For any case producing actual UI, inspect rendered desktop and mobile outputs side by side without knowing which version produced each output if possible. Judge separately: brief/brand fit, coherence and distinctiveness, hierarchy/readability, interaction and accessibility, responsive layout, and visual polish. Record examples supporting each score. Do not infer visual quality from Markdown or code inspection alone.
