---
name: build-motion
description: Build the animation — components, keyframes, timelines, and export pipeline. Adapts output to the deliverable format (prototype, GIF, short movie). Activate when someone wants to implement an animation, code the motion, build the sequence, or says "build motion".
---

## When to use

- After tokens and tasks are defined (or when you have enough context to build)
- When it's time to write actual animation code
- When exporting the final deliverable in the specified format

## Before writing any animation code

**Read everything that exists:**
- `.animation/<slug>/MOTION_SPEC.md` — intent, storyboard, state map, timeline, and constraints
- `.animation/<slug>/TASKS.md` — build order
- Animation token file — the motion value system

**Understand the project's animation landscape:**
- What animation library is installed? (Check dependencies.) Don't add a library if one exists.
- How do existing components handle motion? Match their approach.
- What CSS animation patterns exist already? (`@keyframes`, `transition` declarations, animation utility classes)
- How does the project handle responsive layouts? Use the same breakpoint system.
- How does the project handle dark mode? Use the same mechanism.
- Are fonts loaded before the page renders? Text animations need fonts ready first.
- Where do animation assets live? (SVG directory, image assets, Lottie files)

**Build the static baseline first.** Before any animation code:
- All elements in their static-frame positions
- Layout correct, typography set, colors applied, spacing right
- Verified as a working static screen
- THEN add motion on top

**Use component states from Figma — never invent colors or styles.** When animating interactive elements (buttons, inputs, tabs, toggles, cards), pull the exact visual values for every state from the Figma file or the token system:

- **Default state** — the resting appearance
- **Hover state** — exact background, border, text color from Figma
- **Active / pressed state** — exact color change from Figma
- **Selected / checked state** — exact highlight color from Figma
- **Disabled state** — exact opacity and color from Figma
- **Focus state** — exact focus ring style from Figma

Never use a random or generic color for a state change. If a button's selected state is `#1a73e8` in Figma, use `#1a73e8` — not a shade you think looks similar. If you can't find the state in Figma, ask the designer:

**What color should the [element] be when [state]?**
- A) Let me share a Figma screenshot
- B) Use the same color as [another element]
- C) Here's the hex value: [let them type it]

---

## Motion vibe — how should it feel?

Before picking a technical motion personality, help the designer describe the vibe in their own words. Ask:

**What does this animation feel like?** Pick one that's closest:
- A) A bouncing ball with gravity — asymmetric, fast down, slow up, settles naturally
- B) A sliding drawer — smooth, consistent, predictable
- C) A camera pan — slow, cinematic, intentional
- D) A card being dealt — quick flick, lands with a tap
- E) Breathing — slow in, slow out, rhythmic and calm
- F) A pop-up book — elements spring up with surprise and energy
- G) A typewriter — precise, one thing at a time, mechanical rhythm
- H) Describe it in your own words

Use their answer to recommend the matching motion personality below.

## Motion personalities

Commit to a personality before implementing. This governs every timing and easing decision. The personality should match the vibe above.

### Precise & Mechanical
Short durations (200-400ms). Material-style easing (`cubic-bezier(0.4, 0, 0.2, 1)`). Uniform stagger intervals. No overshoot, no bounce. Movements are clean and predictable — like well-engineered machinery. Every element arrives exactly where expected, exactly when expected.

### Fluid & Organic
Medium durations (300-600ms). Natural deceleration curves (`cubic-bezier(0.25, 0.1, 0.25, 1)`). Variable stagger based on visual weight — heavier elements lead, lighter elements follow. Slight overlap between element entrances. Nothing waits in isolation. Movements feel like water finding its level.

### Bouncy & Playful
Longer durations to accommodate overshoot (400-700ms). Spring physics where the library supports it, bounce curves elsewhere (`cubic-bezier(0.34, 1.56, 0.64, 1)`). Exaggerated anticipation before big moves. Follow-through after landing. Elements have weight and elasticity. The animation has a sense of humor.

### Cinematic & Dramatic
Long durations (600-1200ms). Slow-start-slow-end curves (`cubic-bezier(0.7, 0, 0.3, 1)`). Extended holds between movements — let moments breathe. Depth-of-field effects (blur transitions between layers). Parallax-style layering. Movements feel like a camera is involved.

### Snappy & Decisive
Very short durations (100-250ms). Fast-out curves (`cubic-bezier(0.16, 1, 0.3, 1)`). Minimal or no stagger — elements appear almost simultaneously. No lingering. State changes are instant and confident. Movements feel like a tap response — immediate and precise.

---

## Implementation constraints

### Only animate composited properties
Prefer `transform` (translate, scale, rotate) and `opacity`. These run on the GPU compositor and don't trigger layout or paint.

Avoid animating: `width`, `height`, `top`, `left`, `right`, `bottom`, `margin`, `padding`, `border-width`. These trigger layout recalculations on every frame.

For shadow animation, animate the `opacity` of a pseudo-element instead of animating `box-shadow` directly.

Use `will-change` only when animation is about to start, and remove it when animation completes. Don't leave it on permanently.

### Mobile-first motion
Build for 375px width first. Choreography on mobile should be simpler — fewer staggered layers, shorter total duration, less simultaneous motion. Scale up complexity at wider breakpoints with `min-width` queries.

Touch screens have no hover. If the animation involves hover triggers, provide a tap alternative or a different interaction model on mobile.

### Dark mode
Use CSS custom properties for every color that participates in the animation. Support both `prefers-color-scheme: dark` and manual `[data-theme="dark"]` toggling.

Dark mode shadows are denser (higher opacity). Glows are softer (lower intensity, wider spread). Test the animation in both modes — color transitions that look smooth on light backgrounds can flash on dark backgrounds.

### Reduced motion is mandatory
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

In JS animation libraries, check `window.matchMedia('(prefers-reduced-motion: reduce)')` and skip to the end state.

The reduced motion fallback must show a usable end state — not a broken intermediate frame. If the animation reveals content, the content must be visible. If it shows a loading state, the loaded state must appear immediately.

### Performance target
60fps on mid-range devices. Profile using browser DevTools Performance panel — not just visual inspection. If frames drop, the cause is almost always animating a non-composited property or running too many simultaneous animations.

### Positioning bugs — coordinate system awareness

When placing cursors, ripples, tooltips, or any element that needs precise positioning:

**Scaled containers break coordinates.** If `body` or a parent has `transform: scale()`, `getBoundingClientRect()` returns screen pixels but your element is positioned in the scaled coordinate space. Fix:
- Use `getBoundingClientRect()` on both the element AND `document.body`
- Subtract the body offset
- Derive scale from `bodyRect.width / originalWidth` — don't parse the transform string
- Divide coordinates by the calculated scale

**Target the visible element, not the semantic element.** A `<button>` may be wider than its visible content (full-width flex child vs. a small label). When positioning a ripple or cursor on it, target the inner span or visible content wrapper — not the full-width container.

**When debugging a positioning bug:**
1. Identify the coordinate system — is there a scale transform on any ancestor?
2. Identify the target — is the positioned element landing on the wrong thing because the clickable area is larger than the visible content?
3. Fix both before retrying — don't cycle through approaches one at a time

---

## Build step by step — check in with the designer

Don't build the entire animation silently and show the result at the end. Build in stages, preview each one, and get feedback before moving on.

### Before starting, show the overview

Summarize the build plan so the designer knows what's coming:

*"Here's what I'm going to build, step by step:*
1. *[First element] — [how it animates]*
2. *[Second element] — [how it animates]*
3. *[Third element] — [how it animates]*
4. *Wire them together into the full sequence*
5. *Polish and export*

*I'll preview each step and check with you before moving to the next. You can adjust anything along the way."*

Then ask:

**Does this plan look right, or would you change anything?**
- A) Looks good — let's start
- B) I'd change the order
- C) I'd add or remove something
- D) Walk me through it in more detail

### Then confirm the motion personality

**Which motion personality fits?** Pick one:
- A) Precise & Mechanical — clean, predictable
- B) Fluid & Organic — natural, flowing
- C) Bouncy & Playful — spring, overshoot
- D) Cinematic & Dramatic — slow, film-like
- E) Snappy & Decisive — instant, confident
- F) Not sure — recommend one for me

### Before building each element — extract its states

**This is mandatory for every interactive element.** Before animating a button, input, tab, dropdown, toggle, or card, you MUST:

1. **Pull all its states from Figma** using `get_design_context` or `get_context_for_code_connect`
2. **List what you found** — show the designer: "I found these states for [element]: default (#hex), hover (#hex), active (#hex), selected (#hex)"
3. **If you can't find a state**, ask:

**I need the [state] appearance for [element]. How should it look?**
- A) Let me share a Figma screenshot of that state
- B) Same as default but with [describe change]
- C) Here's the hex value
- D) Skip this state — it doesn't change

**Use Figma values first. If not available, use best-practice defaults — not random values.**

Common UI patterns have well-known behaviors. If Figma doesn't define a state, apply these defaults instead of asking:

- **Button hover** — slightly darken the background (10-15% darker) or lighten if dark theme
- **Button pressed/active** — darken further (20-25%), scale down to `0.98` for a press feel
- **Button disabled** — `opacity: 0.4`, `cursor: not-allowed`
- **Input focus** — 2px border in the primary/accent color, subtle box-shadow glow
- **Input hover** — border color slightly darker than default
- **Input disabled** — `opacity: 0.5`, muted background
- **Input error** — red border, red helper text
- **Tab selected** — bottom border or background change in primary color, bold text
- **Tab hover** — subtle background tint
- **Toggle on** — slide thumb, change track color to primary
- **Dropdown open** — subtle shadow elevation, smooth height reveal
- **Card hover** — lift with shadow (`translateY(-2px)`, increased box-shadow)
- **Link hover** — underline or color shift

These are standard flat/material patterns. Use them as fallbacks — but always prefer Figma values when they exist. Only ask the designer when a component doesn't fit any common pattern.

### Build in stages

Follow the build order (Phase 4 tasks). After completing each stage, show the designer what you've built:

**Stage 1: Static elements with first animation**
Build the first animated element. Preview it. Ask:

**How does this first animation feel?**
- A) Feels right — keep going
- B) Too fast
- C) Too slow
- D) Wrong easing — feels off
- E) Wrong direction — it should come from a different side
- F) Let me describe what's wrong

**Stage 2: Add remaining elements**
Add the next elements one at a time or in small groups. After each addition, ask:

**Does this work with the previous elements?**
- A) Yes — looks good together
- B) The timing between elements is off
- C) The sequence order should change
- D) One of the elements doesn't look right

**Stage 3: Full sequence**
Once all elements are animating, play the full sequence. Ask:

**How does the full animation feel?**
- A) Love it — this is what I imagined
- B) Overall good, but some timing needs tweaking
- C) The choreography order needs changing
- D) Some elements should animate differently
- E) The speed overall needs to change (faster / slower)

If they pick A, ask these refinement questions one at a time before moving to polish:

**Should there be pauses between actions?**
- A) Continuous — one action flows right into the next
- B) Short pauses — brief holds between actions to let each one land
- C) Long holds — give the viewer time to absorb each step
- D) Mixed — some parts flow, some parts pause
- E) It's good as is

**Should clicks and interactions have visual feedback?**
- A) Yes — highlight the click area, show a cursor, make it obvious
- B) Subtle — small visual cue but not distracting
- C) No feedback — just show the result of the action
- D) Not applicable — no clicks in this animation
- E) It's good as is

**Do you want a ripple effect on clicks?** (expanding circle that fades out, ~500ms)
- A) Yes — on every clickable element (buttons, dropdowns, tabs, links)
- B) Yes — but only on main action buttons
- C) Yes — but only on specific elements (tell me which)
- D) No ripple — keep it clean

If they pick A, B, or C — implement the ripple as a pseudo-element or child element **inside** the clickable element, positioned at `50% / 50%` so it always centers on the element regardless of scaling or layout. Never position the ripple based on page coordinates — it must be relative to the element itself. The element needs `overflow: hidden` and `position: relative` to contain the ripple.

**How do the transitions between states feel?**
- A) Too abrupt — needs smoother blending
- B) Too slow — speed up the transitions
- C) Want a different style (crossfade / slide / morph / cut)
- D) They feel right — no changes

**Stage 4: Tighten the timing**
After the main animation is approved, fine-tune the pace. Ask these one at a time:

**How does the overall speed feel?**
- A) Perfect — don't change anything
- B) Everything should be faster — tighten it up
- C) Everything should be slower — let it breathe
- D) Some parts are too fast, others too slow — let me point them out

**How long should each action hold before the next one starts?**
- A) No hold — flow straight into the next action
- B) Quick beat — just enough to register (~200ms)
- C) Comfortable pause — let the viewer absorb it (~500ms)
- D) Long hold — dramatic pause (~1s+)
- E) Different holds for different actions — let me specify

**How fast should elements stagger in?**
- A) Tight — almost simultaneous (~50ms between)
- B) Quick cascade — clearly sequential (~100ms)
- C) Relaxed — each element gets its moment (~200ms)
- D) Dramatic — slow reveal one by one (~300ms+)
- E) It's good as is

**How snappy should click responses be?**
- A) Instant — state changes immediately on click
- B) Quick feedback — short transition (~100ms)
- C) Smooth — visible but not slow (~200-300ms)
- D) Deliberate — slow enough to notice (~400ms+)
- E) Not applicable

Apply the timing changes, preview again, then ask:

**Stage 5: Final polish**
Handle remaining details. Ask:

**Anything else to polish?** Pick all that apply:
- A) Loop transition needs work
- B) Needs smoother easing on specific elements
- C) An element needs more/less movement
- D) Colors during transition don't look right
- E) Ready to export — no more changes

Adjust until they pick E.

---

## Format-specific builds

### Prototype
Build interactive components with real animation code. Include all trigger mechanisms (buttons, scroll observers, viewport detection). Provide a way to replay the animation. Document what interactions are available and what they trigger.

### GIF

**The GIF must perfectly match the HTML animation.** The GIF is a recording of the HTML — not a separate animation. Every frame of the GIF must show exactly what the browser shows at that moment. If the HTML animation looks right but the GIF looks different, the capture process is wrong — fix the capture, don't change the animation.

Build the animation in code first — get it working in the browser. Then capture:
- Use Puppeteer, Playwright, or manual screen recording
- **Set `deviceScaleFactor: 3`** in Puppeteer/Playwright — never use the default (2). 3x produces noticeably sharper text and edges.
- Export at the dimensions and frame rate specified in the brief
- Check loop behavior: seamless loop needs a clean seam frame, one-shot needs a clear ending

**GIF quality defaults:**
- **3x retina rendering** — capture at 3x the target dimensions, then downscale. Produces sharper text and edges than 1x or 2x.
- **Floyd-Steinberg dithering** — smoother gradients, less banding in color transitions. Use instead of ordered or no dithering.
- **Unsharp filter after downscale** — apply a light sharpen pass after resizing to restore crispness lost during downscaling.
- **256 color palette optimized per frame** — let each frame use its best 256 colors, not a single global palette.
- Verify file size meets the brief's constraint. If too large, reduce frame rate or dimensions before reducing quality.

### Short Movie / MP4

Build the animation in code first.

**Principle: fake the clock — capture frame by frame, never in real-time.** Real-time recording produces timing drift. Instead, advance the animation by exactly `1000ms / targetFPS` per frame, capture a screenshot at each position, then stitch into video. This guarantees the export matches the browser exactly. Real-time screen capture produces inconsistent frame rates — animations run faster in captured frames than in real-time because the browser renders frames as fast as it can while the recorder samples at its own rate. Instead:

- **Pause all CSS animations and JS timers** at the start
- **Step through the animation frame by frame** — advance the animation timeline by exactly `1000ms / targetFPS` per frame (e.g., 33.33ms per frame for 30fps)
- **Set `deviceScaleFactor: 3`** — always 3x, never 2x
- **Capture each frame as a screenshot** at the exact animation state
- **Stitch the frames into a video** at the target frame rate
- This guarantees every frame is captured at the correct animation state, with no dropped frames, no speed inconsistencies, and no timing drift

For Puppeteer/Playwright, use `page.evaluate()` to advance `document.getAnimations()` or manually set CSS `animation-delay` / JS timeline position per frame, then `page.screenshot()` each one.

**MP4 quality defaults:**
- **3x retina rendering** — capture frames at 3x target resolution, downscale to final size. Sharper text and UI edges.
- **CRF 10** — near-lossless quality. Don't use CRF 14+ for final output — the difference is visible on detailed UI.
- **Unsharp filter after downscale** — crisper details after resizing from 3x to 1x.
- **H.264 High profile** — best compatibility across players and platforms.
- **30fps for UI demos, 24fps for cinematic feel** — match what was decided in the motion spec.

If audio is specified in the brief, sync animation to the audio timeline — not the other way around. Export in the format specified (MP4, WebM, MOV). Consider title and end cards if the context warrants them.

---

## What not to do

- Don't produce generic, floaty motion with no intent. Every movement must have a reason that traces back to the brief.
- Don't animate everything at once. Stagger and sequence create hierarchy. If everything moves simultaneously, nothing has emphasis.
- Don't deviate from the architecture timeline without discussing it. The timing was designed deliberately.
- Don't skip the static baseline. Motion on a broken layout makes the broken layout harder to debug.
- Don't forget reduced motion. It's not polish — it's a requirement.
