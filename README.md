# Agentic Cookbook

Production-minded patterns for building **reliable AI agents** — the ones that actually run unattended, not demos that fall over on the second message.

Each pattern is a short write-up plus a runnable, **credential-free** template you can import into [n8n](https://n8n.io) and wire to your own keys. No secrets, no client data, no vendor lock-in to a single model.

> Built and maintained by [Alex Magee](https://alex-magee.com). These are the patterns behind the automations I ship for real businesses — distilled down to the reusable core.

## The patterns

| # | Pattern | Use it when |
|---|---------|-------------|
| 01 | [Orchestrator + Tools](patterns/01-orchestrator-tools.md) | One agent needs to call several capabilities (search, write, post) and decide the order itself. |
| 02 | [RAG Agent](patterns/02-rag-agent.md) | Answers must be grounded in *your* knowledge base, not the model's training data. |
| 03 | [Voice / Booking Agent](patterns/03-voice-booking-agent.md) | A conversational front desk that checks availability and books appointments. |
| 04 | [Browser Automation Agent](patterns/04-browser-automation-agent.md) | The task lives behind a UI with no API — the agent has to *use the website*. |
| 05 | [Follow-Up / Chaser Cadence](patterns/05-followup-chaser.md) | Nudge someone on a schedule until they act (pay, send a doc, leave a review) — then stop. |
| 06 | [Classify & Route (Intake)](patterns/06-classify-route.md) | Inbound items must be understood, categorised, and sent to the right place automatically. |
| 07 | [Screen & Score](patterns/07-screen-score.md) | A flood of applications/leads must be read, scored against criteria, and ranked. |

These are the building blocks behind real deployed automations — e.g. the [Agenture](https://aiagenture.co.uk/use-cases) product line (AI receptionist, booking manager, quote/document chasers, screeners, reminder & follow-up agents) is assembled from patterns 03, 05, 06 and 07.

## Principles these patterns follow

1. **Tools over prompt-stuffing.** Give the agent typed tools with clear contracts; don't cram everything into one mega-prompt.
2. **Human-in-the-loop where it matters.** Anything outbound or irreversible (sending an email, taking a booking, spending money) routes through an approval step.
3. **Memory is a decision, not a default.** Add conversation/Postgres memory only when multi-turn context actually changes the output.
4. **Measure drift.** AI workflows degrade silently. Every production agent needs a fixed test set and a pass-rate you watch over time — see [why I measure AI workflows](https://alex-magee.com/blog).

## Using a template

1. Open n8n → **Workflows → Import from File** and select a JSON from [`examples/`](examples/).
2. Open each credential-bearing node and attach **your own** credentials (the templates ship with none).
3. Replace placeholder IDs/URLs (calendars, sheets, webhooks) with yours.
4. Run the trigger and watch the execution log.

## License

[MIT](LICENSE) — use freely, no attribution required (though a link back is always welcome).
