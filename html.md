# Comprehensive HTML Reference Guide: Basic to Advanced

HTML (HyperText Markup Language) is the standard markup language for documents designed to be displayed in a web browser. 


## 1. HTML Version History
* **HTML 1.0 (1993):** The initial release, basic text formatting and links.
* **HTML 2.0 (1995):** Introduced standard form elements.
* **HTML 3.2 (1997):** Added tables, applets, and text flow around images.
* **HTML 4.01 (1999):** Standardized styling separation (CSS), added multimedia options.
* **XHTML (2000):** HTML written as strict XML (case-sensitive, strict closing tags).
* **HTML5 (2014 - Present):** The modern living standard. Introduced semantic tags, native multimedia (`<video>`, `<audio>`), local storage, and the `<canvas>` element for graphics.


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
    </body>
</html>
```

### Line-by-Line Explanation:
- `<!DOCTYPE html>`: Document Type Declaration. It tells the browser that this is an HTML5 document, ensuring it renders in standards mode.
- `<html>`: The root element that wraps all content on the entire page. The `lang` attribute helps screen readers and search engines.
- `<head>`: Contains meta-information about the document. Nothing in the `<head>` is displayed directly on the webpage (except the title).
- `<meta charset="UTF-8">`: Specifies the character encoding. UTF-8 covers almost all characters and symbols in the world.
- `<meta name="viewport" ...>`: Ensures the page is responsive and scales correctly on mobile devices.
- `<title>`: Defines the title of the document, shown in the browser's title bar or tab. Crucial for SEO.
- `<link>`: Links external resources, most commonly CSS stylesheets.
- `<style>`: Used to write internal CSS directly within the HTML document.
- `<script>`: Embeds or links executable client-side scripts (JavaScript).
- `<body>`: Contains all the visible content of the web page (text, images, links, etc.).
- `<noscript>`: Defines alternate content to display if the user has disabled JavaScript in their browser.
- `<base>`: (Not in example, but useful) Specifies the base URL/target for all relative URLs in a document. Goes in the `<head>`.

---

## 4. HTML Tags

Tags are the building blocks of HTML. They usually come in pairs (start tag and end tag), wrapping content.

### Document Tags
- `<html>`, `<head>`, `<body>`: Define the core document structure.

### Metadata Tags
- `<meta>`: Provides structured metadata about a web page.
- `<title>`: The document's title.
- `<link>`: Links external resources.
- `<base>`: Base URL for relative links.

### Text Formatting Tags
- `<b>`: Bold text (presentational, use `<strong>` for semantic importance).
- `<i>`: Italic text (presentational, use `<em>` for semantic emphasis).
- `<u>`: Underline text.
- `<s>`: Strikethrough text.
- `<sub>`: Subscript (e.g., H₂O).
- `<sup>`: Superscript (e.g., E = mc²).
- `<small>`: Smaller text (often used for copyright).

### Heading Tags
- `<h1>` to `<h6>`: Represent section headings. `<h1>` is the most important (main title), `<h6>` is the least.
- **Best Practice:** Use only one `<h1>` per page. Maintain strict hierarchy (don't skip from `<h1>` to `<h3>`).

### Paragraph Tags
- `<p>`: Defines a paragraph of text.
- `<br>`: Line break (empty element).
- `<hr>`: Thematic break / horizontal rule.
- `<pre>`: Preformatted text. Preserves whitespace and line breaks exactly as written in the code.
- `<blockquote>`: Indicates a block of text quoted from another source.

### Anchor Tags
- `<a>`: Creates a hyperlink to other web pages, files, email addresses, or locations within the same page.
- **Syntax:** `<a href="url">Link Text</a>`

### Image Tags
- `<img>`: Embeds an image. Empty element.
- **Syntax:** `<img src="image.jpg" alt="Description">`
- **Important:** Always use the `alt` attribute for accessibility and SEO.

### Audio & Video Tags (HTML5)
- `<audio>`: Embeds sound content.
- `<video>`: Embeds video content.
- `<source>`: Specifies multiple media resources for `<video>` and `<audio>`.
- `<track>`: Specifies text tracks (subtitles, captions).

### Figure Tags
- `<figure>`: Self-contained content, like illustrations, diagrams, or photos.
- `<figcaption>`: Defines a caption for a `<figure>` element.

### List Tags
- `<ul>`: Unordered list (bullet points).
- `<ol>`: Ordered list (numbered).
- `<li>`: List item (used inside `<ul>` or `<ol>`).
- `<dl>`: Description list.
- `<dt>`: Description term.
- `<dd>`: Description detail.

### Table Tags
- `<table>`: Defines a table.
- `<tr>`: Table row.
- `<th>`: Table header cell (bold, centered by default).
- `<td>`: Table data cell.
- `<thead>`, `<tbody>`, `<tfoot>`: Semantic grouping of table rows.
- `<caption>`: Table caption.

### Form Tags
- `<form>`: An interactive container for collecting user input.
- `<label>`: A caption for an input element (critical for accessibility).

### Input Tags
- `<input>`: A typed data field. Behaves differently based on its `type` attribute.
- `<textarea>`: A multi-line text input control.
- `<select>`: A drop-down list.
- `<option>`: An option within a `<select>` or `<datalist>`.
- `<optgroup>`: Groups related options in a `<select>`.

### Button Tags
- `<button>`: A clickable button. Can contain text, images, or icons.

### Layout / Semantic Tags (HTML5)
- `<header>`: Introductory content or navigational links.
- `<nav>`: Navigation links.
- `<main>`: The dominant content of the `<body>`.
- `<section>`: A generic section of a document.
- `<article>`: Independent, self-contained content (e.g., a blog post).
- `<aside>`: Content tangentially related to the content around it (sidebar).
- `<footer>`: The footer for its nearest sectioning content.

### Inline Elements
Elements that do not start on a new line and only take up as much width as necessary.
- `<span>`: Generic inline container.
- `<a>`, `<strong>`, `<em>`, `<img>`, `input`, `button`.

### Block Elements
Elements that start on a new line and take up the full width available.
- `<div>`: Generic block container.
- `<p>`, `<h1>`-`<h6>`, `<ul>`, `<ol>`, `<section>`, `<article>`.

### Embedded Content Tags
- `<iframe>`: Embeds another HTML page into the current page.
- `<embed>`: Embeds external content (often non-HTML).
- `<object>`: Embeds external resources (PDFs, Flash).
- `<param>`: Defines parameters for an `<object>`.

### SVG & Canvas Tags
- `<svg>`: Defines Scalable Vector Graphics directly in HTML.
- `<canvas>`: Used to draw graphics, on the fly, via JavaScript.

### Interactive Tags
- `<details>`: A disclosure widget from which the user can obtain additional information.
- `<summary>`: A visible heading for a `<details>` element.
- `<dialog>`: A dialog box or modal window.

### Template & Script Tags
- `<template>`: Holds HTML that is not rendered immediately but can be instantiated using JavaScript.
- `<script>`: Embeds JavaScript.

### Deprecated Tags
Tags no longer recommended for use in HTML5 (use CSS instead).
- `<font>`, `<center>`, `<strike>`, `<marquee>`, `<b>` (use strong/css), `<i>` (use em/css).


## 5. HTML Elements

### What is an HTML Element?
An HTML element is everything from the start tag to the end tag. It defines a specific part of a web page.
```html
<p>This is a paragraph element.</p>
```

### Anatomy of an Element

```text
  Start Tag       Content        End Tag
  ---------    --------------    -------
    <p>        Hello, World!       </p>
```

### Empty Elements (Self-closing)
Some elements don't contain content and don't have an end tag. They are called empty or void elements.
Examples: `<br>`, `<hr>`, `<img>`, `<input>`, `<meta>`.

### Nested Elements
HTML elements can be placed inside other HTML elements. This is called nesting.
```html
<div>
    <p>This is a paragraph <strong>inside</strong> a div.</p>
</div>
```

### Element Relationships
Imagine the HTML structure as a family tree (the DOM tree):

```text
<html>
 ├── <head> (Child of html, Sibling of body)
 │    └── <title> (Child of head)
 └── <body> (Child of html, Sibling of head)
      ├── <h1> (Child of body, Parent of text)
      └── <div> (Child of body, Parent of p)
           └── <p> (Child of div, Descendant of body)
```
- **Parent:** The element containing another element. (The `<div>` is the parent of `<p>`).
- **Child:** The element inside a parent. (The `<p>` is the child of `<div>`).
- **Sibling:** Elements that share the same parent. (`<h1>` and `<div>` are siblings).
- **Descendant:** Any element nested within an element, no matter how deep.

---

## 6. HTML Attributes

Attributes provide additional information about an element. They are always specified in the start tag and usually come in name/value pairs like: `name="value"`.

### Global Attributes
These attributes can be used on *any* HTML element.

- `id`: Specifies a unique id for an element. Used for CSS targeting, JavaScript manipulation, and fragment links.
- `class`: Specifies one or more class names for an element. Used mainly for CSS styling and JavaScript.
- `style`: Applies inline CSS styles to an element.
- `title`: Provides extra information about an element (often displayed as a tooltip on hover).
- `hidden`: A boolean attribute indicating the element is not yet, or is no longer, relevant (hides it).
- `tabindex`: Specifies the tabbing order of an element (for keyboard navigation).
- `draggable`: Specifies whether an element is draggable (using the Drag and Drop API).
- `spellcheck`: Specifies whether the element is to have its spelling and grammar checked.
- `contenteditable`: Specifies whether the content of an element is editable by the user.
- `translate`: Specifies whether the content of an element should be translated or not.
- `lang`: Specifies the language of the element's content.
- `dir`: Specifies the text direction (`ltr` for left-to-right, `rtl` for right-to-left).
- `accesskey`: Specifies a shortcut key to activate/focus an element.
- `data-*`: Used to store custom, private data to the page or application. (e.g., `data-user-id="123"`).
- `aria-*`: Accessible Rich Internet Applications attributes. Used to improve accessibility for screen readers.

### Tag-Specific Attributes
- `href` (used in `<a>`, `<link>`): Specifies the URL of the linked resource.
- `src` (used in `<img>`, `<script>`, `<iframe>`): Specifies the path to the embedded media/script.
- `alt` (used in `<img>`): Specifies alternate text for an image if the image cannot be displayed.
- `disabled` (used in `<input>`, `<button>`): Disables the element.
- `action`, `method` (used in `<form>`): Specifies where and how to send form data.


## 7. HTML Forms

Forms are used to collect user input. The user input is most often sent to a server for processing.

### Form Elements

- `<form>`: The outer wrapper.
  - `action`: The URL where the form data is submitted.
  - `method`: The HTTP method used (`GET` appends data to URL, `POST` sends data in the request body - safer for sensitive data).
- `<input>`: The most versatile form element (see Section 8).
- `<textarea>`: For multi-line text input (like comments or messages).
- `<button>`: A clickable button. Types include `submit`, `reset`, and `button`.
- `<label>`: Defines a label for a form element. Clicking the label focuses the input. Connect to input via `for` attribute matching input's `id`.
- `<select>`: Creates a drop-down list.
- `<option>`: Defines an option in a drop-down list.
- `<optgroup>`: Groups related options in a `<select>` list.
- `<fieldset>`: Groups related elements in a form, often drawing a box around them.
- `<legend>`: Defines a caption for the `<fieldset>`.
- `<datalist>`: Specifies a list of pre-defined options for an `<input>` element (provides autocomplete functionality).
- `<output>`: Represents the result of a calculation.
- `<progress>`: Represents the completion progress of a task.
- `<meter>`: Represents a scalar measurement within a known range (like disk usage).

### Form Example
```html
<form action="/submit" method="POST">
    <fieldset>
        <legend>Personal Information</legend>
        
        <label for="fname">First Name:</label>
        <input type="text" id="fname" name="fname" required>
        <br><br>
        
        <label for="country">Country:</label>
        <select id="country" name="country">
            <optgroup label="North America">
                <option value="us">United States</option>
                <option value="ca">Canada</option>
            </optgroup>
        </select>
        
    </fieldset>
    <br>
    <button type="submit">Submit</button>
</form>
```


## 8. Input Types

The `<input>` element is the most important form element. Its behavior changes drastically based on the `type` attribute.

### Common Input Types
- `text`: Default single-line text field. `<input type="text">`
- `password`: Masks characters (bullets or asterisks) for passwords.
- `email`: Automatically validates that the input looks like an email address.
- `number`: Accepts only numbers. Often provides up/down spinner arrows.
- `url`: Validates input as an absolute URL.
- `tel`: For telephone numbers (brings up a numeric keypad on mobile).
- `color`: Provides a color picker widget.
- `date`: Provides a calendar date picker widget.
- `datetime-local`: Date and time picker (without time zone).
- `month`: Month and year picker.
- `week`: Week and year picker.
- `time`: Time picker.
- `search`: Styled specifically for search queries.
- `file`: Allows the user to select one or more files to upload.
- `image`: Displays an image as a submit button.
- `hidden`: Stores data invisible to the user but sent on submission.
- `radio`: A radio button (select *only one* from a group sharing the same `name`).
- `checkbox`: A checkbox (select *multiple* from a group).
- `range`: A slider control for a number within a range.
- `reset`: A button that resets all form fields to their initial values.
- `submit`: A button that submits the form.
- `button`: A generic button with no default behavior (often used with JavaScript).

### Validation Attributes
HTML5 introduced built-in client-side form validation without needing JavaScript.

- `required`: Specifies that an input field must be filled out before submitting.
- `readonly`: The user cannot modify the value, but it is submitted.
- `disabled`: The element is unusable, unclickable, and its value is *not* submitted.
- `maxlength` / `minlength`: Specifies the maximum/minimum number of characters allowed.
- `pattern`: Specifies a Regular Expression (Regex) that the input's value is checked against.
- `placeholder`: Provides a short hint that describes the expected value.
- `autocomplete`: Specifies whether a form or input should have autocomplete on or off.
- `autofocus`: Automatically focuses the input field when the page loads.
- `multiple`: Specifies that the user is allowed to enter/select more than one value (used in `email` or `file`).
- `accept`: Specifies the types of files that the server accepts (used with `type="file"`).
- `min` / `max`: Specifies the minimum/maximum numerical or date value.
- `step`: Specifies the legal number intervals (e.g., `step="2"` allows 0, 2, 4...).


## 9. HTML5 Features

HTML5 revolutionized web development by introducing powerful native APIs and semantic elements.

- **Semantic HTML:** Introduced meaningful tags (`<header>`, `<nav>`, `<article>`, etc.) to replace generic `<div>` soup.
- **Audio & Video:** Native embedding of media without third-party plugins (like Flash) using `<audio>` and `<video>`.
- **Canvas (`<canvas>`):** A 2D drawing API for rendering graphics, animations, and games via JavaScript.
- **SVG (`<svg>`):** Scalable Vector Graphics for resolution-independent images directly in the DOM.
- **Drag and Drop API:** Native support for dragging elements and dropping them into targets.
- **Web Storage API:**
  - **Local Storage:** Stores data with no expiration date. Data persists even when the browser is closed.
  - **Session Storage:** Stores data for one session (data is lost when the browser tab is closed).
- **Web Workers:** Allows JavaScript to run in the background (on a separate thread) without affecting the performance of the UI.
- **Geolocation API:** Allows the user to share their physical location with the web application.
- **History API:** Allows manipulation of the browser session history (enabling Single Page Applications).
- **Responsive Images:**
  - `srcset` attribute on `<img>` allows providing different image sizes for different screen resolutions.
  - `<picture>` element allows art direction (loading entirely different images based on screen width or format support).
- **Native Form Validation:** Built-in validation using attributes like `required`, `pattern`, `type="email"`.


## 10. Semantic HTML

Semantic HTML refers to using HTML tags that carry meaning about the content they enclose, rather than merely defining how the content should look. 

### Why is it important?
1. **Accessibility (a11y):** Screen readers use semantic tags to understand the page structure and navigate effectively.
2. **SEO (Search Engine Optimization):** Search engines prioritize content inside semantic tags (like `<article>` or `<h1>`) to understand the page's primary topics.
3. **Maintainability:** Easier for developers to read and maintain the code.

### Semantic Elements
- `<header>`: Represents introductory content, typically containing a logo, navigation, or heading. Can be used for the whole page or within an `<article>`.
- `<nav>`: Represents a section containing navigation links.
- `<main>`: Represents the dominant, unique content of the page. There should only be one `<main>` per page.
- `<section>`: A thematic grouping of content, typically with a heading.
- `<article>`: Represents an independent, self-contained composition (e.g., a blog post, a news article, a forum post).
- `<aside>`: Content that is tangentially related to the main content (e.g., a sidebar, pull quotes, advertising).
- `<footer>`: Represents a footer for its nearest sectioning content. Usually contains copyright, author info, or secondary links.
- `<figure>`: Self-contained content, often referenced as a single unit from the main flow (images, diagrams, code snippets).
- `<figcaption>`: The caption for a `<figure>`.
- `<details>` & `<summary>`: Creates an accordion-like expandable widget natively.
- `<time>`: Represents a specific period in time (useful for computers to parse dates).
- `<mark>`: Represents text that has been highlighted for reference or relevance.

### Example Layout
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


## 11. Tables

HTML tables are used to display tabular data (data arranged in rows and columns). **Never use tables for page layout.**

### Table Elements
- `<table>`: The container for the table.
- `<tr>` (Table Row): Defines a horizontal row.
- `<th>` (Table Header): Defines a header cell. Text is bold and centered by default. Indicates what the column/row is about.
- `<td>` (Table Data): Defines a standard data cell.
- `<thead>`: Groups header content.
- `<tbody>`: Groups body content.
- `<tfoot>`: Groups footer content (often totals or summaries).
- `<caption>`: Provides a title or explanation for the table.

### Attributes
- `colspan`: Makes a cell span across multiple columns.
- `rowspan`: Makes a cell span across multiple rows.

### Example Table
```html
<table border="1">
    <caption>Employee Directory</caption>
    <thead>
        <tr>
            <th>Name</th>
            <th>Department</th>
            <th>Role</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Jane Doe</td>
            <td>Engineering</td>
            <td>Developer</td>
        </tr>
        <tr>
            <td>John Smith</td>
            <td>Marketing</td>
            <td>Manager</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td colspan="3">Total Employees: 2</td>
        </tr>
    </tfoot>
</table>
```


## 12. Lists

HTML provides three main ways to list information.

### Unordered Lists (`<ul>`)
Used when the order of items does not matter (bullet points).
```html
<ul>
    <li>Apples</li>
    <li>Bananas</li>
    <li>Cherries</li>
</ul>
```

### Ordered Lists (`<ol>`)
Used when the sequence of items is important (numbered steps).
```html
<ol>
    <li>Preheat oven.</li>
    <li>Mix ingredients.</li>
    <li>Bake for 30 mins.</li>
</ol>
```

### Description Lists (`<dl>`)
Used to define terms and their descriptions (like a dictionary).
- `<dt>`: Description Term.
- `<dd>`: Description Details.
```html
<dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language</dd>
    <dt>CSS</dt>
    <dd>Cascading Style Sheets</dd>
</dl>
```

### Nested Lists
You can put lists inside of other lists.
```html
<ul>
    <li>Coffee</li>
    <li>Tea
        <ul>
            <li>Green Tea</li>
            <li>Black Tea</li>
        </ul>
    </li>
    <li>Milk</li>
</ul>
```

---

## 13. Media

### Images (`<img>`)
Embeds a static image.
```html
<img src="path/to/image.jpg" alt="A beautiful landscape" width="500" height="300" loading="lazy">
```
*Tip: `loading="lazy"` defers loading the image until it is close to the viewport, improving performance.*

### Responsive Images (`<picture>`)
Provides different image sources based on screen size or supported formats (like WebP).
```html
<picture>
    <source srcset="image.webp" type="image/webp">
    <source media="(max-width: 799px)" srcset="image-mobile.jpg">
    <source media="(min-width: 800px)" srcset="image-desktop.jpg">
    <img src="image-fallback.jpg" alt="Responsive image">
</picture>
```

### Audio (`<audio>`)
```html
<audio controls>
    <source src="music.ogg" type="audio/ogg">
    <source src="music.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
</audio>
```
*(Attributes: `controls`, `autoplay`, `loop`, `muted`)*

### Video (`<video>`)
```html
<video width="320" height="240" controls poster="thumbnail.jpg">
    <source src="movie.mp4" type="video/mp4">
    Your browser does not support the video tag.
</video>
```
*(Attributes: `controls`, `autoplay`, `loop`, `muted`, `poster`)*

### Iframes (`<iframe>`)
Embeds another HTML document inside the current one (e.g., embedding a YouTube video or Google Map).
```html
<iframe src="https://www.youtube.com/embed/dQw4w9WgXcQ" width="560" height="315" title="YouTube video"></iframe>
```


## 14. HTML Entities

Some characters are reserved in HTML. For example, you cannot use the less than (`<`) or greater than (`>`) signs in your text because the browser might mix them up with tags. To display these characters, we use **character entities**.

### Format
Entities start with an ampersand (`&`) and end with a semicolon (`;`).

### Common Entities Reference Table

| Character | Description | Entity Name | Entity Number |
| :---: | :--- | :--- | :--- |
| `<` | Less than | `&lt;` | `&#60;` |
| `>` | Greater than | `&gt;` | `&#62;` |
| `&` | Ampersand | `&amp;` | `&#38;` |
| `"` | Double quote | `&quot;` | `&#34;` |
| `'` | Single quote | `&apos;` | `&#39;` |
| | Non-breaking space | `&nbsp;` | `&#160;` |
| `©` | Copyright | `&copy;` | `&#169;` |
| `®` | Registered trademark | `&reg;` | `&#174;` |
| `€` | Euro | `&euro;` | `&#8364;` |
| `¥` | Yen | `&yen;` | `&#165;` |

*Note: `&nbsp;` is heavily used to prevent browsers from truncating multiple spaces or breaking lines awkwardly.*


## 15. Accessibility (a11y)

Web accessibility ensures that websites are usable by people with disabilities (visual, auditory, motor, or cognitive).

### Key Practices:
- **`alt` Text:** EVERY `<img>` must have an `alt` attribute. If it's purely decorative, use an empty string (`alt=""`), which tells screen readers to ignore it. If it has meaning, describe it accurately.
- **Semantic HTML:** Use `<button>` for actions, `<a>` for navigation. Screen readers understand these natively. Do not use a `<div>` with an `onclick` event to mimic a button.
- **Form Labels:** Always associate a `<label>` with its `<input>` using the `for` and `id` attributes.
- **Keyboard Navigation:** Users should be able to navigate the entire site using only the `Tab` key. Ensure interactive elements are focusable.
- **Focus Management:** Visible focus indicators (outlines) must be present so keyboard users know where they are. Do not remove `outline: none` in CSS without providing an alternative.
- **ARIA Attributes:** Accessible Rich Internet Applications. Use ARIA to add semantic context where HTML falls short (e.g., custom dropdowns).
  - `aria-label`: Provides a label for an element that doesn't have visible text (e.g., an icon-only button).
  - `aria-hidden="true"`: Hides elements from screen readers.
  - `aria-expanded="true/false"`: Indicates if a collapsible menu is open or closed.


## 16. SEO in HTML

Search Engine Optimization (SEO) makes your site more visible on search engines like Google. HTML plays a massive role in On-Page SEO.

### Crucial SEO Elements:
- **`<title>`:** The most critical SEO element. Should be unique per page, descriptive, and around 50-60 characters.
- **Meta Description:** `<meta name="description" content="A summary of the page.">`. Shows up in search engine results below the title. Helps click-through rates.
- **Heading Hierarchy:** Use one `<h1>` for the main topic. Use `<h2>` for major sections, `<h3>` for sub-sections. Search engines use this to understand the page outline.
- **Semantic Tags:** Wrap core content in `<main>` and `<article>`.
- **Image Optimization:** Use descriptive filenames (`black-shoes.jpg` not `IMG_123.jpg`), use `alt` tags, and ensure images load quickly (lazy loading, modern formats).
- **Canonical Tags:** `<link rel="canonical" href="https://example.com/page">`. Tells search engines which URL is the "master" version if there is duplicate content.
- **Open Graph (OG) Tags:** Used by social media platforms (Facebook, LinkedIn, Twitter) to generate preview cards when your link is shared.
  ```html
  <meta property="og:title" content="My Page Title">
  <meta property="og:image" content="https://example.com/image.jpg">
  ```
- **Robots Meta Tag:** Instructs search engine crawlers. `<meta name="robots" content="index, follow">` (default).


## 17. Best Practices

Writing clean, maintainable HTML is a sign of a professional developer.

- **Use Lowercase:** While HTML5 is case-insensitive, writing tags and attributes in lowercase (`<p>`, not `<P>`) is the industry standard and easier to type/read.
- **Quote Attributes:** Always enclose attribute values in double quotes (`class="container"`).
- **Proper Indentation:** Indent nested elements consistently (usually 2 or 4 spaces) to show parent/child relationships clearly.
- **Meaningful IDs and Classes:** Name classes based on the element's purpose, not its appearance (e.g., `class="error-message"`, not `class="red-text"`).
- **Close All Tags:** Even though HTML5 allows omitting some closing tags (like `</p>`), explicitly closing them prevents unexpected rendering issues.
- **Validate Your Code:** Run your HTML through the W3C Markup Validation Service to catch errors.
- **Keep Structure Separate from Presentation:** Never use inline styles (`style="..."`) unless absolutely necessary. Keep CSS in external stylesheets.


## 18. Deprecated HTML Tags

These tags are obsolete in HTML5 and should **never** be used in modern web development. They were mostly used for styling, which is now entirely the job of CSS.

| Deprecated Tag | What it did | Modern Replacement |
| :--- | :--- | :--- |
| `<font>` | Changed font size/color | CSS `font-family`, `color`, `font-size` |
| `<center>` | Centered content | CSS `text-align: center` or Flexbox/Grid |
| `<strike>` | Strikethrough text | `<del>`, `<s>`, or CSS `text-decoration: line-through` |
| `<marquee>` | Scrolling text | CSS Animations |
| `<b>` | Bold text | `<strong>` (for importance) or CSS `font-weight: bold` |
| `<i>` | Italic text | `<em>` (for emphasis) or CSS `font-style: italic` |
| `<frame>` / `<frameset>`| Page layouts with frames | `<iframe>` or CSS layout techniques |
| `<acronym>` | Defined an acronym | `<abbr>` |
| `<applet>` | Embedded Java applets | `<object>` or `<embed>` |

---

## 19. Common Beginner Mistakes

### Mistake 1: Using `<div>` instead of Semantic Tags (Div Soup)
**Bad:** `<div class="header">`, `<div class="nav">`, `<div class="footer">`
**Fix:** Use `<header>`, `<nav>`, `<footer>`.

### Mistake 2: Using `<br>` for spacing
**Bad:** Using `<br><br><br>` to create a gap between paragraphs.
**Fix:** Use CSS margin or padding (`margin-bottom: 20px`).

### Mistake 3: Forgetting the `alt` attribute on Images
**Bad:** `<img src="logo.png">`
**Fix:** `<img src="logo.png" alt="Company Logo">`

### Mistake 4: Skipping Heading Levels
**Bad:** Going from `<h1>` directly to `<h4>`.
**Fix:** Maintain hierarchical order: `<h1>` -> `<h2>` -> `<h3>`.

### Mistake 5: Using `<a>` without an `href` to make a button
**Bad:** `<a onclick="doSomething()">Click Me</a>`
**Fix:** Use a button element: `<button type="button" onclick="doSomething()">Click Me</button>`

### Mistake 6: Unclosed Tags
**Bad:** `<div><p>Hello</div>`
**Fix:** `<div><p>Hello</p></div>`

---

## 20. Real Projects: HTML Structure Walkthroughs

*(Note: These examples show the semantic HTML structure. CSS would be needed to make them look complete.)*

### A. Personal Portfolio
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Jane Doe - Portfolio</title>
</head>
<body>
    <header>
        <nav>
            <ul>
                <li><a href="#about">About</a></li>
                <li><a href="#projects">Projects</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <section id="hero">
            <h1>Hi, I'm Jane Doe</h1>
            <p>Full-Stack Web Developer.</p>
        </section>

        <section id="about">
            <h2>About Me</h2>
            <p>I build modern web applications...</p>
        </section>

        <section id="projects">
            <h2>My Work</h2>
            <article>
                <h3>E-commerce App</h3>
                <img src="project1.jpg" alt="Screenshot of E-commerce App">
                <p>A full-stack React and Node application.</p>
                <a href="#">View Code</a>
            </article>
        </section>
    </main>

    <footer id="contact">
        <h2>Contact Me</h2>
        <a href="mailto:jane@example.com">Email Me</a>
    </footer>
</body>
</html>
```

### B. Login Form
```html
<main>
    <section class="login-container">
        <h1>Welcome Back</h1>
        <form action="/login" method="POST">
            <div>
                <label for="email">Email Address:</label>
                <input type="email" id="email" name="email" required autofocus>
            </div>
            
            <div>
                <label for="password">Password:</label>
                <input type="password" id="password" name="password" required>
            </div>
            
            <div>
                <input type="checkbox" id="remember" name="remember">
                <label for="remember">Remember me</label>
            </div>
            
            <button type="submit">Sign In</button>
        </form>
        <p>Don't have an account? <a href="/register">Sign up</a></p>
    </section>
</main>
```

### C. Blog Article Layout
```html
<main>
    <article>
        <header>
            <h1>Understanding CSS Grid</h1>
            <p>By John Doe | <time datetime="2023-11-01">Nov 1, 2023</time></p>
        </header>

        <figure>
            <img src="grid-banner.jpg" alt="CSS Grid diagram">
            <figcaption>A visual representation of CSS Grid.</figcaption>
        </figure>

        <section>
            <h2>Introduction</h2>
            <p>CSS Grid is a powerful two-dimensional layout system...</p>
        </section>

        <section>
            <h2>Basic Concepts</h2>
            <p>To get started, you need a container...</p>
            <pre><code>
.container {
    display: grid;
}
            </code></pre>
        </section>

        <footer>
            <p>Tags: <a href="#">CSS</a>, <a href="#">Layout</a></p>
        </footer>
    </article>
</main>
```


## 21. HTML Cheat Sheet

### Most Used Tags
- `<html>`, `<head>`, `<body>`, `<title>`, `<meta>`, `<link>`, `<script>`, `<style>`
- `<h1>` to `<h6>`, `<p>`, `<a>`, `<img>`, `<div>`, `<span>`
- `<ul>`, `<ol>`, `<li>`
- `<table>`, `<tr>`, `<th>`, `<td>`
- `<form>`, `<input>`, `<button>`, `<label>`

### Semantic Tags
- `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`, `<figure>`, `<figcaption>`, `<time>`

### Common Input Types
- `text`, `email`, `password`, `number`, `date`, `file`, `checkbox`, `radio`, `submit`

### Important Attributes
- **Global:** `class`, `id`, `style`, `hidden`, `data-*`
- **Links:** `href`, `target="_blank"`
- **Images:** `src`, `alt`, `loading="lazy"`
- **Forms:** `action`, `method`, `name`, `value`, `placeholder`, `required`, `disabled`

### Common HTML Entities
- Space: `&nbsp;`
- `<`: `&lt;`
- `>`: `&gt;`
- `&`: `&amp;`
- `©`: `&copy;`


