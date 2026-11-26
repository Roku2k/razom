# Razom delivery workflow

Humans own intent, specs and release decisions. The agent does the implementation.

## Environments

| Branch    | Environment | Who merges into it      | Purpose                                   |
|-----------|-------------|-------------------------|-------------------------------------------|
| `feature/*` | PR preview | —                     | Quick look at one feature in isolation    |
| `dev`     | Dev         | Valerii (agent PRs)     | Integration of all finished features      |
| `release` | UAT         | Valerii (PR from `dev`) | Acceptance testing of the next release    |
| `main`    | Production  | Valerii (PR from `release`) | Live users                            |

The agent never merges and never targets `release` or `main`.

## Steps

1. **Decision note** — `/decisions/NNN-title.md`: problem, evidence, options, choice.
2. **Spec** — `/specs/NN-feature.md` from `specs/_template.md`. Open a PR with only the spec.
3. **Spec review** — the agent reviews the spec automatically (`spec-review.yml`).
   Fix the spec until the verdict is "ready for implementation", then merge the spec.
4. **Implementation** — open an issue: `@claude implement specs/NN-feature.md`.
   The agent branches from `dev`, writes code and tests, and opens a PR into `dev`.
5. **Checks + preview** — tests run on the PR; Vercel deploys a preview.
   Review the code and the AC checklist in the PR. Ask for fixes with `@claude ...` comments.
6. **Merge to dev** — the feature joins the dev environment. Check it works with other features.
7. **Cut a release** — PR `dev → release`. The UAT environment updates.
8. **UAT** — test every acceptance criterion of every feature in the release on UAT.
   Bugs: open an issue with `@claude fix ...` → fix goes to `dev` → re-promote to `release`.
9. **Release to production** — PR `release → main` with release notes. Merge = deploy.
   Smoke-test the main flows on production.
10. **Measure & learn** — check the spec's success metrics after release.
    Add a line to `/evals/log.md` for each feature.

## Hotfix (production bug)
Branch `hotfix/*` from `main` → PR into `main` → after merge, merge `main` back into
`release` and `dev` so nothing is lost.

## Rollback
Vercel dashboard → Deployments → promote the previous production deployment.

## Eval log format (`/evals/log.md`)
| Feature | Spec review issues | Passed AC first try? | Agent mistakes | Spec mistakes | Template change |
|---------|-------------------|----------------------|----------------|---------------|-----------------|
