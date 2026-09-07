# OB.1 Agent Workforce

The operating repo for OB.1's agent workforce: the four-agent sales funnel, the CRM the agents write into, and the live data-flow map.

| Part | Where | What |
|---|---|---|
| Plan and breakdown | [docs/agent-workforce/](./docs/agent-workforce/) | Source plan, foreman breakdown, 60-second summary, Instinct update |
| Live data-flow map | [docs/agent-workforce/dataflow.html](./docs/agent-workforce/dataflow.html) | Animated system map with a documented event contract |
| CRM | [crm/](./crm/) | Comp AI CRM (MIT), vendored as a git subtree from `trycompai/crm@release` |
| CRM setup for OB.1 | [docs/crm/SETUP.md](./docs/crm/SETUP.md) | Boot it locally in five minutes, deploy it in an afternoon |
| Agents to CRM wiring | [docs/crm/INTEGRATION.md](./docs/crm/INTEGRATION.md) | How Deal Reviver, Pre-Call Briefer, Signal, and Prospector read from and write to the CRM |

## Principle

Four agents, one funnel: **touch → book → close → refill.** Every agent drops a copy-paste-ready draft in a fixed folder each morning. Nothing sends itself. Chris approves and hits send. The CRM is where the agents keep their notes, and nothing about a person is guessed.

## Verified state (2026-09-07)

- `crm/`: `bun install`, `db:deploy`, `db:seed`, `check-types` all pass. API serves on :3001, app on :3000 against a local Postgres 16.
- Sign-in needs a Google OAuth client for the `ob1ai.co` domain. See SETUP.

## Keeping the CRM current

```sh
git subtree pull --prefix=crm https://github.com/trycompai/crm.git release --squash
```
