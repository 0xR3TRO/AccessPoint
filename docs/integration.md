# Backend Integration Guide

AccessPoint ships as a front-end-only demo using `localStorage` and `sessionStorage`. This document explains how to replace the mock layer with real API calls.

## What to Replace

| Mock Module            | Replace With                                      |
| ---------------------- | ------------------------------------------------- |
| `src/storage.js`       | REST/GraphQL API client                           |
| `src/session.js`       | HTTP-only cookie or JWT-based session             |
| `src/recovery.js`      | Server-side token generation + email delivery     |
| `Utils.hashPassword()` | Remove entirely — hashing must happen server-side |

## Step-by-Step

### 1. Create an API Client

Create a new file `src/api.js` that wraps `fetch`:

```js
const API = (() => {
    const BASE = "https://api.example.com";

    async function post(path, body) {
        const res = await fetch(BASE + path, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            credentials: "include", // send cookies
            body: JSON.stringify(body),
        });
        return res.json();
    }

    async function get(path) {
        const res = await fetch(BASE + path, { credentials: "include" });
        return res.json();
    }

    return { post, get };
})();
```

### 2. Update `auth-controller.js`

Replace direct `Storage` / `Session` calls with API calls:

```js
async function register({ name, email, password, confirmPassword }) {
    // Client-side validation stays the same
    // ...

    const result = await API.post("/auth/register", { name, email, password });
    if (!result.success) return result;

    // Session is now managed by the server (HTTP-only cookie)
    return result;
}

async function login({ email, password }) {
    const result = await API.post("/auth/login", { email, password });
    return result;
}

async function logout() {
    await API.post("/auth/logout");
}

async function getCurrentUser() {
    return API.get("/auth/me");
}
```

### 3. Update `recovery.js`

```js
async function requestReset(email) {
    return API.post("/auth/forgot-password", { email });
    // Server sends the email — no token displayed in the UI
}

async function resetPassword(token, newPassword) {
    return API.post("/auth/reset-password", { token, newPassword });
}
```

### 4. Update `script.js`

Add `async/await` to form handlers since API calls are now asynchronous. The existing `setTimeout` wrappers can be removed.

### 5. Handle Tokens (JWT)

If using JWTs instead of cookies:

```js
// session.js — store token in memory (NOT localStorage for security)
let accessToken = null;

function setToken(token) {
    accessToken = token;
}
function getToken() {
    return accessToken;
}
function clear() {
    accessToken = null;
}
```

Add an `Authorization` header in your API client:

```js
headers: {
  "Content-Type": "application/json",
  "Authorization": "Bearer " + Session.getToken(),
}
```

## Expected API Endpoints

| Method | Endpoint                | Body                        | Response                         |
| ------ | ----------------------- | --------------------------- | -------------------------------- |
| POST   | `/auth/register`        | `{ name, email, password }` | `{ success, message, user }`     |
| POST   | `/auth/login`           | `{ email, password }`       | `{ success, message, user }`     |
| POST   | `/auth/logout`          | —                           | `{ success }`                    |
| GET    | `/auth/me`              | —                           | `{ id, name, email, createdAt }` |
| POST   | `/auth/forgot-password` | `{ email }`                 | `{ success, message }`           |
| POST   | `/auth/reset-password`  | `{ token, newPassword }`    | `{ success, message }`           |

## Security Checklist

- [ ] Hash passwords server-side with bcrypt or Argon2.
- [ ] Use HTTPS everywhere.
- [ ] Set `HttpOnly`, `Secure`, and `SameSite` flags on session cookies.
- [ ] Rate-limit auth endpoints.
- [ ] Add CSRF protection.
- [ ] Validate and sanitize all inputs on the server.
- [ ] Log authentication events for auditing.
