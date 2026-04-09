---
name: build-order
description: Convert a motion spec into an ordered build plan. Each task produces a visible, testable piece of the animation. Outputs .animation/<slug>/TASKS.md. Activate when someone wants to break down an animation into buildable steps, plan the build order, create a task list for animation work, or says "build order".
---

## When to use

- After the brief, architecture, and tokens are defined
- Before starting any build work in `/build-motion`
- When you need a clear sequence of what to build and in what order

## How it works

Read the existing animation documents:
- `.animation/<slug>/MOTION_SPEC.md` — intent, storyboard, state map, and timeline
- Animation token file — the motion value system

If any are missing, ask the designer to describe the animation or suggest running the earlier phases first.

Scan the project for existing animation components, utilities, or patterns that can be reused. Reference them in tasks instead of recreating them.

### Build order logic

Animation tasks follow a specific dependency chain that differs from general UI tasks:

**Phase A — Ground.** The static baseline must be correct before any motion is added. Tasks in this phase produce visible, reviewable screens with zero animation.

**Phase B — Layers.** Each animated element gets its own task. A layer task takes one element from its starting state to its ending state with the correct timing and easing. Layers are ordered by the timeline in the architecture — background layers before foreground, containers before content.

**Phase C — Choreography.** Once individual layers work, wire them together. Choreography tasks add stagger timing, sync points, and the cascade sequence. This is where the animation starts to feel like a composed piece rather than isolated parts.

**Phase D — Conditions.** Handle the edges: interactive triggers, interruption behavior, error states, empty states. What happens when the animation is triggered mid-sequence? What happens on slow connections or missing assets?

**Phase E — Resilience.** Accessibility, responsiveness, performance, dark mode. Each is its own task because each requires testing in isolation.

**Phase F — Delivery.** Format-specific output: export the GIF, render the video, package the prototype. Only applies after the animation is working.

### Task requirements

Every task must:
- Produce something you can see or test on its own
- Reference which scene, layer, or state from the architecture it implements
- State whether it creates new code, extends existing components, or reuses existing animation utilities
- Be completable in a focused session — if it can't be, break it smaller

### What to leave out

- Directory creation, file scaffolding, or dependency installation (unless the project has zero animation infrastructure)
- Tasks that are purely organizational with no visible output
- Generic "research" or "explore" tasks — those belong in earlier phases

## Output

Save as `TASKS.md` in `.animation/<slug>/`.

```markdown
# Build Plan: [Animation Name]

## A. Ground
- [ ] **Static baseline** — Build the static-frame layout with all elements in starting positions. No animation. Verify it works as a standalone static screen. Uses tokens from [token file].

## B. Layers
- [ ] **[Element name]** — Animate [property] from [start] to [end]. Duration: [value]. Easing: [curve]. Trigger: [event]. Architecture ref: [scene/layer].
- [ ] **[Element name]** — [same pattern]

## C. Choreography
- [ ] **[Sequence name]** — Wire [layers] together with stagger timing [values]. Verify the cascade matches the architecture timeline. Mark sync points.

## D. Conditions
- [ ] **[Trigger/state]** — Handle [interaction or edge case]. Define enter/exit behavior.
- [ ] **Interruption** — Implement behavior when animation is triggered mid-sequence (per architecture).

## E. Resilience
- [ ] **Reduced motion** — Implement prefers-reduced-motion fallback. End state must be usable, not broken.
- [ ] **Responsive** — Adapt for mobile (375px), tablet (768px), desktop (1280px+). Simplify choreography on small screens.
- [ ] **Performance** — Profile at 60fps on mid-range hardware. Only animate composited properties where possible.
- [ ] **Dark mode** — Verify color transitions work in both themes.

## F. Delivery
- [ ] **[Format-specific]** — Export GIF / render video / package prototype per brief specs.
```

Order tasks so the designer can stop at any point and have a working (if incomplete) animation. Phase A alone produces a correct static screen. Phases A+B produce individual animated elements. A+B+C produces the full choreographed sequence.
