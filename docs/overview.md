# Overview

**AccessPoint** is a lightweight, client-side authentication system built with vanilla HTML, CSS, and JavaScript. It provides a complete user flow — registration, login, password recovery, and session management — without relying on any frameworks or external dependencies.

## Purpose

The project serves as:

- A **ready-to-use front-end auth UI** that can be connected to any backend API.
- A **learning resource** demonstrating modular JavaScript architecture, form validation, and session management patterns.
- A **starting point** for prototypes where full-stack auth is not yet needed.

## Key Principles

| Principle          | Description                                                                                                                       |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| **Simplicity**     | No build tools, no frameworks — just open `index.html` in a browser.                                                              |
| **Security-aware** | Follows best practices (no plaintext passwords, vague error messages on recovery, XSS sanitization) even in mock mode.            |
| **Modular**        | Each concern lives in its own file under `/src`. Swap out `storage.js` with a real API client and everything else stays the same. |
| **Responsive**     | Mobile-first CSS with smooth transitions and clear error states.                                                                  |

## What It Does

1. **Registration** — validates name, email, password strength, and confirmation; stores the user in `localStorage`.
2. **Login** — authenticates against the mock database and creates a session in `sessionStorage`.
3. **Password Recovery** — generates a time-limited reset token (displayed in the UI for demo purposes), validates it, and allows setting a new password.
4. **Dashboard** — shows the logged-in user's profile and provides a sign-out button.
5. **Session Management** — auto-expires after 30 minutes; restores session on page reload.
