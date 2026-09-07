# Foreman ledger — agent-workforce-breakdown
Baseline: e90ecac (branch claude/agent-workforce-breakdown-m6pp7g), clean tree
Mode: Full (Agent tool + shell)

| Task | Seat | Write set | Status |
|---|---|---|---|
| T-DOCS: breakdown + summary md | LEAD | docs/agent-workforce/README.md, SUMMARY.md | IN_PROGRESS |
| T-VIZ: realtime data-flow html | WORKHORSE (sonnet) | docs/agent-workforce/dataflow.html | DISPATCHED |
| T-VERIFY: blind verify T-VIZ | verifier (sonnet, read-only) | none | PENDING |
| T-MAIL: update Instinct via AgentMail | LEAD | none | BLOCKED: no API key / inbox address in env |

## 2026-09-07 update
Repo moved to masterjedi-ob1/agent-workforce (main). CRM vendored via git subtree.
| Task | Seat | Status |
|---|---|---|
| T-VIZ | WORKHORSE | DONE, LEAD verified by screenshot dark/light, integer KPI fix by LEAD |
| T-CRM: vendor + boot | LEAD | DONE: install, migrate, seed, check-types, api 200, app 307 |
| T-MAIL | LEAD | BLOCKED: api.agentmail.to denied by session egress policy (403 CONNECT); AgentMail MCP disconnected |
| T6: CRM activity to dataflow event relay | WORKHORSE | PENDING |
