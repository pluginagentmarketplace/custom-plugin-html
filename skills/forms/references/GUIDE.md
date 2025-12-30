# Forms Reference Guide

Production-grade HTML5 forms and Constraint Validation API guide based on 2024-2025 best practices.

## 📚 Table of Contents

1. [Form Fundamentals](#form-fundamentals)
2. [Input Types](#input-types)
3. [Validation](#validation)
4. [Accessible Forms](#accessible-forms)
5. [Common Patterns](#common-patterns)
6. [JavaScript Validation](#javascript-validation)
7. [Troubleshooting](#troubleshooting)

---

## Form Fundamentals

### Basic Form Structure

```html
<form id="my-form"
      action="/api/submit"
      method="POST"
      enctype="multipart/form-data"
      novalidate
      aria-labelledby="form-title">

  <h2 id="form-title">Form Title</h2>

  <!-- Fields go here -->

  <button type="submit">Submit</button>
</form>
```

### Form Attributes

| Attribute | Purpose | Values |
|-----------|---------|--------|
| `action` | Submission URL | URL or empty for same page |
| `method` | HTTP method | `GET`, `POST` |
| `enctype` | Encoding type | `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain` |
| `novalidate` | Disable browser validation | Boolean |
| `autocomplete` | Autofill behavior | `on`, `off` |
| `name` | Form identifier | String |
| `target` | Response target | `_self`, `_blank`, etc. |

### When to Use Each Method

```
GET:
✅ Search forms
✅ Filter forms
✅ Bookmarkable results
❌ Sensitive data
❌ Large data

POST:
✅ Login/Registration
✅ File uploads
✅ Sensitive data
✅ Modifying data
❌ Bookmarkable results
```

---

## Input Types

### Text-Based Inputs

```html
<!-- Standard text -->
<input type="text" name="username">

<!-- Email with built-in validation -->
<input type="email" name="email">

<!-- Password (masked) -->
<input type="password" name="password" minlength="8">

<!-- Phone (numeric keyboard on mobile) -->
<input type="tel" name="phone" pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}">

<!-- URL with validation -->
<input type="url" name="website">

<!-- Search (with clear button) -->
<input type="search" name="q" placeholder="Search...">
```

### Numeric Inputs

```html
<!-- Number with constraints -->
<input type="number"
       name="quantity"
       min="1"
       max="100"
       step="1"
       value="1">

<!-- Range slider -->
<input type="range"
       name="rating"
       min="0"
       max="10"
       step="1"
       value="5">
<output for="rating">5</output>
```

### Date/Time Inputs

```html
<!-- Date picker -->
<input type="date"
       name="birthdate"
       min="1900-01-01"
       max="2025-12-31">

<!-- Time picker -->
<input type="time"
       name="appointment"
       min="09:00"
       max="17:00"
       step="1800"> <!-- 30 min increments -->

<!-- Date + Time -->
<input type="datetime-local"
       name="meeting"
       min="2025-01-01T00:00"
       max="2025-12-31T23:59">

<!-- Month picker -->
<input type="month" name="card_expiry">

<!-- Week picker -->
<input type="week" name="delivery_week">
```

### Special Inputs

```html
<!-- Color picker -->
<input type="color" name="theme_color" value="#3498db">

<!-- File upload -->
<input type="file"
       name="document"
       accept=".pdf,.doc,.docx"
       multiple>

<!-- Hidden field -->
<input type="hidden" name="csrf_token" value="abc123">
```

### Selection Inputs

```html
<!-- Single checkbox -->
<label>
  <input type="checkbox" name="newsletter" value="yes">
  Subscribe to newsletter
</label>

<!-- Checkbox group (in fieldset) -->
<fieldset>
  <legend>Interests</legend>
  <label><input type="checkbox" name="interests" value="tech"> Tech</label>
  <label><input type="checkbox" name="interests" value="sports"> Sports</label>
  <label><input type="checkbox" name="interests" value="music"> Music</label>
</fieldset>

<!-- Radio group (in fieldset) -->
<fieldset>
  <legend>Payment Method</legend>
  <label><input type="radio" name="payment" value="card" checked> Credit Card</label>
  <label><input type="radio" name="payment" value="paypal"> PayPal</label>
  <label><input type="radio" name="payment" value="bank"> Bank Transfer</label>
</fieldset>

<!-- Dropdown -->
<label for="country">Country</label>
<select id="country" name="country" required>
  <option value="">Select a country</option>
  <option value="us">United States</option>
  <option value="uk">United Kingdom</option>
  <option value="ca">Canada</option>
</select>

<!-- Datalist (autocomplete suggestions) -->
<label for="browser">Browser</label>
<input list="browsers" id="browser" name="browser">
<datalist id="browsers">
  <option value="Chrome">
  <option value="Firefox">
  <option value="Safari">
  <option value="Edge">
</datalist>
```

---

## Validation

### HTML5 Validation Attributes

```html
<!-- Required field -->
<input type="text" name="name" required>

<!-- Pattern matching -->
<input type="text"
       name="username"
       pattern="[a-zA-Z0-9_]{3,20}"
       title="3-20 alphanumeric characters or underscores">

<!-- Length constraints -->
<input type="text"
       name="bio"
       minlength="10"
       maxlength="500">

<!-- Numeric constraints -->
<input type="number"
       name="age"
       min="18"
       max="120"
       step="1">

<!-- Multiple values -->
<input type="email" name="cc" multiple>

<!-- Accept specific files -->
<input type="file" name="image" accept="image/*">
```

### Common Validation Patterns

```html
<!-- US Phone -->
<input type="tel"
       name="phone"
       pattern="\d{3}-\d{3}-\d{4}"
       placeholder="123-456-7890"
       title="Format: 123-456-7890">

<!-- US ZIP Code -->
<input type="text"
       name="zip"
       pattern="\d{5}(-\d{4})?"
       placeholder="12345 or 12345-6789">

<!-- Strong Password -->
<input type="password"
       name="password"
       pattern="(?=.*\d)(?=.*[a-z])(?=.*[A-Z])(?=.*[@$!%*?&]).{8,}"
       title="8+ chars with uppercase, lowercase, number, and special char">

<!-- Credit Card -->
<input type="text"
       name="card"
       pattern="\d{13,19}"
       inputmode="numeric"
       autocomplete="cc-number">
```

### CSS Validation States

```css
/* Valid field */
input:valid {
  border-color: green;
}

/* Invalid field */
input:invalid {
  border-color: red;
}

/* Required field */
input:required {
  border-left: 3px solid orange;
}

/* Optional field */
input:optional {
  border-left: 3px solid gray;
}

/* User has interacted */
input:user-invalid {
  border-color: red;
  background-color: #ffe6e6;
}

/* In range */
input:in-range {
  border-color: green;
}

/* Out of range */
input:out-of-range {
  border-color: red;
}

/* Placeholder shown */
input:placeholder-shown {
  font-style: italic;
}
```

---

## Accessible Forms

### Label Association

```html
<!-- Method 1: for/id (preferred) -->
<label for="email">Email</label>
<input type="email" id="email" name="email">

<!-- Method 2: Wrapping -->
<label>
  Email
  <input type="email" name="email">
</label>

<!-- Method 3: aria-labelledby -->
<span id="email-label">Email Address</span>
<input type="email" aria-labelledby="email-label">

<!-- Method 4: aria-label (visually hidden) -->
<input type="search" aria-label="Search products">
```

### Error Messages

```html
<!-- Before submission -->
<div class="field">
  <label for="email">Email</label>
  <input type="email"
         id="email"
         name="email"
         required
         aria-describedby="email-hint">
  <p id="email-hint" class="hint">We'll never share your email</p>
</div>

<!-- After validation error -->
<div class="field has-error">
  <label for="email">Email</label>
  <input type="email"
         id="email"
         name="email"
         required
         aria-invalid="true"
         aria-describedby="email-hint email-error">
  <p id="email-hint" class="hint">We'll never share your email</p>
  <p id="email-error" class="error" role="alert">
    Please enter a valid email address
  </p>
</div>
```

### Error Summary

```html
<div id="error-summary"
     role="alert"
     aria-labelledby="error-heading"
     tabindex="-1">
  <h3 id="error-heading">There are 2 errors in this form</h3>
  <ul>
    <li><a href="#email">Email address is required</a></li>
    <li><a href="#password">Password is too short</a></li>
  </ul>
</div>
```

### Required Fields

```html
<!-- Visual + ARIA indicator -->
<label for="name">
  Full Name
  <span class="required" aria-hidden="true">*</span>
</label>
<input type="text"
       id="name"
       name="name"
       required
       aria-required="true">

<!-- Legend for required fields -->
<p class="form-legend">
  Fields marked with <span aria-hidden="true">*</span>
  <span class="visually-hidden">asterisk</span>
  are required.
</p>
```

### Autocomplete

```html
<!-- Personal info -->
<input type="text" name="name" autocomplete="name">
<input type="email" name="email" autocomplete="email">
<input type="tel" name="phone" autocomplete="tel">

<!-- Address -->
<input type="text" name="address" autocomplete="street-address">
<input type="text" name="city" autocomplete="address-level2">
<input type="text" name="state" autocomplete="address-level1">
<input type="text" name="zip" autocomplete="postal-code">
<select name="country" autocomplete="country">...</select>

<!-- Payment -->
<input type="text" name="cc_name" autocomplete="cc-name">
<input type="text" name="cc_number" autocomplete="cc-number" inputmode="numeric">
<input type="text" name="cc_exp" autocomplete="cc-exp">
<input type="text" name="cc_csc" autocomplete="cc-csc" inputmode="numeric">

<!-- Credentials -->
<input type="text" name="username" autocomplete="username">
<input type="password" name="password" autocomplete="current-password">
<input type="password" name="new_password" autocomplete="new-password">
<input type="text" name="otp" autocomplete="one-time-code" inputmode="numeric">
```

---

## Common Patterns

### Contact Form

```html
<form action="/api/contact" method="POST" aria-labelledby="contact-title">
  <h2 id="contact-title">Contact Us</h2>

  <div class="field">
    <label for="name">Name <span aria-hidden="true">*</span></label>
    <input type="text" id="name" name="name" required autocomplete="name">
  </div>

  <div class="field">
    <label for="email">Email <span aria-hidden="true">*</span></label>
    <input type="email" id="email" name="email" required autocomplete="email">
  </div>

  <div class="field">
    <label for="subject">Subject</label>
    <input type="text" id="subject" name="subject">
  </div>

  <div class="field">
    <label for="message">Message <span aria-hidden="true">*</span></label>
    <textarea id="message" name="message" rows="5" required minlength="10"></textarea>
  </div>

  <button type="submit">Send Message</button>
</form>
```

### Login Form

```html
<form action="/login" method="POST" aria-labelledby="login-title">
  <h2 id="login-title">Sign In</h2>

  <div class="field">
    <label for="username">Email or Username</label>
    <input type="text" id="username" name="username" required autocomplete="username" autofocus>
  </div>

  <div class="field">
    <label for="password">Password</label>
    <input type="password" id="password" name="password" required autocomplete="current-password">
    <a href="/forgot-password" class="field-link">Forgot password?</a>
  </div>

  <div class="field">
    <label class="checkbox">
      <input type="checkbox" name="remember" value="1">
      Remember me
    </label>
  </div>

  <button type="submit">Sign In</button>
</form>
```

### File Upload

```html
<form action="/upload" method="POST" enctype="multipart/form-data">
  <div class="field">
    <label for="documents">Upload Documents</label>
    <input type="file"
           id="documents"
           name="documents"
           accept=".pdf,.doc,.docx"
           multiple
           aria-describedby="file-hint">
    <p id="file-hint" class="hint">PDF or Word documents, max 10MB each</p>
  </div>

  <button type="submit">Upload</button>
</form>
```

---

## JavaScript Validation

### Constraint Validation API

```javascript
const form = document.querySelector('form');
const inputs = form.querySelectorAll('input, textarea, select');

// Disable native validation UI
form.noValidate = true;

form.addEventListener('submit', (e) => {
  let isValid = true;

  inputs.forEach(input => {
    if (!validateInput(input)) {
      isValid = false;
    }
  });

  if (!isValid) {
    e.preventDefault();
    showErrorSummary();
    focusFirstError();
  }
});

// Validate on blur
inputs.forEach(input => {
  input.addEventListener('blur', () => validateInput(input));
});

function validateInput(input) {
  const errorEl = document.getElementById(`${input.id}-error`);

  if (input.checkValidity()) {
    clearError(input, errorEl);
    return true;
  }

  const message = getErrorMessage(input);
  showError(input, errorEl, message);
  return false;
}

function getErrorMessage(input) {
  const v = input.validity;

  if (v.valueMissing) return `${getLabel(input)} is required`;
  if (v.typeMismatch) return `Please enter a valid ${input.type}`;
  if (v.patternMismatch) return input.dataset.error || 'Invalid format';
  if (v.tooShort) return `Minimum ${input.minLength} characters`;
  if (v.tooLong) return `Maximum ${input.maxLength} characters`;
  if (v.rangeUnderflow) return `Minimum value is ${input.min}`;
  if (v.rangeOverflow) return `Maximum value is ${input.max}`;

  return input.validationMessage;
}

function getLabel(input) {
  const label = document.querySelector(`label[for="${input.id}"]`);
  return label ? label.textContent.replace('*', '').trim() : 'This field';
}

function showError(input, errorEl, message) {
  input.setAttribute('aria-invalid', 'true');
  if (errorEl) {
    errorEl.textContent = message;
    errorEl.hidden = false;
  }
}

function clearError(input, errorEl) {
  input.removeAttribute('aria-invalid');
  if (errorEl) {
    errorEl.textContent = '';
    errorEl.hidden = true;
  }
}
```

### Custom Validation

```javascript
// Cross-field validation (password confirmation)
const password = document.getElementById('password');
const confirm = document.getElementById('password-confirm');

confirm.addEventListener('input', () => {
  if (password.value !== confirm.value) {
    confirm.setCustomValidity('Passwords do not match');
  } else {
    confirm.setCustomValidity('');
  }
});

// Async validation (check username availability)
const username = document.getElementById('username');
let debounceTimer;

username.addEventListener('input', () => {
  clearTimeout(debounceTimer);
  debounceTimer = setTimeout(async () => {
    const response = await fetch(`/api/check-username?u=${username.value}`);
    const { available } = await response.json();

    if (!available) {
      username.setCustomValidity('Username is already taken');
    } else {
      username.setCustomValidity('');
    }

    validateInput(username);
  }, 300);
});
```

---

## Troubleshooting

### Decision Tree

```
Form not submitting?
├─ JavaScript error in console?
│   └─ Yes → Fix the JS error
├─ Required fields empty?
│   └─ Yes → Fill required fields
├─ Validation failing?
│   └─ Yes → Check validation attributes
└─ Form action correct?
    └─ No → Fix action URL

Validation not working?
├─ novalidate attribute set?
│   └─ Yes → Handle validation in JS
├─ Pattern syntax correct?
│   └─ No → Fix regex
├─ Type attribute correct?
│   └─ No → Use correct type
└─ JS preventing default?
    └─ Yes → Check JS validation logic

Autocomplete not working?
├─ autocomplete attribute set?
│   └─ No → Add correct value
├─ Name attribute present?
│   └─ No → Add name
├─ Form has action?
│   └─ No → Add action
└─ Browser blocking?
    └─ Yes → Check browser settings
```

### Quick Fixes

| Issue | Solution |
|-------|----------|
| No keyboard on mobile | Use correct `type` or `inputmode` |
| Browser validation ugly | Add `novalidate`, use JS |
| Pattern not matching | Test regex at regex101.com |
| Autofill wrong data | Use specific `autocomplete` values |
| File type bypass | Validate on server too |
| Required not working | Check for whitespace-only values |

---

## 🔗 References

- [MDN Form Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form)
- [Constraint Validation API](https://developer.mozilla.org/en-US/docs/Web/HTML/Constraint_validation)
- [HTML Autocomplete Values](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete)
- [WAI Forms Tutorial](https://www.w3.org/WAI/tutorials/forms/)
- [Web.dev Learn Forms](https://web.dev/learn/forms/)
