---
name: playback-review
description: Evaluate a built animation against its motion spec and tokens. Requires visual evidence — code review alone cannot evaluate motion. Produces .animation/<slug>/PLAYBACK_REVIEW.md with screenshots. Activate when someone wants to review an animation, critique motion quality, check if an animation is ready, or says "playback review".
---

## When to use

- After building animation components (Phase 6)
- When you want structured feedback before delivery
- This phase never runs automatically — only on explicit request

## How it works

The review evaluates the built animation against three sources of truth:
1. The motion spec (intent, storyboard, state map, timeline, and constraints)
2. The animation tokens (motion values and static baseline)
3. The running animation itself (what actually happens on screen)

### Visual evidence is mandatory

Code tells you what the animation *should* do. Only visual evidence tells you what it *actually* does. You must capture screenshots or recordings of the running animation.

**Capture at three breakpoints:**
- Mobile: 375 x 812
- Tablet: 768 x 1024
- Desktop: 1280 x 800

**Capture at key moments:**
- Static frame (baseline, before any motion)
- A mid-sequence frame (during the most important transition)
- End state (where the animation settles)

**Also capture:**
- Interactive states if applicable (hover trigger, click trigger, scroll-triggered moment)
- Dark mode variant at desktop
- Reduced motion variant (what the user sees with `prefers-reduced-motion: reduce`)

Save screenshots to `.animation/<slug>/screenshots/` with descriptive names (e.g., `desktop-static-frame.png`, `mobile-midpoint.png`, `dark-mode-end-state.png`).

Use whatever screenshot method is available: browser DevTools, Playwright, preview tools, or ask the designer to provide recordings.

---

## Evaluation criteria

### Does the timing feel right?
- Are durations appropriate for the content? Quick enough to not bore, slow enough to register.
- Does the pacing match the motion personality chosen in the brief?
- Do stagger intervals create a clear reading order?
- Are there breathing moments between movements, or does the sequence feel rushed?
- Does total runtime match the brief's duration spec?

### Does the choreography tell a story?
- Does the sequence match the architecture timeline?
- Is the entry order logical? (backgrounds before foregrounds, containers before content)
- Are transitions between scenes smooth or jarring?
- At every moment, does the viewer know where to look?
- If the animation is interrupted, does it recover gracefully?

### Does the motion have character?
- Are easing curves consistent with the chosen personality?
- Do elements land naturally? (No abrupt stops unless the personality demands it)
- Is there anticipation before important moves? Follow-through after?
- Do spring/bounce animations feel physical, not random?
- Does the animation avoid generic "AI float" — unmotivated, directionless drift?

### Does it achieve the emotional intent?
- Does the viewer feel what the brief says they should feel?
- Does the emotional arc match the planned arc?
- Does the motion reinforce the intended user behavior?
- Is the animation serving the user or performing for the designer?

### Is the static baseline solid?
- Does the static frame work as a standalone design?
- Are all elements correctly positioned before animation begins?
- If animation fails to load, is the fallback screen usable?

### Performance
- 60fps on mid-range devices?
- Only composited properties animated (`transform`, `opacity`)?
- No layout-triggering animations (`width`, `height`, `top`, `left`)?
- `will-change` used only when needed, cleaned up after?
- No jank visible in DevTools performance profile?

### Accessibility
- `prefers-reduced-motion: reduce` respected?
- Reduced motion fallback shows usable end state, not broken intermediate?
- No information conveyed solely through motion?
- No flashing exceeding 3 per second?

### Responsive adaptation
- Works at mobile width without horizontal overflow?
- Choreography simplified (not just scaled down) on small screens?
- Touch-only interactions provided where hover animations exist?

### Dark mode
- Color transitions smooth in dark mode? No white flashes or jarring jumps?
- Shadows and glows adjusted for dark backgrounds?
- Animated accent colors still have sufficient contrast?

### Token consistency
- All timing values use token variables, not hardcoded numbers?
- All easing curves use token variables?
- Visual state values (opacity, scale, offset) use tokens?
- No magic numbers in the animation code?

### Format-specific

**Prototype**: All interactions triggerable? Animation replayable? States reachable through intended user path?

**GIF**: Dimensions match brief? Frame rate smooth? File size within constraint? Loop seam clean (or one-shot end frame correct)? Colors not over-dithered?

**Short movie**: Resolution and codec match brief? Audio synced? No rendering artifacts?

---

## Output

Save as `PLAYBACK_REVIEW.md` in `.animation/<slug>/`.

Structure:

```markdown
# Review: [Animation Name]

**Reviewed**: [date]
**Spec**: [link to MOTION_SPEC.md]

## Verdict
[2-3 sentences. Does this animation meet the brief? What's the overall quality?]

## Visual evidence
[List screenshots captured, with descriptions of what each shows]

## Must fix
Things that prevent the animation from meeting the brief.
- [ ] [What's wrong] — [Where in the code] — [How to fix it]

## Should fix
Things that degrade quality but don't block delivery.
- [ ] [What's wrong] — [Where] — [Suggested fix]

## Worth improving
Polish that would elevate the animation beyond the brief's requirements.
- [ ] [Observation] — [Suggestion]

## Working well
What's done right. Specific and actionable — not flattery.
- [What works and why it should stay]
```
