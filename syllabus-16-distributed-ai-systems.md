# Syllabus 16: Distributed AI Systems
**Status:** Enterprise Gap - When One Script Becomes Many Moving Parts
**Format:** One chat session per unit, self-paced

---

## What This Is

Many AI prototypes are one script. Real AI systems often become APIs, queues, workers, databases, caches, webhooks, schedulers, and human approval steps. This syllabus teaches you the patterns that keep those moving parts from duplicating work, losing work, spending too much, or failing silently.

---

## Unit 1 - Queues and Background Jobs

**Goal:** Move slow AI work out of the request path without losing track of it.

**Topics:**
- Why long-running model calls should often run as background jobs
- Job queues in plain language
- Redis, Celery, RQ, cloud tasks, and managed queues
- Job status: queued, running, succeeded, failed, waiting for human
- Polling vs. webhooks for job completion

**Session starter:**
> "Teach me job queues and background workers for AI workflows. I want a FastAPI endpoint that accepts work quickly, puts a long AI task in a queue, tracks status, and returns the result later."

---

## Unit 2 - Idempotency, Retries, and Timeouts

**Goal:** Make repeated or partial work safe.

**Topics:**
- What idempotency means and why webhooks often need it
- Retry policies with exponential backoff and jitter
- Timeouts for APIs, model calls, databases, and tools
- Deduplication keys for repeated requests
- Avoiding duplicate emails, payments, file writes, or client actions

**Session starter:**
> "Explain idempotency, retries, and timeouts for AI systems. Show me how to make a webhook-triggered workflow safe when the same request arrives twice or a model call times out halfway through."

---

## Unit 3 - Rate Limits, Backpressure, and Circuit Breakers

**Goal:** Protect the system when demand or dependency failures spike.

**Topics:**
- Client rate limits vs. model-provider rate limits
- Backpressure in plain language: slowing intake before the system collapses
- Circuit breakers when a dependency is failing
- Graceful degradation when a model, database, or tool is unavailable
- Protecting cost limits during traffic spikes

**Session starter:**
> "Help me design rate limits, backpressure, and circuit breakers for an AI workflow API. I want to protect model-provider limits, monthly cost, queue length, and client experience when dependencies are slow or failing."

---

## Unit 4 - State, Caching, and Consistency

**Goal:** Decide what the system remembers, where it remembers it, and how fresh it must be.

**Topics:**
- Stateless services vs. stateful workflows
- What to store in Postgres, Redis, object storage, and vector databases
- Caching model outputs, retrieval results, and expensive tool calls
- Cache invalidation in plain language
- Consistency trade-offs: fresh, fast, cheap, and correct

**Session starter:**
> "Teach me state and caching for AI systems. What belongs in Postgres, Redis, object storage, or a vector database? When should I cache model outputs or retrieval results, and how do I avoid stale or unsafe results?"

---

## Unit 5 - Workflow Orchestration

**Goal:** Coordinate multi-step AI processes without turning them into fragile chains.

**Topics:**
- Orchestration vs. simple scripts
- Step tracking, retries, compensation, and human approval
- When a workflow engine helps vs. when it is overkill
- Temporal, Airflow, Dagster, LangGraph, and simple homegrown orchestration
- How this connects to multi-agent orchestration in Syllabus 21
- Designing workflows that can resume after interruption

**Session starter:**
> "Help me understand workflow orchestration for AI systems. When do I need a workflow engine like Temporal, Airflow, Dagster, LangGraph, or a multi-agent framework? How do I design multi-step AI workflows that can retry, pause for approval, and resume?"

---

## Practice Project

Take one long-running AI workflow. Redesign it as an API plus queue plus worker. Add job status tracking, retry policy, timeout rules, idempotency keys, and a simple plan for what happens when the model provider is down.
