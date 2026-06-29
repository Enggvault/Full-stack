# The Complete CSS Guide
Welcome to the ultimate guide to CSS. This comprehensive reference is designed to take you from a complete beginner to an advanced front-end developer.


## 1. Introduction to CSS

### What is CSS?
CSS stands for **Cascading Style Sheets**. It is a stylesheet language used to describe the presentation, formatting, and layout of a document written in HTML or XML.

### Why CSS is used
CSS is used to style and lay out web pages. It controls colors, fonts, spacing, layout structure, and responsiveness across different screen sizes. Without CSS, a web page would just be plain, unstyled HTML text.

### How CSS works with HTML
HTML provides the structural foundation (the bones), while CSS provides the visual aesthetics (the skin and clothes). CSS targets HTML elements using "selectors" and applies stylistic "properties" to them.

### CSS vs HTML vs JavaScript
- **HTML:** Defines the structure and content.
- **CSS:** Defines the visual style and layout.
- **JavaScript:** Defines behavior, logic, and interactivity.

### Role of CSS in frontend development
CSS is fundamental to creating visually engaging, accessible, and responsive user interfaces. It transforms raw data into a polished user experience.

### Advantages of CSS
- **Separation of Concerns:** Keeps design separate from content.
- **Reusability:** One CSS file can style multiple HTML pages.
- **Faster Page Load:** Reduces the amount of HTML markup.
- **Responsiveness:** Allows pages to adapt to different devices.

### Limitations of CSS
- CSS is not a programming language (no loops or complex logic natively, though it is getting more powerful).
- Cross-browser compatibility issues can sometimes occur.

### How browsers apply CSS
Browsers parse the HTML to create a DOM (Document Object Model) and parse the CSS to create a CSSOM (CSS Object Model). They combine these to form a Render Tree, calculate the layout, and finally paint the pixels on the screen.


## 2. CSS Versions and History

CSS has evolved significantly over the years.

| Version | Year | Main Features | Importance |
| ------- | ---- | ------------- | ---------- |
| **CSS1** | 1996 | Basic font, color, margin, and padding properties. | Introduced styling to the web. |
| **CSS2** | 1998 | Positioning, z-index, media types (print vs screen). | Allowed complex layouts. |
| **CSS2.1** | 2011 | Refined CSS2, fixed errors, removed poorly supported features. | Became the stable baseline for modern CSS. |
| **CSS3** | 2012+ | Modules like Flexbox, Grid, rounded corners, shadows, gradients, animations, transitions. | Revolutionized web design with native styling. |
| **Modern CSS** | Present | Custom properties (variables), Container Queries, `:has()`, Grid Level 2. | Focuses on robust, logic-driven styling. |

**CSS Modules Concept:** Instead of one massive CSS4 specification, the W3C split CSS into smaller "modules" (e.g., Color Module, Grid Module) that update independently.
**CSS Living Standard:** CSS is now continually updated by the CSS Working Group without overarching version numbers.


## 3. Ways to Add CSS

### Inline CSS
Applied directly to an HTML element using the `style` attribute. (Not recommended for large projects).
```html
<h1 style="color: red; font-size: 20px;">Hello World</h1>
```

### Internal CSS
Placed inside a `<style>` tag within the HTML `<head>`. Useful for single-page sites.
```html
<head>
  <style>
    h1 { color: blue; }
  </style>
</head>
```

### External CSS
Linked via a separate `.css` file. Best practice for performance and maintainability.
```html
<link rel="stylesheet" href="style.css">
```

### Browser Default CSS (User Agent Stylesheet)
Every browser applies basic default styles to HTML elements (e.g., `<h1>` is large and bold, `<ul>` has padding and bullets).

### CSS Priority (Inline vs Internal vs External)
Priority from highest to lowest:
1. Inline CSS
2. Internal CSS / External CSS (whichever is linked *last* in the HTML `<head>`)
3. Browser Default CSS

## 4. CSS Syntax

A CSS rule consists of a selector and a declaration block.

- **Selector:** Points to the HTML element you want to style.
- **Declaration Block:** Contains one or more declarations separated by semicolons.
- **Declaration:** Contains a property and a value, separated by a colon.
- **Property:** The style attribute you want to change (e.g., `color`).
- **Value:** The specific setting for the property (e.g., `red`).

### Example:
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


## 5. CSS Selectors

Selectors define which HTML elements the CSS rules apply to.

### Basic Selectors
* **Universal selector `*`:** Selects all elements.
* **Element selector:** Selects elements based on tag name (e.g., `p`, `h1`).
* **Class selector:** Selects elements with a specific class. Starts with a dot (`.btn`).
* **ID selector:** Selects a specific element with an ID. Starts with a hash (`#header`).
* **Group selector:** Selects multiple elements to apply the same style (e.g., `h1, h2, p`).

### Combinator Selectors
* **Descendant selector (space):** `div p` (selects all `<p>` inside a `<div>`).
* **Child selector `>`:** `div > p` (selects only direct `<p>` children of `<div>`).
* **Adjacent sibling selector `+`:** `h1 + p` (selects the `<p>` immediately following an `<h1>`).
* **General sibling selector `~`:** `h1 ~ p` (selects all `<p>` elements that are siblings after an `<h1>`).

### Attribute Selectors
* `[type]`: Selects elements with the `type` attribute.
* `[type="text"]`: Exact match.
* `[href^="https"]`: Starts with "https".
* `[href$=".pdf"]`: Ends with ".pdf".
* `[class*="icon-"]`: Contains the string "icon-".

### Pseudo-classes
Target the *state* of an element.
* `:hover`: When the mouse pointer is over the element.
* `:focus`: When an element (like an input) receives focus.
* `:active`: When an element is being clicked.
* `:visited`: A link the user has already visited.
* `:first-child` / `:last-child`: Selects the first/last element among its siblings.
* `:nth-child(n)`: Selects the nth child (e.g., `:nth-child(2)` or `:nth-child(even)`).
* `:nth-of-type(n)`: Similar, but only counts specific elements.
* `:not(selector)`: Selects everything *except* the matched selector.
* `:checked`: Selects checked radio buttons or checkboxes.
* `:disabled` / `:enabled`: Targets form element states.
* `:required` / `:optional`: Targets input requirements.
* `:root`: Selects the highest-level parent of the document (usually `<html>`).

### Pseudo-elements
Target specific *parts* of an element.
* `::before`: Inserts content *before* the element's actual content.
* `::after`: Inserts content *after* the element's actual content.
* `::first-letter`: Styles the first letter of text.
* `::first-line`: Styles the first line of text.
* `::selection`: Styles the portion of text highlighted by the user.
* `::placeholder`: Styles the placeholder text of an input.
* `::marker`: Styles the list item marker (bullet/number).


## 6. CSS Specificity and Cascade

### What is the Cascade?
Cascade is the algorithm determining which CSS rule applies when multiple rules target the same element. It considers Importance, Specificity, and Source Order.

### What is Specificity?
Specificity is a scoring system used by browsers to decide which rule is most relevant.

**Specificity Hierarchy (Highest to Lowest):**
1. Inline styles (`style="..."`) -> Score: 1000
2. IDs (`#header`) -> Score: 100
3. Classes, pseudo-classes, attributes (`.btn`, `:hover`) -> Score: 10
4. Elements and pseudo-elements (`p`, `::before`) -> Score: 1

### Specificity Table

| Selector | Example | Specificity Score |
| :--- | :--- | :--- |
| Element | `p` | 0, 0, 0, 1 |
| Class | `.text` | 0, 0, 1, 0 |
| ID | `#main` | 0, 1, 0, 0 |
| Inline Style | `style="color: red;"`| 1, 0, 0, 0 |

### `!important`
Appending `!important` to a property overrides all specificity rules. **Avoid using it** unless absolutely necessary (like overriding external library styles).

### Source Order
If two rules have the exact same specificity, the one declared *last* in the CSS file wins.

### Inheritance
Some properties (like `color` and `font-family`) are inherited by child elements from their parents. Layout properties (like `margin` and `border`) are not inherited.

```css
/* Specificity Example */
#title {
  color: red; /* Score: 100 - This wins */
}

.title {
  color: blue; /* Score: 10 */
}
```


## 7. CSS Units

Units define sizes for widths, margins, fonts, etc.

### Absolute Units
Always the same size. Best for print, avoid for responsive web layouts.
* `px` (Pixels): 1px = 1/96th of 1 inch. (Most common absolute unit).
* `cm` (Centimeters), `mm` (Millimeters), `in` (Inches).
* `pt` (Points): 1pt = 1/72 of 1 inch.
* `pc` (Picas): 1pc = 12 pt.

### Relative Units
Size relative to another length property. Essential for responsive design.
* `%`: Relative to the parent element.
* `em`: Relative to the font-size of the element itself (or its parent).
* `rem`: Relative to the font-size of the root element (`<html>`). **Best for typography.**
* `vw`: 1% of the viewport width.
* `vh`: 1% of the viewport height.
* `vmin` / `vmax`: 1% of the viewport's smaller/larger dimension.
* `ch`: Relative to the width of the "0" (zero) character.
* `ex`: Relative to the x-height of the current font.

| Unit | When to use |
| :--- | :--- |
| `px` | Fine-tuning small details (borders, small gaps). |
| `rem` | Font sizes, global spacing. Ensures accessibility scaling. |
| `%` | Layout widths relative to containers. |
| `vh` / `vw` | Full-screen sections (Hero banners). |


## 8. CSS Colors

CSS provides multiple ways to define colors.

* **Named colors:** Standard color names (e.g., `red`, `blue`, `tomato`).
* **HEX:** Hexadecimal code (e.g., `#ff0000` is red).
* **RGB:** Red, Green, Blue from 0-255 (e.g., `rgb(255, 0, 0)`).
* **RGBA:** RGB with Alpha for opacity (0 to 1) (e.g., `rgba(255, 0, 0, 0.5)`).
* **HSL:** Hue (0-360), Saturation (%), Lightness (%) (e.g., `hsl(0, 100%, 50%)`).
* **HSLA:** HSL with Alpha transparency.
* **CurrentColor:** Takes the value of the current `color` property.
* **Transparent:** Fully transparent color.
* **Modern Functions:** `color-mix()`, `oklch()`, `oklab()` offer advanced perceptual color manipulation.

```css
.box {
  color: red;
  background-color: #ff0000;
  border-color: rgb(255, 0, 0);
  box-shadow: 0 4px rgba(255, 0, 0, 0.5);
}
```


## 9. CSS Text Properties

Used to format text.

* `color`: Text color.
* `text-align`: Aligns text (`left`, `right`, `center`, `justify`).
* `text-decoration`: Underline, line-through, none.
* `text-transform`: `uppercase`, `lowercase`, `capitalize`.
* `text-indent`: Indents the first line of a paragraph.
* `letter-spacing`: Space between characters.
* `word-spacing`: Space between words.
* `line-height`: Vertical space between lines of text.
* `white-space`: Controls how whitespace/line breaks are handled (`nowrap`, `pre-wrap`).
* `text-shadow`: Adds shadow to text (`h-shadow v-shadow blur color`).
* `direction` / `unicode-bidi`: Sets text direction (LTR vs RTL).
* `vertical-align`: Vertical alignment of inline elements.
* `text-overflow`: Handles overflowing text (e.g., `ellipsis`...).
* `overflow-wrap` / `word-break`: Controls word breaking to prevent overflow.

```css
p {
  text-align: center;
  text-transform: uppercase;
  letter-spacing: 2px;
  line-height: 1.6;
}
```

---

## 10. CSS Font Properties

* `font-family`: The typeface. Always provide fallbacks (e.g., `Arial, Helvetica, sans-serif`).
* `font-size`: The size of the font (use `rem` for accessibility).
* `font-weight`: Boldness (`normal`, `bold`, `100` - `900`).
* `font-style`: `normal`, `italic`, `oblique`.
* `font-variant`: e.g., `small-caps`.
* `font-stretch`: Condenses or expands fonts.
* `font`: Shorthand property for all font properties.
* `@font-face`: Rule to load custom web fonts from a server.

**Web Safe Fonts:** Fonts universally installed across devices (Arial, Times New Roman, Verdana).
**Google Fonts Usage:** Link the font in HTML, then use `font-family: 'FontName', sans-serif;`.

```css
body {
  font-family: 'Open Sans', Arial, sans-serif;
  font-size: 1rem;
  font-weight: 400;
}
```


## 11. CSS Box Model

Every element in CSS is a rectangular box. The Box Model dictates how space is calculated.

1. **Content:** The text or image itself.
2. **Padding:** Transparent space *inside* the border, around the content.
3. **Border:** A line surrounding the padding.
4. **Margin:** Transparent space *outside* the border, pushing elements apart.

### `box-sizing`
By default (`content-box`), padding and border *increase* the total width of an element.
Using `box-sizing: border-box;` forces padding and border to be included *within* the specified width. (Highly recommended globally).

### Box Model Diagram
```text
+-------------------------------------------+
|                 Margin                    |
|  +-------------------------------------+  |
|  |              Border                 |  |
|  |  +-------------------------------+  |  |
|  |  |           Padding             |  |  |
|  |  |  +-------------------------+  |  |  |
|  |  |  |                         |  |  |  |
|  |  |  |       CONTENT           |  |  |  |
|  |  |  |                         |  |  |  |
|  |  |  +-------------------------+  |  |  |
|  |  +-------------------------------+  |  |
|  +-------------------------------------+  |
+-------------------------------------------+
```

```css
.card {
  width: 300px;
  padding: 20px;
  border: 2px solid black;
  margin: 30px;
  box-sizing: border-box; /* Width stays exactly 300px */
}
```


## 12. Width, Height, Min, Max

* `width` / `height`: Explicit size dimensions.
* `min-width` / `min-height`: Prevents the element from getting smaller than this value.
* `max-width` / `max-height`: Prevents the element from getting larger than this value (crucial for responsive images: `max-width: 100%`).
* `auto`: Default. Browser calculates size based on content and layout.
* `fit-content`: Wraps tightly around the content inside.
* `max-content`: Expands to the width of the longest unbroken string.
* `min-content`: Shrinks to the width of the longest word.


## 13. Border Properties

* `border`: Shorthand (`width style color`).
* `border-width`: e.g., `2px`.
* `border-style`: `solid`, `dashed`, `dotted`, `none`.
* `border-color`: e.g., `black`.
* `border-radius`: Rounds the corners. `50%` creates a circle (on a square element).
* Individual sides: `border-top`, `border-right`, `border-bottom`, `border-left`.
* `outline`: Similar to border, but draws *outside* the element dimensions and doesn't affect layout space.
* `outline-offset`: Space between border and outline.


## 14. Margin and Padding

* **Margin:** Outer spacing. Pushes other elements away.
* **Padding:** Inner spacing. Pushes content inward.
* **Shorthand:**
  - `margin: 10px;` (All 4 sides)
  - `margin: 10px 20px;` (Top/Bottom 10px, Left/Right 20px)
  - `margin: 10px 20px 30px;` (Top 10px, L/R 20px, Bottom 30px)
  - `margin: 10px 20px 30px 40px;` (Top, Right, Bottom, Left - Clockwise)
* **Auto margin:** `margin: 0 auto;` horizontally centers block-level elements.
* **Margin Collapse:** Vertical margins of adjacent block elements sometimes overlap (collapse) into a single margin equal to the largest of the two sizes.


## 15. Background Properties

* `background-color`: Solid color.
* `background-image`: URL to an image (`url('image.jpg')`).
* `background-repeat`: `repeat`, `no-repeat`, `repeat-x`, `repeat-y`.
* `background-position`: `center center`, `top left`, etc.
* `background-size`: `cover` (fills container), `contain` (shows whole image).
* `background-attachment`: `scroll` (default), `fixed` (parallax effect).
* `background-clip` / `origin`: Controls background painting area relative to padding/border.
* `background`: Shorthand property.
* **Gradients:**
  - `linear-gradient(to right, red, blue)`
  - `radial-gradient(circle, red, blue)`
  - `conic-gradient(red, yellow, green)`

```css
.hero {
  background: linear-gradient(to right, #111, #444), url('bg.jpg');
  background-size: cover;
  background-position: center;
}
```

---

## 16. Display Property

Controls the rendering box type of an element.

* `block`: Starts on a new line, takes 100% width. (e.g., `<div>`, `<h1>`).
* `inline`: Does not start a new line, takes only required width. Width/height margins cannot be set manually. (e.g., `<span>`, `<a>`).
* `inline-block`: Inline behavior (side-by-side) but allows setting width, height, and margins.
* `none`: Completely removes the element from the document layout.
* `flex`: Enables Flexbox layout for children.
* `grid`: Enables Grid layout for children.
* `inline-flex` / `inline-grid`: Flex/Grid container that acts as an inline element.
* `table`: Behaves like `<table>`.
* `contents`: Makes the element conceptually disappear, pushing its children up to the parent.

---

## 17. Position Property

Changes how elements are positioned in the document.

* `static` (Default): Normal document flow. `top`, `left`, etc., have no effect.
* `relative`: Normal flow, but can be nudged using `top`, `bottom`, `left`, `right`. It remains in the space it originally occupied.
* `absolute`: Removed from normal flow. Positioned relative to the *closest positioned parent* (a parent with anything other than `static`).
* `fixed`: Removed from normal flow. Positioned relative to the viewport (window). Stays in place during scrolling.
* `sticky`: Toggles between `relative` and `fixed` depending on scroll position.
* `z-index`: Controls the Z-axis (stacking order). Higher numbers are in front. Only works on positioned elements.

```css
.header {
  position: sticky;
  top: 0;
  z-index: 100;
}
```


## 18. Float and Clear

*Historically used for layouts, now mostly used for wrapping text around images.*

* `float: left` / `float: right`: Pushes element to a side, letting text wrap around it.
* `clear`: Specifies what sides of an element cannot have floating elements (`clear: both`).
* **Clearfix:** A CSS hack applied to a parent container to force it to wrap around its floating children.
* **Why it's old for layout:** Floating was never meant for page structures, leading to collapsing parent bugs. Modern layouts should use **Flexbox** or **Grid**.


## 19. Overflow

Controls what happens when content is too big for its container.

* `overflow`: Shorthand for X and Y axes.
* `overflow-x` / `overflow-y`: Individual axis control.
* `visible` (Default): Content spills out.
* `hidden`: Content is clipped, rest is invisible.
* `scroll`: Always shows scrollbars, clipped content.
* `auto`: Shows scrollbars *only* if necessary.
* `clip`: Clips content precisely at the border-box (no scrolling allowed).


## 20. CSS Flexbox

A 1-dimensional layout model for arranging items in rows OR columns.

* **Flex container:** The parent element (`display: flex`).
* **Flex items:** The direct children.
* **Main axis:** The primary direction of flow (defined by `flex-direction`).
* **Cross axis:** Perpendicular to the main axis.

### Container Properties:
* `display: flex;`
* `flex-direction`: `row` (default), `column`, `row-reverse`, `column-reverse`.
* `flex-wrap`: `nowrap` (default), `wrap` (wrap to next line), `wrap-reverse`.
* `flex-flow`: Shorthand for direction + wrap.
* `justify-content`: Aligns items on the **Main Axis** (`flex-start`, `center`, `space-between`, `space-around`).
* `align-items`: Aligns items on the **Cross Axis** (`stretch`, `center`, `flex-start`).
* `align-content`: Aligns wrapped *lines* on the Cross Axis.
* `gap`, `row-gap`, `column-gap`: Spacing between items.

### Item Properties:
* `order`: Changes the visual sequence of items.
* `flex-grow`: How much an item grows to fill available space (number).
* `flex-shrink`: How much an item shrinks when space is tight.
* `flex-basis`: The initial main size of the item.
* `flex`: Shorthand for grow, shrink, and basis (e.g., `flex: 1`).
* `align-self`: Overrides the container's `align-items` for a specific item.

### Examples:
```css
/* Center a div perfectly */
.center-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

/* Navbar */
.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```


## 21. CSS Grid

A 2-dimensional layout model for arranging items in both rows AND columns simultaneously.

* **Grid container:** The parent (`display: grid`).
* **Grid items:** Direct children.
* **Grid lines:** The dividing lines that make up the grid.
* **Grid tracks:** The space between two grid lines (a row or a column).
* **Grid areas:** A rectangular space spanning one or more cells.

### Properties:
* `display: grid;`
* `grid-template-columns`: Defines column widths (e.g., `100px 1fr 2fr`).
* `grid-template-rows`: Defines row heights.
* `grid-template-areas`: Defines layout via named areas.
* `grid-column` / `grid-row`: Spans items across tracks (e.g., `grid-column: 1 / 3`).
* `gap`: Space between rows and columns.
* `justify-items` / `align-items`: Aligns items within their cells.
* `place-items`: Shorthand for align/justify items.
* `justify-content` / `align-content`: Aligns the entire grid within the container.
* `place-content`: Shorthand for align/justify content.

### Grid Specific Functions:
* `fr` unit: Represents a fraction of the available space.
* `repeat(count, size)`: E.g., `repeat(3, 1fr)` creates 3 equal columns.
* `minmax(min, max)`: Sets size constraints.
* `auto-fit` / `auto-fill`: Automatically creates as many tracks as will fit.

### Example: Responsive Image Gallery
```css
.gallery {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}
```


## 22. CSS Responsive Design

Designing web pages that look good on all devices.

* **Mobile-first design:** Writing CSS for mobile devices first, then using media queries to add styles for larger screens.
* **Breakpoints:** Specific widths where the layout needs to change.
* **Fluid layout:** Using `%`, `vw`, `vh`, and `fr` units instead of fixed `px`.
* **Responsive images:** `img { max-width: 100%; height: auto; }`.
* **Media queries:** Allow conditional CSS rules based on screen size.
* **Container Queries:** (Modern) Apply styles based on the size of a parent *container*, rather than the viewport (`@container`).

### Media Query Example:
```css
/* Mobile styles default */
.container { flex-direction: column; }

/* Tablet and larger */
@media (min-width: 768px) {
  .container {
    flex-direction: row;
  }
}
```


## 23. CSS Transforms

Modifies the coordinate space of the CSS visual formatting model.

* `transform`: The property to apply transformations.
* `translate(x, y)`: Moves the element.
* `rotate(deg)`: Rotates the element (e.g., `45deg`).
* `scale(x, y)`: Scales the element size (1 is normal, 2 is double).
* `skew(x-deg, y-deg)`: Tilts the element.
* `transform-origin`: Changes the point around which a transform is applied (default is center).
* `perspective`: Gives a 3D depth effect.
* **2D vs 3D:** Transforms can operate on X/Y axes (2D) or X/Y/Z axes (3D).


## 24. CSS Transitions

Allows changes in property values to occur smoothly over a specified duration.

* `transition-property`: Which property to animate (e.g., `background-color`, or `all`).
* `transition-duration`: How long it takes (`0.3s`).
* `transition-timing-function`: Speed curve (`ease`, `linear`, `ease-in-out`, `cubic-bezier()`).
* `transition-delay`: Wait time before starting.
* `transition`: Shorthand (`property duration timing-function delay`).

### Example:
```css
.button {
  background-color: blue;
  transition: transform 0.2s ease, background-color 0.2s;
}
.button:hover {
  background-color: darkblue;
  transform: translateY(-3px); /* Button lifts up slightly */
}
```


## 25. CSS Animations

For complex, multi-step animations (more powerful than transitions).

* `@keyframes`: Defines the animation sequence using percentages (`0%` to `100%`) or keywords (`from`, `to`).
* `animation-name`: Links to the `@keyframes` name.
* `animation-duration`: How long one cycle takes.
* `animation-timing-function`: Speed curve.
* `animation-delay`: Delay before start.
* `animation-iteration-count`: Number of times to play (e.g., `infinite`).
* `animation-direction`: `normal`, `reverse`, `alternate` (back and forth).
* `animation-fill-mode`: What styles to apply before/after playing (`forwards` keeps final state).
* `animation-play-state`: `running` or `paused`.
* `animation`: Shorthand property.

### Example: Fade In and Slide Up
```css
@keyframes slideUpFade {
  0% { opacity: 0; transform: translateY(20px); }
  100% { opacity: 1; transform: translateY(0); }
}

.card {
  animation: slideUpFade 0.5s ease-out forwards;
}
```


## 26. CSS Variables (Custom Properties)

CSS Variables allow storing specific values for reuse throughout a document.

* **What are they?** Entities defined by CSS authors that contain specific values to be reused throughout a document.
* **`:root`:** The highest-level pseudo-class. Defining variables here makes them globally available.
* **Syntax:** Variables start with `--`.
* **Fallback:** Provide a fallback value in case the variable is missing: `var(--my-color, red)`.
* **Theme System:** Incredible for Dark/Light mode switching.

### Example:
```css
:root {
  --primary-color: #2563eb;
  --spacing: 16px;
}

.button {
  background-color: var(--primary-color);
  padding: var(--spacing);
}
```


## 27. CSS Functions

CSS offers built-in functions to calculate values dynamically.

* `var(--name)`: Inserts the value of a custom property.
* `calc()`: Performs math operations (e.g., `calc(100% - 50px)`).
* `min(a, b)`: Uses the smallest of comma-separated values.
* `max(a, b)`: Uses the largest of comma-separated values.
* `clamp(min, ideal, max)`: Scales a value between a min and max bound (amazing for fluid typography).
* `rgb()` / `rgba()` / `hsl()` / `hsla()`: Color functions.
* `linear-gradient()` / `radial-gradient()`: Generates gradient images.
* `repeat()`: Repeats grid tracks.
* `minmax()`: Sets grid track size limits.
* `url('path')`: Links an external resource.
* `attr(attribute)`: Retrieves the value of an attribute of the selected element.


## 28. CSS Filters and Effects

Modifies visual rendering of images and backgrounds.

* `filter: blur(5px)`: Blurs element.
* `brightness(150%)`: Adjusts brightness.
* `contrast(200%)`: Adjusts contrast.
* `grayscale(100%)`: Makes element black and white.
* `hue-rotate(90deg)`: Shifts color hues.
* `invert(100%)`: Inverts colors.
* `opacity(50%)`: Transparency (0 to 1).
* `saturate(200%)`: Increases color intensity.
* `sepia(100%)`: Adds vintage yellow-brown tone.
* `backdrop-filter`: Applies filters to the area *behind* an element (e.g., frosted glass effect).
* `box-shadow`: Adds shadow to boxes.
* `text-shadow`: Adds shadow to text.


## 29. CSS Object Fit and Object Position

Useful for `<img>` and `<video>` tags to maintain aspect ratios without distortion.

* `object-fit: fill`: Stretches to fit (default, distorts).
* `object-fit: contain`: Scales to fit without cropping (leaves empty space).
* `object-fit: cover`: Scales to fill entirely, cropping edges if necessary (highly recommended for hero images).
* `object-fit: none`: No scaling applied.
* `object-fit: scale-down`: Smaller of `none` or `contain`.
* `object-position`: Controls X/Y alignment of the image within its box (e.g., `center top`).


## 30. CSS Lists

Styling `<ul>` and `<ol>`.

* `list-style-type`: Changes the bullet (`disc`, `circle`, `square`, `decimal`, `none`).
* `list-style-position`: `inside` or `outside` the list item padding.
* `list-style-image`: Uses an image as a custom bullet.
* `list-style`: Shorthand for type, position, image.


## 31. CSS Tables

Styling HTML `<table>`.

* `border-collapse: collapse;`: Merges double borders into single borders.
* `border-spacing`: Distance between borders (if `separate`).
* `caption-side`: Places caption `top` or `bottom`.
* `empty-cells`: `show` or `hide` empty cells.
* `table-layout: fixed;`: Forces table to respect set widths, rendering faster.


## 32. CSS Forms Styling

Forms require specific targeting.

* `input`, `textarea`, `select`, `button`: Style padding, borders, typography.
* `::placeholder`: Style the placeholder text.
* `:focus`: Crucial for accessibility (e.g., `outline: 2px solid blue;`).
* `:disabled`: Style greyed-out inactive inputs.
* `:valid` / `:invalid`: Style inputs based on HTML5 validation.
* `input[type="checkbox"]` / `radio`: Often hidden and replaced with custom styled labels using `::before`/`::after`.


## 33. CSS Pseudo-class Practical Usage

* **Hover button:** `.btn:hover { background: darker; transform: scale(1.05); }`
* **Focus input:** `input:focus { border-color: blue; box-shadow: 0 0 5px blue; }`
* **Checked checkbox:** `input:checked + label { color: green; }`
* **Zebra table rows:** `tr:nth-child(even) { background-color: #f4f4f4; }`
* **Disabled button:** `button:disabled { opacity: 0.5; cursor: not-allowed; }`
* **Active nav link:** `.nav-link.active { font-weight: bold; border-bottom: 2px solid; }`


## 34. CSS Layout Techniques

Evolution of layouts:
* **Normal flow:** Default block stacking and inline wrapping.
* **Box model layout:** Manual margins and paddings.
* **Float layout (Legacy):** Using `float` and `clearfix` for side-by-side structures.
* **Flexbox layout:** Ideal for 1D alignments (Navbars, aligning icons, single rows/columns).
* **Grid layout:** Ideal for 2D page architecture (Dashboards, image galleries, complex pages).
* **Absolute positioning:** Overlapping elements, modals, dropdown menus.
* **Sticky header:** `position: sticky; top: 0;` keeps headers visible.
* **Card layout:** Usually Grid or Flexbox container with styled inner elements.


## 35. CSS Architecture

Keeping CSS maintainable in large projects.

* **Naming Classes:** Use lowercase, hyphenated names (`header-title`).
* **BEM (Block Element Modifier):** A methodology naming convention `.block__element--modifier` (e.g., `.card__title--large`).
* **Utility Classes:** Small, single-purpose classes (`.text-center`, `.mt-4`).
* **Component-based CSS:** Grouping styles by UI component rather than page.
* **Global CSS:** Resets, typography, variables.
* **Scoped CSS:** CSS isolated to a specific component (common in React/Vue).
* **CSS Modules:** Automatically generates unique class names to prevent style leakage.
* **Tailwind CSS:** A popular utility-first CSS framework.
* **SCSS/Sass:** CSS preprocessors that add logic, nesting, and mixins.


## 36. CSS Performance

* **Avoid too many expensive animations:** Animate `transform` and `opacity`. Animating `width`, `height`, or `margin` causes expensive browser reflows.
* **Avoid unnecessary selectors:** Deeply nested selectors (`div ul li a span`) are slower than direct classes (`.nav-link`).
* **Minify CSS:** Remove whitespace and comments for production.
* **Remove unused CSS:** Tools like PurgeCSS help.
* **Use external stylesheets:** Allows browser caching.
* **Avoid inline styles:** Difficult to maintain and override.
* **Use responsive images:** Prevents loading massive images on mobile.
* **Reduce layout shift:** Pre-define `width` and `height` or `aspect-ratio` on images.


## 37. CSS Accessibility

* **Focus states:** NEVER set `outline: none;` without providing a custom visual focus state.
* **Color contrast:** Ensure text has a high contrast ratio against backgrounds (WCAG standard).
* **Readable font size:** Base font size should be at least `16px`.
* **Reduced motion:** Respect user OS settings for minimal animation.

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation: none !important;
    transition: none !important;
    scroll-behavior: auto !important;
  }
}
```


## 38. Modern CSS Features

* **CSS variables:** `--var-name`.
* **Math functions:** `clamp()`, `min()`, `max()`.
* **`aspect-ratio`:** Easily set 16:9, 1:1 ratios without hacky padding.
* **`gap` in Flexbox:** Flexbox now supports `gap` like Grid.
* **`scroll-behavior: smooth;`:** Smooth scrolling to anchor links.
* **`scroll-snap`:** For creating native carousel/snap scrolling experiences.
* **`accent-color`:** Easily theme native checkboxes and radio buttons.
* **Container queries (`@container`):** Style based on parent size, not viewport size.
* **Nesting:** Native CSS nesting (similar to Sass) is now supported in modern browsers.
* **Cascade Layers (`@layer`):** Explicitly control specificity hierarchy.
* **`:is()` and `:where()`:** Groups selectors cleanly.
* **`:has()`:** The parent selector (styles a parent based on its children).


## 39. Common Beginner Mistakes

* **Using inline CSS everywhere:** Hard to maintain. (Use external files).
* **Not understanding Box Model:** Setting width 100% + padding causes horizontal scrolling. (Use `box-sizing: border-box`).
* **Confusing margin and padding:** Margin is outside (pushes away), padding is inside (creates inner space).
* **Using fixed width (`px`) everywhere:** Breaks on mobile. (Use `%`, `vw`, or `max-width`).
* **Not writing responsive CSS:** Forgetting media queries.
* **Overusing absolute position:** Layout becomes rigid and breaks easily. (Use Flex/Grid).
* **Not using semantic class names:** E.g., `.red-text` instead of `.error-message`.
* **Ignoring browser default styles:** Not resetting margins/paddings.
* **Forgetting units:** `margin: 10;` is invalid. Needs `px`, `rem`, etc. (Except `0`).
* **Using `!important` too much:** Ruins the cascade.
* **Not handling mobile screens:** Always check on smaller viewports.


## 40. CSS Best Practices

* Use external CSS files.
* Keep CSS organized (Reset -> Variables -> Typography -> Layout -> Components).
* Use meaningful class names.
* Follow Mobile-First approach.
* Use CSS variables for themes and repeated values.
* Avoid unnecessary repetition (DRY).
* Use Flexbox for 1D layout and Grid for 2D layout.
* Keep selectors simple and shallow.
* Write reusable classes.
* Test responsiveness on real devices.
* Maintain accessibility standards.
* Avoid deprecated properties (like `float` for layout).

---

## 41. Real Practical Examples

### Simple Profile Card
```html
<div class="card">
  <img src="avatar.jpg" alt="Profile" class="card-img">
  <h2 class="card-name">John Doe</h2>
  <p class="card-title">Frontend Developer</p>
  <button class="card-btn">Follow</button>
</div>
```
```css
.card {
  max-width: 300px;
  margin: 20px auto;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  text-align: center;
  font-family: sans-serif;
}
.card-img {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
}
.card-btn {
  background: #2563eb;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s;
}
.card-btn:hover {
  background: #1d4ed8;
}
```

### Responsive Navbar
```html
<nav class="navbar">
  <div class="logo">MySite</div>
  <ul class="nav-links">
    <li><a href="#">Home</a></li>
    <li><a href="#">About</a></li>
  </ul>
</nav>
```
```css
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #333;
  padding: 1rem 2rem;
}
.logo { color: white; font-size: 1.5rem; font-weight: bold; }
.nav-links {
  display: flex;
  gap: 20px;
  list-style: none;
}
.nav-links a {
  color: white;
  text-decoration: none;
}

@media (max-width: 600px) {
  .navbar { flex-direction: column; gap: 10px; }
}
```

---

## 42. CSS Cheat Sheet

| Category | Common Properties |
| :--- | :--- |
| **Box Model** | `width`, `height`, `margin`, `padding`, `border`, `box-sizing` |
| **Typography** | `font-family`, `font-size`, `font-weight`, `color`, `text-align`, `line-height` |
| **Flexbox** | `display: flex`, `flex-direction`, `justify-content`, `align-items`, `gap`, `flex-wrap` |
| **Grid** | `display: grid`, `grid-template-columns`, `gap`, `place-items`, `grid-column` |
| **Position** | `position` (`relative`, `absolute`, `fixed`, `sticky`), `top`, `z-index` |
| **Backgrounds**| `background-color`, `background-image`, `background-size: cover` |
| **Animations** | `transition`, `transform`, `animation`, `@keyframes` |
| **Units** | `px`, `rem`, `em`, `%`, `vh`, `vw`, `fr` |
| **Colors** | HEX (`#fff`), RGB (`rgb(0,0,0)`), Custom Prop (`var(--color)`) |
