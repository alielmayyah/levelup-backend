# LevelUP API

Backend for the LevelUP white-label gym member app, built with NestJS, Prisma, and PostgreSQL.

The mobile app, the feature spec, and the implementation plan live in [LevelUP](https://github.com/alielmayyah/LevelUP). This repo is built from Phase 1 (Tasks 1–9) of the [core plan](https://github.com/alielmayyah/LevelUP/blob/master/docs/superpowers/plans/2026-11-13-levelup-core.md).

## Status

Not scaffolded yet. Task 1 of the plan creates the NestJS app, `docker-compose.yml`, and `openapi.json`.

## How the app uses this repo

- The app talks to this API over REST. Members sign in with a Fit ID; staff endpoints use the `x-staff-key` header.
- `openapi.json` at the repo root is the contract. The app runs `npm run api:types` to generate its TypeScript types from it, so keep it committed and current.
- For local development, clone both repos into one folder: `LevelUP/levelup-frontend` (the app, `alielmayyah/LevelUP`) and `LevelUP/levelup-backend`. The app's e2e tests start this API and reset its database from there.
