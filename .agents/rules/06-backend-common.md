---
trigger: always_on
---

# Backend Rules (Universal)

Applies to all Express/Node work. Mongoose query-optimization detail lives in the `express-mongodb-backend` skill and loads automatically for DB/route work — do not duplicate it here.

## Route structure
- One router file per resource (e.g. `routes/themes.js`, `routes/portfolios.js`).
- Controllers live in `controllers/`, one file per resource, matching route names.
- No business logic inside route files — routes call controllers only.

## Error handling
- All async route handlers wrapped in a shared error-handling middleware — no repeated try/catch per route.
- Errors return a consistent shape: `{ error: { message, code } }`.

## Module System
- Use ES modules (`import`/`export`) everywhere — never `require`/`module.exports`.
- Set `"type": "module"` in `package.json`.
- Always include the file extension in relative imports: `import themeRouter from './routes/themes.js'`.
- For CommonJS-only packages: `import pkg from 'some-cjs-package'; const { thing } = pkg;`
- `__dirname`/`__filename` aren't available in ESM — derive them:
  ```js
  import { fileURLToPath } from 'url';
  import { dirname } from 'path';
  const __filename = fileURLToPath(import.meta.url);
  const __dirname = dirname(__filename);
  ```
- Dynamic imports (`await import(...)`) only where genuinely needed (conditional loading) — not as a default pattern.

## File size
- Same 120-line guidance as frontend — split large controllers into smaller service functions.
