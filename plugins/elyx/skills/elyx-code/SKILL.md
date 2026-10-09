---
name: elyx-code
description: Translate between Elyx designs and web code. Use when implementing or updating code from an Elyx design, creating or updating an Elyx design from code, importing an existing app into Elyx, or keeping linked design and code in sync.
---

# Elyx and code

Use the `elyx` skill for Elyx inspection, language documentation, authoring, and validation. Follow the target project's `AGENTS.md` and existing code conventions for its framework, components, and checks.

## Establish the correspondence

- Identify the requested direction and scope: a component, screen, design system, or app. For an update, inspect the changed side (including its diff when available) before editing its counterpart. Keep unrelated states and layers intact.
- Look for existing `meta.code` links, code backlinks, component implementations, tokens, assets, and nearby examples. Treat a link as a lead to verify against both sides, not proof that everything currently matches.
- For an existing Elyx target, inspect its resolved structure with `inspect component` and its appearance with `render`, using the relevant context and instance depth. Source text alone cannot reveal the final layout or overrides.
- For code-side feedback, check the repo's scripts and docs and nearby stories for an existing route, preview, or component harness that shows the relevant state. If one is practical, use available browser or component tools to compare appearance and behavior; accessibility and computed-style feedback can explain what a screenshot cannot. Do not add a browser dependency solely for this step. If runtime inspection is unavailable, work from source and existing project checks, and say what could not be observed.

## Elyx to web concept map

Use these as starting analogies, then follow the target framework and the resolved design:

| Elyx | Web code analogy | Check |
| --- | --- | --- |
| Stack `layout: {direction: .row, item-gap: 8px}` | `display: flex; flex-direction: row; gap: 8px` | Preserve order, alignment, wrapping, and responsive behavior. Nested stacks or grid may fit the code better. |
| Freeform children with `left` / `top` | A `position: relative` parent and `position: absolute` children for intentional overlap | Coordinates are relative to the parent; canvas placement need not become app positioning. |
| `.auto`, percentages, `overflow` | Intrinsic sizing, parent-relative percentages, CSS overflow | Elyx clips by default; CSS overflow defaults to visible. A freeform frame without a size can collapse. |
| Tokens | CSS custom properties, Tailwind theme tokens, or the project's token system | Preserve roles, aliases, and units. |
| `@context` | Theme selectors, locale state, or responsive breakpoints | Inspect which tokens or structural variants change in that context. |
| Component instance (`frame : source`) | Component call, such as `<Card />` | Instance overrides become values passed or composed at that use site, not a copied implementation. |
| `@variant` | A prop, state, or style variant in the component API | The Elyx variant inherits its base; inspect the actual code API before choosing a prop. |
| `slot: .true` on a frame | React `<Card>{content}</Card>` for one region; a named `header` prop or component composition for multiple regions | Preserve the region's layout. Elyx source children are example content that instances start with; decide whether code should supply a default or leave content to callers. |
| `connections: {target: ...}` | Navigation or a transition to another screen, component, or state | The target records destination intent; inspect code for the event, guard, and transition behavior. |

## From Elyx to code

- Reuse the project's components and design tokens. Match a token's semantic role as well as its value; preserve token bindings where a corresponding code token exists. If none exists, add a suitable token using project conventions or explain the unresolved mapping. Inspect a component's actual API before choosing props.
- Translate layout intent into the project's normal responsive layout primitives. Preserve content, assets, typography, and the represented states. For interactive controls, preserve selection rules, initial state, disabled behavior, and keyboard access using the project's behavior patterns.
- For a flow, read `connections` declarations in Elyx source and follow their targets across files; do not rely on component inspection alone to find them. Map each meaningful destination and branch to the project's routes or state transitions, then verify the interaction that activates it in code. A destination need not appear on the current board for its connection to be valid.
- For updates, follow the existing design-to-code correspondence and change the affected implementation rather than recreating it.

## From code to Elyx

- Inventory relevant Elyx tokens, components, contexts, and links before creating new ones. Reuse them where they express the code's roles and structure. When importing a design system, establish shared tokens and theme contexts before binding components to them. Preserve the code's base and semantic token layers and resolved lengths; verify representative aliases resolve in each theme.
- Use components for reused controls, variants for meaningful differences, slots for composable regions, and instances for screen composition. Use stacks for normal UI flow and freeform placement for intentional overlap or canvas arrangement.
- Represent a specific, valid UI state. For conditional or data-driven UI, choose representative states rather than combining branches into a screen the app cannot show. Capture theme, language, and responsive differences with existing project conventions and Elyx contexts or variants where appropriate.
- When code has a meaningful flow between represented screens, panels, or states, put a connection on the source control and target the corresponding Elyx layer. For example, after importing `"./checkout.elyx" as Checkout`, a continue button can use `connections: {target: Checkout.paymentScreen}`. Keep destinations as references, including across files; use an overview with `draw-connections: .true` when seeing the flow together helps.
- For updates, edit the linked declaration or instance that corresponds to the code change; preserve the shared component and other instances unless the code change affects them too.

For example, if code defines a shared default and one caller supplies a different title:

```tsx
function PlanCard({ title = "Plan" }: { title?: string }) {
  return <article>{title}</article>
}
<PlanCard title="Starter" />
```

the linked Elyx component carries the default, while the caller's value belongs on its instance:

```elyx
export planCard = frame {
  children: title,
  title = text {contents: "Plan"}
}
starterCard = frame : planCard {
  title {contents: "Starter"}
}
```

If only that callsite changes, update `starterCard`; if the component default changes, update `planCard`. Check the resolved instance before editing either.

## Translation details that are easy to miss

- A stacked child's `left` / `top` are ignored.
- Use `overflow: .visible` in Elyx when content intentionally extends beyond a frame, for example, shadows.
- Elyx text `letter-spacing` is a percentage of font size, and positive `line-height` is a multiplier; `0` means automatic.
- An Elyx variant is an instance of its base with overrides. `@context` selects alternate tokens or contextual variants; it does not itself define a web breakpoint or component prop.

## Link and check the result

- When the correspondence is verified, put `meta.code` on the narrowest useful Elyx declaration. Use repo-relative paths and stable symbols, as in this pair:

  In `elyx/design/blocks/planCard.elyx`, on the `planCard` declaration:

  ```elyx
  meta: {code: "src/components/blocks/plan-card.tsx#PlanCard"}
  ```

  Above `PlanCard` in the code, if the project uses backlinks:

  ```tsx
  // @elyx elyx/design/blocks/planCard.elyx#planCard
  ```

  From design, open the linked implementation; from code, search for its backlink or a matching `meta.code`. Update it when a path or symbol moves. Instances inherit source metadata; do not copy the component link onto them.
- Normalize and run full diagnostics on changed Elyx files, then render the affected state and contexts. Run the project's relevant code checks. If a code preview is available, inspect the changed state and compare its appearance and behavior with the design. Report material gaps or unverified states.
