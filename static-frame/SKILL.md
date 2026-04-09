---
name: static-frame
description: Build the detailed static design and motion token system — the screen that exists before any animation runs. Produces CSS custom properties for timing, easing, visual states, and reusable keyframes. Optionally pulls design context from Figma via MCP. Activate when someone wants to set up the static frame, define the detailed design, create the starting screen, or says "static frame".
---

## When to use

- After the brief and architecture exist (so you know what needs to animate and how)
- Before building any animation code
- When you want a single source of truth for all motion values in the project

## Main rule

**Pull the full context from Figma and match it through a deep analysis.** This is the single most important instruction in this phase. Every color, every pixel, every spacing value must come from the Figma file — not guessed, not approximated, not defaulted. If Figma has the answer, use it. If Figma doesn't have the answer, ask the designer. Never invent a value.

### What makes this phase succeed

These patterns produce the best results. Follow them every time:

1. **Extract exact pixel values from Figma code, not from screenshots.** Use `get_design_context` and `get_variable_defs` to pull precise numbers (e.g., "642px width, 32px padding, 40px input height"). Never estimate proportions from a visual.

2. **Scope to the focus area only.** If the animation targets a tray, modal, or section — build only that part, not the entire page. Ask the designer early: "Should we focus on just the [element] instead of the full screen?" This eliminates wasted effort and concentrates precision where it matters.

3. **Ask the designer for close-up Figma screenshots of tricky components.** When a component has specific details (icon sizes, avatar dimensions, input field annotations), a close-up screenshot with measurements is more reliable than text descriptions. Ask: "Can you share a close-up of [component] from Figma so I can match it exactly?"

4. **When something doesn't match, ask for the specific problem — not a general "what's off?"** Guide the designer to say "8px margin on the right is missing" or "font weight should be 500 not 400" instead of "it doesn't look right." Use the diagnostic checklist to help them pinpoint it.

5. **When matching a specific component, ask the designer to share the Figma screenshot of that component.** Having the visual target alongside the request makes it clear what "done" looks like. Ask: "Can you share a screenshot of how this looks in Figma?"

## How it works

This is the most important phase. If the static HTML doesn't look like the original mock, everything built on top of it will be wrong. Get this right first.

---

### Check MCP connection first

Before asking for a Figma link, verify the Figma MCP is connected. Try calling `get_design_context` or any Figma MCP tool with a test request.

- **If it works** → proceed to get the reference
- **If it fails** → stop and help the designer fix it before continuing. Common issues:
  - Figma MCP server not installed or not running
  - Authentication expired — re-authenticate in the MCP settings
  - Wrong MCP configuration — check the server URL and API key
  - Tell the designer exactly what's wrong and how to fix it. Don't skip Figma integration silently — the static frame depends on accurate design data.

### Get the reference

Ask the designer: **Where is your design?**

- **Figma file** → paste the link, we'll pull it and match it exactly
- **Live page** → point to it, we'll inspect it
- **Nothing yet** → we'll build it from the storyboard

---

### Clarification questions (ask before building)

Before writing any code, ask these questions **one at a time** to avoid guessing:

- **What size is your Figma mock?** Pick one:
  - Mobile — 375px wide
  - Tablet — 768px wide
  - Desktop — 1280px wide
  - Desktop large — 1440px wide
  - Custom — tell me the width and height

- **What is the background?** Pick one:
  - Solid color
  - Gradient
  - Image or pattern
  - Transparent
  - Same as the page it sits on

- **Are there any custom fonts?** Pick one:
  - Yes — tell me the font names
  - No — system fonts are fine
  - Not sure — I'll pull them from Figma

- **Is this a full page or a component/section?** Pick one:
  - Full page — the whole screen
  - A section — part of a larger page
  - A component — a single UI element (card, modal, banner)

- **Any elements that are hidden at first and appear during animation?** Pick one:
  - No — everything is visible from the start
  - Yes — describe which elements start hidden

Ask any follow-up questions based on what you see in the Figma file. If something is ambiguous in the design — overlapping elements, unclear spacing, multiple variants — ask rather than guess.

---

### Detailed analysis (extract every CSS value)

**This step takes time — and that's expected.** Getting the CSS right is the most important part of this phase. Tell the designer upfront:

*"I'm going to go through every component in your design and extract the exact CSS values — colors, spacing, fonts, sizing, borders, shadows. This takes a few minutes but it's worth it. The more accurate the static screen, the better the animation will look on top of it."*

If the design has many components, give progress updates as you go — for example: "Done with the header and navigation, now working on the main content area."

Go component by component:

**For each element in the design, pull:**
- **Position & layout** — display type, flexbox/grid settings, alignment, order
- **Dimensions** — exact width, height, min/max constraints, aspect ratio
- **Spacing** — padding, margin, gap — exact values, not estimates
- **Typography** — font-family, font-weight, font-size, line-height, letter-spacing, text-transform, color
- **Colors** — background, text, border, icon colors — exact hex/rgba values
- **Borders** — width, style, color, radius per corner
- **Shadows** — color, x-offset, y-offset, blur, spread
- **Opacity** — if not fully opaque
- **Overflow** — visible, hidden, scroll
- **Z-index** — layer ordering

**Write CSS values as you go, not after.** Don't analyze first and code later — extract and code simultaneously so nothing gets lost between steps.

### Alignment checklist (run before showing the designer)

After building the static screen, go through this checklist item by item. For each one, compare the built output against the Figma mock and mark it as matching or not. Fix anything that doesn't match before proceeding.

**Layout & Structure**
- [ ] Overall page structure matches (same rows, columns, sections)
- [ ] Element nesting/hierarchy matches the Figma layer order
- [ ] Responsive container width matches the Figma frame width
- [ ] Content is centered/aligned the same way

**Spacing**
- [ ] Padding inside each component matches
- [ ] Margins between components match
- [ ] Gaps in flex/grid layouts match
- [ ] Overall whitespace feels the same — not tighter or looser

**Typography**
- [ ] Font family is correct (not falling back to a system font)
- [ ] Font sizes match for every text element
- [ ] Font weights match (regular, medium, semibold, bold)
- [ ] Line heights match — text isn't more compressed or spread out
- [ ] Letter spacing matches
- [ ] Text color is exact — not a similar shade

**Colors**
- [ ] Background colors match (page, sections, cards, inputs)
- [ ] Text colors match across all elements
- [ ] Border colors match
- [ ] Icon/accent colors match
- [ ] No default browser colors leaking through (link blue, etc.)

**Borders & Corners**
- [ ] Border width matches on all sides
- [ ] Border radius matches on every corner
- [ ] Border style matches (solid, dashed, none)

**Shadows & Effects**
- [ ] Box shadows match (offset, blur, spread, color)
- [ ] No missing shadows on cards/buttons/modals
- [ ] No extra shadows that don't exist in the Figma
- [ ] Blur/backdrop effects match if present

**Sizing**
- [ ] Component widths match (buttons, cards, inputs, panels)
- [ ] Component heights match
- [ ] Image/icon sizes match
- [ ] Aspect ratios are preserved — nothing stretched or squished

**Content**
- [ ] All text content matches (labels, headings, placeholder text)
- [ ] All icons are present and correct
- [ ] All images/placeholders are the right size
- [ ] No elements missing from the Figma
- [ ] No extra elements that don't exist in the Figma

**Interactive States**
- [ ] Input fields look correct (borders, padding, placeholder text)
- [ ] Buttons look correct (size, color, border radius, text)
- [ ] Dropdowns/selects look correct when closed
- [ ] Toggle/checkbox/radio elements match their default state

Pull component variants from Figma using `get_context_for_code_connect` or `get_design_context` to extract all interactive states. For each interactive component, check:

- [ ] **Hover state** — background, border, shadow, text color changes
- [ ] **Active/pressed state** — visual feedback on click/tap
- [ ] **Focus state** — focus ring, border highlight for keyboard navigation
- [ ] **Disabled state** — opacity, color, cursor changes
- [ ] **Error state** — border color, error message, icon if present
- [ ] **Loading state** — spinner, skeleton, or placeholder appearance
- [ ] **Empty state** — what it looks like with no content
- [ ] **Selected/checked state** — for toggles, checkboxes, radio buttons, tabs

Not every component has all states. Only check what exists in the Figma. If a state variant exists in Figma but you can't read it, ask the designer:

**Does this component have additional states I should match?** Pick all that apply:
- A) Hover
- B) Active / pressed
- C) Focus
- D) Disabled
- E) Error
- F) Loading
- G) Empty
- H) Selected / checked
- I) No additional states — default only

### Make interactive elements clickable for review

If the static screen contains elements that have open/closed or visible/hidden states — like dropdowns, menus, accordions, modals, or tooltips — add simple JavaScript so the designer can click to toggle them. This is not animation — it's just so they can visually verify both states match the Figma.

For each togglable element:
- Click to open, click again to close
- No animation on the toggle — instant show/hide is fine
- Style both the open and closed states to match Figma exactly
- If Figma shows the element in its open state, build it open by default so the designer can see it immediately

Ask the designer:

**Are there elements on this screen that open/close? Pick all that apply:**
- A) Dropdown menu
- B) Navigation menu / hamburger
- C) Accordion / expandable section
- D) Modal / dialog
- E) Tooltip / popover
- F) Sidebar / drawer
- G) Tabs — switching between tab content
- H) None — everything is static

For each one they select, add a click toggle so they can verify both states visually.

**If any checklist item fails, fix it before moving to the side-by-side comparison.**

### Side-by-side comparison (during analysis, not after)

Don't wait until the end to compare. Compare as you build, component by component:

1. **After coding each component**, take a screenshot of what you've built so far
2. **Place it next to the Figma mock** at the same viewport size
3. **Check immediately**: Does this component match? Colors, spacing, sizing, position — all of it
4. **Fix differences before moving to the next component.** Don't accumulate errors across multiple components and try to fix them all at the end — that's how things drift

If a Figma screenshot is available (via `get_design_context`), keep it visible as the reference throughout the entire build. Every component you code gets compared against it before you move on.

**What to compare at each step:**
- Does the element sit in the right position relative to its neighbors?
- Are the gaps between elements the same as the mock?
- Does the text look the same — size, weight, color, spacing?
- Do the colors match exactly — not "close enough"?
- Are rounded corners the same radius?
- Is the overall proportion right — nothing stretched or compressed?

This catches errors immediately instead of discovering a screen that looks "off" after 20 components have been coded.

Then build the token system in three layers:

---

### Layer 1: Static baseline

This layer locks in the visual properties of every element in its starting position — the frame the user sees before animation begins or if animation is disabled.

**Visual state properties:**

```css
:root {
  /* Where elements start (opacity) */
  --opacity-hidden: 0;
  --opacity-ghost: 0.1;
  --opacity-subtle: 0.3;
  --opacity-muted: 0.5;
  --opacity-visible: 0.8;
  --opacity-full: 1;

  /* Where elements start (position offsets) */
  --offset-micro: 4px;
  --offset-small: 8px;
  --offset-medium: 16px;
  --offset-large: 32px;
  --offset-hero: 64px;
  --offset-exit: 100%;

  /* Where elements start (scale) */
  --scale-zero: 0;
  --scale-compressed: 0.85;
  --scale-rest: 1;
  --scale-emphasis: 1.05;
  --scale-expanded: 1.1;

  /* Blur levels */
  --blur-none: 0px;
  --blur-hint: 2px;
  --blur-soft: 4px;
  --blur-medium: 8px;
  --blur-heavy: 16px;

  /* Rotation amounts */
  --rotate-nudge: 2deg;
  --rotate-tilt: 5deg;
  --rotate-quarter: 90deg;
  --rotate-half: 180deg;
  --rotate-full: 360deg;

  /* Depth (shadows that change during animation) */
  --shadow-none: 0 0 0 0 transparent;
  --shadow-resting: 0 4px 12px rgba(0, 0, 0, 0.1);
  --shadow-lifted: 0 8px 24px rgba(0, 0, 0, 0.15);
  --shadow-floating: 0 16px 48px rgba(0, 0, 0, 0.2);
}

[data-theme="dark"] {
  --shadow-resting: 0 4px 12px rgba(0, 0, 0, 0.3);
  --shadow-lifted: 0 8px 24px rgba(0, 0, 0, 0.4);
  --shadow-floating: 0 16px 48px rgba(0, 0, 0, 0.5);
}
```

**Color states** (values come from the project's existing design tokens or from the brief):

```css
:root {
  --color-anim-bg-from: /* starting background */;
  --color-anim-bg-to: /* ending background */;
  --color-anim-text-from: /* text reveal color */;
  --color-anim-text-to: /* final text color */;
  --color-anim-accent-glow: /* for pulse/highlight effects */;
}
```

---

### Layer 2: Motion system

This layer defines HOW things move — the timing and character of every transition.

**Duration scale:**

```css
:root {
  --time-instant: 0ms;
  --time-micro: 100ms;     /* hover feedback, toggle states */
  --time-fast: 200ms;      /* button presses, small reveals */
  --time-normal: 300ms;    /* standard entrances and exits */
  --time-moderate: 400ms;  /* content transitions */
  --time-slow: 500ms;      /* panel slides, page transitions */
  --time-deliberate: 700ms; /* dramatic reveals */
  --time-cinematic: 1000ms; /* hero animations */
  --time-epic: 1500ms;     /* full-screen sequences */
}
```

**Stagger intervals** (delay between sequential elements in a cascade):

```css
:root {
  --stagger-tight: 50ms;
  --stagger-normal: 100ms;
  --stagger-relaxed: 150ms;
  --stagger-dramatic: 200ms;
}
```

**Easing curves:**

```css
:root {
  /* Utility */
  --ease-linear: linear;
  --ease-in: cubic-bezier(0.4, 0, 1, 1);
  --ease-out: cubic-bezier(0, 0, 0.2, 1);
  --ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);

  /* Character */
  --ease-snap: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-bounce: cubic-bezier(0.34, 1.56, 0.64, 1);
  --ease-gentle: cubic-bezier(0.25, 0.1, 0.25, 1);
  --ease-dramatic: cubic-bezier(0.7, 0, 0.3, 1);
  --ease-spring: cubic-bezier(0.175, 0.885, 0.32, 1.275);
  --ease-spring-heavy: cubic-bezier(0.68, -0.55, 0.265, 1.55);
}
```

**Reusable keyframes:**

```css
@keyframes fade-in {
  from { opacity: var(--opacity-hidden); }
  to { opacity: var(--opacity-full); }
}

@keyframes slide-up {
  from { transform: translateY(var(--offset-large)); opacity: var(--opacity-hidden); }
  to { transform: translateY(0); opacity: var(--opacity-full); }
}

@keyframes scale-in {
  from { transform: scale(var(--scale-compressed)); opacity: var(--opacity-hidden); }
  to { transform: scale(var(--scale-rest)); opacity: var(--opacity-full); }
}

@keyframes blur-reveal {
  from { filter: blur(var(--blur-medium)); opacity: var(--opacity-hidden); }
  to { filter: blur(var(--blur-none)); opacity: var(--opacity-full); }
}

@keyframes pulse {
  0%, 100% { transform: scale(var(--scale-rest)); }
  50% { transform: scale(var(--scale-emphasis)); }
}
```

**Composed shortcuts:**

```css
:root {
  --anim-fade-in: fade-in var(--time-normal) var(--ease-out) both;
  --anim-slide-up: slide-up var(--time-moderate) var(--ease-snap) both;
  --anim-scale-in: scale-in var(--time-normal) var(--ease-spring) both;
  --anim-blur-reveal: blur-reveal var(--time-slow) var(--ease-gentle) both;
}
```

---

### Layer 3: Accessibility override

This layer collapses all motion to zero for users who prefer reduced motion. It is non-negotiable.

```css
@media (prefers-reduced-motion: reduce) {
  :root {
    --time-instant: 0ms;
    --time-micro: 0ms;
    --time-fast: 0ms;
    --time-normal: 0ms;
    --time-moderate: 0ms;
    --time-slow: 0ms;
    --time-deliberate: 0ms;
    --time-cinematic: 0ms;
    --time-epic: 0ms;
    --stagger-tight: 0ms;
    --stagger-normal: 0ms;
    --stagger-relaxed: 0ms;
    --stagger-dramatic: 0ms;
  }
}
```

---

## Before generating tokens

**Check the project first.** Look for existing motion values:
- CSS custom properties with timing or easing names
- Tailwind config entries for `animation`, `transitionDuration`, `transitionTimingFunction`
- Animation library configuration (Framer Motion defaults, GSAP defaults)
- Theme files or token JSON that include motion values
- If values exist, extend them. Use their naming conventions. Don't create a competing system.

**Read the motion spec.** The spec contains intent, storyboard, state map, and timeline. Token values should match:
- The durations and delays specified in the brief's timeline
- The easing character described in the brief's motion personality
- The visual states needed for each layer in the storyboard

## Figma integration (optional)

If the user has a Figma file, this is the most reliable way to get the static frame right. Ask:

*"Do you have a Figma file or frame for this animation? Paste the link and I'll match it exactly."*

If a link is provided, pull everything and match it precisely:

### Step 1: Pull the design context
- Use `get_design_context` to fetch the screenshot, reference code, and metadata for the target node
- Use `get_variable_defs` to pull ALL design variables — colors, spacing, typography, border radius, shadows, opacity
- Use `search_design_system` to find design system components used in the frame
- Use `get_code_connect_map` if Code Connect mappings exist

### Step 2: Match every component
For each component visible in the Figma frame, verify:
- **Exact colors** — use the Figma variable values, not approximations. `#1a73e8` is not `#1a74e9`
- **Exact spacing** — pull padding, margin, and gap values from the Figma layout. Don't round or estimate
- **Exact typography** — font family, weight, size, line height, letter spacing. All from Figma
- **Exact border radius** — pull from Figma, don't default to generic values
- **Exact shadows** — color, offset, blur, spread. Match the Figma shadow definition
- **Exact sizing** — component width, height, and constraints as defined in the design
- **Component hierarchy** — nesting order, z-index, and layer structure must match the Figma layer panel

### Step 3: Use Figma variables as tokens
Don't invent token values. Map Figma variables directly:
- Figma color variables → CSS `--color-*` properties with exact hex/rgba values
- Figma spacing variables → CSS `--space-*` properties with exact pixel values
- Figma typography styles → CSS font properties matching exactly
- Figma effect styles (shadows, blurs) → CSS shadow/filter properties matching exactly

**Rule: If a value exists in Figma, use it. Never generate a value that contradicts the design file.**

### Step 4: Visual comparison
After building the static screen, compare it side-by-side with the Figma screenshot:
- Take a screenshot of the built output
- Compare against the Figma reference at the same viewport size
- Check: layout alignment, color accuracy, text rendering, spacing, component sizing
- Any visible difference must be fixed before proceeding

If no Figma link is available, skip this section and generate tokens from the motion spec and codebase — but warn the designer that without a Figma reference, accuracy depends on their descriptions.

## Output format

Match the project's existing token format:
- If the project uses CSS custom properties → output as `:root { }` block
- If the project uses Tailwind → output as `theme.extend` config
- If the project uses a theme file or token JSON → output in that format

Always include light and dark mode variants for color-related tokens. Always include the reduced motion override.

## MANDATORY REVIEW — DO NOT SKIP THIS STEP

**This phase is NOT complete until the designer confirms the static screen matches their Figma mock.**

Generating tokens alone is NOT enough. You must build the screen, compare it deeply against Figma, and get the designer's approval.

### Step 1: Build the static HTML screen

Build the actual rendered screen — not just a token file. Every component, every element, fully styled and positioned.

### Step 2: Deep analysis against Figma

Pull a fresh screenshot from Figma using `get_screenshot` or `get_design_context`. Then compare the built screen against the Figma mock, checking each of these:

- **Layout** — Is the overall structure the same? Same columns, rows, nesting?
- **Spacing** — Are paddings, margins, and gaps pixel-accurate?
- **Typography** — Font family, size, weight, line-height, color — all matching?
- **Colors** — Background, text, borders, icons — exact hex values?
- **Border radius** — Same roundness on every corner?
- **Shadows** — Same depth, blur, spread, color?
- **Component sizing** — Width, height, aspect ratios matching?
- **Alignment** — Are elements centered, left-aligned, right-aligned the same way?
- **Content** — Same text, same placeholder content, same image sizes?

**List every difference you find.** Don't say "looks close" — be specific: "The heading font-size is 18px in the build but 20px in Figma" or "The card border-radius is 8px but Figma shows 12px."

### Step 3: Fix differences before showing the designer

Fix every difference you found in Step 2. Don't show the designer a screen you already know doesn't match.

### Step 4: Show the designer and ask

Show the built screen alongside the Figma reference. Then ask:

**How close is this to your Figma mock?**
- A) Looks right — let's move on
- B) Close, but I see some differences
- C) Pretty far off — let's rework it

### Step 5: If differences exist — diagnose with the designer

If they pick B or C, **never ask an open-ended "what's off?"** — always show a checklist so the designer can quickly select what they see. Display this exactly:

**What differences do you see? Pick all that apply:**
- A) Panel or container too wide / too narrow
- B) Text too small / too large
- C) Colors wrong — wrong shade or missing color
- D) Spacing off — things too close or too far apart
- E) Elements missing — something from the Figma isn't showing
- F) Elements misplaced — wrong position on screen
- G) Border / corners wrong
- H) Shadows missing or wrong
- I) Font looks different — wrong typeface or weight
- J) Overall proportion feels off — stretched or compressed
- K) Something else

Then for each item they selected, ask **one at a time**: "Which specific element?" — fix it — "Does this match now? A) Yes B) Still off"

Once all selected items are fixed, re-ask:

**Ready to move on, or do you see anything else?**
- A) Looks right — let's move on
- B) One more thing to fix

**NEVER move to Phase 4 until the designer says "A — looks right."**
