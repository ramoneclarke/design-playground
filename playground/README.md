# Design Playground

A static HTML workspace for iterating on mobile screen designs before building them. Each screen gets its own self-contained HTML file with phone frame mockups that can be viewed in the browser.

## How it works

- **One file per screen** — each screen (e.g., sign-in, product input, settings) is a standalone `.html` file in this folder.
- **`_template.html`** — copy this to start a new screen. It has the boilerplate already set up.
- **`index.html`** — the hub page. It shows thumbnail previews of all screens organized by section. Every new screen should be registered here.

## Phone frame

Every design is rendered inside a phone frame that simulates a real device:

- **Dimensions**: 390 x 844px (iPhone 15 Pro)
- **Scale**: Frames are rendered at `scale(0.7)` with negative margins to compensate, so they fit comfortably on the page without scrolling to see each one
- **Notch**: A CSS `::before` pseudo-element on `.phone-frame` simulates the Dynamic Island
- **Scrollable**: `.phone-screen` has `overflow-y: auto` with hidden scrollbars, so designs taller than 844px can scroll inside the frame

## Layout convention

The page uses a **rows and columns** grid to organize design exploration:

- **Columns (horizontal)** = iterations of the same design direction. When you tweak a design, place the new version to the right of the previous one. This creates a horizontal timeline showing how a direction evolved.
- **Rows (vertical)** = different design directions. When you try a fundamentally different approach (different layout, different hierarchy, different concept), start a new row below.

```
Row 1:  [Final]
Row 2:  [Direction A v1] → [Direction A v2] → [Direction A v3]
Row 3:  [Direction B v1] → [Direction B v2]
Row 4:  [Direction C v1]
```

### Promoting a final design

When a design direction is decided:

1. **Copy** the winning design to **Row 1** (the top row). Do NOT move it — leave the original where it is so the exploration history is preserved.
2. Label the final with a highlight color in its variant label (e.g., `text-green-500` or the project's accent color) and prefix the label with "Final Design —".
3. Add the `data-preview-phone` attribute to the final design's `.phone-frame` element. This attribute controls which phone frame appears as the thumbnail in the hub page.

If no final has been chosen yet, put `data-preview-phone` on whichever variant best represents the current state (usually the first one, Variant A).

## Variant labeling

Each phone frame gets a label above it — a small uppercase, letter-spaced text describing what this variant is:

```html
<span class="text-xs font-medium tracking-widest uppercase text-neutral-500">A — Card Layout</span>
```

- Use a letter prefix (A, B, C...) for quick reference
- Add a short descriptor after the dash (what makes this variant different)
- Finals get a highlight color instead of `text-neutral-500`

## Boilerplate structure

Every playground file needs these pieces (all present in `_template.html`):

### 1. Tailwind CDN + theme config
```html
<script src="https://cdn.tailwindcss.com"></script>
<script>
  tailwind.config = {
    theme: {
      extend: {
        colors: { /* project palette */ },
        fontFamily: { /* project fonts */ },
      },
    },
  }
</script>
```
Configure the project's design tokens here — colors, fonts, spacing, etc. This is the only place where project-specific theme values live.

### 2. Font imports
Import the project's fonts via Google Fonts, Fontshare, or other CDN links. Set the body default font in a `<style>` block.

### 3. Phone frame CSS
The `.phone-frame` and `.phone-screen` styles. Copy from `_template.html` — they rarely change.

### 4. Preview mode script
Handles the `?preview` query parameter. When a URL has `?preview`, the script strips all page chrome (toolbar, other variants) and renders just the phone screen content at full 390x844. The hub's iframe thumbnails use this.

The script looks for the element with `data-preview-phone` to decide which phone frame to show in preview mode.

### 5. Toolbar
A fixed top bar with:
- "← Hub" link back to `index.html`
- Screen title
- Device info (dimensions, device name)

### 6. Variant rows
The main content area. Rows of phone frames as described in "Layout convention" above.

## Hub page (index.html)

The hub page shows all screens as thumbnail cards organized by category sections.

### Adding a new screen to the hub

Each screen entry is a card with an iframe preview:

```html
<a href="screen-name.html" class="block bg-neutral-900 rounded-2xl p-4 border border-neutral-800 hover:border-white/20 transition-colors">
  <div class="aspect-[9/16] bg-neutral-800 rounded-xl mb-3 overflow-hidden relative">
    <iframe src="screen-name.html?preview" class="absolute border-0 pointer-events-none"
      style="width: 390px; height: 844px; transform-origin: top left; top: 0; left: 0;"
      onload="this.style.transform='scale('+this.parentElement.offsetWidth/390+')'">
    </iframe>
  </div>
  <p class="font-medium text-sm">Screen Name</p>
  <p class="text-xs text-neutral-500 mt-0.5">Screen N — in progress</p>
</a>
```

The iframe loads the screen with `?preview`, which triggers the preview mode script to show just the `data-preview-phone` frame content. The `onload` handler auto-scales the iframe to fit the card width.

### Adding a new section to the hub

If the screen belongs to a category that doesn't have a section yet, add one:

```html
<h2 class="text-lg font-semibold text-neutral-300 mb-4 mt-12">Section Name</h2>
<div class="grid grid-cols-2 md:grid-cols-4 gap-4">
  <!-- Screen cards go here -->
</div>
```

## Custom animations

Some screens use custom CSS animations (shimmer, breathe, pulse, float, etc.). Define these in the `<style>` block of the individual screen file. They are not shared globally — each file is self-contained.

## Workflow

1. **Read the code first** — before designing a screen, read the existing app code for that screen to understand what data and interactions it needs.
2. **Copy `_template.html`** — name it with a descriptive slug (e.g., `settings-account.html`).
3. **Configure the theme** — set up the project's colors, fonts, and any custom CSS in the new file.
4. **Design 2-3 variants** — explore meaningfully different directions, not just minor color tweaks. Different layouts, different information hierarchy, different interaction patterns.
5. **Iterate horizontally** — when refining a direction, add the new version to the right of the previous one in the same row.
6. **Explore vertically** — when trying a completely different approach, start a new row.
7. **Promote the winner** — copy the final design to Row 1, label it, set `data-preview-phone` on it.
8. **Register in the hub** — add the screen card to `index.html` in the appropriate section.

## File naming

Use lowercase kebab-case slugs that describe the screen:

```
auth-signin.html
tryon-product-input.html
settings-account.html
outfit-home.html
```

Prefix with the section/feature area, then the specific screen name.

## Page background

The playground page itself (the "workbench" behind the phone frames) uses a dark background (`bg-neutral-900` or `bg-neutral-950`). This makes the phone screens pop visually and gives a gallery/lightbox feel. The dark background is for the playground only — it has nothing to do with the app's design system.
