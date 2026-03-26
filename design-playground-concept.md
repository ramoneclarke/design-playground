# Design Playground — Concept

## The Problem

When designing UI in the actual Next.js or React Native app, design iteration is slow because you're fighting component logic, data fetching, auth guards, navigation, and framework overhead — all irrelevant to visual design. Design and code are coupled, which slows down creative exploration.

Tools like Google Stitch are great for generating initial design ideas and inspiration, but lack the fine control needed to dial in specific details, compare variants side-by-side, or mix elements from different concepts.

## The Solution

A **design playground** — a dedicated folder in the project (`playground/`) where we work with pure HTML and Tailwind CSS to design screens. No React, no logic, no build step. Just visual design in code.

## How It Works

1. **Start with inspiration.** Generate initial concepts in Google Stitch, screenshot existing apps, or sketch ideas.
2. **Replicate in the playground.** Paste a screenshot into Claude Code and have it rebuilt as HTML + Tailwind in a playground file.
3. **Iterate with precision.** Duplicate a design, make targeted edits, compare variants side-by-side. "Move this section here, swap this color, try a different layout for the cards." Fine control that Stitch can't offer.
4. **Mix and match.** Bring in a screenshot of a specific element from one design and apply it to another. Cherry-pick the best parts from multiple variants.
5. **Lock and build.** Once a design is finalized, build it properly in the actual app codebase. Translating HTML/Tailwind to React (Next.js) or React Native (NativeWind) is straightforward — the class names are nearly identical.

## Why This Works

- **Pure HTML + Tailwind is the fastest medium for visual iteration.** No build step, no framework overhead, instant feedback. It's basically Figma but with real CSS.
- **Side-by-side comparison.** Have 3 variants of a screen next to each other in one file, see differences instantly, converge on a final design.
- **Screenshots as input.** Claude Code can replicate any screenshot in HTML/Tailwind, then you direct edits with surgical precision.
- **Separation of concerns.** Design is design. Code is code. You don't deal with auth, API calls, or state management while trying to figure out if a button should be 14px or 16px.
- **Clean handoff.** The finalized HTML/Tailwind designs serve as a precise reference (or even a direct translation base) when building the real screens.

## Folder Structure

```
any-project/
├── playground/              ← design workspace
│   ├── index.html           ← blank hub page — add links as you create screens
│   ├── _template.html       ← starter file for new screens
│   ├── your-screen.html     ← one file per screen you're designing
│   └── ...
```

This is **project-agnostic** — use it for any project, not just one. Each HTML file is self-contained (Tailwind via CDN, no build step). No server needed — just open in a browser.

## Files

### `index.html` (Hub)
A blank page with a grid. As you create screen files, add a link card to the hub so you can navigate between designs. A commented-out HTML block is included to copy-paste for each new screen.

### `_template.html` (Starter)
A generic starter for any new screen. Includes:
- **Phone frame** (390x844, iPhone 15 Pro proportions) with notch
- **Variant system** — designs sit side-by-side horizontally, labeled "Variant A", "Variant B", etc. Duplicate the variant block to add more.
- **Toolbar** with back-to-hub link and viewport dimensions
- **Vanilla Tailwind** — no project-specific colors or fonts baked in

The template has commented-out placeholders for project-specific theme overrides (custom colors, fonts). Add these when you start a new project's designs.

## How to Use

1. **Copy `_template.html`** → rename to your screen (e.g. `try-on-result.html`)
2. **Add project theming** if needed — uncomment the Tailwind config block and add your colors/fonts
3. **Design inside the `phone-screen` div** — this is your canvas
4. **Add variants** — duplicate the entire "Variant A" block to create B, C, etc. They appear side-by-side
5. **Add a link card** in `index.html` so you can find it from the hub
6. **Open in browser** — just double-click the HTML file, no server needed

## Layout Conventions

The playground uses a **grid system** for organizing designs within a file:

- **Rows (vertical)** = completely different design directions. Row 1 is always the finalized design.
- **Columns (horizontal)** = iterations/edits of the same design direction. Place refinements next to the design they evolved from.

### The "Final Design" pattern

When a design direction is finalized:
1. Copy it to **Row 1** at the top of the file
2. Label it `Final Design — [Screen Name]` (in coral text, not neutral)
3. Keep all iteration rows below for reference

This means opening any playground file immediately shows you the decided design at the top, with the full design journey below it.

## Google Stitch Integration

Google Stitch (via MCP) is the **ideation engine** — it generates initial design concepts quickly. The playground is the **refinement engine** — it takes those concepts and dials them in with precision.

### Stitch → Playground workflow

1. **Generate in Stitch** — Create a project, describe the screen, generate variants. Stitch is fast for exploring wildly different directions.
2. **Pull into playground** — Use the Stitch MCP tools (`get_screen`) to fetch the screenshot + HTML. Download both with `curl`.
3. **Adapt to project** — The Stitch HTML uses its own fonts/colors. Rebuild the design using the project's actual design system (fonts, colors, tokens).
4. **Iterate in playground** — This is where the real design work happens. Stitch gives you 80% — the playground gets you to 100%.

### When to go back to Stitch

- When you're stuck on a direction and need fresh ideas
- When you want to explore a completely different layout approach
- When you need AI-generated imagery for mockups (Stitch generates placeholder images)

### When to stay in the playground

- When you're refining spacing, typography, color balance
- When you're comparing two similar variants side-by-side
- When you need pixel-level control over specific elements
- When you're applying project-specific design tokens

## Workflow with Claude Code

### Starting a new screen design

1. "Fetch this Stitch screen and bring it into the playground"
2. "Replicate this screenshot in the playground"
3. "Create a new playground file for the scan results screen"

### Iterating

- "Make a variation next to this one with [change]"
- "Take the card layout from this screenshot and apply it to variant B"
- "Use the frontend-design skill to refine this"
- "Compare these two — which is better and why?"

### Finalizing

- "This is the final design — move it to the top"
- "Document this design in the implementation plan"
- "Now build this in the actual app"

## Using Across Projects

The playground system is **project-agnostic**. To use it in a new project:

1. Copy the `playground/` folder (`index.html`, `_template.html`) into the new project
2. Copy `documents/design-playground-concept.md` for reference
3. Update the template's Tailwind config with the new project's design tokens
4. Start designing

The Stitch MCP integration works with any project — create a Stitch project per app, generate screens there, pull them into the playground.
