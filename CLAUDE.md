# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

A small Express REST API (users + health check) used as the starter project for a Claude Code course.

## Commands

- `npm run dev` — start the API with auto-reload on http://localhost:3000 (`PORT` overrides)
- `npm test` — run all tests (Node's built-in `node --test` runner)
- `node --test tests/users.test.js` — run one test file; add `--test-name-pattern="404"` to run matching tests only
- `npm run lint` — ESLint (`eslint:recommended`)

CI (`.github/workflows/ci.yml`) runs lint then test on Node 22 — both must pass.

## Architecture

- `server.js` builds the Express app, mounts each router under its path (`/users`, `/health`), and exports `app`. It only calls `app.listen` when run directly, so tests import `app` without opening a port.
- `routes/` holds one file per resource; each exports an `express.Router()` that `server.js` mounts.
- `db/store.js` is the only data layer: an in-memory array seeded with two users. State is module-level, so it resets on restart and is shared across tests in the same process.

## Conventions

- Use CommonJS (`require` / `module.exports`), not ES modules — ESLint is configured with `sourceType: "script"`.
- Routes access data only through functions exported from `db/store.js`; never touch the `users` array directly from a route.
- Return errors as JSON `{ error: "<message>" }` with the matching status code (400 for invalid input, 404 for missing resources).
- Write tests with `node:test` + `node:assert` and `supertest` against the exported `app`; don't add Jest/Mocha.
- Add a new resource as a new file in `routes/` and mount it in `server.js`.
