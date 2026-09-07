# Hosting the CRM and the Agent Workforce

Short answer: no, Railway is not required. Your Docker box can run the CRM today. The one thing it cannot give you is always-on, and the agents only earn their keep if they run when you are asleep.

## What each piece needs

| Piece | Needs | On your Docker box | Notes |
|---|---|---|---|
| Postgres | Disk, uptime | Yes, `docker compose up -d` | Back it up nightly. It becomes the pipeline of record. |
| API (NestJS, :3001) | Uptime, a public HTTPS URL for the Google OAuth callback | Yes | Public URL is the catch. See below. |
| App (Next.js, :3000) | Uptime, same origin family as the API | Yes | Chloe and Andrew need to reach it from their machines. |
| Agent (eve) | Uptime, durable sessions | Runs with `bun run start` (it wraps `eve start`) | Built for Vercel. Self-hosting compiles clean here. A full self-hosted run was not verified in the sandbox. |
| Mailbox sync | A scheduler hitting `POST /internal/sync/mailboxes` with `CRON_SECRET` | cron on the box | Dies when the box sleeps. |

## The public URL problem

Google OAuth needs a callback it can send the browser back to. On your own machine that is `http://localhost:3001/api/auth/callback/google`, which works only for a browser on that machine. For Chloe to sign in, the API needs a real hostname with HTTPS.

Two free ways to get one on the box:
- Cloudflare Tunnel: `cloudflared tunnel` maps `crm.ob1ai.co` and `crm-api.ob1ai.co` to :3000 and :3001. No inbound ports opened. Set `AUTH_COOKIE_DOMAIN=ob1ai.co`.
- Tailscale: team-only. Each person joins the tailnet, `tailscale cert` gives HTTPS. Nothing is public. Google's callback still works because the browser, not Google, follows the redirect.

Cloudflare Tunnel is the better fit. Kathy or a client can be added later without joining a VPN.

## Recommended path

**Now (this week):** everything on the Docker box behind a Cloudflare Tunnel. Cost is zero. You already have the hardware. Add a nightly `pg_dump` to Drive.

**When it must survive the box being off:** move in this order, only as each becomes the bottleneck.
1. Postgres to Supabase (already connected to OB.1). One `DATABASE_URL` change.
2. Agent to Vercel, since eve is Vercel's own framework and the free tier covers a one-minute dispatch schedule.
3. API and app to Railway or Fly only if the box itself becomes unreliable. Roughly $10 to $20 a month for both.

## Data sovereignty

Every layer above keeps the data on infrastructure you control or already pay for. Set `CRM_TELEMETRY_DISABLED=1` everywhere. The agent sandbox is deny-all egress by default.
