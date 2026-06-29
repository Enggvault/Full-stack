# Comprehensive HTML Reference Guide

> **HTML** (HyperText Markup Language) is the standard **markup language** for creating web pages. It defines the **structure and content** of a webpage.


## Table of Contents
1. [HTML Version History](#1-html-version-history)
2. [HTML5 Boilerplate](#2-the-html5-boilerplate)
3. [HTML Tags](#3-html-tags)
4. [HTML Elements](#4-html-elements)
5. [HTML Attributes](#5-html-attributes)
6. [HTML Forms](#6-html-forms)
7. [Input Types & Validation](#7-input-types--validation)
8. [HTML5 Features](#8-html5-features)
9. [Semantic HTML](#9-semantic-html)
10. [Tables](#10-tables)
11. [Lists](#11-lists)
12. [Media](#12-media)
13. [HTML Entities](#13-html-entities)
14. [Accessibility (a11y)](#14-accessibility-a11y)
15. [SEO in HTML](#15-seo-in-html)
16. [Best Practices](#16-best-practices)
17. [Deprecated Tags](#17-deprecated-html-tags)
18. [Common Beginner Mistakes](#18-common-beginner-mistakes)
19. [HTML Cheat Sheet](#19-html-cheat-sheet)


## 1. HTML Version History

| Version | Year | Key Features |
|---------|------|--------------|
| **HTML 1.0** | 1993 | Basic text formatting and links |
| **HTML 2.0** | 1995 | Introduced standard form elements |
| **HTML 3.2** | 1997 | Added tables, applets, text flow around images |
| **HTML 4.01** | 1999 | Standardized CSS separation, multimedia |
| **XHTML** | 2000 | HTML as strict XML (case-sensitive, strict closing tags) |
| **HTML5** | 2014–Present | Semantic tags, native media, local storage, `<canvas>` |

> **HTML5** is the current **living standard** — continuously updated and the one you should learn.


## 2. The HTML5 Boilerplate

Every valid HTML5 document follows this structural foundation:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Document description here">
    <title>Document Title</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <!-- Page content goes here -->
</body>
</html>
```

### Line-by-Line Explanation

| Tag | Purpose |
|-----|---------|
| `<!DOCTYPE html>` | Tells browser this is an **HTML5** document (standards mode) |
| `<html lang="en">` | Root element; `lang` helps screen readers and SEO |
| `<head>` | Container for **meta-information** (not visible on page) |
| `<meta charset="UTF-8">` | Character encoding — **UTF-8** covers almost all world characters |
| `<meta name="viewport" ...>` | Makes page **responsive** on mobile devices |
| `<title>` | Shows in browser tab — **crucial for SEO** |
| `<link>` | Links external resources, mainly **CSS stylesheets** |
| `<body>` | All **visible content** of the web page |

---

## 3. HTML Tags

Tags are the building blocks of HTML. They usually come in **pairs** (start + end tag), wrapping content.

### Document Tags
- `<html>`, `<head>`, `<body>` — Define the core document structure.

### Metadata Tags
- `<meta>` — Structured metadata about the web page.
- `<title>` — The document's title (shown in browser tab).
- `<link>` — Links external resources.
- `<base>` — Base URL for relative links.

### Text Formatting Tags
- `<b>` — **Bold** text (presentational). Use `<strong>` for semantic importance.
- `<i>` — *Italic* text (presentational). Use `<em>` for semantic emphasis.
- `<u>` — Underline text.
- `<s>` — Strikethrough text.
- `<sub>` — Subscript (e.g., H₂O).
- `<sup>` — Superscript (e.g., E = mc²).
- `<mark>` — Highlights text (yellow background). Used for search result highlights.
- `<small>` — Smaller text (often for copyright).

### Heading Tags
- `<h1>` to `<h6>` — Section headings. `<h1>` = most important, `<h6>` = least.
- **Best Practice:** Use only **one `<h1>` per page**. Maintain strict hierarchy.

### Paragraph Tags
- `<p>` — Paragraph of text.
- `<br>` — Line break (empty element).
- `<hr>` — Thematic break / horizontal rule.
- `<pre>` — Preformatted text. Preserves whitespace exactly as written.
- `<blockquote>` — Block of text quoted from another source.

### Anchor Tag
- `<a href="url">Link Text</a>` — Creates a hyperlink.
- Key attributes: `href`, `target="_blank"` (opens in new tab), `rel="noopener"`

### Image Tag
- `<img src="image.jpg" alt="Description">` — Embeds an image (empty/void element).
- **Always use `alt` attribute** for accessibility and SEO.

### Audio & Video Tags (HTML5)
- `<audio controls>` — Embeds sound content.
- `<video controls poster="thumbnail.jpg">` — Embeds video content.
- `<source>` — Specifies multiple media resources.
- `<track>` — Specifies text tracks (subtitles, captions).

### List Tags
- `<ul>` — Unordered list (bullet points).
- `<ol>` — Ordered list (numbered).
- `<li>` — List item (used inside `<ul>` or `<ol>`).
- `<dl>`, `<dt>`, `<dd>` — Description list, term, and detail.

### Table Tags
- `<table>`, `<tr>`, `<th>`, `<td>` — Table, row, header cell, data cell.
- `<thead>`, `<tbody>`, `<tfoot>` — Semantic grouping of table rows.
- `<caption>` — Table caption.

### Form Tags
- `<form action="/submit" method="POST">` — Container for user input.
- `<label for="id">` — Caption for an input (critical for accessibility).
- `<input>`, `<textarea>`, `<select>`, `<option>`, `<button>`.
- `<fieldset>` + `<legend>` — Groups related form elements.
- `<datalist>` — Pre-defined options for an `<input>` (autocomplete).

### Layout / Semantic Tags (HTML5)
- `<header>` — Introductory content or navigational links.
- `<nav>` — Navigation links.
- `<main>` — The dominant content of the page (only **one** per page).
- `<section>` — A generic section of a document.
- `<article>` — Independent, self-contained content (e.g., a blog post).
- `<aside>` — Content tangentially related to the main content (sidebar).
- `<footer>` — Footer for its nearest sectioning content.

### Inline vs Block Elements

```
BLOCK elements:         Start on a new line, take up full width.
  <div>, <p>, <h1>-<h6>, <ul>, <ol>, <section>, <article>

INLINE elements:        Do not start new line, take only needed width.
  <span>, <a>, <strong>, <em>, <img>, <input>, <button>

INLINE-BLOCK:           Inline behavior but allows width/height.
  <img>, <input>
```

### Embedded Content Tags
- `<iframe>` — Embeds another HTML page.
- `<embed>` — Embeds external content (non-HTML).
- `<svg>` — Scalable Vector Graphics directly in HTML.
- `<canvas>` — Draws graphics via JavaScript.

### Interactive Tags
- `<details>` + `<summary>` — Native accordion/expandable widget.
- `<dialog>` — A dialog box or modal window.

### Deprecated Tags (Never Use)
`<font>`, `<center>`, `<strike>`, `<marquee>` — Use CSS instead.


## 4. HTML Elements

### What is an HTML Element?
An HTML element is everything from the **start tag** to the **end tag**, including the content.

```
  Start Tag    Content       End Tag
  ---------  -----------    -------
    <p>       Hello World!    </p>
```

### Empty Elements (Void Elements)
Some elements have **no content** and **no end tag**:
- `<br>`, `<hr>`, `<img>`, `<input>`, `<meta>`, `<link>`

### DOM Tree — Element Relationships

```
<html>
 ├── <head>          (Child of html, Sibling of body)
 │    └── <title>    (Child of head)
 └── <body>          (Child of html, Sibling of head)
      ├── <h1>       (Child of body)
      └── <div>      (Child of body)
           └── <p>   (Child of div, Descendant of body)
```

| Term | Meaning |
|------|---------|
| **Parent** | Element containing another element (`<div>` is parent of `<p>`) |
| **Child** | Element inside a parent (`<p>` is child of `<div>`) |
| **Sibling** | Elements that share the same parent |
| **Descendant** | Any element nested within another, no matter how deep |

---

## 5. HTML Attributes

Attributes provide **additional information** about an element. Always specified in the **start tag** as `name="value"` pairs.

### Global Attributes (usable on ANY element)

| Attribute | Description |
|-----------|-------------|
| `id` | Unique identifier — for CSS targeting, JS, and fragment links |
| `class` | One or more class names — mainly for CSS styling and JavaScript |
| `style` | Applies inline CSS styles |
| `title` | Extra info shown as tooltip on hover |
| `hidden` | Hides the element |
| `tabindex` | Controls keyboard tab order |
| `contenteditable` | Makes element's content editable by the user |
| `lang` | Specifies language of element's content |
| `data-*` | Custom private data: `data-user-id="123"` |
| `aria-*` | **ARIA attributes** — improves screen reader accessibility |

### Tag-Specific Attributes

| Attribute | Used In | Description |
|-----------|---------|-------------|
| `href` | `<a>`, `<link>` | URL of the linked resource |
| `src` | `<img>`, `<script>`, `<iframe>` | Path to embedded media/script |
| `alt` | `<img>` | Alternate text if image can't display |
| `disabled` | `<input>`, `<button>` | Disables the element |
| `action`, `method` | `<form>` | Where and how to send form data |
| `placeholder` | `<input>` | Hint text shown when input is empty |
| `required` | `<input>` | Makes field mandatory |

---

## 6. HTML Forms

Forms are used to **collect user input** and send it to a server for processing.

```html
<form action="/submit" method="POST">
    <fieldset>
        <legend>Personal Information</legend>

        <label for="fname">First Name:</label>
        <input type="text" id="fname" name="fname" required>

        <label for="country">Country:</label>
        <select id="country" name="country">
            <optgroup label="Asia">
                <option value="in">India</option>
                <option value="jp">Japan</option>
            </optgroup>
        </select>
    </fieldset>

    <button type="submit">Submit</button>
</form>
```

### Key Form Elements

| Element | Description |
|---------|-------------|
| `<form action method>` | Outer wrapper. `GET` appends data to URL; `POST` sends in body (safer) |
| `<label for="id">` | Links label to input — clicking label focuses input |
| `<fieldset>` + `<legend>` | Groups related inputs with a visible border |
| `<datalist>` | Provides autocomplete suggestions for `<input>` |
| `<output>` | Represents the result of a calculation |
| `<progress>` | Shows completion progress |

---

## 7. Input Types & Validation

### Common Input Types

| Type | Description |
|------|-------------|
| `text` | Default single-line text field |
| `password` | Masks characters for passwords |
| `email` | Validates email format automatically |
| `number` | Accepts only numbers |
| `tel` | Telephone number (numeric keypad on mobile) |
| `url` | Validates absolute URL format |
| `date` | Calendar date picker |
| `time` | Time picker |
| `file` | Allows selecting files to upload |
| `radio` | Select **one** from a group (same `name`) |
| `checkbox` | Select **multiple** from a group |
| `range` | Slider control for a number within a range |
| `hidden` | Stores data invisible to user, sent on submission |
| `color` | Color picker widget |
| `search` | Styled for search queries |
| `submit` | Submits the form |

### Validation Attributes (HTML5 Built-in)

| Attribute | Description |
|-----------|-------------|
| `required` | Field must be filled before submitting |
| `readonly` | Value shown but not editable (still submitted) |
| `disabled` | Element unusable and **not submitted** |
| `maxlength` / `minlength` | Max/min number of characters |
| `pattern` | Regular Expression the value must match |
| `placeholder` | Hint text for expected value |
| `min` / `max` | Min/max numerical or date value |
| `step` | Legal number intervals |
| `multiple` | Allow multiple values (for `email` or `file`) |
| `autofocus` | Auto-focuses the field when page loads |


## 8. HTML5 Features

HTML5 revolutionized web development with powerful **native APIs** and **semantic elements**.

| Feature | Description |
|---------|-------------|
| **Semantic HTML** | Meaningful tags: `<header>`, `<nav>`, `<article>` etc. |
| **Audio & Video** | Native media embedding without plugins (`<audio>`, `<video>`) |
| **Canvas** | 2D drawing API via JavaScript (`<canvas>`) |
| **SVG** | Scalable Vector Graphics directly in the DOM |
| **Drag and Drop API** | Native drag-and-drop support |
| **Local Storage** | Stores data with **no expiration** (persists after browser close) |
| **Session Storage** | Stores data for one session (cleared when tab closes) |
| **Web Workers** | Runs JavaScript in background thread (doesn't block UI) |
| **Geolocation API** | Gets user's physical location (requires permission) |
| **History API** | Manipulates browser session history (enables SPAs) |
| **Responsive Images** | `srcset` attribute and `<picture>` element for different screen sizes |
| **Native Form Validation** | Built-in validation without JavaScript |


## 9. Semantic HTML

**Semantic HTML** means using tags that carry **meaning about the content** they enclose, not just how it looks.

### Why It Matters

1. **Accessibility (a11y):** Screen readers use semantic tags to navigate effectively.
2. **SEO:** Search engines prioritize content inside `<article>`, `<h1>`, `<main>`.
3. **Maintainability:** Easier for developers to read and maintain code.

### Semantic Elements

| Element | Description |
|---------|-------------|
| `<header>` | Introductory content — logo, nav, heading |
| `<nav>` | Navigation links section |
| `<main>` | The dominant, unique content. **Only one per page** |
| `<section>` | Thematic grouping of content, typically with a heading |
| `<article>` | Independent, self-contained content (blog post, news article) |
| `<aside>` | Tangentially related content — sidebar, pull quotes |
| `<footer>` | Footer — copyright, author info, secondary links |
| `<figure>` | Self-contained content referenced from the main flow |
| `<figcaption>` | Caption for a `<figure>` |
| `<time>` | Represents a specific period in time |
| `<mark>` | Highlighted text for reference (search term highlights) |
| `<details>` + `<summary>` | Native accordion — expandable/collapsible widget |

### Example Semantic Layout

```html
<body>
    <header>
        <h1>My Tech Blog</h1>
        <nav>
            <ul>
                <li><a href="/">Home</a></li>
                <li><a href="/about">About</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <article>
            <header>
                <h2>Understanding Semantic HTML</h2>
                <p>Published on <time datetime="2023-10-27">October 27, 2023</time></p>
            </header>
            <p>Semantic HTML is vital for the modern web...</p>
        </article>
    </main>

    <aside>
        <h3>Related Links</h3>
        <!-- Sidebar content -->
    </aside>

    <footer>
        <p>&copy; 2023 My Tech Blog</p>
    </footer>
</body>
```


## 10. Tables

HTML tables display **tabular data** (rows and columns). **Never use tables for page layout!**

### Table Structure

```html
<table>
    <caption>Employee Directory</caption>
    <thead>
        <tr>
            <th>Name</th>
            <th>Department</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Jane Doe</td>
            <td>Engineering</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td colspan="2">Total: 1 Employee</td>
        </tr>
    </tfoot>
</table>
```

### Key Table Elements

| Element | Description |
|---------|-------------|
| `<table>` | Container for the table |
| `<tr>` | Table Row |
| `<th>` | Table Header cell — **bold, centered** by default |
| `<td>` | Table Data cell |
| `<thead>` | Groups header rows |
| `<tbody>` | Groups body rows |
| `<tfoot>` | Groups footer rows (totals, summaries) |
| `<caption>` | Title or explanation for the table |
| `colspan` | Makes a cell span multiple columns |
| `rowspan` | Makes a cell span multiple rows |


## 11. Lists

### Unordered Lists (`<ul>`) — Order doesn't matter

```html
<ul>
    <li>Apples</li>
    <li>Bananas</li>
</ul>
```

### Ordered Lists (`<ol>`) — Order matters

```html
<ol>
    <li>Preheat oven.</li>
    <li>Mix ingredients.</li>
    <li>Bake for 30 mins.</li>
</ol>
```

### Description Lists (`<dl>`) — Terms and definitions

```html
<dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language</dd>
    <dt>CSS</dt>
    <dd>Cascading Style Sheets</dd>
</dl>
```

### Nested Lists

```html
<ul>
    <li>Tea
        <ul>
            <li>Green Tea</li>
            <li>Black Tea</li>
        </ul>
    </li>
</ul>
```


## 12. Media

### Images

```html
<img src="path/to/image.jpg" alt="A beautiful landscape" width="500" loading="lazy">
```
> `loading="lazy"` — Defers loading until close to viewport. **Improves performance.**

### Responsive Images (`<picture>`)

```html
<picture>
    <source srcset="image.webp" type="image/webp">
    <source media="(max-width: 799px)" srcset="image-mobile.jpg">
    <source media="(min-width: 800px)" srcset="image-desktop.jpg">
    <img src="image-fallback.jpg" alt="Responsive image">
</picture>
```

### Audio

```html
<audio controls>
    <source src="music.ogg" type="audio/ogg">
    <source src="music.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
</audio>
```
*Attributes: `controls`, `autoplay`, `loop`, `muted`*

### Video

```html
<video width="320" controls poster="thumbnail.jpg">
    <source src="movie.mp4" type="video/mp4">
    Your browser does not support the video tag.
</video>
```
*Attributes: `controls`, `autoplay`, `loop`, `muted`, `poster`*

### iFrames

```html
<iframe src="https://www.youtube.com/embed/dQw4w9WgXcQ"
        width="560" height="315" title="YouTube video">
</iframe>
```


## 13. HTML Entities

Some characters are **reserved in HTML** (e.g., `<`, `>`, `&`). Use **character entities** to display them.

> Format: Start with `&`, end with `;`

| Character | Entity Name | Entity Number | Description |
|:---------:|-------------|---------------|-------------|
| `<` | `&lt;` | `&#60;` | Less than |
| `>` | `&gt;` | `&#62;` | Greater than |
| `&` | `&amp;` | `&#38;` | Ampersand |
| `"` | `&quot;` | `&#34;` | Double quote |
| `'` | `&apos;` | `&#39;` | Single quote |
| (space) | `&nbsp;` | `&#160;` | Non-breaking space |
| `©` | `&copy;` | `&#169;` | Copyright |
| `®` | `&reg;` | `&#174;` | Registered trademark |
| `€` | `&euro;` | `&#8364;` | Euro |

> **`&nbsp;`** is heavily used to prevent browsers from collapsing multiple spaces.


## 14. Accessibility (a11y)

Web accessibility ensures websites are usable by people with **disabilities** (visual, auditory, motor, cognitive).

### Key Practices

| Practice | Description |
|----------|-------------|
| **`alt` text** | Every `<img>` MUST have `alt`. Decorative images use `alt=""` |
| **Semantic HTML** | Use `<button>` for actions, `<a>` for navigation — not `<div>` with onclick |
| **Form Labels** | Always associate `<label>` with its `<input>` using `for` and `id` |
| **Keyboard Navigation** | Entire site must be navigable via `Tab` key |
| **Focus Indicators** | Never remove `outline: none` without providing an alternative |
| **ARIA Attributes** | `aria-label`, `aria-hidden`, `aria-expanded` for custom components |

### Common ARIA Attributes

| Attribute | Description |
|-----------|-------------|
| `aria-label` | Label for elements without visible text (e.g., icon-only button) |
| `aria-hidden="true"` | Hides element from screen readers |
| `aria-expanded="true/false"` | Indicates if a collapsible section is open or closed |
| `role="button"` | Defines the role of an element for assistive technology |


## 15. SEO in HTML

**SEO** (Search Engine Optimization) makes your site more visible on search engines.

### Crucial SEO Elements

| Element | Best Practice |
|---------|---------------|
| **`<title>`** | Unique per page, descriptive, **50–60 characters** |
| **Meta description** | `<meta name="description" content="...">` — shows in search results |
| **Heading hierarchy** | One `<h1>` per page, then `<h2>`, `<h3>` for sections |
| **Semantic tags** | Wrap core content in `<main>` and `<article>` |
| **Image optimization** | Descriptive filenames, `alt` tags, `loading="lazy"` |
| **Canonical tag** | `<link rel="canonical" href="https://example.com/page">` — avoids duplicate content |
| **Open Graph (OG) Tags** | Used by social media to generate preview cards |

```html
<meta property="og:title" content="My Page Title">
<meta property="og:image" content="https://example.com/image.jpg">
<meta name="robots" content="index, follow">
```

---

## 16. Best Practices

| Practice | Rule |
|----------|------|
| **Use lowercase** | `<p>` not `<P>` — industry standard |
| **Quote attributes** | Always use double quotes: `class="container"` |
| **Proper indentation** | Indent nested elements (2 or 4 spaces) |
| **Meaningful IDs/Classes** | Name by **purpose**, not appearance: `class="error-message"` not `class="red-text"` |
| **Close all tags** | Prevents unexpected rendering issues |
| **Validate code** | Use W3C Markup Validation Service |
| **Separate structure from style** | Avoid inline styles — use external CSS |

---

## 17. Deprecated HTML Tags

These tags are **obsolete in HTML5** and should **never** be used. Use CSS instead.

| Deprecated Tag | What it did | Modern Replacement |
|:---|:---|:---|
| `<font>` | Changed font size/color | CSS `font-family`, `color` |
| `<center>` | Centered content | CSS `text-align: center` or Flexbox |
| `<strike>` | Strikethrough text | `<del>`, `<s>`, or CSS `text-decoration: line-through` |
| `<marquee>` | Scrolling text | CSS Animations |
| `<frame>` / `<frameset>` | Page layouts with frames | `<iframe>` or CSS layout |
| `<acronym>` | Defined an acronym | `<abbr>` |
| `<applet>` | Embedded Java applets | `<object>` or `<embed>` |


## 18. Common Beginner Mistakes

### ❌ Mistake 1: Using `<div>` instead of Semantic Tags ("Div Soup")
```html
<!-- BAD -->
<div class="header"> <div class="nav"> <div class="footer">

<!-- GOOD -->
<header>  <nav>  <footer>
```

### ❌ Mistake 2: Using `<br>` for Spacing
```html
<!-- BAD -->
<br><br><br>

<!-- GOOD -->
Use CSS: margin-bottom: 20px;
```

### ❌ Mistake 3: Forgetting `alt` on Images
```html
<!-- BAD -->
<img src="logo.png">

<!-- GOOD -->
<img src="logo.png" alt="Company Logo">
```

### ❌ Mistake 4: Skipping Heading Levels
```html
<!-- BAD -->
<h1>Title</h1>  <h4>Subtitle</h4>   ← skipped h2 and h3

<!-- GOOD -->
<h1>Title</h1>  <h2>Section</h2>  <h3>Sub-section</h3>
```

### ❌ Mistake 5: Using `<a>` without `href` as a Button
```html
<!-- BAD -->
<a onclick="doSomething()">Click Me</a>

<!-- GOOD -->
<button type="button" onclick="doSomething()">Click Me</button>
```

### ❌ Mistake 6: Unclosed Tags
```html
<!-- BAD -->
<div><p>Hello</div>

<!-- GOOD -->
<div><p>Hello</p></div>
```


## 19. HTML Cheat Sheet

### Most Used Tags
```
Structure:  <html>, <head>, <body>, <title>, <meta>, <link>, <script>
Headings:   <h1> to <h6>
Text:       <p>, <br>, <hr>, <strong>, <em>, <span>
Lists:      <ul>, <ol>, <li>
Links:      <a href="url">
Images:     <img src="" alt="">
Containers: <div> (block), <span> (inline)
Table:      <table>, <tr>, <th>, <td>
Form:       <form>, <input>, <button>, <label>, <select>, <textarea>
```

### Semantic Tags
```
<header>  <nav>  <main>  <section>  <article>  <aside>  <footer>
<figure>  <figcaption>  <time>  <mark>  <details>  <summary>
```

### Common Input Types
```
text  email  password  number  date  file  checkbox  radio  submit  range
```

### Important Attributes
```
Global:  class, id, style, hidden, data-*, aria-*, lang
Links:   href, target="_blank", rel="noopener"
Images:  src, alt, loading="lazy", width, height
Forms:   action, method, name, value, placeholder, required, disabled
```

### Common HTML Entities
```
Non-breaking space: &nbsp;
Less than:          &lt;
Greater than:       &gt;
Ampersand:          &amp;
Copyright:          &copy;
Registered:         &reg;
```
