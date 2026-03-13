# Customization Guide

AccessPoint is designed to be easy to extend and restyle. This document covers the most common customization scenarios.

## Theming

All visual properties are controlled by CSS custom properties in `styles.css`. To change the look, edit the `:root` block:

```css
:root {
    --c-primary: #4f46e5; /* Main brand color */
    --c-primary-hover: #4338ca; /* Hover state */
    --c-primary-light: #e0e7ff; /* Light tint (focus rings, backgrounds) */
    --c-bg: #f4f6f9; /* Page background */
    --c-surface: #ffffff; /* Card background */
    --c-text: #1e293b; /* Primary text */
    --c-text-muted: #64748b; /* Secondary text */
    --c-error: #ef4444; /* Error color */
    --c-success: #22c55e; /* Success color */
    --radius-lg: 1rem; /* Card corner radius */
}
```

## Adding Fields

1. Add the HTML input inside the relevant `<form>` in `index.html` using the existing `.field` pattern.
2. Add a validation function in `src/validator.js`.
3. Call the validator inside the form handler in `script.js`.

Example — adding a "Phone number" field to registration:

```js
// validator.js
function validatePhone(phone) {
    if (!phone) return { valid: false, message: "Phone number is required." };
    if (!/^\+?[0-9\s\-()]{7,15}$/.test(phone)) {
        return { valid: false, message: "Enter a valid phone number." };
    }
    return { valid: true, message: "" };
}
```

## Adding Views

1. Create a new `<section class="view card" id="view-myview">` in `index.html`.
2. Register it in `UIController` by adding an entry to the `views` object in `src/ui-controller.js`.
3. Call `UIController.showView("myview")` from a navigation link or programmatically.

## Changing Session Duration

Edit the expiry constant in `src/session.js`:

```js
// Default: 30 minutes
session.expiresAt = Date.now() + 30 * 60 * 1000;

// Example: 2 hours
session.expiresAt = Date.now() + 2 * 60 * 60 * 1000;
```

## Changing Password Rules

Edit the `rules.password` object in `src/validator.js`:

```js
password: {
  minLength: 12,                    // raise minimum
  requireUppercase: /[A-Z]/,
  requireLowercase: /[a-z]/,
  requireNumber: /[0-9]/,
  requireSpecial: /[!@#$%^&*]/,    // narrow the allowed specials
}
```

## Replacing the Logo

The logo is an inline SVG inside each view's `.card__logo` div. Replace the `<svg>` element with your own SVG, an `<img>` tag, or a text-only logo.
