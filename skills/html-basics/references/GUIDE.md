# HTML Basics Reference Guide

Production-grade reference for HTML5 fundamentals based on 2024-2025 best practices.

## 📚 Table of Contents

1. [Document Structure](#document-structure)
2. [Essential Meta Tags](#essential-meta-tags)
3. [Text Content](#text-content)
4. [Grouping & Sections](#grouping--sections)
5. [Links & Navigation](#links--navigation)
6. [Images & Media](#images--media)
7. [Common Patterns](#common-patterns)
8. [Troubleshooting](#troubleshooting)

---

## Document Structure

### Minimal Valid HTML5 Document

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Page Title</title>
</head>
<body>
  <main>
    <h1>Page Heading</h1>
    <p>Content goes here.</p>
  </main>
</body>
</html>
```

### Why Each Part Matters

| Element | Purpose | SEO Impact | A11y Impact |
|---------|---------|------------|-------------|
| `<!DOCTYPE html>` | Triggers standards mode | None | None |
| `<html lang="en">` | Document language | Minor | **Critical** |
| `<meta charset>` | Character encoding | None | Minor |
| `<meta viewport>` | Mobile responsiveness | **High** | Medium |
| `<title>` | Page title | **High** | **Critical** |

---

## Essential Meta Tags

### Minimum Required

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### SEO Recommended

```html
<meta name="description" content="Page description (150-160 chars)">
<meta name="robots" content="index, follow">
<link rel="canonical" href="https://example.com/page">
```

### Social Media (Open Graph)

```html
<meta property="og:title" content="Page Title">
<meta property="og:description" content="Description">
<meta property="og:image" content="https://example.com/image.jpg">
<meta property="og:url" content="https://example.com/page">
<meta property="og:type" content="website">
```

### Twitter Cards

```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Page Title">
<meta name="twitter:description" content="Description">
<meta name="twitter:image" content="https://example.com/image.jpg">
```

---

## Text Content

### Heading Hierarchy (Critical for A11y & SEO)

```html
<!-- CORRECT: Logical hierarchy -->
<h1>Main Title</h1>
  <h2>Section 1</h2>
    <h3>Subsection 1.1</h3>
    <h3>Subsection 1.2</h3>
  <h2>Section 2</h2>

<!-- WRONG: Skipped levels -->
<h1>Main Title</h1>
  <h3>Section</h3>  <!-- Skipped h2! -->
```

### Text Semantics Quick Reference

```html
<strong>Strong importance</strong>    <!-- Not just bold -->
<em>Stress emphasis</em>              <!-- Not just italic -->
<mark>Highlighted/relevant</mark>     <!-- Search results -->
<del>Deleted text</del>               <!-- Strikethrough with meaning -->
<ins>Inserted text</ins>              <!-- Added content -->
<abbr title="HyperText Markup Language">HTML</abbr>
<time datetime="2025-01-15">January 15, 2025</time>
<code>inline code</code>
<pre><code>block of code</code></pre>
```

---

## Grouping & Sections

### Landmark Elements

```html
<body>
  <header>        <!-- Banner landmark -->
    <nav>         <!-- Navigation landmark -->
      ...
    </nav>
  </header>

  <main>          <!-- Main landmark (once per page) -->
    <article>     <!-- Self-contained content -->
      <header>    <!-- Article header -->
        <h1>Title</h1>
      </header>
      <section>   <!-- Thematic section -->
        ...
      </section>
    </article>

    <aside>       <!-- Complementary landmark -->
      ...
    </aside>
  </main>

  <footer>        <!-- Contentinfo landmark -->
    ...
  </footer>
</body>
```

### When to Use Each

| Element | Use When | Don't Use When |
|---------|----------|----------------|
| `<main>` | Primary unique content | Multiple times per page |
| `<article>` | Independently distributable | Just grouping content |
| `<section>` | Thematic grouping with heading | Just for styling |
| `<aside>` | Tangentially related | Main content |
| `<div>` | No semantic meaning needed | A semantic element fits |

---

## Links & Navigation

### Link Best Practices

```html
<!-- External link with security -->
<a href="https://external.com"
   target="_blank"
   rel="noopener noreferrer">
  External Site
</a>

<!-- Download link -->
<a href="/files/doc.pdf" download="document.pdf">
  Download PDF
</a>

<!-- Skip link for accessibility -->
<a href="#main-content" class="skip-link">
  Skip to main content
</a>

<!-- Email link -->
<a href="mailto:contact@example.com">Email Us</a>

<!-- Phone link -->
<a href="tel:+1234567890">Call Us</a>
```

### Navigation Pattern

```html
<nav aria-label="Main navigation">
  <ul>
    <li><a href="/" aria-current="page">Home</a></li>
    <li><a href="/about">About</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>
```

---

## Images & Media

### Responsive Images

```html
<!-- Basic responsive image -->
<img
  src="image.jpg"
  alt="Description of image"
  width="800"
  height="600"
  loading="lazy"
  decoding="async"
>

<!-- Art direction with picture -->
<picture>
  <source
    media="(min-width: 1200px)"
    srcset="large.jpg"
  >
  <source
    media="(min-width: 768px)"
    srcset="medium.jpg"
  >
  <img
    src="small.jpg"
    alt="Description"
    width="400"
    height="300"
  >
</picture>

<!-- Resolution switching -->
<img
  src="image-800.jpg"
  srcset="image-400.jpg 400w,
          image-800.jpg 800w,
          image-1200.jpg 1200w"
  sizes="(max-width: 600px) 400px,
         (max-width: 1000px) 800px,
         1200px"
  alt="Description"
>
```

### Video with Accessibility

```html
<video
  controls
  width="640"
  height="360"
  poster="thumbnail.jpg"
>
  <source src="video.mp4" type="video/mp4">
  <source src="video.webm" type="video/webm">
  <track
    kind="captions"
    src="captions.vtt"
    srclang="en"
    label="English"
    default
  >
  <p>
    Your browser doesn't support HTML5 video.
    <a href="video.mp4">Download the video</a>.
  </p>
</video>
```

---

## Common Patterns

### Breadcrumb Navigation

```html
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/products">Products</a></li>
    <li><a href="/products/widgets" aria-current="page">Widgets</a></li>
  </ol>
</nav>
```

### Card Component

```html
<article class="card">
  <img src="image.jpg" alt="" aria-hidden="true">
  <div class="card-content">
    <h3><a href="/article">Article Title</a></h3>
    <p>Brief description of the article...</p>
    <time datetime="2025-01-15">Jan 15, 2025</time>
  </div>
</article>
```

### Figure with Caption

```html
<figure>
  <img src="chart.png" alt="Bar chart showing sales growth from 2020-2024">
  <figcaption>
    Figure 1: Annual sales growth showing 25% increase over 4 years.
  </figcaption>
</figure>
```

---

## Troubleshooting

### Decision Tree: Which Element to Use?

```
Is it the main content of the page?
├─ Yes → <main>
└─ No → Is it self-contained/syndicated content?
         ├─ Yes → <article>
         └─ No → Does it have a thematic heading?
                  ├─ Yes → <section>
                  └─ No → Is it navigation?
                           ├─ Yes → <nav>
                           └─ No → Is it tangentially related?
                                    ├─ Yes → <aside>
                                    └─ No → <div>
```

### Common Mistakes & Fixes

| Mistake | Problem | Fix |
|---------|---------|-----|
| Multiple `<h1>` | Confuses screen readers | One `<h1>` per page |
| Skipped headings | Breaks outline | Follow h1→h2→h3 order |
| `<br>` for spacing | Semantic abuse | Use CSS margin/padding |
| Empty links | Inaccessible | Add text or aria-label |
| `<div>` everywhere | No semantics | Use appropriate elements |
| Missing `alt` | Inaccessible images | Always provide alt text |

### Validation Checklist

```
□ DOCTYPE is first line
□ html has lang attribute
□ head has charset meta
□ head has viewport meta
□ One h1 per page
□ Headings don't skip levels
□ All images have alt text
□ All links have accessible text
□ main element present
□ No duplicate IDs
```

---

## 🔗 References

- [MDN HTML Reference](https://developer.mozilla.org/en-US/docs/Web/HTML)
- [HTML Living Standard](https://html.spec.whatwg.org/)
- [W3C Validator](https://validator.w3.org/)
- [Can I Use](https://caniuse.com/)
