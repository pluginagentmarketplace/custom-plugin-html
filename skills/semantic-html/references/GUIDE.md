# Semantic HTML Reference Guide

Production-grade reference for meaningful HTML5 markup based on 2024-2025 best practices.

## 📚 Table of Contents

1. [Why Semantic HTML](#why-semantic-html)
2. [Document Outline](#document-outline)
3. [Sectioning Elements](#sectioning-elements)
4. [Landmark Regions](#landmark-regions)
5. [Page Templates](#page-templates)
6. [Structured Data](#structured-data)
7. [Conversion Patterns](#conversion-patterns)
8. [Troubleshooting](#troubleshooting)

---

## Why Semantic HTML

### The Triple Benefit

```
┌─────────────────────────────────────────────────────────────┐
│                    SEMANTIC HTML BENEFITS                    │
├──────────────────┬──────────────────┬───────────────────────┤
│   ACCESSIBILITY  │       SEO        │    FUTURE-PROOF       │
├──────────────────┼──────────────────┼───────────────────────┤
│ Screen readers   │ Search engines   │ AI assistants         │
│ understand       │ understand       │ Voice interfaces      │
│ structure        │ content          │ Smart displays        │
│                  │ hierarchy        │ Automated tools       │
└──────────────────┴──────────────────┴───────────────────────┘
```

### Semantic vs Non-Semantic

```html
<!-- NON-SEMANTIC (div soup) -->
<div class="header">
  <div class="nav">
    <div class="nav-item">Home</div>
  </div>
</div>
<div class="main">
  <div class="article">
    <div class="title">Article Title</div>
    <div class="content">Content here...</div>
  </div>
</div>

<!-- SEMANTIC (meaningful) -->
<header>
  <nav aria-label="Main">
    <ul>
      <li><a href="/">Home</a></li>
    </ul>
  </nav>
</header>
<main>
  <article>
    <h1>Article Title</h1>
    <p>Content here...</p>
  </article>
</main>
```

---

## Document Outline

### Heading Hierarchy Rules

```
✅ CORRECT                    ❌ WRONG
─────────────────────         ─────────────────────
<h1>Page Title</h1>           <h1>Page Title</h1>
  <h2>Section 1</h2>            <h3>Section 1</h3>  ← Skipped h2!
    <h3>Subsection</h3>         <h1>Section 2</h1>  ← Multiple h1!
  <h2>Section 2</h2>              <h4>Sub</h4>      ← Skipped h3!
    <h3>Subsection</h3>
    <h3>Subsection</h3>
```

### Document Outline Algorithm

```html
<body>
  <h1>Site Name</h1>                    <!-- Outline: 1. Site Name -->

  <article>
    <h2>Article Title</h2>              <!--   1.1. Article Title -->
    <section>
      <h3>Section A</h3>                <!--     1.1.1. Section A -->
    </section>
    <section>
      <h3>Section B</h3>                <!--     1.1.2. Section B -->
      <h4>Subsection B.1</h4>           <!--       1.1.2.1. Subsection B.1 -->
    </section>
  </article>

  <aside>
    <h2>Related</h2>                    <!--   1.2. Related -->
  </aside>
</body>
```

---

## Sectioning Elements

### Quick Reference

| Element | Purpose | Needs Heading? | ARIA Role |
|---------|---------|----------------|-----------|
| `<article>` | Self-contained, distributable | Yes | article |
| `<section>` | Thematic grouping | Yes | region* |
| `<nav>` | Navigation links | No | navigation |
| `<aside>` | Tangential content | Recommended | complementary |
| `<header>` | Introductory content | No | banner* |
| `<main>` | Primary content | No | main |
| `<footer>` | Footer information | No | contentinfo* |

*\* Role applies only when direct child of body*

### When to Use Each

#### `<article>` - Independent Content

```html
<!-- Blog post - can be syndicated independently -->
<article>
  <h2>How to Use Semantic HTML</h2>
  <p>Posted on <time datetime="2025-01-15">January 15, 2025</time></p>
  <p>Content...</p>
</article>

<!-- Comment - self-contained unit -->
<article>
  <h3>Comment by User</h3>
  <p>Great article!</p>
</article>

<!-- Product listing - independent item -->
<article>
  <h3>Product Name</h3>
  <p>$29.99</p>
</article>
```

#### `<section>` - Thematic Grouping

```html
<!-- Chapters of a document -->
<section aria-labelledby="ch1">
  <h2 id="ch1">Chapter 1: Introduction</h2>
  <p>Content...</p>
</section>

<!-- Tab panels -->
<section aria-labelledby="tab1">
  <h2 id="tab1">Features</h2>
  <p>Feature list...</p>
</section>
```

#### `<nav>` - Navigation

```html
<!-- Main navigation -->
<nav aria-label="Main">
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>

<!-- Breadcrumb navigation -->
<nav aria-label="Breadcrumb">
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/products">Products</a></li>
    <li><a aria-current="page">Widget</a></li>
  </ol>
</nav>

<!-- Pagination -->
<nav aria-label="Pagination">
  <ul>
    <li><a href="?page=1">1</a></li>
    <li><a href="?page=2" aria-current="page">2</a></li>
    <li><a href="?page=3">3</a></li>
  </ul>
</nav>
```

#### `<aside>` - Related Content

```html
<!-- Sidebar -->
<aside aria-label="Sidebar">
  <h2>Popular Posts</h2>
  <ul>...</ul>
</aside>

<!-- Pull quote -->
<aside>
  <blockquote>
    "Important quote from the article"
  </blockquote>
</aside>

<!-- Related links -->
<aside aria-label="Related topics">
  <h2>Related Topics</h2>
  <ul>...</ul>
</aside>
```

---

## Landmark Regions

### Required Landmarks

```html
<body>
  <!-- banner: Site header (once per page) -->
  <header>...</header>

  <!-- main: Primary content (once per page) -->
  <main>...</main>

  <!-- contentinfo: Site footer (once per page) -->
  <footer>...</footer>
</body>
```

### Complete Landmark Structure

```html
<body>
  <header>                              <!-- banner -->
    <nav aria-label="Main">...</nav>    <!-- navigation -->
  </header>

  <nav aria-label="Breadcrumb">...</nav> <!-- navigation -->

  <main>                                <!-- main -->
    <article>                           <!-- article -->
      <section aria-labelledby="s1">    <!-- region (if named) -->
        <h2 id="s1">Section</h2>
      </section>
    </article>

    <aside aria-label="Related">        <!-- complementary -->
      ...
    </aside>
  </main>

  <footer>                              <!-- contentinfo -->
    <nav aria-label="Footer">...</nav>  <!-- navigation -->
  </footer>
</body>
```

### Screen Reader Landmark Navigation

When landmarks are correct, screen reader users can navigate by:
- `D` - Next landmark
- `Shift+D` - Previous landmark
- Rotor/Elements list - Jump to any landmark

---

## Page Templates

### Blog Homepage

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Blog Name</title>
</head>
<body>
  <header>
    <a href="/">Blog Name</a>
    <nav aria-label="Main">
      <ul>
        <li><a href="/" aria-current="page">Home</a></li>
        <li><a href="/archive">Archive</a></li>
        <li><a href="/about">About</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <h1>Latest Posts</h1>

    <article>
      <h2><a href="/post-1">First Post Title</a></h2>
      <p>By <a rel="author" href="/author">Author</a></p>
      <time datetime="2025-01-15">January 15, 2025</time>
      <p>Excerpt...</p>
    </article>

    <article>
      <h2><a href="/post-2">Second Post Title</a></h2>
      <p>By <a rel="author" href="/author">Author</a></p>
      <time datetime="2025-01-10">January 10, 2025</time>
      <p>Excerpt...</p>
    </article>

    <nav aria-label="Pagination">
      <ul>
        <li><a href="?page=1" aria-current="page">1</a></li>
        <li><a href="?page=2">2</a></li>
        <li><a href="?page=3">3</a></li>
      </ul>
    </nav>
  </main>

  <aside aria-label="Sidebar">
    <section aria-labelledby="categories">
      <h2 id="categories">Categories</h2>
      <ul>...</ul>
    </section>
  </aside>

  <footer>
    <nav aria-label="Footer">
      <ul>
        <li><a href="/privacy">Privacy</a></li>
        <li><a href="/terms">Terms</a></li>
      </ul>
    </nav>
    <p>&copy; 2025 Blog Name</p>
  </footer>
</body>
</html>
```

### Single Article Page

```html
<main>
  <article itemscope itemtype="https://schema.org/Article">
    <header>
      <nav aria-label="Breadcrumb">
        <ol>
          <li><a href="/">Home</a></li>
          <li><a href="/blog">Blog</a></li>
          <li><span aria-current="page">Article Title</span></li>
        </ol>
      </nav>

      <h1 itemprop="headline">Article Title</h1>

      <p>
        By <a rel="author" itemprop="author" href="/author">Author Name</a>
        on <time itemprop="datePublished" datetime="2025-01-15">January 15, 2025</time>
      </p>
    </header>

    <div itemprop="articleBody">
      <section aria-labelledby="intro">
        <h2 id="intro">Introduction</h2>
        <p>Content...</p>
      </section>

      <section aria-labelledby="main-point">
        <h2 id="main-point">Main Point</h2>
        <p>Content...</p>

        <figure>
          <img src="diagram.png" alt="Diagram showing...">
          <figcaption>Figure 1: Explanation</figcaption>
        </figure>
      </section>

      <section aria-labelledby="conclusion">
        <h2 id="conclusion">Conclusion</h2>
        <p>Content...</p>
      </section>
    </div>

    <footer>
      <section aria-labelledby="tags">
        <h2 id="tags">Tags</h2>
        <ul>
          <li><a rel="tag" href="/tag/html">HTML</a></li>
          <li><a rel="tag" href="/tag/semantics">Semantics</a></li>
        </ul>
      </section>

      <section aria-labelledby="share">
        <h2 id="share">Share</h2>
        <ul>...</ul>
      </section>
    </footer>
  </article>

  <aside aria-label="Related articles">
    <h2>Related Articles</h2>
    <ul>
      <li><a href="/related-1">Related Article 1</a></li>
      <li><a href="/related-2">Related Article 2</a></li>
    </ul>
  </aside>

  <section aria-labelledby="comments">
    <h2 id="comments">Comments</h2>

    <article>
      <header>
        <h3>Comment by User1</h3>
        <time datetime="2025-01-16">January 16, 2025</time>
      </header>
      <p>Great article!</p>
    </article>
  </section>
</main>
```

---

## Structured Data

### Schema.org with Microdata

```html
<!-- Article -->
<article itemscope itemtype="https://schema.org/Article">
  <h1 itemprop="headline">Article Title</h1>
  <img itemprop="image" src="hero.jpg" alt="...">
  <p itemprop="description">Article summary...</p>
  <time itemprop="datePublished" datetime="2025-01-15">Jan 15</time>
  <a itemprop="author" href="/author">Author</a>
</article>

<!-- Product -->
<article itemscope itemtype="https://schema.org/Product">
  <h1 itemprop="name">Product Name</h1>
  <img itemprop="image" src="product.jpg" alt="...">
  <p itemprop="description">Product description</p>
  <div itemprop="offers" itemscope itemtype="https://schema.org/Offer">
    <span itemprop="price" content="29.99">$29.99</span>
    <meta itemprop="priceCurrency" content="USD">
  </div>
</article>

<!-- Organization -->
<footer itemscope itemtype="https://schema.org/Organization">
  <span itemprop="name">Company Name</span>
  <a itemprop="url" href="https://example.com">Website</a>
  <a itemprop="email" href="mailto:info@example.com">Contact</a>
</footer>
```

---

## Conversion Patterns

### From Div Soup to Semantic

| Old Pattern | New Pattern |
|-------------|-------------|
| `<div class="header">` | `<header>` |
| `<div class="nav">` | `<nav aria-label="...">` |
| `<div class="main">` | `<main>` |
| `<div class="article">` | `<article>` |
| `<div class="section">` | `<section aria-labelledby="...">` |
| `<div class="sidebar">` | `<aside aria-label="...">` |
| `<div class="footer">` | `<footer>` |
| `<div class="title">` | `<h1>` / `<h2>` |
| `<span class="date">` | `<time datetime="...">` |

---

## Troubleshooting

### Semantic Audit Checklist

```
□ One <main> per page
□ One <h1> per page
□ Headings follow h1→h2→h3 order
□ All <nav> elements have unique labels
□ <header> and <footer> at page level (not in main)
□ <section> has heading or accessible name
□ <article> has heading
□ <aside> has accessible name
□ No <div> used where semantic element fits
□ Landmark regions cover all content
```

### Common Mistakes

| Mistake | Why It's Wrong | Fix |
|---------|----------------|-----|
| `<main>` inside `<header>` | Wrong hierarchy | Move main outside header |
| Multiple `<main>` | Only one allowed | Merge or use sections |
| `<section>` for styling | Semantic abuse | Use `<div>` or add heading |
| `<article>` for any box | Overuse | Only for syndicated content |
| Skip heading levels | Breaks outline | Follow h1→h2→h3 |
| No `aria-label` on nav | Ambiguous | Add unique label |

---

## 🔗 References

- [MDN: Using HTML Sections and Outlines](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements)
- [W3C: ARIA Landmarks](https://www.w3.org/WAI/ARIA/apg/practices/landmark-regions/)
- [HTML5 Doctor: Let's Talk About Semantics](http://html5doctor.com/lets-talk-about-semantics/)
- [Schema.org](https://schema.org/)
