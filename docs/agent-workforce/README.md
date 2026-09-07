# Agent Workforce — Simplified Breakdown (Foreman Edition)

**Source of truth:** [AGENT_WORKFORCE_PLAN.md](./AGENT_WORKFORCE_PLAN.md) (2026-08-27). This page breaks it into seats, tickets, and gates so anyone on the OB.1 team can follow without reading the full plan.
**Live picture:** [dataflow.html](./dataflow.html) — realtime data-flow map of the same funnel.

## One sentence

Four agents, one funnel: **touch → book → close → refill.** Every agent drops a copy-paste-ready draft in a fixed folder each morning. Nothing sends itself. Chris approves and hits send.

## Why only four

The audit found 8 of 8 Tier 1 deals cold, zero client touches in 48 hours, and 13 dead scheduled tasks. The gap is not intelligence, it is follow-through. Thirteen agents would recreate the same graveyard. Four agents, each feeding the next stage of one funnel, is the smallest system that moves revenue.

## The seats (foreman routing)

| Seat | Class | Who | What it does |
|---|---|---|---|
| LEAD | Frontier | Fable Foreman (Buddy) | Plans, routes, reviews drafts, changes state (deregister tasks, git init, CLAUDE.md refresh). Never types boilerplate. |
| WORKHORSE | Mid-tier | Sonnet-class worker | Writes and registers each SKILL.md, runs the first live pass, reports DONE with evidence. |
| VERIFIER | Mid-tier, read-only | Blind verifier | Gets the original ticket, checks the task fires and the output meets spec. PASS / FAIL only. |
| HUMAN GATE | Chris | Chris | Approves and sends. The only seat with a send button. |

## The four agents

| # | Agent | Fires | Reads | Writes | Feeds |
|---|---|---|---|---|---|
| 1 | Deal Reviver | Daily 8:45 AM, after the Deal Touches report | Deal Touches report, Gmail thread context | `ob1-ai/outbound/YYYY-MM-DD_revive/` (one file per cold deal) | TOUCH |
| 2 | Pre-Call Briefer | Daily 7:00 AM | Calendar (next 48h), Granola, Gmail, Tavily | `head-of-sales/pre-call-research/inbox/YYYY-MM-DD_<party>.md` | BOOK → CLOSE |
| 3 | Signal (LinkedIn) | Mon/Wed/Fri 7:00 AM | The week's real work | `head-of-marketing/linkedin-writer/output/` + 30-second publish checklist | Air cover |
| 4 | Prospector | Monday 9:00 AM | pipeline MCP, ReadyLink, neo-lead-data-raw | `ob1-ai/outbound/YYYY-MM-DD_prospects.md` (10 to 15 names, one opener each) | REFILL → TOUCH |

Build order is the funnel order. Deal Reviver first because 100 percent of priced pipeline is untouched today.

## Ticket board

| Ticket | Work | Seat | Write set | Verify | Est. |
|---|---|---|---|---|---|
| T0 | Deregister 13 orphaned scheduled tasks; `git init` in `agent-workspaces` | LEAD | `.claude/scheduled-tasks/`, `agent-workspaces/.git` | Self (state change) | 15 min |
| T1 | Deal Reviver SKILL.md, register, run once, 8 drafts land | WORKHORSE | `skills/deal-reviver/`, scheduled task | Blind verifier + LEAD reads drafts | 45 min |
| T2 | Pre-Call Briefer, verify against tomorrow's calendar | WORKHORSE | `skills/pre-call-briefer/`, scheduled task | Blind verifier | 30 min |
| T3 | Signal MWF task | WORKHORSE | `skills/signal/`, scheduled task | Blind verifier | 20 min |
| T4 | Prospector, dry run only, no sends | WORKHORSE | `skills/prospector/`, staged campaign | Blind verifier + LEAD reads list | 45 min |
| T5 | Refresh `shared/CLAUDE.md` to today's state | LEAD | `shared/CLAUDE.md` | Self | 20 min |

Dispatch is sequential. All tickets touch `.claude/scheduled-tasks/`, so write sets overlap and parallel dispatch is off the table. Roughly three hours to a running funnel.

## Gates that never move

- Drafts only. No agent sends, publishes, or launches a campaign.
- Verify from a committed state. If `git status` is dirty after the verifier runs, the verification is void.
- A worker that goes quiet is LOST. Prove the process stopped, reconcile partial edits, then retry once with a stronger seat.
- Cheaper seats only where they clear the quality bar. Budget picks among passing seats; it never lowers the bar.

## Blockers Chris clears (five minutes)

1. Stripe MCP auth. Connector settings or `/mcp` in an interactive session.
2. Confirm the pipeline MCP LinkedIn seat is active before T4.
3. Say "go". T0 to T5 run in order.

## Not building now

CFO/runway agent (waiting on Stripe auth, not a build), CRM hygiene, handoff monitor, executive dashboard, distressed-business scan. Revisit after two weeks of the funnel running.

## Two-week scorecard

| Metric | Now | Target |
|---|---|---|
| Tier 1 COLD deals | 8 | ≤3 |
| Client touches drafted per week | 0 | ≥10 |
| Client touches sent per week | 0 | ≥6 |
| LinkedIn drafts published | 0 of 3 | ≥2 of 3 |
| Qualified prospects staged | 0 | ≥10 |
