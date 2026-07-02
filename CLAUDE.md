# Smart AI Bank — OpenShift Training App

Demo banking application for an OpenShift training assignment. **Not production software** — it exists so trainees can practice deploying a microservice app to OpenShift via Git import. Keep everything simple, readable, and easy to deploy.

## Context

- Audience: KSKL (Karja Suchana Kendra), a Nepalese government company, being trained on Kubernetes/OpenShift.
- Training already covered: creating an OpenShift cluster, deploying apps, controlling cluster access.
- This repo is built by the trainer (me), then handed to trainees who must replicate the deployment on OpenShift themselves.

## Architecture

Monorepo, two microservices + Postgres:

| Path | Service | Stack |
|---|---|---|
| `/frontend` | Web UI | Next.js (Node.js) |
| `/backend` | REST API | Node.js |

- Database: PostgreSQL.
- Frontend discovers the backend API URL via an **environment variable**.
- Backend gets database configuration via **environment variables**, sourced from a Kubernetes/OpenShift **Secret**.
- Database migrations + seed data run **manually from the backend pod** via `npm run migration`. Do not auto-run migrations on startup.

## Application Features

1. Login/logout with two roles:
   - Customer → `guest` role
   - Bank manager → `manager` role
2. Customer views: bank statement, KYC details, account balance, account details. All data is fake and seeded by the database migration.
3. "Chatbox" AI feature: user picks a canned question (e.g., "Summarize my monthly transactions") and gets a pre-generated reply. **No real AI model call** — it only needs to feel real (typing delay, chat UI).

## Development Rules

- Use Docker / docker-compose for local testing to prove the app works end to end.
- App display name: **"Smart AI Bank"**.
- Design for OpenShift Git-import deployment: each service must build standalone from its subdirectory (own `package.json`, own Dockerfile).
- No production hardening needed (no real auth providers, payments, or PII) — but keep the demo believable.

## Git Workflow

- Work on a branch and open a PR for each piece of work, then merge the PR. Never commit directly to `main`.
- Keep PRs scoped and organized — trainees will read this repo's history.

## Trainee Assignment Scope (for docs/README you write)

Besides deploying the app, the assignment includes an access-control task:
- Create a `developer` user with **edit** permission scoped only to the application's project (namespace).
- An `admin` user retains full cluster access.
