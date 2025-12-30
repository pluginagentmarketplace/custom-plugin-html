# Accessibility Reference Guide

Production-grade WCAG 2.2 and ARIA implementation guide based on 2024-2025 standards.

## 📚 Table of Contents

1. [WCAG 2.2 Overview](#wcag-22-overview)
2. [Essential Checks](#essential-checks)
3. [ARIA Patterns](#aria-patterns)
4. [Keyboard Navigation](#keyboard-navigation)
5. [Forms Accessibility](#forms-accessibility)
6. [Media Accessibility](#media-accessibility)
7. [Testing Guide](#testing-guide)
8. [Troubleshooting](#troubleshooting)

---

## WCAG 2.2 Overview

### The Four Principles

| Principle | Meaning | Key Guidelines |
|-----------|---------|----------------|
| **Perceivable** | Can users perceive content? | Alt text, contrast, captions |
| **Operable** | Can users operate UI? | Keyboard, timing, navigation |
| **Understandable** | Can users understand? | Labels, errors, consistency |
| **Robust** | Works with assistive tech? | Valid HTML, ARIA |

### Conformance Levels

```
Level A   → Minimum accessibility (must have)
Level AA  → Standard requirement (legal baseline)
Level AAA → Enhanced accessibility (ideal)
```

### WCAG 2.2 New Criteria (AA Level)

| ID | Name | Requirement |
|----|------|-------------|
| 2.4.11 | Focus Not Obscured | Focus never completely hidden by sticky elements |
| 2.5.7 | Dragging Movements | Provide single-pointer alternative to drag |
| 2.5.8 | Target Size | Interactive targets ≥24×24px |
| 3.2.6 | Consistent Help | Help mechanisms in same location |
| 3.3.7 | Redundant Entry | Don't ask for same info twice |
| 3.3.8 | Accessible Auth | No cognitive tests for login |

---

## Essential Checks

### Quick Audit Checklist

```
PERCEIVABLE
□ All images have alt text (or alt="" for decorative)
□ Color contrast ≥4.5:1 for text, ≥3:1 for large text
□ Color not sole means of conveying info
□ Text resizable to 200% without breaking
□ Video has captions, audio has transcript

OPERABLE
□ All functionality available via keyboard
□ No keyboard traps
□ Skip link to main content
□ Focus order logical
□ Focus visible on all interactive elements
□ Touch targets ≥24×24px

UNDERSTANDABLE
□ Page language declared (<html lang="en">)
□ All form inputs have labels
□ Error messages are clear and helpful
□ Navigation consistent across pages

ROBUST
□ Valid HTML (no duplicate IDs)
□ ARIA used correctly
□ Works with screen readers
```

### Code Audit Patterns

#### Images

```html
<!-- ✅ Informative image -->
<img src="chart.png" alt="Sales increased 25% from Q1 to Q4 2024">

<!-- ✅ Decorative image -->
<img src="divider.png" alt="" role="presentation">

<!-- ✅ Complex image with long description -->
<figure>
  <img src="flowchart.png" alt="Development workflow diagram">
  <figcaption>
    Figure 1: The development workflow starts with planning,
    moves to development, then testing, and finally deployment.
  </figcaption>
</figure>

<!-- ❌ Bad: missing alt -->
<img src="photo.jpg">

<!-- ❌ Bad: unhelpful alt -->
<img src="chart.png" alt="image">
```

#### Buttons and Links

```html
<!-- ✅ Button with clear purpose -->
<button type="button">Add to Cart</button>

<!-- ✅ Icon button with label -->
<button type="button" aria-label="Close dialog">
  <svg aria-hidden="true">...</svg>
</button>

<!-- ✅ Link with context -->
<a href="/article">Read more about web accessibility</a>

<!-- ❌ Bad: empty button -->
<button><svg>...</svg></button>

<!-- ❌ Bad: non-descriptive link -->
<a href="/article">Click here</a>
```

#### Focus Management

```html
<!-- ✅ Custom focus styles -->
<style>
  :focus {
    outline: 2px solid #005fcc;
    outline-offset: 2px;
  }

  :focus:not(:focus-visible) {
    outline: none;
  }

  :focus-visible {
    outline: 2px solid #005fcc;
    outline-offset: 2px;
  }
</style>

<!-- ❌ Bad: removing focus without replacement -->
<style>
  *:focus { outline: none; }
</style>
```

---

## ARIA Patterns

### When to Use ARIA

```
First Rule of ARIA:
"Don't use ARIA if you can use native HTML"

Native HTML            vs    ARIA Equivalent
──────────────────────────────────────────────
<button>               >     <div role="button">
<a href>               >     <span role="link">
<input type="checkbox" >     <div role="checkbox">
<nav>                  >     <div role="navigation">
```

### Common ARIA Patterns

#### Disclosure (Show/Hide)

```html
<button aria-expanded="false" aria-controls="content-1">
  Show Details
</button>
<div id="content-1" hidden>
  Hidden content here...
</div>

<script>
button.addEventListener('click', () => {
  const expanded = button.getAttribute('aria-expanded') === 'true';
  button.setAttribute('aria-expanded', !expanded);
  content.hidden = expanded;
});
</script>
```

#### Alert/Status Messages

```html
<!-- Alert (interrupts) -->
<div role="alert">
  Error: Your session has expired
</div>

<!-- Status (polite, waits) -->
<div role="status">
  File saved successfully
</div>

<!-- Live region for dynamic content -->
<div aria-live="polite" aria-atomic="true">
  <p>3 items in your cart</p>
</div>
```

#### Modal Dialog

```html
<div role="dialog"
     aria-modal="true"
     aria-labelledby="dialog-title"
     aria-describedby="dialog-desc">

  <h2 id="dialog-title">Confirm Delete</h2>
  <p id="dialog-desc">This action cannot be undone.</p>

  <button type="button">Cancel</button>
  <button type="button">Delete</button>
</div>

<script>
// Focus management
dialog.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') closeDialog();
  if (e.key === 'Tab') trapFocus(e);
});

function openDialog() {
  dialog.hidden = false;
  firstFocusable.focus();
  document.body.style.overflow = 'hidden';
}

function closeDialog() {
  dialog.hidden = true;
  triggerButton.focus();  // Return focus
  document.body.style.overflow = '';
}
</script>
```

#### Tabs

```html
<div class="tabs">
  <div role="tablist" aria-label="Account Settings">
    <button role="tab"
            id="tab-1"
            aria-selected="true"
            aria-controls="panel-1">
      Profile
    </button>
    <button role="tab"
            id="tab-2"
            aria-selected="false"
            aria-controls="panel-2"
            tabindex="-1">
      Security
    </button>
    <button role="tab"
            id="tab-3"
            aria-selected="false"
            aria-controls="panel-3"
            tabindex="-1">
      Notifications
    </button>
  </div>

  <div role="tabpanel"
       id="panel-1"
       aria-labelledby="tab-1"
       tabindex="0">
    Profile content...
  </div>

  <div role="tabpanel"
       id="panel-2"
       aria-labelledby="tab-2"
       tabindex="0"
       hidden>
    Security content...
  </div>

  <div role="tabpanel"
       id="panel-3"
       aria-labelledby="tab-3"
       tabindex="0"
       hidden>
    Notifications content...
  </div>
</div>
```

---

## Keyboard Navigation

### Standard Keyboard Patterns

| Key | Action |
|-----|--------|
| Tab | Move to next focusable element |
| Shift+Tab | Move to previous focusable element |
| Enter | Activate links, buttons, submit forms |
| Space | Activate buttons, toggle checkboxes |
| Arrow keys | Navigate within widgets (tabs, menus, etc.) |
| Escape | Close dialogs, menus, dropdowns |
| Home/End | Jump to first/last item in list |

### Focus Order Best Practices

```html
<!-- ✅ Natural focus order follows visual order -->
<header>
  <nav>
    <a href="/">Home</a>        <!-- Focus 1 -->
    <a href="/about">About</a>  <!-- Focus 2 -->
  </nav>
</header>
<main>
  <h1>Welcome</h1>
  <button>Action</button>       <!-- Focus 3 -->
</main>

<!-- ❌ Avoid positive tabindex -->
<button tabindex="3">Bad</button>
<button tabindex="1">Order</button>
<button tabindex="2">Practice</button>
```

### Skip Links

```html
<!-- First element in body -->
<a href="#main-content" class="skip-link">
  Skip to main content
</a>

<header>...</header>

<main id="main-content" tabindex="-1">
  <!-- tabindex="-1" allows programmatic focus -->
</main>

<style>
.skip-link {
  position: absolute;
  left: -9999px;
  top: auto;
  width: 1px;
  height: 1px;
  overflow: hidden;
}

.skip-link:focus {
  position: fixed;
  top: 10px;
  left: 10px;
  width: auto;
  height: auto;
  padding: 1rem;
  background: #000;
  color: #fff;
  z-index: 9999;
}
</style>
```

---

## Forms Accessibility

### Complete Accessible Form

```html
<form aria-labelledby="form-title">
  <h2 id="form-title">Contact Us</h2>

  <!-- Required field with hint -->
  <div class="field">
    <label for="name">
      Full Name
      <span aria-hidden="true">*</span>
    </label>
    <input type="text"
           id="name"
           name="name"
           required
           aria-required="true"
           autocomplete="name">
  </div>

  <!-- Field with hint text -->
  <div class="field">
    <label for="email">Email</label>
    <input type="email"
           id="email"
           name="email"
           required
           aria-required="true"
           aria-describedby="email-hint"
           autocomplete="email">
    <p id="email-hint" class="hint">
      We'll never share your email
    </p>
  </div>

  <!-- Field with error -->
  <div class="field">
    <label for="phone">Phone</label>
    <input type="tel"
           id="phone"
           name="phone"
           aria-invalid="true"
           aria-errormessage="phone-error"
           autocomplete="tel">
    <p id="phone-error" class="error" role="alert">
      Please enter a valid phone number
    </p>
  </div>

  <!-- Radio group -->
  <fieldset>
    <legend>Preferred contact method</legend>
    <label>
      <input type="radio" name="contact" value="email" checked>
      Email
    </label>
    <label>
      <input type="radio" name="contact" value="phone">
      Phone
    </label>
  </fieldset>

  <!-- Checkbox -->
  <label class="checkbox">
    <input type="checkbox"
           name="newsletter"
           value="yes">
    Subscribe to newsletter
  </label>

  <button type="submit">Send Message</button>
</form>
```

### Error Handling

```html
<!-- Error summary at top of form -->
<div role="alert" aria-labelledby="error-summary">
  <h3 id="error-summary">Please fix the following errors:</h3>
  <ul>
    <li><a href="#email">Email is required</a></li>
    <li><a href="#phone">Phone number is invalid</a></li>
  </ul>
</div>

<!-- Individual field errors -->
<label for="email">Email</label>
<input type="email"
       id="email"
       aria-invalid="true"
       aria-errormessage="email-error">
<p id="email-error" class="error">
  Email is required
</p>
```

---

## Media Accessibility

### Images

```html
<!-- Informative -->
<img src="team.jpg" alt="Our team of 5 developers at the 2024 conference">

<!-- Decorative -->
<img src="border.png" alt="" role="presentation">

<!-- Complex with long description -->
<figure>
  <img src="chart.png"
       alt="Quarterly sales chart"
       aria-describedby="chart-desc">
  <figcaption id="chart-desc">
    Q1: $50K, Q2: $75K, Q3: $90K, Q4: $120K.
    Total growth of 140% year over year.
  </figcaption>
</figure>
```

### Video

```html
<video controls>
  <source src="video.mp4" type="video/mp4">
  <track kind="captions"
         src="captions-en.vtt"
         srclang="en"
         label="English"
         default>
  <track kind="descriptions"
         src="descriptions-en.vtt"
         srclang="en"
         label="Audio Descriptions">
</video>
```

### Audio

```html
<figure>
  <figcaption>Interview with CEO, June 2024</figcaption>
  <audio controls>
    <source src="interview.mp3" type="audio/mpeg">
  </audio>
  <a href="transcript.html">Read transcript</a>
</figure>
```

---

## Testing Guide

### Manual Testing Checklist

```
KEYBOARD TESTING
□ Navigate entire page using only Tab/Shift+Tab
□ All interactive elements focusable
□ Focus order logical
□ Focus visible at all times
□ Can activate all controls with Enter/Space
□ Can escape from modals/dropdowns
□ No keyboard traps

SCREEN READER TESTING
□ Page title announced
□ Landmarks navigable
□ Headings provide structure
□ Images have alt text announced
□ Form labels announced
□ Errors announced
□ Live regions update announced

ZOOM TESTING
□ Content readable at 200% zoom
□ No horizontal scroll at 400% zoom
□ Text doesn't overflow containers
□ Images don't obscure text

COLOR/CONTRAST
□ Content visible in high contrast mode
□ Information not conveyed by color alone
□ Text contrast ≥4.5:1
```

### Automated Testing Commands

```bash
# axe-core CLI
npx @axe-core/cli https://example.com

# pa11y
npx pa11y https://example.com

# Lighthouse accessibility audit
npx lighthouse https://example.com --only-categories=accessibility

# HTML validation
npx html-validate index.html
```

---

## Troubleshooting

### Common Issues Decision Tree

```
Screen reader not announcing element?
├─ Does element have accessible name?
│   └─ No → Add label, aria-label, or aria-labelledby
├─ Is aria-hidden="true" set?
│   └─ Yes → Remove aria-hidden
├─ Is element display:none or visibility:hidden?
│   └─ Yes → Use different hiding method
└─ Is role correct for element type?
    └─ No → Use correct native element or role

Keyboard can't reach element?
├─ Is element natively focusable?
│   └─ No → Add tabindex="0"
├─ Is tabindex="-1" set?
│   └─ Yes → Remove or change to 0
└─ Is element disabled?
    └─ Yes → Consider aria-disabled instead

Focus not visible?
├─ Is outline:none in CSS?
│   └─ Yes → Add custom focus styles
├─ Is element behind other content?
│   └─ Yes → Adjust z-index or layout
└─ Is browser's focus ring sufficient?
    └─ No → Add explicit :focus-visible styles
```

### Quick Fixes

| Issue | Fix |
|-------|-----|
| Missing alt | Add `alt="description"` |
| Low contrast | Use [contrast checker](https://webaim.org/resources/contrastchecker/) |
| No label | Add `<label for="id">` |
| Not keyboard accessible | Add `tabindex="0"` + key handlers |
| Focus not visible | Add `:focus` CSS |
| Skip link missing | Add as first body element |
| Lang missing | Add `lang="en"` to `<html>` |
| ARIA misuse | Prefer native HTML elements |

---

## 🔗 References

- [WCAG 2.2 Quick Reference](https://www.w3.org/WAI/WCAG22/quickref/)
- [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)
- [WebAIM Resources](https://webaim.org/resources/)
- [Deque University](https://dequeuniversity.com/)
- [A11y Project Checklist](https://www.a11yproject.com/checklist/)
