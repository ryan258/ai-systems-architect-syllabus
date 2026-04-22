# Syllabus 16: Distributed AI Systems
**Status:** Production Spine - Managing Distributed Runtime Complexity
**Format:** One chat session per unit, self-paced

---

## What This Is

Once single-service deployment and repeatable delivery are under control, many workflows need a more complex runtime shape: queues, workers, retries, state stores, caches, and orchestration. This syllabus is about making that jump without losing correctness or operability.

It sits after Syllabi 12-15 on purpose. First you learn how to control one live service. Then you learn how to split that service into moving parts without turning it into a failure factory. Multi-agent specialization comes later in Syllabus 21. This module is about distributed runtime control first.

---

## Unit 1 - Queues and Background Jobs

**Goal:** Move slow or failure-prone work out of the request path without losing job truth.

**Topics:**
- Why long-running model work should often be accepted quickly and executed later
- Queue and worker boundaries in plain language
- Redis, Celery, cloud tasks, and managed queues as implementation options
- Job states: accepted, queued, running, waiting for human, failed, succeeded
- Polling, callbacks, and operator status surfaces

**Session starter:**
> "Teach me queues and background jobs for AI workflows. I want the API to accept work quickly, create a durable job record, process the slow AI steps in the background, and expose useful job status to both users and operators."

---

## Unit 2 - Idempotency, Retries, and Timeouts

**Goal:** Keep duplicate delivery and partial failure from creating duplicate side effects.

**Topics:**
- Idempotency keys at the request, job, and side-effect boundary
- Retry policies with backoff and jitter
- Timeouts for provider calls, retrieval, writes, and whole jobs
- Dead-letter paths for poison work
- Avoiding duplicate notifications, writes, approvals, or external actions

**Session starter:**
> "Explain idempotency, retries, and timeouts for a distributed AI workflow. I want duplicate events and partial failures to be safe, and I want a matrix that tells me what is retryable, what times out, and when work should escalate."

---

## Unit 3 - Rate Limits, Backpressure, and Circuit Breakers

**Goal:** Protect the system and its dependencies during bursts, retries, and provider trouble.

**Topics:**
- Client, tenant, queue, and dependency-level rate limits
- Backpressure as deliberate load control instead of silent collapse
- Circuit breakers for failing providers and expensive dependencies
- Graceful degradation when the system is overloaded
- Protecting both latency and budget during traffic spikes

**Session starter:**
> "Help me design overload protection for a distributed AI system. I want rate limits, backpressure, circuit breakers, degraded mode, and cost-triggered safety actions for when demand or a provider starts misbehaving."

---

## Unit 4 - State, Caching, and Consistency

**Goal:** Decide what the distributed system remembers, where truth lives, and how stale is too stale.

**Topics:**
- Durable versus ephemeral state
- Sources of truth for job status, human approvals, and final outputs
- What belongs in Postgres, Redis, object storage, and vector stores
- Caching retrieval results, enrichment, or model-adjacent data safely
- Freshness, invalidation, privacy, and rebuild rules

**Session starter:**
> "Teach me state, caching, and consistency for a distributed AI system. I want to know what the source of truth is for job state, what can be cached safely, and how to avoid stale or privacy-risky data reuse."

---

## Unit 5 - Workflow Orchestration

**Goal:** Coordinate multi-step AI work with clear checkpoints, resume behavior, and human approval states.

**Topics:**
- When a simple queue-plus-worker model is enough and when it is not
- Step tracking, checkpoints, retries, compensation, and human pauses
- Temporal, Dagster, LangGraph, and simple homegrown orchestration as different levels of control
- Resume and recovery behavior after interruption
- How this groundwork connects later to multi-agent orchestration in Syllabus 21

**Session starter:**
> "Help me design workflow orchestration for an AI system. I want to know when a queue and worker are enough, when I need a workflow engine, and how to model checkpoints, retries, human approval, and resume behavior."

---

## Unit 6 - Distributed AI Systems Review Package

**Goal:** Turn the distributed runtime decisions into one reviewer-ready packet.

**Topics:**
- Assembling queue design, idempotency rules, overload controls, state policy, and orchestration choice into one package
- Checking that job state, retry behavior, degraded modes, and resume logic agree with each other
- Writing rollout blockers and a final distributed-systems decision
- Naming the later changes that should force a re-review

**Session starter:**
> "Help me assemble a distributed AI systems review package for one workflow. I want the queue model, idempotency and retry rules, overload controls, state and caching policy, orchestration choice, and a final rollout recommendation in one packet."

---

## Practice Project

Take one workflow that already has repeatable delivery controls from Syllabus 15 and redesign it as a distributed runtime package that includes:

- a queue architecture and job lifecycle brief
- an idempotency and retry-timeout matrix
- an overload protection and circuit-breaker policy
- a state map and caching policy
- an orchestration decision brief and recovery notes
- a final rollout recommendation with blockers and next steps
