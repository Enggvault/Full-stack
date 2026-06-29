# The Complete CSS Reference Guide

> **CSS** (Cascading Style Sheets) is a stylesheet language that describes the **presentation, formatting, and layout** of HTML documents.

---

## Table of Contents
1. [Introduction](#1-introduction-to-css)
2. [Ways to Add CSS](#2-ways-to-add-css)
3. [CSS Syntax](#3-css-syntax)
4. [Selectors](#4-css-selectors)
5. [Specificity & Cascade](#5-css-specificity-and-cascade)
6. [Units](#6-css-units)
7. [Colors](#7-css-colors)
8. [Text & Font Properties](#8-text--font-properties)
9. [Box Model](#9-css-box-model)
10. [Display Property](#10-display-property)
11. [Position Property](#11-position-property)
12. [Flexbox](#12-css-flexbox)
13. [Grid](#13-css-grid)
14. [Responsive Design](#14-responsive-design)
15. [Transforms & Transitions](#15-transforms--transitions)
16. [Animations](#16-css-animations)
17. [CSS Variables](#17-css-variables-custom-properties)
18. [CSS Functions](#18-css-functions)
19. [Filters & Effects](#19-filters--effects)
20. [Architecture & Performance](#20-architecture--performance)
21. [CSS Cheat Sheet](#21-css-cheat-sheet)

---

## 1. Introduction to CSS

### What is CSS?
CSS stands for **Cascading Style Sheets**. It controls **colors, fonts, spacing, layout, and responsiveness** across different screen sizes.

### HTML vs CSS vs JavaScript
```
HTML        →  Structure  (the bones)
CSS         →  Presentation  (the skin and clothes)
JavaScript  →  Behavior  (the muscles)
```

### CSS Version History

| Version | Year | Key Features |
|---------|------|--------------|
| **CSS1** | 1996 | Basic font, color, margin, padding |
| **CSS2** | 1998 | Positioning, z-index, media types |
| **CSS3** | 2012+ | Flexbox, Grid, animations, variables, gradients |
| **Modern CSS** | Present | Custom properties, Container Queries, `:has()` |

> CSS is now a **Living Standard** — updated continuously via separate modules.

### Advantages of CSS
- **Separation of Concerns** — keeps design separate from content
- **Reusability** — one CSS file styles multiple HTML pages
- **Responsiveness** — adapts pages to different devices

---

## 2. Ways to Add CSS

### Priority: Inline > Internal > External > Browser Default

```html
<!-- 1. Inline CSS (highest priority, not recommended for large projects) -->
<h1 style="color: red; font-size: 20px;">Hello</h1>

<!-- 2. Internal CSS (inside <head>, useful for single-page sites) -->
<style>
  h1 { color: blue; }
</style>

<!-- 3. External CSS (Best practice — separate .css file) -->
<link rel="stylesheet" href="style.css">
```

> **Best practice:** Always use **External CSS** for real projects.

---

## 3. CSS Syntax

```css
/* This is a CSS comment */
selector {
  property: value;
  property: value;
}

h1 {
  color: blue;
  font-size: 24px;
}
```

| Term | Definition |
|------|-----------|
| **Selector** | Points to the HTML element to style |
| **Declaration Block** | `{ }` containing one or more declarations |
| **Property** | The style attribute you want to change (`color`) |
| **Value** | The specific setting for the property (`red`) |

---

## 4. CSS Selectors

### Basic Selectors
| Selector | Syntax | Description |
|----------|--------|-------------|
| **Universal** | `*` | Selects ALL elements |
| **Element** | `p`, `h1` | Selects by tag name |
| **Class** | `.btn` | Selects elements with that class (reusable) |
| **ID** | `#header` | Selects unique element with that ID |
| **Group** | `h1, h2, p` | Applies same style to multiple elements |

### Combinator Selectors
| Selector | Syntax | Description |
|----------|--------|-------------|
| **Descendant** | `div p` | All `<p>` inside a `<div>` (any depth) |
| **Child** | `div > p` | Only **direct** `<p>` children of `<div>` |
| **Adjacent Sibling** | `h1 + p` | `<p>` immediately following an `<h1>` |
| **General Sibling** | `h1 ~ p` | All `<p>` siblings after an `<h1>` |

### Attribute Selectors
```css
[type]            /* Has the attribute */
[type="text"]     /* Exact match */
[href^="https"]   /* Starts with "https" */
[href$=".pdf"]    /* Ends with ".pdf" */
[class*="icon-"]  /* Contains "icon-" */
```

### Pseudo-classes (Target element **states**)
```css
:hover          /* Mouse is over the element */
:focus          /* Element receives focus */
:active         /* Element is being clicked */
:visited        /* A visited link */
:first-child    /* First element among siblings */
:last-child     /* Last element among siblings */
:nth-child(n)   /* nth child (e.g., :nth-child(even)) */
:not(selector)  /* Everything EXCEPT matched selector */
:checked        /* Checked radio/checkbox */
:disabled       /* Disabled form element */
:root           /* Highest-level parent (usually <html>) */
```

### Pseudo-elements (Target specific **parts** of element)
```css
::before         /* Inserts content BEFORE element's content */
::after          /* Inserts content AFTER element's content */
::first-letter   /* Styles the first letter of text */
::first-line     /* Styles the first line of text */
::selection      /* Styles highlighted/selected text */
::placeholder    /* Styles placeholder text of an input */
::marker         /* Styles the list item marker (bullet/number) */
```

---

## 5. CSS Specificity and Cascade

### What is the Cascade?
The algorithm determining which CSS rule applies when **multiple rules target the same element**. It considers: **Importance → Specificity → Source Order**.

### Specificity Scoring

| Selector | Example | Score |
|----------|---------|-------|
| **Inline style** | `style="..."` | 1000 |
| **ID** | `#header` | 100 |
| **Class / Pseudo-class / Attribute** | `.btn`, `:hover` | 10 |
| **Element / Pseudo-element** | `p`, `::before` | 1 |

```css
/* Example */
#title   { color: red; }   /* Score: 100 — WINS */
.title   { color: blue; }  /* Score: 10 */
```

### Key Rules
- **`!important`** — Overrides all specificity. **Avoid unless absolutely necessary.**
- **Source Order** — If two rules have equal specificity, the **last one** declared wins.
- **Inheritance** — Properties like `color` and `font-family` are **inherited** by children. Layout properties (`margin`, `border`) are NOT inherited.

---

## 6. CSS Units

### Absolute Units (fixed size — avoid for responsive layouts)
| Unit | Description |
|------|-------------|
| `px` | Pixels — most common absolute unit |
| `pt` | Points (1pt = 1/72 inch) — used for print |

### Relative Units (essential for responsive design)
| Unit | Relative To | Best Used For |
|------|-------------|---------------|
| `%` | Parent element | Layout widths |
| `em` | Font-size of the element itself | Component-level spacing |
| **`rem`** | Font-size of root `<html>` | **Typography, global spacing** |
| `vw` | 1% of viewport width | Full-screen sections |
| `vh` | 1% of viewport height | Hero banners, full-screen |
| `ch` | Width of "0" character | Readable text column widths |

> **Rule of Thumb:** Use `rem` for font sizes, `%` or `fr` for layouts, `px` for fine details.

---

## 7. CSS Colors

```css
.box {
  color: red;                        /* Named color */
  background-color: #ff0000;         /* HEX */
  border-color: rgb(255, 0, 0);      /* RGB (0-255) */
  box-shadow: 0 4px rgba(0,0,0,0.5); /* RGBA (with opacity) */
  outline: hsl(0, 100%, 50%);        /* HSL — Hue, Saturation, Lightness */
}
```

| Format | Example | Notes |
|--------|---------|-------|
| **Named** | `red`, `tomato` | Simple but limited |
| **HEX** | `#ff0000` | Most common |
| **RGB** | `rgb(255, 0, 0)` | Each channel 0–255 |
| **RGBA** | `rgba(255,0,0,0.5)` | RGB + alpha (opacity 0–1) |
| **HSL** | `hsl(0, 100%, 50%)` | Great for theme creation |
| **HSLA** | `hsla(0,100%,50%,0.5)` | HSL + alpha |

---

## 8. Text & Font Properties

### Text Properties
```css
p {
  color: #333;
  text-align: center;          /* left | right | center | justify */
  text-decoration: underline;  /* none | underline | line-through */
  text-transform: uppercase;   /* uppercase | lowercase | capitalize */
  letter-spacing: 2px;
  word-spacing: 4px;
  line-height: 1.6;
  text-indent: 20px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
  text-overflow: ellipsis;     /* Truncates overflowing text with "..." */
  white-space: nowrap;
}
```

### Font Properties
```css
body {
  font-family: 'Inter', Arial, sans-serif; /* Always provide fallbacks */
  font-size: 1rem;          /* Use rem for accessibility */
  font-weight: 700;         /* normal | bold | 100–900 */
  font-style: italic;       /* normal | italic | oblique */
  font-variant: small-caps;
  font: 700 1rem 'Inter', sans-serif; /* Shorthand */
}
```

> **Google Fonts:** Link in HTML, use `font-family: 'FontName', sans-serif;`

---

## 9. CSS Box Model

> **Every HTML element is a rectangular box.** The Box Model controls how space is calculated.

```
+-------------------------------------------+
|                   Margin                  |  ← Outer space (pushes elements apart)
|  +-------------------------------------+  |
|  |              Border                 |  |  ← Visible line
|  |  +-------------------------------+  |  |
|  |  |           Padding             |  |  |  ← Inner space (between border & content)
|  |  |  +-------------------------+  |  |  |
|  |  |  |       CONTENT           |  |  |  |  ← Actual text / image
|  |  |  +-------------------------+  |  |  |
|  |  +-------------------------------+  |  |
|  +-------------------------------------+  |
+-------------------------------------------+
```

### `box-sizing`
```css
/* Default (content-box): padding + border INCREASE total width */
/* border-box: padding + border are INCLUDED in the width — RECOMMENDED */
*, *::before, *::after {
  box-sizing: border-box;
}
```

### Width, Height, Min, Max
```css
.card {
  width: 300px;
  min-width: 200px;    /* Won't shrink below this */
  max-width: 600px;    /* Won't grow beyond this */
  height: auto;
  max-width: 100%;     /* Critical for responsive images */
}
```

### Margin & Padding Shorthand
```css
/* All 4 sides */
margin: 10px;

/* Top/Bottom | Left/Right */
margin: 10px 20px;

/* Top | Left/Right | Bottom */
margin: 10px 20px 30px;

/* Top | Right | Bottom | Left (Clockwise) */
margin: 10px 20px 30px 40px;

/* Center a block element horizontally */
margin: 0 auto;
```

> **Margin Collapse:** Vertical margins of adjacent block elements can **merge** into the larger of the two.

### Border Properties
```css
.box {
  border: 2px solid #333;      /* Shorthand: width style color */
  border-radius: 8px;          /* Round corners */
  border-radius: 50%;          /* Perfect circle (on square element) */
  outline: 2px solid blue;     /* Outside border, doesn't affect layout */
}
```

---

## 10. Display Property

Controls the **rendering box type** of an element.

| Value | Description |
|-------|-------------|
| **`block`** | New line, full width. (`<div>`, `<h1>`, `<p>`) |
| **`inline`** | No new line, content width only. Cannot set width/height. (`<span>`, `<a>`) |
| **`inline-block`** | Inline behavior, but CAN set width/height and margins |
| **`none`** | Completely removes the element from layout |
| **`flex`** | Enables Flexbox on children |
| **`grid`** | Enables Grid on children |
| **`inline-flex`** | Flex container that acts as inline element |

---

## 11. Position Property

Controls how elements are **positioned** in the document.

| Value | Description |
|-------|-------------|
| **`static`** | Default. Normal document flow. `top/left` have no effect |
| **`relative`** | Normal flow, but can be nudged. **Stays in its original space** |
| **`absolute`** | Removed from flow. Positioned relative to **closest positioned parent** |
| **`fixed`** | Removed from flow. Positioned relative to **viewport**. Stays on scroll |
| **`sticky`** | Toggles between `relative` and `fixed` based on scroll position |

```css
/* Sticky navbar */
.header {
  position: sticky;
  top: 0;
  z-index: 100;   /* z-index: controls stacking order (higher = in front) */
}

/* Overlay / modal */
.overlay {
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
}
```

---

## 12. CSS Flexbox

> **Flexbox** is a **1-dimensional** layout model — arranges items in a **row OR column**.

```
Flex Container (parent)
│
├── Flex Item 1
├── Flex Item 2
└── Flex Item 3

Main Axis  →  Direction of flex items (set by flex-direction)
Cross Axis →  Perpendicular to main axis
```

### Container Properties
```css
.container {
  display: flex;
  flex-direction: row;            /* row | column | row-reverse | column-reverse */
  flex-wrap: wrap;                /* nowrap | wrap | wrap-reverse */
  justify-content: space-between; /* Align on MAIN axis */
  align-items: center;            /* Align on CROSS axis */
  align-content: flex-start;      /* Align wrapped lines */
  gap: 16px;                      /* Space between items */
}
```

### Item Properties
```css
.item {
  order: 1;           /* Visual order */
  flex-grow: 1;       /* How much item GROWS to fill space */
  flex-shrink: 0;     /* How much item SHRINKS */
  flex-basis: 200px;  /* Initial size */
  flex: 1;            /* Shorthand: grow shrink basis */
  align-self: flex-end; /* Override container's align-items */
}
```

### Common Patterns
```css
/* Perfect center */
.center {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

/* Navbar: logo left, links right */
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

---

## 13. CSS Grid

> **Grid** is a **2-dimensional** layout model — arranges items in **rows AND columns simultaneously**.

### Container Properties
```css
.grid {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;      /* 3 columns */
  grid-template-rows: auto 1fr auto;       /* 3 rows */
  grid-template-areas:
    "header header header"
    "sidebar main main"
    "footer footer footer";
  gap: 20px;                               /* Space between cells */
  justify-items: stretch;                  /* Align items within cells */
  align-items: stretch;
}
```

### Item Properties
```css
.item {
  grid-column: 1 / 3;    /* Span from column line 1 to 3 */
  grid-row: 1 / 2;
  grid-area: header;     /* Place in named area */
}
```

### Grid Functions
```css
/* fr unit: fraction of available space */
grid-template-columns: 1fr 2fr;

/* repeat() */
grid-template-columns: repeat(3, 1fr);     /* 3 equal columns */

/* minmax() */
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); /* Responsive gallery! */
```

### Flexbox vs Grid

| | Flexbox | Grid |
|-|---------|------|
| **Dimensions** | 1D (row OR column) | 2D (rows AND columns) |
| **Best for** | Components, navbars, alignment | Page layouts, galleries |
| **Control** | Content-driven | Layout-driven |

---

## 14. Responsive Design

Designing pages that **look good on all devices**.

### Key Concepts
- **Mobile-first** — write CSS for mobile first, use `min-width` media queries for larger screens
- **Breakpoints** — specific widths where layout changes
- **Fluid layout** — use `%`, `vw`, `vh`, `fr` instead of fixed `px`

### Media Queries
```css
/* Mobile first (default) */
.container { flex-direction: column; }

/* Tablet */
@media (min-width: 768px) {
  .container { flex-direction: row; }
}

/* Desktop */
@media (min-width: 1200px) {
  .container { max-width: 1200px; margin: 0 auto; }
}
```

### Common Breakpoints
```
Mobile:   < 768px
Tablet:   768px – 1024px
Desktop:  > 1024px
```

### Responsive Images
```css
img { max-width: 100%; height: auto; }
```

---

## 15. Transforms & Transitions

### Transforms
```css
.box {
  transform: translate(50px, 20px);  /* Move X, Y */
  transform: rotate(45deg);          /* Rotate */
  transform: scale(1.5);             /* Scale (1 = normal) */
  transform: skew(10deg, 5deg);      /* Tilt */
  transform-origin: center;          /* Point of transformation */
}
```

### Transitions (smooth property changes)
```css
.button {
  background-color: blue;
  transition: background-color 0.3s ease, transform 0.2s;
  /* property | duration | timing-function | delay */
}
.button:hover {
  background-color: darkblue;
  transform: translateY(-3px);  /* Lift effect on hover */
}
```

**Timing Functions:** `ease` | `linear` | `ease-in` | `ease-out` | `ease-in-out` | `cubic-bezier()`

---

## 16. CSS Animations

> For **complex, multi-step animations** (more powerful than transitions).

```css
/* 1. Define the animation sequence */
@keyframes slideUpFade {
  0%   { opacity: 0; transform: translateY(20px); }
  100% { opacity: 1; transform: translateY(0); }
}

/* 2. Apply to an element */
.card {
  animation-name: slideUpFade;
  animation-duration: 0.5s;
  animation-timing-function: ease-out;
  animation-delay: 0.2s;
  animation-iteration-count: 1;       /* or 'infinite' */
  animation-direction: normal;        /* normal | reverse | alternate */
  animation-fill-mode: forwards;      /* Keep final state after animation */
  animation-play-state: running;      /* running | paused */

  /* Shorthand */
  animation: slideUpFade 0.5s ease-out 0.2s forwards;
}
```

---

## 17. CSS Variables (Custom Properties)

> CSS Variables allow **storing values for reuse** throughout a document. Incredibly powerful for theming.

```css
/* Define in :root for global access */
:root {
  --primary-color: #2563eb;
  --secondary-color: #64748b;
  --spacing-md: 16px;
  --border-radius: 8px;
  --font-heading: 'Inter', sans-serif;
}

/* Use with var() */
.button {
  background-color: var(--primary-color);
  padding: var(--spacing-md);
  border-radius: var(--border-radius);
  color: var(--text-color, white);   /* Fallback value */
}

/* Dark mode with variables */
[data-theme="dark"] {
  --primary-color: #3b82f6;
  --bg-color: #1e293b;
}
```

---

## 18. CSS Functions

| Function | Example | Description |
|----------|---------|-------------|
| `var()` | `var(--color)` | Insert a custom property value |
| `calc()` | `calc(100% - 50px)` | Perform math operations |
| `min()` | `min(50%, 300px)` | Use the **smallest** of the values |
| `max()` | `max(2rem, 16px)` | Use the **largest** of the values |
| `clamp()` | `clamp(1rem, 2vw, 2rem)` | Scales between min and max — fluid typography |
| `rgb()` / `hsl()` | `rgb(255,0,0)` | Color functions |
| `linear-gradient()` | `linear-gradient(to right, red, blue)` | Gradient backgrounds |
| `radial-gradient()` | `radial-gradient(circle, red, blue)` | Circular gradients |
| `repeat()` | `repeat(3, 1fr)` | Repeat grid tracks |
| `minmax()` | `minmax(200px, 1fr)` | Grid track size constraints |

---

## 19. Filters & Effects

```css
.image {
  filter: blur(5px);
  filter: brightness(150%);
  filter: contrast(200%);
  filter: grayscale(100%);
  filter: invert(100%);
  filter: saturate(200%);
  filter: sepia(100%);
}

/* Frosted glass effect */
.glass {
  backdrop-filter: blur(10px);
  background: rgba(255, 255, 255, 0.1);
}

/* Shadows */
.card {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.5);
}
```

### Object Fit (for `<img>` and `<video>`)

| Value | Description |
|-------|-------------|
| `contain` | Scale to fit, no cropping (may leave empty space) |
| **`cover`** | Scale to fill, **crops edges** — great for hero images |
| `fill` | Stretches to fit (default, can distort) |

```css
.hero-image {
  width: 100%;
  height: 400px;
  object-fit: cover;
  object-position: center top;
}
```

---

## 20. Architecture & Performance

### CSS Architecture

| Approach | Description |
|----------|-------------|
| **BEM** | `block__element--modifier` naming convention (`.card__title--large`) |
| **Utility Classes** | Small, single-purpose classes (`.text-center`, `.mt-4`) |
| **Component CSS** | Group styles by UI component |
| **CSS Modules** | Auto-generates unique class names (prevents style leakage) |
| **Tailwind CSS** | Utility-first CSS framework |
| **SCSS/Sass** | Preprocessor adding nesting, mixins, variables |

### Performance Tips

- ✅ **Animate `transform` and `opacity`** — GPU-accelerated, no reflow
- ❌ **Avoid animating `width`, `height`, `margin`** — causes expensive reflows
- ✅ **Minify CSS** for production
- ✅ **Use external stylesheets** — allows browser caching
- ✅ **Remove unused CSS** (PurgeCSS, UnCSS)
- ❌ **Avoid deeply nested selectors** — `div ul li a span` is slow, use `.nav-link` instead
- ✅ **Pre-define `width` and `height` on images** — prevents layout shift (CLS)

---

## 21. CSS Cheat Sheet

### Box Model
```
margin → border → padding → content (outside-in)
box-sizing: border-box  ← always set this globally!
margin: 0 auto;         ← horizontal centering
```

### Flexbox Quick Reference
```
display: flex;
flex-direction: row | column
justify-content: flex-start | center | flex-end | space-between | space-around
align-items: stretch | center | flex-start | flex-end
flex: 1;  ← item grows to fill space
gap: 16px;
```

### Grid Quick Reference
```
display: grid;
grid-template-columns: repeat(3, 1fr);
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
gap: 20px;
grid-column: 1 / 3;  ← span 2 columns
```

### Common Patterns
```css
/* Center anything */
display: flex; justify-content: center; align-items: center;

/* Responsive grid gallery */
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));

/* Sticky header */
position: sticky; top: 0; z-index: 100;

/* Full-page hero */
height: 100vh; background-size: cover; background-position: center;

/* Truncate text */
white-space: nowrap; overflow: hidden; text-overflow: ellipsis;

/* Fluid font size */
font-size: clamp(1rem, 2.5vw, 2rem);

/* Overlay */
position: absolute; inset: 0; background: rgba(0,0,0,0.5);
```
