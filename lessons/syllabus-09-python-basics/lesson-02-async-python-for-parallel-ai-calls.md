# Lesson 09.02: Async Python for Parallel AI Calls

**Parent syllabus:** [Syllabus 09: Advanced Python for AI Workflow Architecture](../../syllabus-09-python-basics.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Concurrency decision memo and async runner

---

## Outcome

By the end of this lesson, you can decide when async Python actually helps an AI workflow and build a bounded async runner that handles concurrency, timeouts, and partial failure without turning simple scripts into operational debt.

You will produce:

- a concurrency decision memo
- an async runner with bounded parallelism
- timeout and rate-limit handling rules

---

## Why This Matters

Async code solves one real problem and creates several fake ones when used carelessly.

Common failures:

- A script uses `asyncio.gather()` on a large list with no concurrency cap.
- Rate limits get worse because the code fan-outs too aggressively.
- Results arrive out of order, but the workflow has no clear way to map them back.
- Exceptions inside tasks are swallowed or grouped badly.
- Async is used for one API call where sync code would be simpler and safer.
- Cancellation and timeout behavior are undefined, so the script hangs under failure.

Async helps when the work is I/O-bound and parallelism is real. It hurts when it is added because it sounds advanced.

---

## Best-Practice Principles

1. **Use async for I/O-bound fan-out, not as decoration.**  
   If the script does one request at a time or is CPU-bound, async usually adds complexity without value.

2. **Bound concurrency explicitly.**  
   A semaphore or queue limit is part of your cost and reliability control surface.

3. **Separate business logic from scheduling logic.**  
   The function that decides what to do should not also manage task orchestration details.

4. **Set timeout and cancellation rules up front.**  
   If a task runs too long, the workflow should know whether to retry, skip, or fail the batch.

5. **Preserve identity per task.**  
   Each concurrent request should carry an item ID or trace ID so logs stay reconstructable.

6. **Plan for partial completion.**  
   Some items may finish, some may fail, and the workflow still needs a coherent result.

7. **Prefer the simpler path when performance does not matter.**  
   Maintainability wins when the scale is small.

---

## Concepts

### Concurrency Decision

A deliberate choice about whether parallel execution is worth the added complexity.

### Coroutine

A function defined with `async def` that can suspend and resume around await points.

### Fan-Out and Fan-In

Fan-out starts many tasks. Fan-in collects the results.

### Bounded Parallelism

A cap on how many tasks may run at once.

This protects:

- rate limits
- cost
- operator understanding

### Timeout Budget

The maximum allowed time for one request or one batch phase.

### Partial Failure

A batch where some tasks fail but the whole workflow still completes with a useful summary or retry plan.

---

## Concurrency Decision Template

Use this structure:

```markdown
# Concurrency Decision Memo: [Workflow Name]

## Goal

What is the workflow trying to speed up?

## Current Path

Sync or async?

## Why Async Helps or Does Not Help

- ...

## Unit of Parallel Work

What is one task?

## Concurrency Limit

How many tasks may run at once, and why?

## Timeout Rules

- Per task:
- Whole batch:

## Partial Failure Policy

What happens when some tasks fail?

## Rate-Limit Strategy

- Backoff:
- Retry cap:
- Fallback:
```

Example runner pattern:

```python
import asyncio
from dataclasses import dataclass


@dataclass
class BatchResult:
    successes: list
    failures: list


async def run_batch(items, worker, concurrency_limit: int = 5) -> BatchResult:
    semaphore = asyncio.Semaphore(concurrency_limit)
    successes = []
    failures = []

    async def guarded(item):
        async with semaphore:
            try:
                async with asyncio.timeout(30):
                    result = await worker(item)
                    successes.append((item["id"], result))
            except Exception as exc:
                failures.append((item["id"], type(exc).__name__, str(exc)))

    await asyncio.gather(*(guarded(item) for item in items))
    return BatchResult(successes=successes, failures=failures)
```

This pattern is intentionally small. The important part is bounded concurrency and visible partial failure.

---

## Walkthrough

Use this example:

> A workflow drafts first-pass support replies for 20 tickets pulled from a queue.

### Step 1 - Decide whether async is justified

Good reasons:

- each ticket requires a network call to an LLM API
- the calls are independent
- batch latency matters

Bad reasons:

- there is only one ticket at a time
- the slow part is local parsing or file processing
- no measurable bottleneck exists yet

The first decision is architectural, not syntactic.

### Step 2 - Define the unit of parallel work

One task should be one ticket draft request.

Not:

- one huge batch prompt with 20 tickets mixed together
- one task that also writes logs, updates databases, and sends alerts

Keep the unit narrow enough to reason about failure cleanly.

### Step 3 - Cap the concurrency

If the provider allows only modest throughput or the prompts are expensive, a limit of 3 to 5 may be better than 20.

This is where reliability, cost, and provider behavior meet.

Unbounded concurrency is not "faster." It is often just burstier failure.

### Step 4 - Define timeout and retry behavior

Per-task timeout:

- stop the individual request if it exceeds the budget

Whole-batch timeout:

- stop the batch and report which items completed

Retry guidance:

- retry transient network failures or rate limits carefully
- do not retry malformed business output forever

### Step 5 - Preserve item identity in logs

Every task should emit:

- run ID
- ticket ID
- attempt number
- start and end time
- selected route or model
- final status

Without stable identity, async troubleshooting becomes guesswork.

### Step 6 - Decide what partial completion means

For this workflow:

- successful drafts move to human review
- failed drafts are recorded in a retry or manual-review list
- the batch summary reports counts and failure reasons

The batch should not pretend success if 8 of 20 items failed.

### Step 7 - Know when sync is better

Use sync code when:

- the script is run rarely
- one request happens at a time
- the team maintaining it is small
- performance is already acceptable

Async is a tool, not a maturity badge.

---

## Practice

Choose one workflow that processes a batch of independent items.

Create:

1. a concurrency decision memo
2. an async runner or a written decision to stay sync
3. per-task and whole-batch timeout rules
4. a partial failure reporting rule

If you choose async, include:

- bounded concurrency
- stable item IDs in results
- one timeout
- one failure collection path

If you decide to stay sync, explain why that is the more maintainable choice.

---

## Review Checklist

Your concurrency artifact is acceptable when:

- It states whether async is justified.
- The unit of parallel work is clear.
- Concurrency is bounded.
- Timeout behavior is explicit.
- Partial failure is handled deliberately.
- Logs or results preserve per-item identity.
- The design does not depend on unbounded `gather()` behavior.

---

## Common Failure Modes

- **Async by reflex:** The code becomes harder to maintain without real performance gain.
- **Unbounded fan-out:** Too many requests are launched at once.
- **No timeout policy:** Hung requests stall the whole batch.
- **No failure collection:** One exception collapses the batch with poor context.
- **Lost identity:** Results cannot be mapped back to source items.
- **Provider thrash:** Rate-limit errors increase because concurrency was not sized intentionally.

---

## Portfolio Evidence

Save:

- the concurrency decision memo
- the async runner or sync-decision note
- a sample batch summary showing successes and failures

This shows that you can use concurrency as an engineering decision instead of a code-style preference.

---

## References

- Python `asyncio`: https://docs.python.org/3/library/asyncio.html
- Python Coroutines and Tasks: https://docs.python.org/3/library/asyncio-task.html
- OpenTelemetry Concepts: https://opentelemetry.io/docs/concepts/
