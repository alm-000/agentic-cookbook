# Pattern 03 — Voice / Booking Agent

**Use when:** you want a conversational front desk that answers enquiries, checks live availability, and books appointments — over chat or voice.

## Shape

```
webhook (voice/chat) ──▶ agent ──┬─ tool: check_availability (calendar)
       ▲                         ├─ tool: create_booking (calendar + CRM)
       └── respond ◀─────────────┴─ tool: log_interaction (CRM)
                  buffer memory
```

The agent holds a short conversation, calls a calendar tool to find open slots, confirms with the caller, then writes the booking back to the calendar and logs the interaction.

## Decisions that actually matter

- **Confirm before committing.** The agent should read the chosen slot back to the caller and get a yes before it writes a booking. This is the human-in-the-loop step for an otherwise-autonomous flow.
- **Idempotent bookings.** Network retries happen. Key the booking on a request ID so a retry doesn't double-book.
- **Bound the conversation.** Memory is per-call and short; you don't want last week's caller's context leaking into this one.
- **Fail to a human.** If the caller asks something outside scope, hand off cleanly ("I'll have someone call you back") and log it — don't improvise.

## Reliability notes

- Test the availability tool against edge cases: fully booked day, timezone boundaries, back-to-back slots.
- Track booking success rate and abandonment as your headline metrics.

## Template

[`examples/voice-booking-agent.json`](../examples/voice-booking-agent.json) — webhook-triggered agent with calendar + logging tool stubs and per-session memory. Bring your own calendar/CRM credentials.
