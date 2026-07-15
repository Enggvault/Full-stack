# CSS Reference

> **Module 03** · Prerequisites: [HTML ←](../02-html/notes.md) · Next: [JavaScript →](../04-javascript/notes.md)

---

## Table of Contents

1. [What is CSS?](#1-what-is-css)
2. [Ways to Add CSS](#2-ways-to-add-css)
3. [Selectors](#3-selectors)
4. [Specificity & the Cascade](#4-specificity--the-cascade)
5. [Units](#5-units)
6. [Colors](#6-colors)
7. [Typography](#7-typography)
8. [The Box Model](#8-the-box-model)
9. [Display Property](#9-display-property)
10. [Position Property](#10-position-property)
11. [Flexbox](#11-flexbox)
12. [CSS Grid](#12-css-grid)
13. [Responsive Design](#13-responsive-design)
14. [Transforms & Transitions](#14-transforms--transitions)
15. [Animations](#15-animations)
16. [CSS Custom Properties](#16-css-custom-properties)
17. [CSS Functions](#17-css-functions)
18. [Filters & Effects](#18-filters--effects)
19. [Architecture & Performance](#19-architecture--performance)

---

## 1. What is CSS?

**CSS (Cascading Style Sheets)** is a stylesheet language that controls the visual presentation of HTML documents — colors, typography, spacing, layout, and motion.

```
HTML        →  Structure  (what exists and what it means)
CSS         →  Presentation  (how it looks)
JavaScript  →  Behavior  (what it does)
```

CSS is a **living standard**, continuously updated via separate specification modules (Selectors Level 4, CSS Grid Level 2, etc.).

---

## 2. Ways to Add CSS

```html
<!-- 1. External stylesheet — best practice for all real projects -->
<link rel="stylesheet" href="styles.css">

<!-- 2. Internal — useful for single-page demos; avoids a network request -->
<style>
  h1 { color: #1d4ed8; }
</style>

<!-- 3. Inline — highest specificity; avoid in production -->
<h1 style="color: #1d4ed8;">Title</h1>
```

**Priority (highest to lowest):** Inline > Internal > External > Browser default

---

## 3. Selectors

### Basic Selectors

| Selector | Syntax | Selects |
|:---------|:-------|:--------|
| Universal | `*` | Every element |
| Element | `p` | All `<p>` elements |
| Class | `.btn` | Elements with `class="btn"` |
| ID | `#hero` | The element with `id="hero"` |
| Group | `h1, h2, p` | All matched elements |

### Combinator Selectors

| Selector | Syntax | Selects |
|:---------|:-------|:--------|
| Descendant | `div p` | All `<p>` inside a `<div>` (any depth) |
| Child | `ul > li` | Only **direct** `<li>` children of `<ul>` |
| Adjacent sibling | `h2 + p` | `<p>` immediately following an `<h2>` |
| General sibling | `h2 ~ p` | All `<p>` siblings after an `<h2>` |

### Attribute Selectors

```css
[type]            /* Has the attribute */
[type="email"]    /* Exact match */
[href^="https"]   /* Starts with */
[href$=".pdf"]    /* Ends with */
[class*="icon-"]  /* Contains */
```

### Pseudo-classes

```css
:hover          /* Mouse over the element */
:focus          /* Element has keyboard/programmatic focus */
:active         /* Element is being clicked */
:checked        /* Checked input */
:disabled       /* Disabled form element */
:first-child    /* First sibling */
:last-child     /* Last sibling */
:nth-child(2n)  /* Even-numbered siblings */
:not(.hidden)   /* Any element that does not match .hidden */
:root           /* The <html> element */
```

### Pseudo-elements

```css
::before         /* Insert content before element's content */
::after          /* Insert content after element's content */
::first-letter   /* Style the first character */
::first-line     /* Style the first rendered line */
::selection      /* User-selected text */
::placeholder    /* Placeholder text in inputs */
```

---

## 4. Specificity & the Cascade

When multiple rules target the same element and property, the **cascade** determines which rule wins using three factors in order of priority:

1. **Importance** — `!important` overrides all other declarations.
2. **Specificity** — A numerical score per selector.
3. **Source order** — The last declared rule wins among equal specificity.

### Specificity Scoring

| Selector type | Score |
|:--------------|------:|
| Inline `style=""` | 1-0-0-0 |
| ID `#id` | 0-1-0-0 |
| Class `.class`, pseudo-class `:hover`, attribute `[type]` | 0-0-1-0 |
| Element `p`, pseudo-element `::before` | 0-0-0-1 |

```css
#title   { color: red; }   /* 0-1-0-0 — wins */
.title   { color: blue; }  /* 0-0-1-0 */
h1       { color: green; } /* 0-0-0-1 */
```

### Inheritance

Properties like `color`, `font-family`, and `line-height` are **inherited** by children. Layout properties (`margin`, `border`, `padding`) are not. Use `inherit` to force inheritance, `initial` to reset to the browser default.

---

## 5. Units

### Absolute Units

| Unit | Description |
|:-----|:------------|
| `px` | Pixels — fine for borders and shadows; avoid for layout and fonts |

### Relative Units

| Unit | Relative to | Best Used For |
|:-----|:------------|:--------------|
| `%` | Parent element | Layout widths, heights |
| `em` | `font-size` of the current element | Component-level spacing |
| `rem` | `font-size` of `<html>` (default 16px) | **Global typography and spacing** |
| `vw` | 1% of viewport width | Full-width sections |
| `vh` | 1% of viewport height | Hero sections, full-screen layouts |
| `ch` | Width of the `0` character | Readable prose column widths |
| `fr` | Fraction of available grid space | CSS Grid track sizing |
| `clamp(min, ideal, max)` | Viewport-dependent | Fluid typography |

> **Rule of thumb:** `rem` for font sizes, `%` or `fr` for layout, `px` for fine details like borders.

---

## 6. Colors

```css
.element {
  color: #1d4ed8;                        /* HEX */
  background-color: rgb(29, 78, 216);    /* RGB */
  border-color: rgba(29, 78, 216, 0.5);  /* RGBA — with opacity */
  outline: hsl(224, 76%, 48%);           /* HSL — Hue Saturation Lightness */
}
```

| Format | Example | Notes |
|:-------|:--------|:------|
| Named | `cornflowerblue` | Limited palette |
| HEX | `#1d4ed8` | Most common |
| RGB | `rgb(29, 78, 216)` | Each channel 0–255 |
| RGBA | `rgba(29, 78, 216, 0.5)` | Fourth value is opacity 0–1 |
| HSL | `hsl(224, 76%, 48%)` | Intuitive for theme creation |

---

## 7. Typography

```css
body {
  font-family: 'Inter', Arial, sans-serif; /* Always provide fallbacks */
  font-size: 1rem;           /* Base size — scales with user preferences */
  font-weight: 400;          /* 100 (thin) to 900 (black) */
  font-style: normal;        /* normal | italic | oblique */
  line-height: 1.6;          /* Unitless multiplier is best practice */
  letter-spacing: 0.02em;
  color: #1e293b;
}

p {
  text-align: left;            /* left | center | right | justify */
  text-decoration: none;       /* underline | line-through | none */
  text-transform: none;        /* uppercase | lowercase | capitalize */
  text-overflow: ellipsis;     /* Truncates overflowing text with "…" */
  white-space: nowrap;
}
```

**Google Fonts:** Link in `<head>`, then reference by name:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
```

---

## 8. The Box Model

Every HTML element is a rectangular box. The Box Model defines how dimensions are calculated.

```
+──────────────────────────────────────+
│              MARGIN                  │  ← Outer space — transparent
│  +──────────────────────────────+    │
│  │            BORDER            │    │  ← Visible border
│  │  +────────────────────────+  │    │
│  │  │         PADDING        │  │    │  ← Inner space between border & content
│  │  │  +──────────────────+  │  │    │
│  │  │  │    CONTENT       │  │  │    │  ← Actual text, image, etc.
│  │  │  +──────────────────+  │  │    │
│  │  +────────────────────────+  │    │
│  +──────────────────────────────+    │
+──────────────────────────────────────+
```

### `box-sizing`

```css
/* Default (content-box): width = content width only.
   Padding and border add to the total. Confusing. */

/* Recommended: width = content + padding + border. */
*, *::before, *::after {
  box-sizing: border-box;
}
```

### Margin Shorthand

```css
margin: 10px;                /* all four sides */
margin: 10px 20px;           /* top/bottom | left/right */
margin: 10px 20px 30px 40px; /* top | right | bottom | left (clockwise) */
margin: 0 auto;              /* center a block element horizontally */
```

> **Margin collapse:** Adjacent vertical block margins merge into the larger of the two.

### Border

```css
.card {
  border: 1px solid #e2e8f0;  /* width style color */
  border-radius: 8px;          /* rounded corners */
  border-radius: 50%;          /* circle (on a square element) */
}
```

---

## 9. Display Property

| Value | Behavior |
|:------|:---------|
| `block` | Starts on a new line, full available width. `<div>`, `<p>`, `<h1>` |
| `inline` | Flows within text, content-width only. Cannot set `width`/`height`. `<span>`, `<a>` |
| `inline-block` | Flows inline, but accepts `width`, `height`, and `margin` |
| `none` | Removes the element entirely — not rendered, takes no space |
| `flex` | Enables Flexbox on the element's children |
| `grid` | Enables CSS Grid on the element's children |
| `inline-flex` | Flex container that itself behaves as an inline element |

---

## 10. Position Property

| Value | Behavior |
|:------|:---------|
| `static` | Default — normal document flow; `top`/`left` have no effect |
| `relative` | Offset from its normal position; still occupies its original space |
| `absolute` | Removed from flow; positioned relative to nearest **positioned** ancestor |
| `fixed` | Removed from flow; positioned relative to the **viewport**; does not scroll |
| `sticky` | Behaves as `relative` until a scroll threshold, then becomes `fixed` |

```css
/* Sticky navigation */
.navbar {
  position: sticky;
  top: 0;
  z-index: 100; /* controls stacking — higher is in front */
}

/* Absolute overlay within a relative parent */
.card {
  position: relative;
}
.card-badge {
  position: absolute;
  top: 8px;
  right: 8px;
}
```

---

## 11. Flexbox

Flexbox is a **1-dimensional** layout model that arranges items in a row or a column.

```
Flex Container
│
├── Flex Item 1
├── Flex Item 2
└── Flex Item 3

Main Axis  →  Direction of items (row: horizontal | column: vertical)
Cross Axis →  Perpendicular to the main axis
```

### Container Properties

```css
.container {
  display: flex;
  flex-direction: row;            /* row | row-reverse | column | column-reverse */
  flex-wrap: wrap;                /* nowrap | wrap */
  justify-content: space-between; /* Aligns on the MAIN axis */
  align-items: center;            /* Aligns on the CROSS axis */
  align-content: flex-start;      /* Aligns wrapped lines (only when flex-wrap: wrap) */
  gap: 16px;                      /* Space between items */
}
```

### Item Properties

```css
.item {
  flex-grow: 1;       /* How much the item grows relative to siblings */
  flex-shrink: 0;     /* How much it shrinks below flex-basis */
  flex-basis: 200px;  /* Initial size before growing/shrinking */
  flex: 1;            /* Shorthand: grow shrink basis */
  align-self: flex-end; /* Override container's align-items for this item */
  order: 2;           /* Visual reordering (not tab order) */
}
```

### Common Patterns

```css
/* Center anything */
.center {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

/* Navbar: logo left, links right */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

---

## 12. CSS Grid

CSS Grid is a **2-dimensional** layout model that arranges items in rows and columns simultaneously.

### Container Properties

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;   /* 3 columns */
  grid-template-rows: auto 1fr auto;    /* 3 rows */
  gap: 24px;                            /* row-gap and column-gap */
}

/* Named template areas */
.layout {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 240px 1fr;
}
```

### Item Properties

```css
.header-area { grid-area: header; }
.sidebar     { grid-area: sidebar; }
.main        { grid-area: main; }

/* Span specific lines */
.hero {
  grid-column: 1 / -1;  /* Span all columns */
  grid-row: 1 / 3;
}
```

### Grid Functions

```css
/* repeat() */
grid-template-columns: repeat(3, 1fr);  /* Three equal columns */

/* minmax() */
grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
/* Self-adjusting responsive gallery — no media queries needed */
```

### Flexbox vs Grid

| | Flexbox | Grid |
|:--|:--------|:-----|
| **Dimensions** | 1D (row **or** column) | 2D (rows **and** columns) |
| **Best for** | Components, alignment, navbars | Page layouts, galleries, dashboards |
| **Control** | Content-driven | Layout-driven |

---

## 13. Responsive Design

Responsive design ensures a layout works well across screen sizes — from a 320px phone to a 2560px monitor.

**Principles:**
- **Mobile-first:** Write CSS for small screens first; use `min-width` queries to progressively enhance for larger screens.
- **Fluid layout:** Use `%`, `fr`, `vw`, `vh` instead of fixed `px` widths.
- **Flexible images:** `max-width: 100%` prevents images from overflowing their container.

### Media Queries

```css
/* Mobile (default — no query needed) */
.cards {
  display: grid;
  grid-template-columns: 1fr;
}

/* Tablet */
@media (min-width: 768px) {
  .cards { grid-template-columns: repeat(2, 1fr); }
}

/* Desktop */
@media (min-width: 1200px) {
  .cards { grid-template-columns: repeat(3, 1fr); }
}
```

### Common Breakpoints

| Label | Width |
|:------|:------|
| Mobile (default) | < 768px |
| Tablet | 768px – 1199px |
| Desktop | ≥ 1200px |

---

## 14. Transforms & Transitions

### Transforms

```css
.box {
  transform: translate(20px, -10px);  /* Move X, Y */
  transform: rotate(45deg);
  transform: scale(1.1);              /* 1 = original size */
  transform: skew(10deg);
  /* Chain multiple: */
  transform: translateY(-4px) scale(1.05);
}
```

### Transitions

```css
.button {
  background-color: #1d4ed8;
  transform: translateY(0);
  /* property | duration | timing-function | delay */
  transition: background-color 0.2s ease, transform 0.2s ease;
}

.button:hover {
  background-color: #1e40af;
  transform: translateY(-2px); /* subtle lift */
}
```

**Timing functions:** `ease` · `linear` · `ease-in` · `ease-out` · `ease-in-out` · `cubic-bezier(x1,y1,x2,y2)`

> Animate `transform` and `opacity` — these are GPU-composited and do not cause layout reflow. Avoid animating `width`, `height`, `margin`, or `top`.

---

## 15. Animations

`@keyframes` enables complex, multi-step animations.

```css
/* 1. Define the keyframes */
@keyframes fadeSlideUp {
  from {
    opacity: 0;
    transform: translateY(16px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 2. Apply the animation */
.card {
  animation: fadeSlideUp 0.4s ease-out both;
  /* name | duration | timing | fill-mode */
}

/* Stagger children */
.card:nth-child(2) { animation-delay: 0.1s; }
.card:nth-child(3) { animation-delay: 0.2s; }
```

| Property | Values |
|:---------|:-------|
| `animation-iteration-count` | `1` (default), `infinite` |
| `animation-direction` | `normal`, `reverse`, `alternate` |
| `animation-fill-mode` | `none`, `forwards`, `backwards`, `both` |
| `animation-play-state` | `running`, `paused` |

---

## 16. CSS Custom Properties

CSS custom properties (variables) store values for reuse and enable theming.

```css
/* Declare in :root for global access */
:root {
  --color-primary: #1d4ed8;
  --color-text: #1e293b;
  --color-bg: #ffffff;
  --radius-md: 8px;
  --spacing-md: 16px;
  --font-sans: 'Inter', system-ui, sans-serif;
}

/* Use with var() */
.button {
  background: var(--color-primary);
  border-radius: var(--radius-md);
  padding: var(--spacing-md);
  font-family: var(--font-sans);
  color: var(--color-on-primary, white); /* fallback */
}

/* Dark mode */
[data-theme="dark"] {
  --color-text: #e2e8f0;
  --color-bg: #0f172a;
  --color-primary: #3b82f6;
}
```

---

## 17. CSS Functions

| Function | Example | Description |
|:---------|:--------|:------------|
| `var()` | `var(--color-primary)` | Insert a custom property value |
| `calc()` | `calc(100% - 240px)` | Arithmetic across different units |
| `min()` | `min(50%, 600px)` | Smaller of the values |
| `max()` | `max(2rem, 18px)` | Larger of the values |
| `clamp()` | `clamp(1rem, 2.5vw, 2rem)` | Responsive value with min and max bounds |
| `linear-gradient()` | `linear-gradient(135deg, #1d4ed8, #7c3aed)` | Gradient background |
| `repeat()` | `repeat(3, 1fr)` | Grid track repetition |
| `minmax()` | `minmax(200px, 1fr)` | Grid track size constraint |

---

## 18. Filters & Effects

```css
/* Image filters */
img {
  filter: grayscale(100%);
  filter: brightness(1.2);
  filter: blur(4px);
  filter: contrast(150%);
}

/* Frosted glass */
.glass-card {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

/* Shadows */
.card {
  box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1), 0 2px 4px -2px rgba(0,0,0,0.1);
}
```

### `object-fit` — for `<img>` and `<video>`

| Value | Behavior |
|:------|:---------|
| `cover` | Fills the box, crops edges — best for hero images |
| `contain` | Scales to fit without cropping |
| `fill` | Stretches to fill (may distort) |

```css
.hero-image {
  width: 100%;
  height: 400px;
  object-fit: cover;
  object-position: center top;
}
```

---

## 19. Architecture & Performance

### Naming Conventions

| Approach | Pattern | Example |
|:---------|:--------|:--------|
| **BEM** | `block__element--modifier` | `.card__title--featured` |
| **Utility** | Single-purpose classes | `.text-center`, `.mt-4` |
| **Component** | Scoped per UI component | `.button`, `.button--primary` |

### Performance Rules

| Rule | Reason |
|:-----|:-------|
| Animate `transform` and `opacity` | GPU-composited; no reflow or repaint |
| Avoid animating `width`, `height`, `top`, `margin` | Triggers expensive layout recalculation |
| Use `will-change: transform` sparingly | Hints GPU layer creation; overuse wastes memory |
| Avoid deeply nested selectors | `.nav ul li a span` is slow; use `.nav-link` |
| Set explicit `width` and `height` on images | Prevents Cumulative Layout Shift (CLS) |
| Minify CSS in production | Removes whitespace and comments |

### Reset Baseline

```css
/* Recommended CSS reset for consistent cross-browser baseline */
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  font-size: 100%; /* Respects browser/OS font size preferences */
}

img, video {
  max-width: 100%;
  height: auto;
}
```

---

> **Next:** [04 — JavaScript →](../04-javascript/notes.md)
