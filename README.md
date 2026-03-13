# AccessPoint

A seamless, modern login and registration system built with pure HTML, CSS, and JavaScript — no frameworks, no build tools, no dependencies.

![HTML](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)

## Features

- **User Registration** — name, email, password with real-time strength meter
- **User Login** — credential validation with clear error feedback
- **Password Recovery** — token-based reset flow with expiration
- **Session Management** — auto-expiring mock sessions with restore-on-reload
- **Dashboard** — simple user profile view after authentication
- **Responsive Design** — mobile-first layout with smooth transitions
- **Accessible** — semantic HTML, ARIA labels, focus-visible outlines
- **Secure by Default** — XSS sanitization, hashed passwords (mock), vague recovery messages to prevent user enumeration

## Repository Structure

```
AccessPoint/
├── index.html              Single-page app with all views
├── styles.css              Mobile-first responsive styles
├── script.js               Main controller (events, routing)
│
├── src/
│   ├── utils.js            Helpers (hashing, tokens, sanitize, debounce)
│   ├── storage.js          Mock user database (localStorage)
│   ├── validator.js        Email, password, name validation rules
│   ├── session.js          Mock session handler (sessionStorage)
│   ├── recovery.js         Password reset flow (token lifecycle)
│   ├── auth-controller.js  Register, login, logout logic
│   └── ui-controller.js    View switching, messages, DOM updates
│
├── assets/
│   └── favicon.svg         App favicon
│
├── docs/
│   ├── overview.md         System description
│   ├── architecture.md     Module map & data flow diagrams
│   ├── validation-rules.md Field validation rules reference
│   ├── recovery-flow.md    Password reset process explained
│   ├── customization.md    Theming, adding fields & views
│   └── integration.md      How to connect a real backend API
│
└── README.md
```

## Getting Started

1. **Clone the repository**

    ```bash
    git clone https://github.com/your-username/AccessPoint.git
    cd AccessPoint
    ```

2. **Open in a browser**

    ```bash
    # Any static file server works. For example:
    npx serve .
    # or simply open index.html directly in your browser
    ```

3. **Try it out**
    - Create an account on the registration page.
    - Log out and log back in.
    - Use "Forgot your password?" to test the reset flow.

No build step, no install — just open and go.

## Modules

| Module                 | Responsibility                                                                                                 |
| ---------------------- | -------------------------------------------------------------------------------------------------------------- |
| **utils.js**           | ID generation, mock password hashing/verification, token creation, date formatting, debounce, XSS sanitization |
| **storage.js**         | CRUD operations for users and reset tokens via `localStorage`                                                  |
| **validator.js**       | Validates email, password (with strength scoring), password confirmation, and display name                     |
| **session.js**         | Creates, reads, refreshes, and destroys sessions in `sessionStorage` (30-min expiry)                           |
| **recovery.js**        | Generates time-limited reset tokens, validates them, and updates passwords                                     |
| **auth-controller.js** | Orchestrates registration, login, logout, and current-user retrieval                                           |
| **ui-controller.js**   | Manages view transitions, field error display, global messages, password strength meter, and loading states    |

## Future Improvements

- [ ] **Real Backend Integration** — connect to a REST or GraphQL API (see [docs/integration.md](docs/integration.md))
- [ ] **JWT Authentication** — replace mock sessions with JSON Web Tokens
- [ ] **Two-Factor Authentication (2FA)** — TOTP-based second factor
- [ ] **User Roles & Permissions** — admin, editor, viewer role system
- [ ] **OAuth / Social Login** — Google, GitHub, Apple sign-in
- [ ] **Remember Me** — persistent sessions with secure refresh tokens
- [ ] **Account Settings** — change name, email, password from the dashboard
- [ ] **Rate Limiting** — brute-force protection on login and recovery
- [ ] **Internationalization (i18n)** — multi-language support
- [ ] **Accessibility Audit** — full WCAG 2.1 AA compliance

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).

```
MIT License

Copyright (c) 2025 AccessPoint Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
