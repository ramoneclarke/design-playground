# Design Playground

A lightweight design system for iterating on UI screens using pure HTML + Tailwind CSS. No build step, no framework overhead — just visual design in code.

Designed to work with **Claude Code** and **Google Stitch** (via MCP) for a fast ideation-to-refinement workflow.

## Why

- **Stitch** is great for generating initial design ideas fast, but lacks fine control
- **Figma/design tools** are disconnected from code and require manual translation
- **Building in the app** couples design iteration with framework overhead (auth, routing, state)

The playground sits in the middle: real CSS, instant feedback, side-by-side comparison, and near-identical class names to what you'll ship in React/React Native.

## Quick Start

1. Copy the `playground/` folder into your project
2. Open `playground/index.html` in a browser
3. Copy `playground/_template.html` → rename to your screen (e.g. `checkout.html`)
4. Add your project's colors/fonts to the Tailwind config block
5. Start designing inside the `phone-screen` div
6. Add a link card in `index.html`

No server needed — just open HTML files directly in a browser.

## Workflow

```
Google Stitch (ideate) → Playground (refine) → Ship (build in app)
```

### 1. Ideate in Stitch
Generate initial concepts using Google Stitch. Explore wildly different directions quickly. Use the Stitch MCP tools to pull screens into the playground:

```
# In Claude Code
"Fetch this Stitch screen and bring it into the playground"
```

The MCP fetches the screenshot + HTML, downloads them, and adapts the design to your project's design tokens.

### 2. Refine in Playground
This is where the real design work happens. Stitch gives you 80% — the playground gets you to 100%.

- Duplicate variants side-by-side to compare
- Mix elements from different designs
- Fine-tune spacing, typography, color
- Use the `frontend-design` skill for polish passes

### 3. Finalize and Ship
When a design is decided, promote it to Row 1 (top of the file). Then translate the HTML/Tailwind to your actual framework — the class names are nearly identical.

## Layout Conventions

Each playground file uses a **grid system**:

- **Rows (vertical)** = completely different design directions
- **Columns (horizontal)** = iterations/refinements of the same direction
- **Row 1** = always the finalized design

### The "Final Design" pattern

When you've decided on a design:
1. Copy it to Row 1 at the top
2. Label it `Final Design — [Screen Name]`
3. Keep iteration rows below for reference

Opening any file immediately shows the decided design, with the full journey below.

## File Structure

```
your-project/
├── playground/
│   ├── index.html          ← Hub page with links to all screens
│   ├── _template.html      ← Starter file for new screens
│   └── [screen-name].html  ← One file per screen you're designing
```

## Template Features

- **Phone frame** — 390×844px (iPhone 15 Pro) with notch, scaled to 70%
- **Variant system** — designs sit side-by-side, labeled "Variant A", "Variant B", etc.
- **Toolbar** — back-to-hub link, viewport dimensions
- **Tailwind CDN** — no build step, just write classes
- **Project theming** — commented placeholder for custom colors/fonts

## Google Stitch MCP Integration

### When to use Stitch
- Exploring wildly different layout directions
- Getting AI-generated placeholder imagery
- Quick initial concepts for stakeholder discussion

### When to stay in the playground
- Refining spacing, typography, color balance
- Comparing similar variants side-by-side
- Applying project-specific design tokens
- Pixel-level control over specific elements

### Stitch MCP commands
```
# Get a screen from Stitch
mcp__stitch__get_screen(name, projectId, screenId)

# List all screens in a project
mcp__stitch__list_screens(parent)

# Generate new variants
mcp__stitch__generate_variants(name, prompt)
```

## Working with Claude Code

```
"Here's a screenshot from Stitch — replicate this in the playground"
"Make a variation next to this one with [change]"
"Use the frontend-design skill to refine this"
"Compare these two — which is better and why?"
"This is the final design — move it to the top"
"Document this design in the implementation plan"
"Now build this in the actual app"
```

## License

MIT
