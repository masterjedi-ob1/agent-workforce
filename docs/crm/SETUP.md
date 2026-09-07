# CRM Setup for OB.1

Comp AI CRM is three processes and a Postgres: the Next.js app (:3000), the NestJS API (:3001), and the eve agent. Full upstream docs live in [crm/README.md](../../crm/README.md). This page is the OB.1-specific path.

## Local, five minutes

```sh
cd crm
cp .env.example .env
bun install                      # runs Prisma generate; needs DATABASE_URL set
docker compose up -d             # Postgres 17 on :5432 (or point DATABASE_URL at any Postgres 16+)
bun run db:deploy
bun run db:seed                  # optional demo pipeline
bun run dev
```

Set these in `crm/.env` before `bun run dev`:

| Variable | Value for OB.1 |
|---|---|
| `BETTER_AUTH_SECRET` | `openssl rand -base64 32` |
| `ALLOWED_SIGN_IN` | `ob1ai.co` |
| `GOOGLE_CLIENT_ID` / `GOOGLE_CLIENT_SECRET` | Google Cloud OAuth web client, redirect `http://localhost:3001/api/auth/callback/google`, Gmail and Calendar APIs enabled, consent screen set to Internal |

Everything else is optional. `PERPLEXITY_API_KEY` turns on open-web research for the agent. The Context key for company brand data and LinkedIn is entered in Settings → General after first sign-in.

## What was verified in the sandbox

Postgres 16 local, no Docker. Migrations applied, seed loaded, 13 packages type-checked clean, API returned 200 on the auth health route, app redirected to sign-in as expected. Sign-in itself was not exercised because it requires the Google OAuth client, which only Chris can create.

## Deploy, one afternoon

Three deployments plus a managed Postgres. They only need to agree on `DATABASE_URL` and `BETTER_AUTH_SECRET`.

| Piece | Recommended home | Notes |
|---|---|---|
| Postgres | Supabase (already connected to OB.1) or Neon | Use the pooled connection string |
| `apps/api` (NestJS) | Railway or Fly | Long-running process, needs `CRON_SECRET` and a scheduler hitting `POST /internal/sync/mailboxes` |
| `apps/app` (Next.js) | Vercel | Set `API_URL`, `APP_URL`, `AUTH_COOKIE_DOMAIN=ob1ai.co` if both are on subdomains |
| `apps/agent` (eve) | Vercel (eve is Vercel's durable-agent framework) | Runs the one-minute dispatch schedule and the work queue |

Add the production callback `https://<api-host>/api/auth/callback/google` to the Google OAuth client.

## Data sovereignty

Set `CRM_TELEMETRY_DISABLED=1` on every deployment. The agent sandbox has deny-all egress by default. Mail reading is forward-only from the moment a mailbox is connected, so connecting `master_jedi@ob1ai.co` does not import history.
