# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Starter Express 4 API (CommonJS, Node 22) exposing `/users` and `/health`, backed by an in-memory store.

## Commands

- Run the app: `npm start` (or `node server.js`); listens on `PORT` (default 3000)
- Dev with auto-restart: `npm run dev`
- Test all: `npm test` (runs `node --test` with the built-in `node:test` runner, not Jest/Mocha)
- Run one test file: `node --test tests/users.test.js`
- Run tests matching a name: `node --test --test-name-pattern="404"`
- Lint: `npm run lint` (ESLint 8, `eslint:recommended`)

CI (`.github/workflows/ci.yml`) runs `npm run lint` then `npm test` on every push and PR.

## Conventions

- CommonJS only (`require` / `module.exports`); ESLint is configured with `sourceType: "script"`, so no `import`/`export`.
- Unused args must be named `_`, `req`, `res`, or `next` to avoid lint warnings.
- Tests use `node:test` + `node:assert` + `supertest` against the exported `app`; never start a real port in tests.

## Architecture

- `server.js` builds the Express app, mounts routers (`/users`, `/health`), and exports `app`. It only calls `listen` when run directly (`require.main === module`), so tests can import it without opening a port.
- `routes/` holds one router file per resource, each mounted by path prefix in `server.js`.
- `db/store.js` is the data layer: a module-level in-memory array with a `nextId` counter. Routes access data only through its functions (`getAllUsers`, `getUserById`, `createUser`), never directly. State is not persisted, resets on restart, and is shared across tests in the same process.
- Error responses are JSON of the form `{ error: "message" }` (404 for missing user, 400 for missing `name`/`email`).
- `.env` is git-ignored; `.env.example` documents `PORT` only.
