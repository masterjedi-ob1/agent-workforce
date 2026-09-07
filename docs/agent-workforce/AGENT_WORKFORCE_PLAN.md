# Agent Workforce Plan — Sell Now

**Date:** 2026-08-27 · **Owner:** Chris · **Built by:** Fable Foreman (LEAD plan; execution tickets route to WORKHORSE seats)
**Grounding:** audit-report.md (2026-08-27). Real state: 8 of 8 Tier 1 deals COLD, $120K priced / $49.5K weighted pipeline (modeled, from 14 Aug forecast), zero client touches in 48h, 13 dead scheduled tasks, one healthy cloud automation (Deal Touches).

## Principle

Do not build 13 agents. Build 4, each one feeding the next step of the same funnel: **touch → book → close → refill.** Every agent ships copy-paste-ready output to a fixed folder every morning. Nothing publishes or sends on its own; you approve and hit send.

---

## The 4 agents, in build order

### 1. Deal Reviver (sales follow-up) — build first, run today

**Why first:** 100% of priced pipeline is untouched. The Deal Touches report already names the 8 next touches with owner and channel; nobody drafts them. This agent closes that gap.

- **Trigger:** daily 8:45 AM, right after the cloud Deal Touches report lands.
- **Logic:** read today's `OB1-Deal-Touches-*.md` → for every COLD deal with a proposed touch, draft the actual email (or call script) using the `outreach` + `closer` skills, OB.1 brand voice, forbidden-words list. Pull last thread context from Gmail so drafts reference reality, not memory.
- **Output:** `ob1-ai/outbound/YYYY-MM-DD_revive/` — one file per deal, subject line + body, ready to paste. Summary table on top: deal, channel, owner, one-line angle.
- **Connectors:** Gmail (read), Calendar, Granola — all already wired to the working seat.
- **Day-one run:** the 8 drafts for today's COLD list are the acceptance test.

### 2. Pre-Call Briefer (sales prep)

**Why second:** revived deals turn into calls; walking in cold wastes them.

- **Trigger:** daily 7:00 AM (rebuilds the dead `calendar-scan-for-calls`).
- **Logic:** scan next 48h of calendar for external meetings → for each, run `people-dossier` / `company-dossier` light pass + Granola history with that party → one-page brief: who, deal state, last commitments, 3 questions to ask, 1 thing to sell.
- **Output:** `ob1-ai/head-of-sales/pre-call-research/inbox/YYYY-MM-DD_<party>.md` (revives the existing structure).
- **Connectors:** Calendar, Granola, Gmail, Tavily (all available now).

### 3. Signal (LinkedIn content) — MWF

**Why third:** inbound air cover while outbound runs. Cheapest to build; the skill (`linkedin`, `signal`, brand voice) already exists and prior drafts prove the pattern.

- **Trigger:** Mon/Wed/Fri 7:00 AM (rebuilds `mwf-linkedin-post-draft`).
- **Logic:** one post draft from the week's real work (deal lessons, audit findings, build-in-public), LoneWolf or OB.1 voice as flagged, no hype words. Include a 2-line "why this post now."
- **Output:** `ob1-ai/head-of-marketing/linkedin-writer/output/`.
- **Rule:** drafts only. The audit showed the old cadence died because publishing never happened; the agent ends each draft with a 30-second publish checklist to lower friction.

### 4. Prospector (leadgen refill)

**Why fourth:** the funnel needs new names once the 8 current deals are touched and triaged (audit says 4 of 8 may be dead).

- **Trigger:** weekly, Monday 9:00 AM.
- **Logic:** pull from what's already connected: the `pipeline` MCP (LinkedIn prospect database, campaigns, drafts), ob-nucleus ReadyLink leads, and `neo-lead-data-raw`. Score against ICP (SMB, audit-first fit), output 10-15 named prospects with one personalized opener each, feed approved ones into a pipeline-MCP campaign.
- **Output:** `ob1-ai/outbound/YYYY-MM-DD_prospects.md`.
- **Gate:** campaign sends require your explicit approval per batch; agent only drafts and stages.

**Deliberately NOT building now:** CFO/runway agent (blocked on Stripe auth since Jun 5 — unblock is a connector fix, not a build), CRM hygiene, handoff monitor, executive dashboard, distressed-biz scan. Revisit after 2 weeks of the funnel running.

---

## Build plan (foreman routing)

| Ticket | Work | Seat | Est. |
|---|---|---|---|
| T0 | Deregister 13 orphaned scheduled tasks; init git in `agent-workspaces` (safety net) | LEAD (state-changing, judgment) | 15 min |
| T1 | Write + register Deal Reviver SKILL.md; run once live; verify 8 drafts | WORKHORSE, LEAD reviews drafts | 45 min |
| T2 | Write + register Pre-Call Briefer; verify against tomorrow's calendar | WORKHORSE | 30 min |
| T3 | Write + register Signal MWF task | WORKHORSE | 20 min |
| T4 | Write Prospector skill; dry-run against pipeline MCP + ReadyLink; no sends | WORKHORSE, LEAD reviews list quality | 45 min |
| T5 | Refresh `shared/CLAUDE.md` to today's state so agents load truth (audit item #2) | LEAD | 20 min |

Sequential dispatch (shared context, overlapping write sets in `.claude/scheduled-tasks/`). Blind verification per foreman contract on T1-T4: verifier gets the ticket, checks the registered task fires and output meets spec. Total: ~3 hours to a running funnel.

## Blockers to clear (5 minutes of your time, big unlock)

1. **Stripe MCP auth** — via claude.ai connector settings or `/mcp` in an interactive session. Unblocks revenue verification everywhere.
2. **Confirm pipeline MCP account state** (LinkedIn seat active?) before T4 relies on it.
3. **Approve the build** — say "go" and T0-T5 run in this order.

## Success metrics (2-week check)

- Tier 1 COLD count: 8 → ≤3 (touched or reclassified)
- Client touches per week: 0 → ≥10 drafted, ≥6 sent
- LinkedIn: ≥2 of 3 weekly drafts actually published
- New qualified prospects staged: ≥10
