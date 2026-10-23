# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm run dev`: start the API on http://localhost:3000 with auto-reload
- `npm test`: run all tests (Node's built-in `node:test` runner)
- `node --test tests/users.test.js`: run one test file; add `--test-name-pattern="404"` to run matching tests only

## Conventions

- Write tests with `node:test` + `node:assert` + `supertest`, not Jest or Mocha. Import `app` from `../server` and never call `listen()` in tests.
- Keep one route file per resource in `routes/`, each exporting an `express.Router()` that `server.js` mounts under its path.
- Routes must not touch data directly. Add or extend functions in `db/store.js` and call those.
- Return errors as JSON `{ error: "<message>" }` with the matching status (400 for invalid input, 404 for not found). Successful creates return 201.
- Route params arrive as strings. Convert IDs with `Number(...)` before looking them up, because the store compares with `===`.

## Architecture

- Config comes from environment variables (see `.env.example`). Never read or commit `.env`.
- "db/store.js" is the single data-access layer. It is a module-level in-memory array that resets on restart and is shared across all tests in a process, so tests must not assume an exact user count.
- "node_modules" contains the modules for NodeJS
- "routes/<resource>.js" holds the HTTP handling and validation for one resource.
- "tests" contains the test cases 

