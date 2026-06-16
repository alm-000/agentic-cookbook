# Pattern 05 — Follow-Up / Chaser Cadence

**Use when:** you need to nudge someone repeatedly on a schedule until they act — pay an invoice, send a document, confirm an appointment, leave a review — and then stop cleanly.

> This is the engine behind a whole family of real-world automations: quote chasers, document chasers, deadline trackers, review requesters, reminder sequences, and post-appointment follow-ups.

## Shape

```
new item ──▶ wait (step 1) ──▶ done? ──no──▶ send nudge (escalating) ──▶ wait (step 2) ──▶ ...
                                  │                                                          │
                                 yes ──▶ stop + log                          max attempts ──▶ stop + flag
```

A timed sequence with a **state check before every send**. Each step waits, checks whether the desired action has happened, and only sends if it hasn't.

## Decisions that actually matter

- **Check state before every message.** Nothing burns trust like chasing someone for a payment they already made. Re-check the source of truth at each step.
- **Escalate tone, not volume.** Early touches are friendly reminders; later ones are firmer. Define the cadence (e.g. day 2 / day 5 / day 9) and the tone per step as data, not hard-code.
- **Hard stop conditions.** Stop on completion, on opt-out, or at a max-attempts cap — never loop forever.
- **Respect quiet hours and timezones.** Schedule sends into working hours for the recipient.
- **Idempotency.** A retry must not double-send the same nudge — key each send on (item ID, step).

## Template

[`examples/followup-chaser.json`](../examples/followup-chaser.json) — a wait → check-state → conditional-send loop with a max-attempts guard. Bring your own messaging + data-source credentials.
