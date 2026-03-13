# Architecture

## Module Map

```
index.html          Entry point — contains all view templates
styles.css          Global styles (mobile-first, BEM-like naming)
script.js           Main controller — wires events to modules

src/
├── utils.js            Helpers (hashing, tokens, sanitize, debounce)
├── storage.js          Mock database layer (localStorage CRUD)
├── validator.js        Input validation rules
├── session.js          Session create / get / destroy (sessionStorage)
├── recovery.js         Password reset flow (token generation & validation)
├── auth-controller.js  Core auth operations (register, login, logout)
└── ui-controller.js    DOM manipulation, view switching, messages
```

## Data Flow

### Registration

```
User fills form
  → script.js captures submit
    → Validator checks each field
      → AuthController.register()
        → Storage.createUser()          (localStorage)
        → Session.create()              (sessionStorage)
          → UIController.showView("dashboard")
```

### Login

```
User fills form
  → script.js captures submit
    → AuthController.login()
      → Storage.findUserByEmail()
      → Utils.verifyPassword()
      → Session.create()
        → UIController.showView("dashboard")
```

### Password Recovery

```
Step 1 — Request reset:
  User enters email
    → Recovery.requestReset()
      → Storage.findUserByEmail()
      → Utils.generateToken()
      → Storage.saveResetToken()
        → Token shown in UI (demo only)

Step 2 — Reset password:
  User pastes token + new password
    → Recovery.resetPassword()
      → Recovery.validateToken()
      → Validator.validatePassword()
      → Storage.updateUser()
      → Storage.removeResetToken()
        → UIController.showView("login")
```

## Dependencies Between Modules

```
script.js
  ├── UIController   (ui-controller.js)
  ├── AuthController (auth-controller.js)
  │     ├── Validator
  │     ├── Storage
  │     ├── Session
  │     └── Utils
  ├── Recovery       (recovery.js)
  │     ├── Validator
  │     ├── Storage
  │     └── Utils
  └── Validator      (real-time field checks)
```

All modules are IIFEs exposing a single global object. No import/export — script order in `index.html` handles dependency resolution.
