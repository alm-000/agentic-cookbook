# Pattern 07 — Screen & Score

**Use when:** a flood of inbound items — applications, candidates, leads — must be read, scored against defined criteria, ranked, and the top tier actioned fast.

> The pattern behind screening automations: reading every application within minutes and ranking it against your rubric so the team only looks at the best.

## Shape

```
items ──▶ extract fields ──▶ score vs rubric ──▶ tier (A / B / C) ──┬─ A: fast-track + notify
            (structured)      (LLM + rules)                         ├─ B: queue
                                                                    └─ C: polite decline / hold
```

Normalise each item into the same fields, score it against an explicit rubric, bucket into tiers, and route each tier to the right action.

## Decisions that actually matter

- **The rubric is data.** Weighted criteria live in config, not in the prompt — so the business can tune what "good" means without a code change.
- **Scores must be explainable.** Return the per-criterion breakdown, not just a number. A score nobody can interrogate won't be trusted.
- **Combine LLM judgement with hard rules.** Use deterministic rules for must-haves (location, eligibility) and the LLM for fuzzy fit — don't ask the model to do arithmetic it'll get wrong.
- **Human-review the borderline.** Items near a tier boundary go to a person; only the confident extremes auto-route.
- **Guard against bias.** Strip or ignore protected attributes; periodically audit score distributions.

## Template

[`examples/screen-score.json`](../examples/screen-score.json) — extract → rubric-score → tier-switch with a borderline-to-human branch. Bring your own LLM credential and rubric config.
