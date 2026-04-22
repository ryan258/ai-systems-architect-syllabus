# Lesson 16.01: Queues and Background Jobs

**Parent syllabus:** [Syllabus 16: Distributed AI Systems](../../syllabus-16-distributed-ai-systems.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Queue architecture brief and job lifecycle contract

---

## Outcome

By the end of this lesson, you can decide when AI work should leave the request path, define a queue and worker boundary, and document how one job moves from acceptance to completion without disappearing.

You will produce:

- a queue architecture brief
- a job lifecycle contract
- a status and notification plan

---

## Why This Matters

Many AI systems fail because slow work is still treated like a quick API response.

Common failures:

- The API waits on retrieval and model calls until the client times out.
- A background task is launched, but no durable record proves whether it finished.
- Duplicate requests create duplicate jobs because acceptance and execution are not separated.
- Long-running work keeps web workers busy and starves the rest of the service.
- Human review pauses exist in reality, but not in the system state model.
- Privacy-sensitive input is copied into queue payloads and logs without restraint.

Queues and background jobs are not just a scaling trick. They are how you turn "this might finish later" into an explicit operating model.

---

## Best-Practice Principles

1. **Move only the slow or failure-prone work out of the request path.**  
   Keep the acceptance path small, authenticated, and durable.

2. **Record the job before doing the expensive work.**  
   The system should know the job exists even if the worker crashes immediately after.

3. **Make job states explicit.**  
   Queued, running, failed, waiting for human, canceled, and succeeded should mean something operationally.

4. **Keep queue payloads minimal.**  
   Put identifiers and durable references in the queue, not full sensitive documents unless there is a clear need.

5. **Separate acceptance from completion.**  
   The request path should confirm "we accepted the job" rather than pretending "the whole AI workflow is done."

6. **Design status retrieval on purpose.**  
   Polling, callbacks, and dashboards should reflect the same job model.

7. **Add observability and cost controls at the job level.**  
   Attempts, latency, failure reason, and spend should be traceable per job.

---

## Concepts

### Request Path

The synchronous path that receives input, authenticates it, validates it, and returns an immediate response.

### Queue

A buffer between job acceptance and job execution.

The queue helps:

- smooth bursts
- keep web workers free
- separate retry behavior from user request behavior

### Worker

A process that consumes queued jobs and performs the slower workflow steps.

### Job Lifecycle

The allowed states and transitions for one unit of work.

Example states:

- accepted
- queued
- running
- waiting_for_human
- failed
- succeeded

### Delivery Semantics

The practical guarantee the system provides about job handling.

For small AI systems, the real question is usually:

- Can the system safely tolerate duplicate delivery?
- Can the system detect unfinished work?

### Status Channel

The way a caller or operator learns what happened.

Examples:

- status endpoint
- webhook callback
- operator dashboard

---

## Queue Architecture Brief Template

Use this structure:

```markdown
# Queue Architecture Brief: [Workflow Name]

## 1. Service Goal

- What the workflow does:
- Why work leaves the request path:

## 2. Acceptance Boundary

- Request validation:
- Authentication or authorization:
- Immediate response:
- Durable record created before enqueue:

## 3. Job Definition

- Job ID:
- Idempotency key:
- Queue payload contents:
- Sensitive fields excluded from the payload:

## 4. Job Lifecycle

| State | Meaning | Entered when | Exit condition | Owner |
| --- | --- | --- | --- | --- |
| accepted |  |  |  |  |
| queued |  |  |  |  |
| running |  |  |  |  |
| waiting_for_human |  |  |  |  |
| failed |  |  |  |  |
| succeeded |  |  |  |  |

## 5. Worker Responsibilities

- Retrieval or dependency work:
- Model work:
- Human handoff:
- Final write path:

## 6. Status and Notification Plan

- Polling endpoint:
- Callback or webhook rule:
- Operator dashboard fields:

## 7. Privacy, Security, Cost, and Observability Notes

- Logging restrictions:
- Queue retention rule:
- Cost guardrail:
- Trace and metric fields:

## 8. Open Questions

- ...
```

Review questions:

- What is the exact moment the system promises "we accepted your work"?
- Which job states are operationally meaningful?
- Can the worker crash without making the job disappear?

---

## Walkthrough

Use this example:

> A support drafting service receives ticket-created webhooks, retrieves policy context, drafts a response, and places the draft into a human review queue.

### Step 1 - Draw the request boundary

For this service, the request path should:

- verify the webhook signature
- validate the ticket payload
- create a durable job record
- enqueue a job reference
- return an acceptance response quickly

It should not:

- call the model directly
- wait for retrieval
- block on human review

That separation protects latency, reliability, and cost control.

### Step 2 - Define the job contract

For a small system, the queue payload can stay narrow:

- job ID
- ticket ID
- tenant or client ID
- policy version reference
- correlation or trace ID

Do not put the full raw ticket thread into the queue unless there is a specific reason. Store it in a durable system of record and reference it.

### Step 3 - Write the lifecycle states

Useful states for this workflow:

- `accepted`
- `queued`
- `running`
- `waiting_for_human`
- `failed`
- `succeeded`

If human review exists, model it directly. Otherwise operators end up guessing whether the job is stuck or intentionally paused.

### Step 4 - Choose the status path

For this service, a sensible approach is:

- clients can poll a status endpoint for `job_id`
- internal operators can use a dashboard
- an optional callback can fire when the draft reaches review-ready status

Polling and callbacks should both reflect the same underlying job state, not different interpretations.

### Step 5 - Keep the worker focused

Worker responsibilities might include:

- fetch the ticket and policy references
- retrieve supporting context
- call the model
- validate the structured output
- write the draft to the review queue
- update final job state

This is where retries, deadlines, and failure handling belong, not in the user-facing acceptance path.

### Step 6 - Add job-level observability

For each job, the system should record:

- job ID
- tenant or environment
- attempt count
- queue age
- execution duration
- model route
- final state
- redacted failure reason

That makes failures reconstructable without dumping private content into logs.

### Step 7 - Avoid the wrong shortcut

FastAPI background tasks are useful for short local follow-up work, but they are not a durable job system for long AI workflows.

If the work must survive process restarts, retry safely, or support operator review, give it a real queue boundary.

---

## Practice

Choose one AI workflow that currently runs synchronously or is likely to outgrow a simple request path.

Produce a **Queue Architecture Brief** that includes:

1. acceptance boundary
2. queue payload definition
3. job lifecycle states
4. worker responsibilities
5. status and notification plan
6. privacy, security, cost, and observability notes

Keep it short enough that an operator and an API builder could both use it.

---

## Review Checklist

- The acceptance path is clearly separated from the expensive work.
- A durable job record exists before execution begins.
- Job states are explicit and operationally useful.
- Queue payloads avoid unnecessary sensitive data.
- Status retrieval is defined for both callers and operators.
- Worker responsibilities are bounded and clear.
- Observability fields exist at the job level.
- Cost and privacy controls are present.

---

## Common Failure Modes

- Using the web process as the only place a long AI job can run.
- Returning success when the system only means "we started trying."
- Treating human review as an informal side effect instead of a real state.
- Putting too much client data into queue payloads.
- Forgetting to define what happens when the worker crashes mid-job.
- Building a queue without any status surface for users or operators.

---

## Portfolio Evidence

Keep a sanitized copy of your queue architecture brief and lifecycle contract.

Redact:

- client names
- queue names
- endpoint URLs
- tenant identifiers
- exact retention settings if sensitive

What remains is strong evidence that you can move an AI workflow from synchronous prototype to durable service behavior.

---

## References

- [FastAPI Background Tasks](https://fastapi.tiangolo.com/tutorial/background-tasks/)
- [Celery Introduction](https://docs.celeryq.dev/en/main/getting-started/introduction.html)
- [Temporal Documentation](https://docs.temporal.io/)
- [OpenTelemetry Concepts](https://opentelemetry.io/docs/concepts/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
