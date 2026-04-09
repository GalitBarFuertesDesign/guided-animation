---
name: motion-spec
description: Produce a complete motion specification — intent, format, emotional arc, storyboard, state map, and layered timeline in a single document. This is the animation's source of truth before any tokens or code are written. Activate when someone wants to define an animation, plan motion for a feature, create a storyboard, map animation states, or says "motion spec".
---

## When to use

- After running `/challenge-concept` (or when you already have a clear animation concept)
- Before tokens, tasks, or any build work
- When you need one document that captures both the WHY and the HOW of an animation

## How it works

The brief is built through conversation and codebase discovery. It produces a single document with two halves: **intent** (what the animation is for) and **structure** (how the animation is organized in time).

---

### Half 1: Intent

Gather context through two channels simultaneously:

**Talk to the designer.** Understand what they want. For each question, offer a recommended answer:

- What is being animated and what triggered this need?
- Who will see this and in what context?
- What is the deliverable: a **prototype** (interactive, code-based), a **GIF** (exported loop or sequence), or a **short movie** (rendered video, possibly with audio)?
- What existing animations inspire this? Collect links, descriptions, screenshots.
- What emotions should the viewer experience, and when? Map the arc — not every animation is a flat mood.
- What are the hard boundaries? File size, duration, platform, brand rules, deadlines.
- How will you know this succeeded?

### MANDATORY: Ask motion preferences before writing the storyboard

**DO NOT skip these questions. DO NOT write the storyboard until every one is answered.** Ask them ONE AT A TIME with lettered options. Each question is a separate message.

**Q1:** "How fast should this animation feel overall?"
- A) Instant — blink and it's done
- B) Quick — fast but noticeable
- C) Moderate — comfortable pace
- D) Slow — let it breathe
- E) Cinematic — dramatic and deliberate

**Q2:** "How should elements transition in?"
- A) Fade — opacity only
- B) Slide — move in from a direction
- C) Scale — grow or shrink into place
- D) Blur to sharp — start blurry, come into focus
- E) Combination — mix of the above
- F) Not sure — recommend based on the concept

**Q3:** "How should elements transition out?"
- A) Same as the entrance, reversed
- B) Fade out
- C) Slide out
- D) Shrink away
- E) They don't leave — the animation ends with everything visible

**Q4:** "Should elements enter one at a time or together?"
- A) All together — everything appears at once
- B) One at a time — staggered sequence
- C) Grouped — related elements enter together, groups are staggered
- D) Not sure — recommend based on the content

**Q5:** "How should the motion feel?"
- A) Smooth and soft — gentle ease in/out
- B) Snappy — fast start, quick stop
- C) Bouncy — overshoot and settle
- D) Linear — constant speed, mechanical
- E) Springy — elastic, physics-based
- F) Not sure — recommend based on the mood

**Q6:** "Any transitions between scenes?"
- A) Cut — instant switch
- B) Crossfade — one fades out as the next fades in
- C) Slide — scenes slide horizontally or vertically
- D) Morph — elements transform into the next scene
- E) No scenes — it's one continuous sequence

**Q7:** "Does this animation loop?"
- A) No — plays once and stops on the last frame
- B) Yes, seamless — last frame flows directly into the first frame with no gap or blackout
  - Yes, with a pause — holds the last frame for a moment, then starts over
  - Yes, with a transition — last frame fades/slides into the first frame
  - Not sure — recommend based on the format

**Q8** (only ask if they picked a loop option):
"How should the loop seam work?"
- A) Last frame stays visible, first frame builds on top — no blank screen
- B) Reverses back to the start (boomerang)
- C) Quick crossfade between last and first frame
- D) Instant reset — clean cut back to the start

**Q9:** "What's the emotional arc of this animation?"
- A) Builds up — starts calm, ends with impact
- B) Starts strong — grabs attention immediately, then settles
- C) Flat — consistent energy throughout
- D) Surprise — calm buildup, then an unexpected moment
- E) Story — setup → tension → resolution
- F) Not sure — recommend based on the content

**Q10:** "What's the hero moment — the single most important thing the viewer should notice?"
(open — let them describe the key moment, then confirm: "So the hero moment is [X] — is that right?")

**Q11:** "What triggers each part of the animation?"
- A) Time-based — everything runs on a timeline automatically
- B) User actions — clicks, scrolls, or hovers trigger each step
- C) Mixed — some parts auto-play, some wait for interaction
- D) Sequential states — one action completes, then the next begins

**Q12:** "When multiple things are animating, how should they relate?"
- A) One at a time — each element finishes before the next starts
- B) Overlapping — elements start before the previous one finishes
- C) Simultaneous — multiple things move at the same time
- D) Mixed — depends on the moment
- E) Not sure — recommend based on the concept

**Q13:** "Where should the viewer's eye be at each moment?"
- A) Always in the same spot — the action comes to the eye
- B) Moving across the screen — the eye follows the action left to right / top to bottom
- C) Jumping between focus points — attention shifts between specific areas
- D) Not sure — recommend based on the layout

**Scan the project.** Before asking questions the codebase can answer:

- Animation libraries already installed (Framer Motion, GSAP, anime.js, Lottie, Motion One, CSS utilities)
- Existing motion patterns — how other parts of the project animate
- Design tokens that include timing or easing values
- Assets that might participate (SVGs, illustrations, icons, image directories)
- Component structure around the elements to be animated

If the project already has animation conventions, the brief must build on them.

---

### Transition options — pick before framing

Before writing the full storyboard, show the designer 2-3 transition options for each key element. Let them see the choices and pick what feels right.

For each major element in the animation, ask one question at a time with lettered options. Example:

**What should [element name] do?** Pick one:
- A) Fade up from below — slides up while fading in
- B) Scale in — starts small, grows to full size
- C) Blur to sharp — starts blurry, sharpens into place
- D) Slide in from the side
- E) Just appear — no transition
- F) Something else — describe it

Tailor the options to match the motion preferences the designer already chose. If they picked "bouncy," include spring/overshoot options. If they picked "snappy," keep options fast.

When there are multiple elements that could animate (e.g., dropdown, form values, new rows, chat messages), ask about them as a **checklist**:

**What should animate between screens? Pick all that apply:**
- A) Dropdown opening/closing smoothly
- B) Form values appearing as if typed
- C) New rows sliding in
- D) Chat messages appearing with a typing indicator
- E) Clean crossfades between states
- F) Something else — describe it

**Rules:**
- **Always display options as a lettered list.** Never list them as examples in a paragraph.
- One element at a time unless asking a checklist question about related items.
- 2-4 options per question — not more.
- Describe in plain language — what the viewer would see, not CSS properties.
- Recommend one: "I'd go with A because..."
- Once all transitions are picked, use those choices to build the storyboard below.

---

### Half 2: Structure

**STOP — before writing the storyboard, confirm you have asked all motion preference questions (Q1-Q7) AND transition options for each key element. If you skipped any, go back and ask them now. The storyboard depends on these answers.**

Once intent and transitions are clear, frame everything together into the storyboard. The transition choices above directly feed into the keyframes below.

**A. Storyboard — what the viewer sees at each moment**

Identify every keyframe: the moments where the visual state changes meaningfully. Everything between keyframes is interpolation.

For each keyframe, describe:
- What is on screen (elements, positions, visual state)
- What just changed from the previous keyframe
- How long this state holds before the next change

Group keyframes into scenes if the animation has distinct phases. A micro-interaction may have one scene. A product video may have many.

The storyboard should match the emotional arc — the peak emotion corresponds to the most visually significant keyframe.

**B. State map — what triggers each change**

Define every state the animation can be in:
- What does this state look like?
- What event moves the animation INTO this state? (time, user action, scroll, data event, viewport entry)
- What event moves it OUT?
- What happens if a new trigger fires mid-transition? (reverse, queue, snap, ignore)

Draw the connections:

```
[resting] --(viewport entry)--> [animating-in]
[animating-in] --(complete)--> [active]
[active] --(scroll away)--> [animating-out]
[animating-out] --(complete)--> [resting]
```

Map branching paths for interactive animations (success vs. error, hover enter vs. leave).

**C. Timeline — how layers overlap**

Visualize when each animated element starts, how long it runs, and how it overlaps with others:

```
0ms     200ms    400ms    600ms    800ms    1000ms
|--------|--------|--------|--------|--------|
[====bg-fade=========]
         [====heading========]
                  [====body-text====]
                           [====cta-button====]
```

For each layer, specify:

| Element | What animates | Start | Duration | Easing | Notes |
|---------|--------------|-------|----------|--------|-------|
| Background | opacity 0 → 1 | 0ms | 400ms | ease-out | First visible |
| Heading | translateY 24px → 0 | 200ms | 300ms | snap | After bg starts |

Mark sync points — moments where timing between layers must be precise.

**D. Responsive and accessible behavior**

- **Mobile (375px)**: How does the choreography simplify?
- **Desktop (1280px+)**: Full choreography.
- **Reduced motion**: What do users with `prefers-reduced-motion: reduce` see instead?

---

### Before defining structure

Look at the project to understand what animation structure already exists:
- Existing animation components, transition wrappers, keyframe definitions
- State management approach (useState, reducers, state machines)
- Routing or navigation transitions
- Asset organization
- If structure exists, extend it. Don't propose architecture that conflicts.

Talk to the designer about structural decisions. Recommend an answer for each:
- How many distinct scenes or states does this animation have?
- What triggers each transition?
- Are there parallel tracks (multiple elements animating simultaneously)?
- Which single frame is the most critical?
- Can the animation be interrupted, and what should happen?

---

## Output

Save as `.animation/<feature-slug>/MOTION_SPEC.md`. The slug is derived from the animation name (e.g., `hero-reveal`, `onboarding-sequence`).

```markdown
# [Animation Name]

## What and why
What is being animated, what triggered the need, and what the animation achieves that a static version cannot.

## Format
**Deliverable**: Prototype / GIF / Short Movie
- Dimensions, duration, frame rate, loop behavior
- Platform and playback context
- Performance or file size constraints

## Emotional arc
The timeline of what the viewer should feel, mapped to on-screen moments.

| Moment | What happens on screen | What the viewer feels |
|--------|----------------------|----------------------|
| Opening | ... | ... |
| Build | ... | ... |
| Peak | ... | ... |
| Resolution | ... | ... |

## Motion personality
How the movement should feel — not which easing curve, but the character of the motion.

## References
Animations that capture elements of what this should be. What specifically to take from each.

## What exists already
Animation infrastructure in the project that this should extend.

## Assets needed
What exists, what must be created, what must be sourced.

## Storyboard
Keyframe breakdown by scene. Each keyframe describes the visual state, what changed, and how long it holds.

### Scene 1: [Name]
| Keyframe | Time | What's on screen | What changed |
|----------|------|-----------------|-------------|
| K1 | 0.0s | ... | Start state |
| K2 | 0.3s | ... | ... |

## State map
All animation states, triggers, transitions, and interruption behavior.

[State diagram and state definition table]

## Timeline
Layered timeline showing all animated elements, their durations, delays, and overlaps.

[ASCII timeline + layer detail table + sync points]

## Responsive behavior
How the animation adapts at mobile, tablet, desktop. How choreography simplifies on smaller screens.

## Reduced motion fallback
What users with prefers-reduced-motion see instead.

## Boundaries
What is in scope, what is out of scope, and what remains undecided.
```

## MANDATORY REVIEW — before completing Phase 2

After writing the motion spec, summarize the key decisions and ask the designer to confirm. DO NOT say "Phase 2 complete" and suggest Phase 3 without this check.

Show a summary of the motion choices:
- Overall speed
- How elements enter/exit
- Stagger behavior
- Motion feel
- Scene transitions
- Loop behavior
- Key moments and their timing

Then ask:

**Does this motion plan match what you have in mind?**
- A) Yes — move on to Phase 3
- B) Some things need changing — let me point them out
- C) Let me see the full spec document first

If they pick B, ask one adjustment at a time until they pick A.
