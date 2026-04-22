# Lesson 16.02: Idempotency, Retries, and Timeouts

**Parent syllabus:** [Syllabus 16: Distributed AI Systems](../../syllabus-16-distributed-ai-systems.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Idempotency policy and retry-timeout matrix

---

## Outcome

By the end of this lesson, you can define how a distributed AI workflow handles duplicate requests, transient failures, and slow dependencies without duplicating side effects or hanging forever.

You will produce:

- an idempotency policy
- a retry-timeout matrix
- a dead-letter and operator escalation rule

---

## Why This Matters

The moment work becomes asynchronous, repeated delivery stops being hypothetical.

Common failures:

- A webhook arrives twice and the system sends two client emails.
- A model call times out, but the worker keeps waiting with no deadline.
- Retries stack on top of each other and turn a temporary outage into a cost spike.
- The system retries non-idempotent steps like writes, approvals, or notifications without any guardrail.
- Jobs fail repeatedly for the same bad input and never reach an operator.
- Status tracking says "failed," but the external side effect already happened once.

Idempotency, retries, and timeouts are how distributed systems stay correct when the network, providers, or workers are imperfect.

---

## Best-Practice Principles

1. **Put idempotency at the boundary where duplication becomes expensive.**  
   If a repeated request can create duplicate work or duplicate side effects, design the key before shipping.

2. **Retry only transient failures.**  
   Invalid input, permission errors, and schema mismatches usually need correction, not more attempts.

3. **Give every important step a timeout or deadline.**  
   Without one, "stuck" and "slow" become the same operational state.

4. **Record side-effect completion separately from attempt failure.**  
   A step can time out after the downstream system already processed it.

5. **Use backoff and jitter.**  
   Aggressive synchronized retries often make dependency failures worse.

6. **Escalate poison work instead of looping forever.**  
   Some jobs need a dead-letter queue or manual review state.

7. **Test duplicate delivery and partial failure on purpose.**  
   If those scenarios are never exercised, they will fail first in production.

---

## Concepts

### Idempotency Key

A stable identifier that lets the system recognize repeated requests as the same intended action.

Examples:

- external event ID
- client request ID
- ticket ID plus action type

### Deduplication Window

The time period in which repeated delivery should resolve to the same logical work rather than creating a new job.

### Retry Policy

A rule that defines:

- what is retryable
- how many attempts are allowed
- how long to wait between attempts

### Timeout

The maximum duration one operation is allowed to consume.

### Deadline

The maximum end-to-end time budget for the whole request or job.

### Dead-Letter Queue

A holding place for jobs that repeatedly fail and now need diagnosis or operator intervention.

---

## Idempotency Policy and Retry-Timeout Matrix Template

Use this structure:

```markdown
# Idempotency Policy and Retry-Timeout Matrix: [Workflow Name]

## 1. Boundary Rules

- External request boundary:
- Internal job boundary:
- Side effects that must never duplicate:

## 2. Idempotency Policy

| Operation | Idempotency key | Dedup window | Duplicate behavior | Source of truth |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## 3. Retry and Timeout Matrix

| Step | Retryable? | Max attempts | Backoff rule | Timeout | Deadline owner |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

## 4. Poison Work and Escalation

- Dead-letter rule:
- Operator escalation rule:
- Human approval or correction path:

## 5. Privacy, Security, Cost, and Observability Notes

- Sensitive fields excluded from retry logs:
- Cost guardrail:
- Required trace fields:

## 6. Open Questions

- ...
```

Review questions:

- Which operations must never duplicate?
- Which failures deserve retry versus manual review?
- Where is the end-to-end deadline enforced?

---

## Walkthrough

Use this example:

> A support drafting service is triggered by ticket webhooks, creates asynchronous jobs, retrieves policy context, drafts a response, and writes the result to a review queue.

### Step 1 - Start at the outer boundary

For this workflow, the webhook event ID is a strong candidate idempotency key.

The system should be able to say:

- if this exact event arrives again, do not create a second job
- if the original job already succeeded, return that outcome
- if the original job is still running, report that state

That protects both cost and correctness.

### Step 2 - Separate job creation from side effects

The workflow has more than one non-repeatable action:

- enqueue the job
- write a draft to the human review queue
- optionally notify another system

Each one may need its own deduplication rule or side-effect ledger. A single top-level key is not enough if later steps can be retried independently.

### Step 3 - Decide what is retryable

For this service:

- temporary model-provider timeouts may be retryable
- transient retrieval-store errors may be retryable
- invalid webhook signatures are not retryable
- malformed response schema after multiple attempts may need manual review
- permission failures are usually not retryable

If everything is retried, the system will pay more to fail louder.

### Step 4 - Write the timeout matrix

Useful timeout layers:

- request acceptance timeout
- queue visibility or processing timeout
- retrieval timeout
- model call timeout
- final write timeout
- overall job deadline

The whole job should also have an end-to-end deadline so old work does not keep consuming money after it has lost business value.

### Step 5 - Add backoff and jitter

For retryable failures:

- start with short exponential backoff
- add jitter so workers do not retry in lockstep
- cap attempts and escalate after the limit

This matters when a dependency is degraded across many jobs at once.

### Step 6 - Plan poison work handling

Some failures are not going away without intervention:

- bad input
- schema mismatch after several attempts
- unsupported tenant configuration
- repeated policy retrieval corruption

Those jobs should move into a dead-letter or operator-review state with enough redacted evidence to diagnose the issue safely.

### Step 7 - Make partial failure visible

One of the hardest cases is:

- the system writes the review item successfully
- the confirmation response times out
- the worker thinks it failed and retries

That is why you need a durable record of side-effect completion and not just final step status.

---

## Practice

Choose one queued AI workflow and create an **Idempotency Policy and Retry-Timeout Matrix** that includes:

1. outer request or webhook idempotency
2. non-repeatable side effects
3. retryable and non-retryable failures
4. timeout values by step
5. end-to-end job deadline
6. poison-work escalation path

Prefer a small, explicit matrix over a vague paragraph about "we retry on failure."

---

## Review Checklist

- The main duplicate-risk boundary has an idempotency key.
- Non-repeatable side effects are identified explicitly.
- Retry rules distinguish transient from permanent failure.
- Timeouts exist for important dependencies and the whole job.
- Backoff and jitter are defined for retries.
- Poison work reaches a dead-letter or operator path.
- Logs and traces contain enough evidence without leaking sensitive data.
- Cost risk from repeated retries is controlled.

---

## Common Failure Modes

- Assuming queues deliver each job exactly once.
- Retrying all failures without classifying them.
- Setting individual step timeouts but no end-to-end deadline.
- Forgetting to guard the final side effect against duplicate execution.
- Sending poison work back into the main queue forever.
- Logging full request payloads to debug duplicate delivery.

---

## Portfolio Evidence

Keep a sanitized copy of your idempotency policy and retry-timeout matrix.

Redact:

- client identifiers
- event formats
- secret headers
- internal timeout thresholds if sensitive
- downstream system names if required

What remains shows that you understand correctness under duplication and partial failure, not just happy-path automation.

---

## References

- [RFC 9110: Idempotent Methods](https://httpwg.org/specs/rfc9110.html#idempotent.methods)
- [Celery User Guide: Tasks](https://docs.celeryq.dev/en/latest/userguide/tasks.html)
- [Temporal Documentation](https://docs.temporal.io/)
- [Google SRE Book: Addressing Cascading Failures](https://sre.google/sre-book/addressing-cascading-failures/)
- [OpenTelemetry Concepts](https://opentelemetry.io/docs/concepts/)
