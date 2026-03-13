# Validation Rules

All validation logic lives in `src/validator.js`. The module exposes four functions, each returning `{ valid: boolean, message: string }`.

## Email

| Rule     | Value                                  |
| -------- | -------------------------------------- |
| Required | Yes                                    |
| Pattern  | `/^[^\s@]+@[^\s@]+\.[^\s@]+$/`         |
| Trimmed  | Yes (whitespace stripped before check) |

**Error messages:**

- `"Email is required."`
- `"Please enter a valid email address."`

## Password

| Rule              | Value                                       |
| ----------------- | ------------------------------------------- | ----------- |
| Required          | Yes                                         |
| Minimum length    | 8 characters                                |
| Uppercase letter  | At least 1 (`/[A-Z]/`)                      |
| Lowercase letter  | At least 1 (`/[a-z]/`)                      |
| Number            | At least 1 (`/[0-9]/`)                      |
| Special character | At least 1 (`/[!@#$%^&\*()\_+\-=[\]{};':"\\ | ,.<>\/?]/`) |

**Strength score:** 0–5 (one point per satisfied rule). Displayed as a visual meter during registration.

| Score | Label     |
| ----- | --------- |
| 0     | _(empty)_ |
| 1     | Weak      |
| 2     | Fair      |
| 3     | Good      |
| 4     | Strong    |
| 5     | Excellent |

## Confirm Password

| Rule       | Value                   |
| ---------- | ----------------------- |
| Required   | Yes                     |
| Must match | Original password field |

**Error messages:**

- `"Please confirm your password."`
- `"Passwords do not match."`

## Name

| Rule               | Value                                                       |
| ------------------ | ----------------------------------------------------------- |
| Required           | Yes                                                         |
| Length             | 2–50 characters                                             |
| Allowed characters | Letters, spaces, hyphens, apostrophes (`/^[a-zA-Z\s'-]+$/`) |

**Error message:**

- `"Name is required."`
- `"Name must be 2-50 characters and contain only letters, spaces, hyphens, or apostrophes."`

## Real-Time Feedback

- Password strength meter updates on every keystroke (debounced at 150 ms).
- Field errors clear when the user focuses the input again.
- Full validation runs on form submit; per-field errors are displayed inline.
