# Website Modification Guide

This document describes where to adjust each part of the project website.

---

## 1. Global Color Theme

All colors are defined as CSS custom properties in the `<style>` block inside `index.html`, under `:root` (approximately line 25–50).

| Variable | Current Value | Controls |
|----------|--------------|----------|
| `--color-bg` | `#faf8f3` | Page background (warm cream) |
| `--color-bg-subtle` | `#f5f2ec` | Card/panel backgrounds |
| `--color-bg-card` | `#f0ede6` | Deeper card background |
| `--color-bg-hero-start` | `#ebe5d8` | Hero gradient start (top) |
| `--color-bg-hero-end` | `#faf8f3` | Hero gradient end (bottom) |
| `--color-text` | `#1a1a1a` | Primary text |
| `--color-text-secondary` | `#4a4a4a` | Body text |
| `--color-text-tertiary` | `#8a8070` | Captions, labels |
| `--color-accent` | `#c75a2e` | Buttons, highlights, active TOC |
| `--color-accent-hover` | `#a84822` | Button hover state |
| `--color-link` | `#287775` | Hyperlinks |
| `--color-border` | `rgba(139, 119, 89, 0.12)` | Section borders |

## 1b. Hero Particle Animation

The animated background in the hero section is controlled by JavaScript near line 1520 of `index.html`. Two groups of particles (blue and teal) flow from the left and right edges toward the center, following a spiral vector field. Trails persist and accumulate to form a swirling circular pattern.

**Master Toggle (line ~1524):**
```javascript
var SHOW_HERO_PARTICLES = true;  // Set to false to disable entirely
```

**Animation Parameters (the `C` object, line ~1540):**

| Parameter | Default | Controls |
|-----------|---------|----------|
| `count` | `55` | Particles per group (total = 110; halved on mobile) |
| `stepSize` | `1.5` | Integration step size (px per step, affects path smoothness) |
| `maxSteps` | `900` | Maximum path length in integration steps |
| `minR` | `14` | Stop radius from center — particles halt when this close (px) |
| `swirl` | `2.2` | Tangential vs radial strength ratio (higher = tighter spirals) |
| `swirlR` | `80` | Distance scale for swirl increase (px; swirl grows as 1/(1+r/swirlR)) |
| `speed` | `0.65` | Steps advanced per animation frame (controls convergence speed) |
| `lineW` | `0.8` | Trail line width (px) |
| `lineA` | `0.16` | Trail line opacity |
| `dotR` | `1.2` | Particle dot radius (px) |
| `dotA` | `0.32` | Particle dot opacity |
| `colA` | `[120, 165, 210]` | Left group color — light blue (RGB) |
| `colB` | `[95, 190, 168]` | Right group color — light teal (RGB) |
| `margin` | `6` | Starting distance from canvas edge (px) |
| `ySpread` | `0.88` | Vertical spread of starting positions (fraction of hero height) |

**Convergence duration:** Approximately `(maxSteps / speed) / 60` seconds ≈ 23s at defaults. Increase `speed` for faster convergence.

**Spiral tightness:** Controlled by `swirl` and `swirlR`. Higher `swirl` = more tangential motion; lower `swirlR` = swirl kicks in farther from center.

**Central symmetry:** Left particle at (margin, y) pairs with right particle at (w−margin, h−y). The vector field V(x,y) satisfies V(−x,−y) = −V(x,y), ensuring point-symmetric trajectories.

## 2. Typography

| Variable | Current Value | Controls |
|----------|--------------|----------|
| `--font-heading` | `'Lora', Georgia, serif` | Section titles, card headings |
| `--font-body` | `'Inter', system-ui, sans-serif` | Body text, buttons, labels |
| `--font-mono` | `'SFMono-Regular', 'Menlo', monospace` | Citation block |

Font sizes and weights are defined per-element in the CSS. Key locations:
- `body` font-size: `17px` (line ~60)
- `.hero h1` font-size: `clamp(1.75rem, 4vw, 2.65rem)` (hero title)
- `.article h2` font-size: `1.85rem` (section headings)
- `.article h3` font-size: `1.35rem` (sub-section headings)
- `.article p` inherits from body

## 3. Layout Widths

| Variable | Current Value | Controls |
|----------|--------------|----------|
| `--width-article` | `920px` | Maximum width of main content area |
| `--width-prose` | `700px` | Maximum width of text paragraphs |

The hero section inner content is capped at `860px` (`.hero-inner max-width`).

## 4. Hero Section

Location: HTML starts at `<header class="hero">`, CSS at `.hero`.

| Element | How to Modify |
|---------|---------------|
| **Background gradient** | `.hero` CSS: `linear-gradient(180deg, var(--color-bg-hero-start) 0%, #f2ede4 45%, var(--color-bg-hero-end) 100%)` |
| **Dot grid pattern** | `.hero::before`: adjust `rgba(160, 130, 90, 0.045)` for opacity, `28px 28px` for grid spacing |
| **Venue badge** | `.hero-venue` — change text "ICML 2026" in HTML |
| **Title** | `<h1>` text inside `.hero-inner` |
| **Tagline** | `.hero-tagline` `<p>` element |
| **Authors** | `.hero-authors` — edit `<a>` tags with names, links, and superscript affiliations |
| **Affiliations** | `.hero-affiliations` — edit text directly |
| **Buttons** | `.hero-actions` — edit `href` and text. Style via `.btn-primary` / `.btn-secondary` |
| **Hero padding** | `.hero` CSS: `padding: 5.5rem 2rem 4rem` |

## 5. Table of Contents (Sidebar)

Location: `<nav class="toc-container">`.

- **Visibility breakpoint**: `@media (max-width: 1200px)` hides the TOC. Change `1200px` to adjust.
- **Position**: `.toc-container` `left: max(20px, calc(50% - 980px))`. Adjust `980px` to move it.
- **Active highlight color**: `.toc-list a.active` background is `var(--color-accent)`.
- **Add/remove entries**: Edit `<li>` items inside `.toc-list`. Each `<a>` should have `href="#section-id"` and `data-section="section-id"`.

## 6. Overview Section

Location: `<section id="overview">`.

- **Teaser figure**: `<div class="figure">` with `images/teaser.svg`, placed between the introductory prose and the effects grid. Change `src` to replace the image.
- **Effect cards** (`.effects-grid`): Two-column grid layout.
  - Card number badge: `.effect-card-number` (color via `--color-accent`)
  - Stat badges: `.effect-stat` (e.g., "Explains ~50% of loss variance")
  - To add more cards: duplicate a `.effect-card` div
- **Highlight box**: `.highlight-box` with left accent border. Edit text inside.

## 7. Two Intrinsic Effects Section

Location: `<section id="two-effects">`.

- **Math formulas**: Uses KaTeX (loaded via CDN). Inline: `$...$`, display: `$$...$$`
- **Scenario cards** (`.scenarios-grid`): Three-column grid.
  - Good scenario has `.scenario-good` class (teal border)
  - Outcome tags: `.outcome-good` (teal) or `.outcome-bad` (red)
  - Icons: Unicode characters in `.scenario-icon` span

## 8. Illustrative Toy Models (Interactive)

Location: `<section id="toy-models">`.

- **Mixing ratio buttons**: `.segmented-btn[data-ratio]` inside `.segmented-btn-group`
  - `--segmented-count: 6` controls slider width calculation
  - Button text labels are in the `<button>` text
- **Video paths**: Defined in JavaScript at the bottom:
  ```
  var BASE = 'videos/toy_models/';
  var FOLDERS = { overlap: 'overlap', aligned: 'aligned', disjoint: 'disjoint' };
  var FILES = { srconly: '...', cotrain_model: '...', ... };
  ```
- **Video replay delay**: `var PAUSE_MS = 5000;` (5 seconds between loops)
- **Video fade animation**: `.sampling-video-fadeout` class, 300ms timeout

## 9. Representation Alignment (Interactive UMAP)

Location: `<section id="representation-alignment">`.

- **Task selector**: `.segmented-btn[data-task]` buttons with values `nutassembly`, `mugcleanup`, `mughang`
- **UMAP iframe**: `<iframe id="umap3d-iframe" src="files/umap3d_nutassembly.html">`
  - Height: `.umap3d-iframe` CSS `height: 620px` (450px on mobile)
  - UMAP data lives in `files/umap_data/{task}/*.json`
  - UMAP HTML template: `files/umap3d_*.html`

## 10. Unified View of Co-Training Methods

Location: `<section id="unified-view">`.

- **Method cards** (`.methods-grid`): Four-column grid.
  - Tag classes: `.tag-alignment`, `.tag-discernibility`, `.tag-both`
  - Highlighted card: `.method-highlight` class
- **Results table** (`.results-table`):
  - Highlighted row: `.row-highlight` class
  - Best cell: `.cell-best` class
  - Table wrapper has `overflow-x: auto` for mobile

## 11. Takeaways Section

Location: `<section id="takeaway">`.

- **Task selector**: `<select id="representation-task-selection">` with options
- **Video files**: Defined in JavaScript `abstractVideoFiles` object:
  ```
  var abstractVideoFiles = {
    "store_clean_dishes": "store_clean_dishes.mp4",
    "pack_items": "pack_items.mp4",
    "pour_ingredients": "pour_ingredients.mp4"
  };
  ```
  Videos are at `videos/abstract/*.mp4`
- **Video comparison layout**: `.video-comparison` is a 2-column grid

## 12. Citation Section

Location: `<section id="citation">`.

- **BibTeX**: Edit the `<pre><code>` block directly
- **Style**: `.citation-block` background is `--color-bg-subtle`

## 13. Footer

Location: `<footer class="site-footer">`.

- Background: `--color-text` (dark)
- Edit `<p>` text for footer content

## 14. Responsive Breakpoints

| Breakpoint | Behavior |
|-----------|----------|
| `1200px` | TOC sidebar hidden |
| `768px` | Single-column layouts, smaller fonts, reduced padding |
| `480px` | Buttons stack vertically, method grid becomes 1-column |

---

## File Structure

```
website_cotraining_old/
├── index.html              # Main page (all HTML + CSS + JS inline)
├── favicon.png             # Browser tab icon
├── MODIFICATION_GUIDE.md   # This document
├── css/
│   └── fontawesome.all.min.css  # Font Awesome icons
├── js/
│   └── fontawesome.all.min.js   # Font Awesome runtime
├── images/
│   ├── teaser.svg               # Main figure (overview section)
│   ├── head.svg                 # Decorative (unused)
│   ├── helpful_combined.svg     # Legacy figure (unused)
│   ├── helpful.svg              # Legacy figure (unused)
│   └── radar2.svg               # Legacy figure (unused)
├── files/
│   ├── umap3d_nutassembly.html  # Interactive UMAP (NutAssembly)
│   ├── umap3d_mugcleanup.html   # Interactive UMAP (MugCleanup)
│   ├── umap3d_mughang.html      # Interactive UMAP (MugHang)
│   └── umap_data/               # JSON data for UMAP visualizations
│       ├── nutassembly/*.json
│       ├── mugcleanup/*.json
│       └── mughang/*.json
└── videos/
    ├── abstract/                 # Takeaway section videos
    │   ├── store_clean_dishes.mp4
    │   ├── pack_items.mp4
    │   └── pour_ingredients.mp4
    └── toy_models/               # Toy model experiment videos
        ├── overlap/*.mp4
        ├── aligned/*.mp4
        └── disjoint/*.mp4
```

## External Dependencies (CDN)

| Resource | URL | Purpose |
|----------|-----|---------|
| Google Fonts | `fonts.googleapis.com` | Inter & Lora typefaces |
| KaTeX CSS | `cdn.jsdelivr.net/npm/katex@0.16.11` | Math rendering styles |
| KaTeX JS | `cdn.jsdelivr.net/npm/katex@0.16.11` | Math rendering engine |
| Google Analytics | `googletagmanager.com` | Site analytics (`G-CN5DPV4JQN`) |

To make fully offline: download these assets and update `<link>` / `<script>` paths.
