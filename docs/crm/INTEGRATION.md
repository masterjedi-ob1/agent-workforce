# Wiring the Four Agents to the CRM

The CRM is the shared memory for the funnel. Each OB.1 agent reads from it before drafting and writes evidence back after Chris acts. The CRM's own eve agent handles enrichment, identity matching, and follow-up scheduling, so our four agents stay thin.

## Division of labor

| Job | Owner | Why |
|---|---|---|
| Who is this person, what company, what did they last say | CRM agent (`identify_contact`, `read_crm_history`, `enrich_company`) | Evidence-priced, never guessed |
| Draft the touch, the brief, the post, the opener | OB.1 agents | Voice, judgment, sales angle |
| Approve and send | Chris | Only send seat |
| Record what happened, schedule the next look | CRM agent (`record_fact`, `schedule_recheck`) | Ledger, not memory |

## Per-agent contract

| Agent | Reads from CRM | Writes to CRM | Transport |
|---|---|---|---|
| Deal Reviver | Deals with stage COLD, last thread per contact via `read_crm_history` | After Chris sends: a fact on the deal ("touch sent, channel, angle") | tRPC API on :3001 or `search_crm` through the agent bridge |
| Pre-Call Briefer | Contact and company history, outstanding work (`list_outstanding_work`) | `write_brief` result attached to the contact | Same |
| Signal | Nothing required. Optional: deals closed this week for build-in-public material | Nothing | n/a |
| Prospector | Existing contacts to dedupe against (`search_crm`) | New contacts and companies as suggestions, never as facts, until Chris approves the batch | tRPC API |

## Event stream to the live map

`docs/agent-workforce/dataflow.html` accepts events through `window.OB1_FLOW.push(event)`. The CRM's `hooks/activity.ts` and `hooks/audit.ts` already emit per-session activity. A small relay that maps CRM activity to the map's event contract closes the loop. Ticket for that lives in the foreman ledger as T6.

## Build order

1. Deploy CRM (SETUP.md). Connect `master_jedi@ob1ai.co` so `read_crm_history` has real threads.
2. Load the eight Tier 1 deals by hand or CSV. Stage them COLD.
3. Point Deal Reviver at the CRM API instead of the markdown Deal Touches report. The report becomes a view, not a source.
4. Pre-Call Briefer reads the CRM instead of raw Granola. Granola still feeds the CRM through the mailbox and meeting sync.
5. Prospector stages into CRM suggestions. Chris approves in the UI.
