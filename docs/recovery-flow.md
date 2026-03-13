# Recovery Flow

This document describes how the password reset process works in AccessPoint.

## Overview

The flow has two phases:

1. **Request** — the user provides their email; the system generates a time-limited token.
2. **Reset** — the user provides the token and a new password; the system updates the account.

## Step-by-Step

### Phase 1: Request Reset

```
POST-equivalent: Recovery.requestReset(email)
```

1. User navigates to the **Reset password** view and enters their email.
2. `Recovery.requestReset(email)` is called:
    - Looks up the user via `Storage.findUserByEmail(email)`.
    - If the user **does not exist**, the system still returns a success message (`"If an account with that email exists, a reset link has been sent."`) to prevent user enumeration.
    - If the user **exists**, a random 32-character token is generated via `Utils.generateToken()`.
    - The token record is stored via `Storage.saveResetToken()` with:
        - `token` — the random string
        - `email` — the user's email
        - `userId` — the user's ID
        - `createdAt` — current timestamp
        - `expiresAt` — current timestamp + 15 minutes
    - Any previous token for the same email is replaced.
3. In a production system, the token would be sent via email. In this demo, it is displayed directly in the UI.
4. After a short delay, the user is automatically taken to the **New password** view with the token pre-filled.

### Phase 2: Reset Password

```
POST-equivalent: Recovery.resetPassword(token, newPassword)
```

1. User enters (or has pre-filled) the reset token along with a new password and confirmation.
2. `Recovery.resetPassword(token, newPassword)` is called:
    - `Recovery.validateToken(token)` checks:
        - Token exists in storage.
        - Token has not expired (`Date.now() <= expiresAt`).
    - `Validator.validatePassword(newPassword)` ensures the new password meets strength requirements.
    - `Storage.updateUser(userId, { passwordHash })` saves the new hashed password.
    - `Storage.removeResetToken(token)` invalidates the used token.
3. A success message is shown and the user is redirected to the login view after 1.5 seconds.

## Security Considerations

| Concern             | Mitigation                                                |
| ------------------- | --------------------------------------------------------- |
| User enumeration    | Same response message whether the email exists or not.    |
| Token expiry        | Tokens expire after 15 minutes.                           |
| Token reuse         | Token is deleted after successful reset.                  |
| One token per email | Requesting a new token invalidates the previous one.      |
| Brute-force         | In production, rate-limiting and CAPTCHA should be added. |

## Token Lifecycle

```
[Generated] ──15 min──▶ [Expired → auto-deleted on next validate]
     │
     ▼
 [Used → deleted immediately]
```
