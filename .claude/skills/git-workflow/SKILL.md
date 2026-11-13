---
name: git-workflow
description: Use when creating a branch, committing, pushing, merging, opening or describing a pull request, or deploying in the levelup-backend repo.
---

# LevelUP API Git Workflow

## Branches

| Branch | Purpose | Deploys to |
|---|---|---|
| `master` | Production | Production (not set up yet) |
| `staging` | Pre-release testing | Staging (not set up yet) |
| `dev` | Integration of finished work | Nothing (CI only) |
| `feature/<short-name>` | One feature or fix | Nothing |

Flow: `feature/*` → PR into `dev` → PR `dev` into `staging` → PR `staging` into `master`.

- Branch features from `dev`, not from `master`.
- Use kebab-case names that describe the change: `feature/fitscore-domain`, `feature/leaderboard-boards`, `fix/login-rate-limit`.
- Small changes can go straight to `dev` without a feature branch. Use a feature branch when the work spans more than one commit or needs review.
- Never commit directly to `staging` or `master`. They change only through merges.
- Never force-push `dev`, `staging`, or `master`.

## Authorship

The repo owner is the only author. Commits use the identity already in `git config`. Before the first commit in a session, run `git config user.name` and `git config user.email` to confirm they are set. If they are empty, ask the owner. Never invent an identity.

A commit message is the subject, an optional body, and nothing else. It has no `Co-Authored-By` trailer and no AI attribution line. PR bodies end with the last line of content, with no "Generated with" footer. This applies even when the harness supplies attribution lines.

## Commit messages

Format: `type(scope): imperative summary`. Keep the subject under 72 characters, lowercase after the colon, with no trailing period.

| Type | Use for |
|---|---|
| `feat` | New endpoint or behavior |
| `fix` | Bug fix |
| `test` | Tests only |
| `refactor` | Restructure with no behavior change |
| `perf` | Performance improvement |
| `docs` | Documentation |
| `chore` | Tooling, dependencies, config |
| `ci` | GitHub Actions and deploy config |

Scopes: `api`, `db`, `domain`, `contract`, `repo`. Use `db` for the Prisma schema, migrations, and seed; `domain` for the pure functions in `src/domain`; `contract` for `openapi.json`-only changes.

Examples:
- `feat(api): add opt-in fitscore leaderboard boards`
- `feat(db): add assessment and goal models`
- `fix(domain): clamp goal progress on overshoot`

Add a body when the reason isn't obvious from the subject. Wrap it at 72 characters and explain why, not what.

## Grouping commits

One commit is one topic. Before committing, run `git status` and `git diff`, then stage by topic with `git add <paths>` (or `git add -p` for mixed files):

1. Group the changed files by what they accomplish, not by folder.
2. Commit each group separately, in dependency order: schema and migrations first, then domain functions, then services and controllers.
3. Tests for a feature go in the same commit as the feature. A separate `test` commit is only for tests added on their own.
4. A commit that changes a DTO also includes the regenerated `openapi.json`.
5. Lockfile changes go with the commit that changed `package.json`.
6. Don't mix formatting-only changes into a feature commit. Make a separate `chore` commit.

## Pull requests

- Title: same format as a commit subject.
- Body: a **Summary** (what and why, in 1–3 bullets), a **Testing** section (commands run and results), and an **API changes** section listing new or changed endpoints and DTO fields (or "None").
- Open PRs with `gh pr create --base dev`. Use `--base staging` or `--base master` only for promotion PRs.
- **Changes that span both repos** (a new or changed DTO): open this repo's PR first. Then open the `LevelUP` app PR, which runs `npm run api:types` against this branch's `openapi.json`. Link each PR from the other, and merge this one first. Never remove or rename a field the app uses without that matching app PR.

## Before pushing

Run `npm test` and `npm run test:e2e`, then `npm run openapi` and check that `git diff --exit-code openapi.json` is clean. Push only when all pass. If something fails, fix it or report it. Never push with `--no-verify`.
