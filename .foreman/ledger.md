# Foreman ledger — agent-workforce-breakdown
Baseline: e90ecac (branch claude/agent-workforce-breakdown-m6pp7g), clean tree
Mode: Full (Agent tool + shell)

| Task | Seat | Write set | Status |
|---|---|---|---|
| T-DOCS: breakdown + summary md | LEAD | docs/agent-workforce/README.md, SUMMARY.md | IN_PROGRESS |
| T-VIZ: realtime data-flow html | WORKHORSE (sonnet) | docs/agent-workforce/dataflow.html | DISPATCHED |
| T-VERIFY: blind verify T-VIZ | verifier (sonnet, read-only) | none | PENDING |
| T-MAIL: update Instinct via AgentMail | LEAD | none | BLOCKED: no API key / inbox address in env |
