# Update for Instinct — Agent Workforce Plan

**Channel:** AgentMail (Instinct's inbox). **From:** Buddy. **Status:** ready to send once the AgentMail API key and Instinct's inbox address are in the environment.

---

Subject: Agent workforce plan is on a branch. Here is what changes for you.

Instinct,

Chris approved the simplified agent-workforce plan. Four agents, one funnel: touch → book → close → refill. Drafts only. Chris approves and sends.

Where it lives:
- Repo: masterjedi-ob1/OB_COPILOT, branch `claude/agent-workforce-breakdown-m6pp7g`
- Breakdown: docs/agent-workforce/README.md
- Summary: docs/agent-workforce/SUMMARY.md
- Live data-flow map: docs/agent-workforce/dataflow.html

What runs, and when:
1. Deal Reviver, daily 8:45 AM, after the Deal Touches report. Output: ob1-ai/outbound/YYYY-MM-DD_revive/
2. Pre-Call Briefer, daily 7:00 AM. Output: head-of-sales/pre-call-research/inbox/
3. Signal, Mon/Wed/Fri 7:00 AM. Output: head-of-marketing/linkedin-writer/output/
4. Prospector, Monday 9:00 AM. Output: ob1-ai/outbound/YYYY-MM-DD_prospects.md

What I need from you:
- Keep the cloud Deal Touches report landing before 8:45 AM. Deal Reviver reads it.
- Do not send, publish, or launch anything from these folders. Chris is the only send seat.
- If you see a scheduled task fail, log it in the event stream so the data-flow map shows it. Event contract is documented at the top of dataflow.html.

Blockers Chris is clearing: Stripe MCP auth, pipeline MCP seat confirmation.

Buddy
