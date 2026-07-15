# HTML Reference

> **Module 02** · Prerequisites: [Full Stack Fundamentals ←](../01-full-stack-fundamentals/notes.md) · Next: [CSS →](../03-css/notes.md)

---

## Table of Contents

1. [What is HTML?](#1-what-is-html)
2. [The HTML5 Boilerplate](#2-the-html5-boilerplate)
3. [HTML Tags](#3-html-tags)
4. [HTML Elements](#4-html-elements)
5. [HTML Attributes](#5-html-attributes)
6. [Semantic HTML](#6-semantic-html)
7. [HTML Forms](#7-html-forms)
8. [Input Types & Native Validation](#8-input-types--native-validation)
9. [Tables](#9-tables)
10. [Lists](#10-lists)
11. [Media](#11-media)
12. [HTML Entities](#12-html-entities)
13. [HTML5 Browser APIs](#13-html5-browser-apis)
14. [Accessibility](#14-accessibility)
15. [SEO in HTML](#15-seo-in-html)
16. [Best Practices](#16-best-practices)
17. [Deprecated Tags](#17-deprecated-tags)
18. [Common Mistakes](#18-common-mistakes)

---

## 1. What is HTML?

**HTML (HyperText Markup Language)** is the standard markup language for documents displayed in a web browser. It defines the **structure and meaning** of content — headings, paragraphs, images, links, forms. HTML is not a programming language; it does not have logic or state.

```
HTML        →  Structure and content (what exists, and what it means)
CSS         →  Presentation (how it looks)
JavaScript  →  Behavior (what it does)
```

### Version History

| Version | Year | Key Features |
|:--------|:-----|:-------------|
| HTML 1.0 | 1993 | Basic text and links |
| HTML 2.0 | 1995 | Standard form elements |
| HTML 4.01 | 1999 | CSS separation, multimedia |
| XHTML | 2000 | HTML as strict XML |
| **HTML5** | 2014 – Present | Semantic elements, native media, storage APIs, canvas |

> HTML5 is the current **living standard**, maintained by WHATWG. It is what this module covers.

---

## 2. The HTML5 Boilerplate

Every valid HTML5 document follows this minimum structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="A brief description of the page content.">
    <title>Page Title — Site Name</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <!-- Visible page content -->
</body>
</html>
```

**Line-by-line breakdown:**

| Declaration | Purpose |
|:------------|:--------|
| `<!DOCTYPE html>` | Instructs the browser to parse the document in HTML5 standards mode |
| `<html lang="en">` | Root element. `lang` assists screen readers and search engines |
| `<meta charset="UTF-8">` | Character encoding — UTF-8 covers all languages and special characters |
| `<meta name="viewport" ...>` | Enables responsive scaling on mobile devices |
| `<title>` | Displayed in the browser tab and used by search engines as the page title |
| `<link rel="stylesheet">` | Links an external CSS file |
| `<body>` | All visible page content belongs here |

---

## 3. HTML Tags

### Document Structure

| Tag | Purpose |
|:----|:--------|
| `<html>` | Root element of the document |
| `<head>` | Container for metadata — not rendered visually |
| `<body>` | All visible content |

### Metadata

| Tag | Purpose |
|:----|:--------|
| `<title>` | Page title (browser tab, search results) |
| `<meta>` | Structured metadata (charset, viewport, description, OG tags) |
| `<link>` | External resource links (CSS, favicons, canonical URLs) |
| `<base>` | Sets the base URL for all relative links |

### Text Content

| Tag | Purpose |
|:----|:--------|
| `<h1>` – `<h6>` | Section headings. One `<h1>` per page; maintain strict hierarchy. |
| `<p>` | Paragraph |
| `<br>` | Line break (void element — no closing tag) |
| `<hr>` | Thematic break / horizontal rule |
| `<pre>` | Preformatted text — preserves whitespace exactly |
| `<blockquote>` | Extended quotation from another source |
| `<strong>` | Semantically important text (rendered bold by default) |
| `<em>` | Semantically emphasized text (rendered italic) |
| `<b>` | Bold — presentational only, no semantic weight |
| `<i>` | Italic — presentational only |
| `<mark>` | Highlighted text (e.g., search match) |
| `<small>` | Fine print, legal text, copyright |
| `<sub>` / `<sup>` | Subscript / Superscript |
| `<del>` / `<ins>` | Deleted / Inserted text (useful in diffs) |
| `<code>` | Inline code fragment |
| `<kbd>` | Keyboard input |
| `<abbr title="...">` | Abbreviation with expansion on hover |

### Links

```html
<!-- External link, opens in new tab, no referrer leak -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
    External Link
</a>

<!-- Internal page link -->
<a href="/about">About</a>

<!-- Fragment link (jumps to element with id="section-2") -->
<a href="#section-2">Jump to Section 2</a>
```

### Images

```html
<!-- Standard image -->
<img src="profile.jpg" alt="Profile photo of Jane Doe" width="200" height="200" loading="lazy">

<!-- Responsive image with multiple sources -->
<picture>
    <source srcset="image.webp" type="image/webp">
    <source media="(max-width: 600px)" srcset="image-mobile.jpg">
    <img src="image-fallback.jpg" alt="Descriptive alt text">
</picture>
```

> `loading="lazy"` defers image loading until the image is near the viewport, improving initial page load.

### Layout / Semantic Tags

Covered in detail in [Section 6 — Semantic HTML](#6-semantic-html).

### Interactive Tags

| Tag | Purpose |
|:----|:--------|
| `<details>` + `<summary>` | Native expand/collapse accordion without JavaScript |
| `<dialog>` | Native modal dialog element |

### Embedded Content

| Tag | Purpose |
|:----|:--------|
| `<iframe>` | Embeds another HTML document |
| `<canvas>` | 2D/3D drawing surface via JavaScript |
| `<svg>` | Scalable Vector Graphics inline in the document |

### Inline vs Block Elements

```
Block elements:    Start on a new line. Occupy the full container width.
  <div>, <p>, <h1>–<h6>, <ul>, <ol>, <section>, <article>, <header>, <footer>

Inline elements:   Flow within text. Occupy only their content width.
  <span>, <a>, <strong>, <em>, <img>, <input>, <button>, <code>
```

---

## 4. HTML Elements

An **element** is the complete unit: opening tag + content + closing tag.

```
  Opening tag    Content       Closing tag
  ──────────    ───────────   ───────────
      <p>        Hello world       </p>
```

**Void elements** have no content and no closing tag:

```html
<br>  <hr>  <img>  <input>  <meta>  <link>
```

### DOM Relationships

```html
<html>
  <head>               ← Child of <html>, sibling of <body>
    <title>Page</title> ← Child of <head>
  </head>
  <body>               ← Child of <html>
    <main>             ← Child of <body>
      <p>Text</p>      ← Child of <main>, descendant of <body>
    </main>
  </body>
</html>
```

| Term | Definition |
|:-----|:-----------|
| **Parent** | The element directly wrapping another (`<main>` is parent of `<p>`) |
| **Child** | An element directly inside a parent |
| **Sibling** | Elements sharing the same direct parent |
| **Descendant** | Any element nested inside another at any depth |

---

## 5. HTML Attributes

Attributes provide additional configuration for elements. They are placed in the opening tag as `name="value"` pairs.

### Global Attributes (valid on any element)

| Attribute | Description |
|:----------|:------------|
| `id` | Unique identifier — used by CSS (`#id`), JavaScript, and anchor links |
| `class` | One or more class names for CSS and JavaScript targeting |
| `style` | Inline CSS — avoid in large projects |
| `hidden` | Removes the element from rendering |
| `tabindex` | Controls keyboard tab order |
| `contenteditable` | Makes the element's content editable |
| `lang` | Specifies the language of the element's content |
| `data-*` | Custom data attributes: `data-user-id="42"` |
| `aria-*` | Accessibility attributes for assistive technologies |
| `title` | Tooltip text shown on hover |

### Element-Specific Attributes

| Attribute | Elements | Description |
|:----------|:---------|:------------|
| `href` | `<a>`, `<link>` | Target URL |
| `src` | `<img>`, `<script>`, `<iframe>` | Path to embedded resource |
| `alt` | `<img>` | Alternative text when image cannot display |
| `action`, `method` | `<form>` | Where and how form data is submitted |
| `placeholder` | `<input>` | Hint text when the field is empty |
| `required` | `<input>` | Makes the field mandatory |
| `disabled` | `<input>`, `<button>` | Disables the element; value is not submitted |
| `readonly` | `<input>` | Value is shown but not editable; value is submitted |
| `name` | `<input>`, `<select>` | Key used when form data is submitted |

---

## 6. Semantic HTML

**Semantic HTML** means using tags that describe the meaning and role of their content, not merely its visual appearance.

### Why Semantics Matter

1. **Accessibility:** Screen readers use semantic tags to navigate. A user navigating by headings or landmarks relies on correct markup.
2. **SEO:** Search engines weight content in `<main>`, `<h1>`, and `<article>` elements differently from generic `<div>` containers.
3. **Maintainability:** Code is self-documenting. `<nav>` is unambiguous; `<div class="nav">` requires reading the CSS.

### Semantic Layout Elements

| Element | Role |
|:--------|:-----|
| `<header>` | Introductory content for a page or section — typically contains logo, primary `<nav>`, and site title |
| `<nav>` | Primary or secondary navigation links |
| `<main>` | The dominant, unique content of the page. **One per page.** |
| `<section>` | A thematic grouping of content — typically has its own heading |
| `<article>` | Independent, self-contained content (blog post, news article, comment) |
| `<aside>` | Content tangentially related to the main content — sidebars, pull quotes |
| `<footer>` | Footer for a page or section — copyright, contact links |
| `<figure>` | Self-contained content referenced from the main flow (image + caption) |
| `<figcaption>` | Caption for a `<figure>` |
| `<time datetime="2026-07-15">` | A date or time — `datetime` provides machine-readable format |
| `<address>` | Contact information for the nearest `<article>` or `<body>` |

### Example Semantic Layout

```html
<body>
    <header>
        <h1>My Tech Blog</h1>
        <nav aria-label="Main navigation">
            <ul>
                <li><a href="/">Home</a></li>
                <li><a href="/about">About</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <article>
            <header>
                <h2>Understanding HTTP Caching</h2>
                <p>Published <time datetime="2026-07-15">July 15, 2026</time></p>
            </header>
            <p>Article content here...</p>
        </article>
    </main>

    <aside>
        <h3>Related Articles</h3>
        <!-- sidebar content -->
    </aside>

    <footer>
        <p>&copy; 2026 My Tech Blog</p>
    </footer>
</body>
```

> **Rule:** If a `<div>` has a class name like `header`, `nav`, `main`, `footer`, or `article` — replace it with the corresponding semantic element.

---

## 7. HTML Forms

Forms collect user input and submit it to a server.

```html
<form action="/api/users" method="POST">
    <fieldset>
        <legend>Create Account</legend>

        <label for="email">Email address</label>
        <input type="email" id="email" name="email" required autocomplete="email">

        <label for="password">Password</label>
        <input type="password" id="password" name="password"
               required minlength="8" autocomplete="new-password">

        <label for="role">Role</label>
        <select id="role" name="role">
            <option value="viewer">Viewer</option>
            <option value="editor">Editor</option>
        </select>
    </fieldset>

    <button type="submit">Create account</button>
</form>
```

### Form Elements

| Element | Purpose |
|:--------|:--------|
| `<form action method>` | Container. `action` is the endpoint; `method` is `GET` or `POST` |
| `<label for="id">` | Associates a text label with an input by matching `for` to `id`. Clicking the label focuses the input. |
| `<fieldset>` + `<legend>` | Groups related inputs with a visible border and label |
| `<input>` | The primary data entry element |
| `<textarea>` | Multi-line text input |
| `<select>` + `<option>` | Dropdown list |
| `<datalist>` | Provides autocomplete suggestions for a text `<input>` |
| `<button type="submit">` | Submits the form |
| `<button type="reset">` | Resets all fields to default values |
| `<button type="button">` | No default behavior — controlled entirely by JavaScript |

### Form Methods

| Method | Behavior |
|:-------|:---------|
| `GET` | Appends form data to the URL as query parameters (`/search?q=term`). Use for read-only operations like search. |
| `POST` | Sends form data in the request body. Use for creating or modifying data. |

> Never use `GET` for sensitive data (passwords, personal information) — query parameters are visible in server logs and browser history.

---

## 8. Input Types & Native Validation

### Input Types

| Type | Description |
|:-----|:------------|
| `text` | Single-line text |
| `email` | Validates email format |
| `password` | Masks input characters |
| `number` | Numeric input with optional `min`, `max`, `step` |
| `tel` | Telephone number (numeric keypad on mobile) |
| `url` | Validates absolute URL format |
| `date` | Date picker |
| `time` | Time picker |
| `datetime-local` | Date and time combined |
| `file` | File selector — use `accept` to restrict file types |
| `checkbox` | Boolean toggle |
| `radio` | Single selection from a group (same `name`) |
| `range` | Slider between `min` and `max` |
| `color` | Color picker |
| `hidden` | Not visible; value submitted with the form |
| `search` | Semantically styled for search queries |

### Validation Attributes

| Attribute | Description |
|:----------|:------------|
| `required` | Field must not be empty |
| `minlength` / `maxlength` | Character count constraints |
| `min` / `max` | Numeric or date value range |
| `pattern` | Regular expression the value must match |
| `step` | Legal intervals for numeric inputs |
| `multiple` | Allows multiple values (for `email` and `file`) |
| `autofocus` | Focuses this field when the page loads |

```html
<!-- Password: minimum 8 chars, at least one digit -->
<input type="password" name="password"
       required minlength="8"
       pattern="(?=.*\d).{8,}"
       title="At least 8 characters, including one digit">
```

---

## 9. Tables

Tables represent **tabular data** — information that has a natural row-and-column structure.

> Never use tables for page layout. Use CSS Flexbox or Grid instead (→ [03 — CSS](../03-css/notes.md#flexbox)).

```html
<table>
    <caption>Q3 Revenue by Region</caption>
    <thead>
        <tr>
            <th scope="col">Region</th>
            <th scope="col">Revenue</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>North America</td>
            <td>$4.2M</td>
        </tr>
        <tr>
            <td>Europe</td>
            <td>$2.8M</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>Total</td>
            <td>$7.0M</td>
        </tr>
    </tfoot>
</table>
```

| Element | Description |
|:--------|:------------|
| `<caption>` | Accessible title for the table |
| `<thead>` | Groups header rows |
| `<tbody>` | Groups body rows |
| `<tfoot>` | Groups footer rows (summaries, totals) |
| `<th scope="col/row">` | Header cell — `scope` identifies what the header describes |
| `<td>` | Data cell |
| `colspan` | Makes a cell span multiple columns |
| `rowspan` | Makes a cell span multiple rows |

---

## 10. Lists

### Unordered List — order is not meaningful

```html
<ul>
    <li>Install Node.js</li>
    <li>Create a new project</li>
</ul>
```

### Ordered List — order is meaningful

```html
<ol>
    <li>Run <code>npm install</code></li>
    <li>Set environment variables</li>
    <li>Run <code>npm run dev</code></li>
</ol>
```

### Description List — term and definition pairs

```html
<dl>
    <dt>REST</dt>
    <dd>Representational State Transfer — an architectural style for APIs</dd>

    <dt>JSON</dt>
    <dd>JavaScript Object Notation — a lightweight data interchange format</dd>
</dl>
```

---

## 11. Media

### Audio

```html
<audio controls>
    <source src="audio.webm" type="audio/webm">
    <source src="audio.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
</audio>
```

*Attributes: `controls`, `autoplay`, `loop`, `muted`*

### Video

```html
<video controls poster="thumbnail.jpg" width="640">
    <source src="video.webm" type="video/webm">
    <source src="video.mp4" type="video/mp4">
    <track src="captions.vtt" kind="captions" srclang="en" label="English">
    Your browser does not support the video element.
</video>
```

> Always provide `<track>` elements for captions — required for accessibility compliance.

### Embedded Content

```html
<!-- YouTube embed -->
<iframe
    src="https://www.youtube.com/embed/VIDEO_ID"
    width="560" height="315"
    title="Video title for screen readers"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media"
    allowfullscreen>
</iframe>
```

---

## 12. HTML Entities

Reserved HTML characters must be escaped using entities to render correctly.

| Character | Entity Name | Number | Description |
|:---------:|:------------|:-------|:------------|
| `<` | `&lt;` | `&#60;` | Less than |
| `>` | `&gt;` | `&#62;` | Greater than |
| `&` | `&amp;` | `&#38;` | Ampersand |
| `"` | `&quot;` | `&#34;` | Double quote |
| `'` | `&apos;` | `&#39;` | Apostrophe |
| ` ` | `&nbsp;` | `&#160;` | Non-breaking space |
| `©` | `&copy;` | `&#169;` | Copyright |
| `®` | `&reg;` | `&#174;` | Registered trademark |

---

## 13. HTML5 Browser APIs

HTML5 introduced browser APIs that JavaScript can access. These are introduced here conceptually; their JavaScript implementations are covered in [04 — JavaScript](../04-javascript/notes.md).

| API | Description |
|:----|:------------|
| **Local Storage** | Persistent key-value storage with no expiry. JavaScript: `localStorage` |
| **Session Storage** | Key-value storage cleared when the tab closes. JavaScript: `sessionStorage` |
| **Canvas** | 2D/3D drawing surface accessed via JavaScript's `CanvasRenderingContext2D` |
| **Geolocation** | Access to device GPS (requires explicit user permission) |
| **Web Workers** | Run JavaScript in a background thread without blocking the UI |
| **History API** | Programmatically manipulate the browser history (enables SPAs without page reloads) |
| **Drag and Drop** | Native browser drag-and-drop event system |

---

## 14. Accessibility

Web accessibility (a11y) ensures that applications are usable by people with disabilities — visual, auditory, motor, and cognitive.

### Core Practices

| Practice | Requirement |
|:---------|:------------|
| **`alt` text** | Every `<img>` must have an `alt` attribute. Decorative images: `alt=""`. Informative images: describe the content. |
| **Semantic markup** | Use `<button>` for actions, `<a>` for navigation. Never use `<div>` or `<span>` with click handlers as interactive controls. |
| **Form labels** | Every `<input>` must have an associated `<label>` using `for`/`id`. |
| **Keyboard navigation** | Every interactive element must be reachable and operable via the `Tab` and `Enter` keys. |
| **Focus indicators** | Do not remove `outline` without providing an equivalent visible focus indicator. |
| **Color contrast** | Text must meet WCAG AA contrast ratio: 4.5:1 for normal text, 3:1 for large text. |
| **Heading hierarchy** | One `<h1>`, then `<h2>`, then `<h3>`. Do not skip levels. |
| **Language attribute** | Set `lang` on `<html>` so screen readers use the correct pronunciation rules. |

### ARIA Attributes

ARIA (Accessible Rich Internet Applications) supplements HTML semantics for custom widgets.

| Attribute | Usage |
|:----------|:------|
| `aria-label="..."` | Names an element when no visible text label exists (e.g., an icon-only button) |
| `aria-labelledby="id"` | Points to another element's `id` as the label |
| `aria-hidden="true"` | Removes an element from the accessibility tree (decorative icons) |
| `aria-expanded="true/false"` | Indicates whether a collapsible region is open |
| `aria-live="polite"` | Announces dynamic content updates to screen readers |
| `role="..."` | Overrides an element's implicit role (use sparingly — prefer semantic HTML) |

```html
<!-- Icon-only button — aria-label provides the accessible name -->
<button type="button" aria-label="Close dialog">
    <svg aria-hidden="true" focusable="false"><!-- icon --></svg>
</button>
```

---

## 15. SEO in HTML

Search Engine Optimization improves a page's visibility in search results.

| Element | Best Practice |
|:--------|:-------------|
| `<title>` | Unique per page. 50–60 characters. Format: `Page Name — Site Name` |
| `<meta name="description">` | 150–160 characters. Shown in search result snippets. |
| Heading hierarchy | One `<h1>` per page containing the primary keyword |
| Semantic elements | Wrap primary content in `<main>`, `<article>`, and correct heading levels |
| Image optimization | Descriptive filenames, meaningful `alt` text, `loading="lazy"`, explicit `width`/`height` |
| Canonical tag | `<link rel="canonical" href="https://example.com/page">` — prevents duplicate content penalties |

**Open Graph tags** — control how the page appears when shared on social media:

```html
<meta property="og:title" content="Page Title">
<meta property="og:description" content="Page description.">
<meta property="og:image" content="https://example.com/social-image.jpg">
<meta property="og:url" content="https://example.com/page">
<meta property="og:type" content="article">
```

---

## 16. Best Practices

| Practice | Rule |
|:---------|:-----|
| **Use lowercase** | `<p>` not `<P>` |
| **Quote all attribute values** | `class="container"` not `class=container` |
| **Close all tags** | Including optional closers — prevents parsing ambiguity |
| **Indent nested elements** | 2 or 4 spaces consistently |
| **Name by purpose** | `class="error-message"` not `class="red-text"` |
| **Separate structure from style** | Use external CSS. Never use `<font>`, `<center>`, or `style` attributes. |
| **Validate** | Use the W3C Markup Validation Service |

---

## 17. Deprecated Tags

These tags are obsolete in HTML5. Replace them with CSS.

| Deprecated | Replacement |
|:-----------|:------------|
| `<font>` | CSS `font-family`, `color`, `font-size` |
| `<center>` | CSS `text-align: center` or Flexbox |
| `<strike>` | `<del>` or CSS `text-decoration: line-through` |
| `<marquee>` | CSS animations |
| `<frame>` / `<frameset>` | `<iframe>` or CSS layout |
| `<acronym>` | `<abbr title="...">` |
| `<big>` / `<small>` (presentational) | CSS `font-size` |

---

## 18. Common Mistakes

### ❌ Div Soup — using `<div>` for everything

```html
<!-- Incorrect -->
<div class="header">
  <div class="nav"></div>
</div>
<div class="footer"></div>

<!-- Correct -->
<header>
  <nav></nav>
</header>
<footer></footer>
```

### ❌ Using `<br>` for spacing

```html
<!-- Incorrect -->
<p>First paragraph</p>
<br><br>
<p>Second paragraph</p>

<!-- Correct — use CSS margin -->
<p>First paragraph</p>
<p>Second paragraph</p>
```

### ❌ Missing `alt` on images

```html
<!-- Incorrect -->
<img src="logo.png">

<!-- Correct -->
<img src="logo.png" alt="EnggVault logo">
```

### ❌ Skipping heading levels

```html
<!-- Incorrect — skips h2 and h3 -->
<h1>Page Title</h1>
<h4>Subsection</h4>

<!-- Correct -->
<h1>Page Title</h1>
<h2>Section</h2>
<h3>Subsection</h3>
```

### ❌ Using `<a>` without `href` as a button

```html
<!-- Incorrect -->
<a onclick="doSomething()">Click me</a>

<!-- Correct -->
<button type="button" onclick="doSomething()">Click me</button>
```

### ❌ Missing `<label>` for inputs

```html
<!-- Incorrect -->
<input type="email" placeholder="Email">

<!-- Correct -->
<label for="email">Email</label>
<input type="email" id="email" name="email">
```

---

> **Next:** [03 — CSS →](../03-css/notes.md)
