# Pattern 01 — Orchestrator + Tools

**Use when:** a single agent needs to choose between several capabilities and sequence them itself — e.g. "research this topic, write a draft, generate an image, then post it."

## Shape

```
trigger ──▶ orchestrator agent ──▶ [ tool: search ]
                  │  ▲             ├ [ tool: write_section ]
                  │  └─────────────┤ [ tool: generate_image ]
                  ▼                └ [ tool: publish ]
            buffer memory
```

The orchestrator is one LLM node with a system prompt that describes its goal and the tools available. Each tool is a **sub-workflow** (or native tool node) with a narrow, well-named contract. The agent decides which tools to call and in what order; you don't hard-code the flow.

## Why sub-workflows as tools

- Each tool is independently testable — run it in isolation with fixed inputs.
- The orchestrator prompt stays short: it reasons about *which* tool, not *how* each tool works.
- You can swap a tool's implementation without touching the agent.

## Reliability notes

- **Constrain tool outputs with a schema.** A tool that returns free text forces the orchestrator to re-parse it; a tool that returns structured JSON doesn't.
- **Cap the loop.** Set a max-iterations guard so a confused agent can't call tools forever.
- **Log every tool call** with its inputs and outputs — this is your debugging trail when the agent makes a strange decision.

## Template

[`examples/orchestrator-tools.json`](../examples/orchestrator-tools.json) — a manual-trigger orchestrator with two placeholder tool nodes and buffer memory. Bring your own model credentials.
