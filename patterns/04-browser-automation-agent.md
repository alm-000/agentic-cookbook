# Pattern 04 — Browser Automation Agent

**Use when:** the task lives behind a UI with no usable API, so the agent has to *operate the website* — fill forms, click through flows, read rendered state.

## Shape

```
trigger ──▶ planner agent ──▶ browser agent ──▶ [ browser session ]
              (writes a plan)   (executes steps)   open / type / click / read
                                      │
                                      ▼
                              verify + return result
```

A **planner** turns the goal into an ordered list of UI steps; a **browser agent** executes them against a headless/remote browser session, reading the page back between steps to decide the next action.

## Decisions that actually matter

- **Separate planning from execution.** The planner reasons about intent; the executor handles the messy reality of selectors and timing. Mixing them produces brittle agents.
- **Read state, don't assume it.** After each action, re-read the page. UIs change, elements move, modals appear — an agent that fires blind clicks will drift.
- **Hard guardrails on destructive actions.** Anything that submits, pays, or deletes goes through an explicit confirmation, the same as a human's "are you sure?".
- **Screenshot the failures.** When a run fails, capture the page state — it's the only way to debug a UI that's since changed.

## Reliability notes

- Browser automation is the *least* reliable agent class. Budget for retries, set per-step timeouts, and alert on repeated failures rather than silently looping.
- Prefer an API the moment one exists — this pattern is a last resort, not a default.

## Template

[`examples/browser-automation-agent.json`](../examples/browser-automation-agent.json) — planner + executor agents wired to a generic browser-automation node. Bring your own browser-service credentials.
