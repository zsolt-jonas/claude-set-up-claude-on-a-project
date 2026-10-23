# Notes: setting up Claude Code on this project

## What's in CLAUDE.md, and what I left out

I kept the four parts the project asks for: a one-line description, the commands, the conventions and the architecture. The commands include how to run a single test file or a single named test, because that's the command Claude can't guess from `package.json`. The conventions are rules taken from the existing code and written as "use X, not Y":

- CommonJS, not ES modules.
- `node:test` + `supertest`, not Jest.
- One router per resource.
- Data access only through `db/store.js`.
- Errors as JSON `{ error }` with the right status code.
- Convert route IDs with `Number()`.

Each one stops a specific mistake Claude would otherwise be likely to make. The architecture section covers what you only see by reading several files together: `server.js` exports `app` and skips `listen()` so tests can import it, and the in-memory store is shared by every test in a run.

I left out the file listing, Express basics and generic advice like "write clean code", because Claude can work all of that out from the code. I also left out the course and submission instructions from the README, which are one-off tasks rather than rules for working in the repo. The only reference to secrets is a rule not to read or commit `.env`. There are no values in the file.

## Permission rules

- **Allow:** `npm test`, `npm run lint` and `node --test …`. These run constantly, only read code and don't touch the network, so confirming each one would just slow things down.
- **Ask:** `git push`. Pushing is visible to others and hard to undo, so I want to confirm every time.
- **Deny:** reading or editing `.env`, and `git push --force` / `git push -f`.

Without the deny rules, Claude could read `.env` while exploring the project. That would put real secrets (such as a database password) into its context, from where they could be repeated in output or end up in a commit. A force-push could overwrite shared history on the remote. Deny rules take priority over the ask rule, so a force-push is blocked outright rather than just confirmed. One limit: `Read(./.env)` only covers Claude's file-reading tool. A shell command like `cat .env` isn't covered by the deny rule. It still asks first, because it isn't allowed.

## Fix along the way

The starter `.gitignore` had spaces at the start of most lines. Git treats those spaces as part of the pattern, so `.env` and `.claude/settings.local.json` were **not** actually ignored. I removed the spaces and confirmed with `git check-ignore`.

## Verification

- `/memory`: I confirm CLAUDE.md shows as loaded
- `/permissions`: I can confirm the allow / ask / deny rules are listed
- Asked "How do I run the tests here?": I conform the answer came from CLAUDE.md

