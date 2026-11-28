# Rules for the Razom development agent

Also follow the Next.js rules in `AGENTS.md`.

## Before you write code
- Every task links to a spec in `/specs`. Read that spec and `specs/_core.md` first.
- If the task has no spec, or the spec is unclear or contradicts `_core.md`,
  STOP and ask in a comment. Do not guess.
- Use the terms exactly as defined in the `_core.md` glossary.

## While you work
- Change only the files the feature needs. No unrelated refactoring.
- Write one automated test per acceptance criterion. Name each test after its
  criterion ID, e.g. `AC-03-2: code passes to next person after 15 minutes`.
- Every edge case listed in the spec needs a test or a written reason why not.
- No secrets, keys or personal data in code, tests or commits.

## When you finish
- Open the PR into `dev`. Never into `release` or `main`.
- PR description must include:
  - Link to the spec
  - Checklist of every acceptance criterion: covered by test / manual check / not done
  - Anything in the spec you found unclear and how you interpreted it
- Do not merge your own PR.

## Environments
- `dev` branch → dev environment (integration, agent PRs land here)
- `release` branch → UAT environment (manual acceptance testing)
- `main` branch → production

## Testing
- Unit tests use Vitest. Put them next to the code (`src/**/name.test.ts`) or in `tests/`.
- Before pushing, run `npm run lint`, `npm run typecheck` and `npm test`. All must pass.
- If a test fails, fix the code, not the test — unless the test contradicts the spec;
  then say so in the PR.
- The "Tests" check runs on every PR and is required for merging.
