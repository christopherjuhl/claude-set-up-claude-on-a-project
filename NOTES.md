# Notes

## What's in CLAUDE.md, and what I left out

`CLAUDE.md` has a one-line description, the commands I actually run (dev, test, lint, plus how to run a single test file or a single named test), a short architecture section, and five conventions. The architecture section covers things that take reading several files to notice: `server.js` exports `app` and only listens when run directly, so tests use it without a port; each resource is its own router in `routes/`; and `db/store.js` keeps state in memory, so it resets on restart and is shared by every test in a run. The conventions are rules already followed in the code, written so Claude can apply them without asking: CommonJS not ES modules, data access only through `db/store.js`, errors as `{ error }` JSON with the right status code, tests with `node:test` and supertest, and one router file per new resource.

I left out the course instructions, the full file list, setup steps and anything about `.env` or secrets. Claude can see the files itself, and the course steps are one-off tasks, not guidance for working on the code. Every line in the file costs context in every session, so anything that wouldn't change what Claude writes didn't earn a place.

## Permission rules

- **Allow:** `npm test`, `node --test` and `npm run lint`. They only read the code and report results, and Claude runs them constantly, so prompting each time would just slow things down.
- **Ask:** `git push`, so nothing reaches GitHub without my OK, and `npm install`, because it changes dependencies and `package-lock.json`.
- **Deny:** reading `.env`, and `git push --force` / `git push -f`.

Without the `.env` deny rule, Claude could read real secrets into the conversation if `.env` ever held them, for example while debugging config. The rule is exact (`./.env`) so `.env.example` stays readable. It only blocks Claude's Read tool, though, so a shell command like `cat .env` would still need a separate rule or a manual check. Without the force-push rule, a single mistaken command could overwrite history on the shared branch.

## Verification

<!-- TODO: fill in after checking in a fresh session -->
- `/memory`: 
- `/permissions`: 
- Asked "How do I run the tests here?": 
