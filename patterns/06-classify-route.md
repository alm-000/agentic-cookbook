# Pattern 06 — Classify & Route (Intake)

**Use when:** inbound items — emails, documents, leads, applications — must be understood, categorised, and sent to the right place automatically.

> The backbone of intake automations: filing systems that read and sort documents, CRM/database managers that keep records clean, onboarders that kick off the right flow, and outreach routers that reply in the right voice.

## Shape

```
inbound ──▶ extract / parse ──▶ classifier ──▶ route by category ──┬─ action A
              (text, fields)      (+ confidence)                    ├─ action B
                                       │                            └─ fallback: human queue
                                  low confidence ───────────────────┘
```

Parse the item into structured fields, classify it (with a confidence score), and route to the matching action. Anything the classifier is unsure about goes to a human queue, not a guessed bucket.

## Decisions that actually matter

- **Categories are data, not code.** Define the taxonomy and per-category actions in config so non-engineers can adjust them.
- **Always return a confidence score** and set a threshold. Below it → human review. A wrong auto-file is worse than a flagged one.
- **Dedupe on the way in.** The same email/document arriving twice should not create two records.
- **Log the decision and its reason.** When routing looks wrong, you need to see *why* the classifier chose what it did.

## Template

[`examples/classify-route.json`](../examples/classify-route.json) — parse → classify (with confidence) → switch-router with a human-fallback branch. Bring your own LLM + destination credentials.
