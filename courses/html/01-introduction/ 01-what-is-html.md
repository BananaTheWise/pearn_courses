# What is HTML?

HTML (Hyper Text Markup Language) is the standard markup language for creating web pages.

- It describes the structure of a web page.
- HTML elements tell the browser how to display content.
- It’s the foundation of every website.

## Why learn HTML?

- It’s easy to learn.
- It’s the starting point for web development.
- It works together with CSS and JavaScript.

## Example

```html
<p>This is a paragraph.</p>

n the next lesson, you'll see a complete HTML document.


---

#### `01-introduction/02-html-document.md`

```markdown
# A Basic HTML Document

Every HTML document has the same basic structure:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Page Title</title>
</head>
<body>
  <h1>Welcome!</h1>
  <p>This is a paragraph.</p>
</body>
</html>

Key parts

    <!DOCTYPE html> declares the document type.

    <html> is the root element.

    <head> contains meta‑information (like the title).

    <body> holds the visible content.

Try it: copy the example and save it as index.html, then open it in your browser.


---

#### `02-html-elements/01-headings-paragraphs.md`

```markdown
# Headings and Paragraphs

Headings range from `<h1>` to `<h6>` – `<h1>` is the most important.

```html
<h1>Main Heading</h1>
<h2>Subheading</h2>
<h3>Sub-subheading</h3>

Paragraphs are created with <p>.

<p>This is a paragraph of text.</p>

Practice

Write a page that has a main heading, a subheading, and at least two paragraphs.


---

#### `02-html-elements/02-links-images.md`

```markdown
# Links and Images

## Links

Use the `<a>` tag with the `href` attribute.

```html
<a href="https://example.com">Visit Example</a>

Images

Use the <img> tag with src and alt.

<img src="photo.jpg" alt="Description of photo">

Exercise

Create a page with a link to your favourite website and an image (use a placeholder URL like https://via.placeholder.com/150).


---

#### `02-html-elements/exercises.json`

```json
{
  "exercises": [
    {
      "id": "ex1",
      "type": "multiple_choice",
      "question": "Which tag is used for the largest heading?",
      "options": ["<h1>", "<h6>", "<heading>", "<head>"],
      "correct_answer": 0,
      "explanation": "<h1> is the largest heading.",
      "points": 5
    },
    {
      "id": "ex2",
      "type": "text",
      "question": "Write the HTML tag for a paragraph.",
      "correct_answer": "<p>",
      "points": 5
    }
  ]
}
