---
name: animate
description: Guided walkthrough of the full animation design process — from concept interrogation through build and review. Six phases, each building on the last. Activate when someone says "animate", "run the animation workflow", "guide me through building an animation", or wants the full structured process.
---

## When to use

- Starting an animation project from scratch and want the full structured process
- Returning to an animation project and want to pick up where you left off
- Want a guided experience instead of running individual skills manually

## The six phases

| # | Skill | What it does | What it produces |
|---|-------|-------------|-----------------|
| 1 | `/challenge-concept` | Pressure-tests the animation concept | Shared clarity (no file) |
| 2 | `/motion-spec` | Documents intent, emotional arc, storyboard, states, and timeline | `MOTION_SPEC.md` |
| 3 | `/static-frame` | Creates the motion token system and static baseline | Token file (CSS / Tailwind / theme) |
| 4 | `/build-order` | Breaks the animation into buildable tasks | `TASKS.md` |
| 5 | `/build-motion` | Builds the animation components and sequence | Working code + deliverable |
| 6 | `/playback-review` | Evaluates the result against the spec | `PLAYBACK_REVIEW.md` + screenshots |

All documents save to `.animation/<slug>/` where the slug is derived from the animation name.

### MANDATORY RULES — NEVER BREAK THESE

These rules override everything else. Every phase, every question, every interaction must follow them.

**RULE 1: ONE QUESTION PER MESSAGE.**
Never ask two or more questions in one message. Not two. Not "quick questions." Not "a few things I need." ONE question, wait for the answer, then ask the next one.

WRONG:
> "What triggers the expansion? Is it clicking the thumbnail card, or does 'Thinking Complete' happen first? Also, does the tray slide left or does the canvas grow from the right? And should the chat content animate in or already be visible?"

RIGHT:
> "What triggers the expansion?"
> - A) User clicks the thumbnail card
> - B) 'Thinking Complete' happens first, then auto-expands
> - C) Something else

**RULE 2: ALWAYS USE LETTERED OPTIONS.**
Never ask an open-ended question when you can offer choices. Convert every question into A/B/C/D options. The user picks a letter instead of typing a paragraph.

WRONG:
> "Does the tray slide left or does the canvas grow from the right? In Screen 2, the tray content is on the left and the workflow canvas is on the right — is this a slide transition or does the whole panel resize?"

RIGHT:
> "How should the tray-to-canvas transition work?"
> - A) Tray slides left, canvas appears from the right
> - B) Whole panel resizes — tray shrinks, canvas grows
> - C) Crossfade — tray fades out, canvas fades in
> - D) Something else — describe it

**RULE 3: NO QUESTION DUMPS.**
Never list multiple questions as numbered items, bullet points, or paragraphs in one message. If you have 5 questions, that's 5 separate messages — each one asked after the previous one is answered.

**RULE 4: INCLUDE "OTHER" OR "NOT SURE."**
Every choice list must have an escape hatch. The user should never feel forced into an option that doesn't fit.

**Why this order:** The motion spec includes both intent AND structure (storyboard, states, timeline) in one document — because for animation, the storyboard IS the spec. Static frame comes next so the motion vocabulary (duration names, easing names) is established before tasks reference them. Build order and build phases speak in token language, not abstract descriptions.

---

## Starting a new animation

When the user invokes `/animate`:

**One question at a time. Never ask two questions in one message.** This rule applies to the orchestrator and every phase within it.

Start by asking: **What are you animating?**

Wait for the answer. Then ask: **Do you want to skip any phases, or run all six?**

Then move through each active phase one at a time.

---

## Moving through phases

For each phase:

1. **Name the phase and what it will produce.** One or two sentences. Then ask: "Ready to start?"
2. **Run the phase.** Follow the corresponding skill's instructions completely. One question at a time — never stack multiple questions.
3. **When done**, briefly state what was produced and move to the next phase.

**Never ask setup questions between phases.** Don't ask about tech stack, triggers, or context outside of the skill that needs that information. Each skill asks its own questions when it runs.

---

## Returning to an existing animation

If `.animation/` already contains subfolders:

1. List what exists: which slugs, which documents are present, which are missing.
2. If one slug, offer to resume from the next missing phase.
3. If multiple slugs, ask which animation to work on.
4. The user can re-run a completed phase (e.g., update the spec after building).

---

## Phase 3 gate — mandatory review

Phase 3 (`/static-frame`) must NOT be marked complete until the designer has seen the built static screen and confirmed it matches their reference. Generating tokens alone is not enough. The AI must build the screen, show it, and ask "How close is this to your reference?" — and only move to Phase 4 when the designer says it looks right.

---

## Phase 3 and Figma

Phase 3 (`/static-frame`) can optionally pull design context from Figma. When entering this phase, ask:

*"Do you have a Figma file for this animation? If so, paste the link — I'll pull the design context to use as the foundation for tokens and the static baseline."*

If they provide a link, the static-frame skill handles the Figma MCP integration. If not, it generates tokens from the spec and codebase alone.

---

## Phase 6 exception

Phase 6 (`/playback-review`) does not run automatically after Phase 5. Animations typically need iteration between building and reviewing. After Phase 5 completes, say:

*"The animation is built. Want to run a playback review now, or iterate first?"*

Only run Phase 6 if they explicitly say yes.

---

## Stopping and resuming

The user can stop at any point. Every phase that produces a file has already saved it. When they return:

- The flow detects existing documents and offers to resume
- Partial progress is useful — a spec without implementation is still a spec
- The user can re-enter at any phase, not just the next sequential one
