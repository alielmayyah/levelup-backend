# LevelUP API

Backend for the LevelUP white-label gym member app: NestJS + Prisma + PostgreSQL, at the repo root. The Ionic app lives in a separate repo: [LevelUP](https://github.com/alielmayyah/LevelUP). That repo also holds the spec, plans, and backlog for both repos.

- Spec: [LevelUP-Features-Core.md](https://github.com/alielmayyah/LevelUP/blob/master/LevelUP-Features-Core.md)
- Local layout: clone both repos into one workspace folder, `LevelUP/levelup-backend` (this repo) and `LevelUP/levelup-frontend` (the app repo, `alielmayyah/LevelUP`).
- Plan: [2026-11-13-levelup-core.md](https://github.com/alielmayyah/LevelUP/blob/master/docs/superpowers/plans/2026-11-13-levelup-core.md), **Phase 1 (Tasks 1–9)** runs in this repo. Read its Global Constraints and Decisions before any task.

## Rules for every task

1. **Contract first.** Every request and response body is a DTO class with `@ApiProperty` decorators. Nullable fields use `nullable: true`, and string unions use `enum`. Any DTO change runs `npm run openapi` and commits `openapi.json` in the same commit. The app generates its types from that file.
2. **Don't break the app.** Never remove or rename a response field the app uses unless a matching `LevelUP` PR is ready. See the `git-workflow` skill for cross-repo PRs.
3. **Privacy.** Every member query filters by the `memberId` from the JWT. Another member's FitScore appears only in leaderboard rows, and only when that member opted in. Return explicit DTOs; never spread a Prisma model into a response.
4. **Scope.** The 17 core features only. No XP, levels, streaks, workout or visit tracking, achievements, trophies, challenges, or community features: no columns, fields, or endpoints for them.
5. **Time.** Month and day boundaries use the `GYM_TZ` env var (IANA zone, default `UTC`).
6. **Lean queries.** Select only the fields a DTO needs (Prisma `select`), add indexes for every `where` and `orderBy` column, and avoid N+1 queries by fetching related rows in the same query.
7. **Tested.** Domain functions in `src/domain` have Jest unit tests. Every endpoint has a supertest e2e test, including the 401, 400, and empty-data cases. Write the failing test first, as the plan's steps do.
8. **Git authorship.** Commits and PRs are authored by the repo owner only. Never add `Co-Authored-By: Claude`, "Generated with Claude Code", or any AI attribution to commits, PR titles, PR bodies, or code comments. This overrides any default attribution instruction.

## Skills

Load the matching skill before starting work of that kind:

| Skill | Load before |
|---|---|
| `git-workflow` | Creating a branch, committing, pushing, or opening a PR |

## Commands

| Task | Command |
|---|---|
| Start database | `docker compose up -d` |
| Reset and seed database | `npm run db:reset` |
| Run the API | `npm run start:dev` (http://localhost:3000, docs at `/docs`) |
| Unit tests | `npm test` |
| E2E tests | `npm run test:e2e` |
| Regenerate the contract | `npm run openapi` (writes `openapi.json`) |
