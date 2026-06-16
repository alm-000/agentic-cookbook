# Pattern 02 — RAG Agent

**Use when:** answers must be grounded in *your* documents (policies, product docs, a knowledge base) rather than the model's general training.

## Shape

```
ingest:   docs ──▶ chunk ──▶ embed ──▶ vector store
answer:   question ──▶ embed ──▶ retrieve top-k ──▶ agent (with context) ──▶ answer
```

Two flows that share a vector store: an **ingestion** flow that you run when source material changes, and an **answer** flow triggered per question.

## Decisions that actually matter

- **Chunk size vs. retrieval quality.** Too large and retrieval returns noise; too small and you lose context. Start at ~500–800 tokens with overlap, then tune against real questions.
- **Always cite.** Have the agent return the source chunk IDs it used. Ungrounded RAG is just a slower hallucination.
- **Re-embed on model change.** Embeddings from different models aren't comparable — if you switch embedding models, re-index everything.
- **Filter before you retrieve.** Metadata filters (date, doc type, tenant) cut the candidate set before semantic search and stop cross-context leakage.

## Reliability notes

- Keep a fixed set of question→expected-source pairs and assert retrieval still surfaces the right chunks after any change.
- If retrieval confidence is low, prefer "I don't have that in my sources" over a confident guess.

## Template

[`examples/rag-agent.json`](../examples/rag-agent.json) — ingestion + answer flows against a generic vector store. Bring your own embedding + LLM credentials and vector store connection.
