# Lesson 09.01: Building Reliable Agentic Loops

**Parent syllabus:** [Syllabus 09: Advanced Python for AI Workflow Architecture](../../syllabus-09-python-basics.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Agentic loop control module and failure policy

---

## Outcome

By the end of this lesson, you can build a Python agent loop that retries the right failures, stops when progress is not happening, records decisions for later review, and degrades safely instead of failing silently.

You will produce:

- a loop failure policy
- a loop control module
- explicit stop and escalation rules

---

## Why This Matters

Agentic workflow code often looks fine right before it burns time, money, or trust.

Common failures:

- The loop retries everything, including failures that should stop immediately.
- A model keeps producing unusable output, but the code treats each attempt as progress.
- Exceptions are logged badly or swallowed entirely.
- A stuck loop keeps making API calls because no hard stop exists.
- The workflow cannot explain why it chose a fallback path.
- Sensitive raw prompts end up in logs because observability was added carelessly.

Reliable agent loops are not about clever control flow. They are about making failure visible, bounded, and reviewable.

---

## Best-Practice Principles

1. **Bound the loop in more than one way.**  
   Use limits on iterations, retries, elapsed time, and consecutive failures.

2. **Separate retryable failures from terminal failures.**  
   Rate limits and transient network errors may justify retry. Bad inputs, invalid schema, and missing permissions usually do not.

3. **Track progress explicitly.**  
   If the loop keeps seeing the same state, the same parse failure, or the same decision output, treat that as no progress.

4. **Log decisions, not raw secrets.**  
   Observability is necessary, but dumping prompts or client data into logs is lazy system design.

5. **Design a degraded path.**  
   A good loop can return "needs human review" or "partial result" instead of pretending success.

6. **Keep policy separate from loop mechanics.**  
   Limits, escalation rules, and retry classes should be visible configuration, not hidden magic numbers.

7. **Make operator review possible after the fact.**  
   The code should leave enough evidence to explain what happened on a low-energy day.

---

## Concepts

### Agentic Loop

A loop where an LLM or workflow step influences what happens next.

Examples:

- decide whether more context is needed
- choose a next tool
- retry with a stricter prompt
- stop and escalate to a human

### Retry Budget

The maximum number of repeated attempts allowed for one operation.

This should be small and intentional.

### No-Progress Signal

Evidence that the loop is still running but not improving the state.

Examples:

- same validation error three times
- same route choice with same failure
- repeated empty output
- same ticket reprocessed without new context

### Graceful Degradation

A controlled fallback when the ideal path fails.

Examples:

- return a partial draft
- mark the record for manual review
- skip one item and continue the batch

### Decision Log

A structured record of what the loop attempted, why it stopped, and which branch it chose.

Typical fields:

- run ID
- item ID
- attempt number
- step name
- route choice
- result type
- failure class
- elapsed time

---

## Loop Failure Policy Template

Use this structure:

```markdown
# Loop Failure Policy: [Workflow Name]

## Goal

What is the loop trying to produce?

## Hard Stops

- Max iterations:
- Max consecutive failures:
- Max elapsed time:
- Max spend or token budget:

## Retryable Failures

- ...

## Non-Retryable Failures

- ...

## No-Progress Signals

- ...

## Degraded Outcome

What should happen if the ideal path fails safely?

## Human Escalation Trigger

When must the workflow stop and hand off?

## Required Log Fields

- ...
```

Example control skeleton:

```python
from dataclasses import dataclass
from time import monotonic, sleep
import logging
import uuid

logger = logging.getLogger(__name__)


@dataclass
class LoopPolicy:
    max_iterations: int = 5
    max_consecutive_failures: int = 2
    retry_delay_seconds: float = 1.0
    max_elapsed_seconds: float = 30.0


def run_with_policy(step_fn, policy: LoopPolicy, item_id: str):
    run_id = str(uuid.uuid4())
    started_at = monotonic()
    consecutive_failures = 0
    last_progress_marker = None

    for attempt in range(1, policy.max_iterations + 1):
        if monotonic() - started_at > policy.max_elapsed_seconds:
            raise TimeoutError(f"{item_id} exceeded elapsed-time budget")

        try:
            result = step_fn(attempt=attempt)
        except TransientWorkflowError:
            consecutive_failures += 1
            logger.warning(
                "workflow_retry",
                extra={"run_id": run_id, "item_id": item_id, "attempt": attempt},
            )
            if consecutive_failures > policy.max_consecutive_failures:
                raise
            sleep(policy.retry_delay_seconds)
            continue

        progress_marker = result.progress_marker
        if progress_marker == last_progress_marker:
            raise RuntimeError(f"{item_id} is looping without progress")

        last_progress_marker = progress_marker
        consecutive_failures = 0

        if result.done:
            return result

    raise RuntimeError(f"{item_id} exhausted loop budget")
```

The point is not this exact code. The point is that the stop rules are explicit.

---

## Walkthrough

Use this example:

> A Python workflow reads a support ticket, drafts a reply with an LLM, validates the reply, and may rerun once if the output is malformed.

### Step 1 - Write the loop policy first

Weak approach:

> Keep trying until the output looks right.

Better approach:

- maximum 3 generation attempts
- maximum 1 retry for rate limits
- stop immediately on validation errors caused by missing required fields
- degrade to "needs_human_review" if the draft still fails

This is where reliability and cost controls start. The loop policy is part of the architecture, not cleanup.

### Step 2 - Separate transient from terminal failures

Retryable examples:

- temporary network timeout
- rate-limit response
- provider internal error

Non-retryable examples:

- missing input file
- invalid authentication configuration
- schema validation failure caused by malformed business output
- policy check failure

If everything is retryable, the loop becomes an expensive denial of service against yourself.

### Step 3 - Detect no-progress states

Suppose the workflow gets this pattern:

1. generation attempt succeeds
2. parser says `missing field: citations`
3. second generation attempt succeeds
4. parser says `missing field: citations`

That is not two independent failures. It is one repeated state.

Treat repeated failure shape as a no-progress signal and stop or degrade.

### Step 4 - Log the control decisions

For each attempt, log:

- run ID
- ticket ID
- attempt count
- route or model path
- failure class
- final status

Do not log:

- raw customer text
- full prompts
- secrets or tokens

This gives observability without turning the logs into a privacy incident.

### Step 5 - Degrade on purpose

For the ticket-drafting workflow, graceful degradation might be:

- return a minimal summary only
- mark the ticket as `needs_human_review`
- record the parse failure for later regression testing

That is much better than returning an unvalidated draft or looping until cost limits are hit.

### Step 6 - Keep the policy readable

A future maintainer should be able to answer:

- What makes the loop stop?
- What is allowed to retry?
- When do we escalate?
- What does a partial result look like?

If those answers are buried in nested `try` blocks, maintainability is already failing.

---

## Practice

Choose one workflow script you already have or want to build.

Create:

1. a loop failure policy
2. a Python loop control module or function
3. one degraded outcome path
4. one human escalation trigger

Minimum requirements:

- at least two hard-stop conditions
- at least two retryable failures
- at least two non-retryable failures
- one no-progress rule
- structured log fields for each attempt

Use a real workflow shape such as:

- support reply drafting
- proposal generation
- meeting notes to task plan
- document triage

---

## Review Checklist

Your loop artifact is acceptable when:

- It defines explicit hard stops.
- Retryable and non-retryable failures are separated.
- No-progress detection exists.
- A degraded outcome exists.
- Log fields are structured and do not expose sensitive raw content.
- The operator can tell why the loop stopped.
- The code can be understood without reading every exception branch.

---

## Common Failure Modes

- **Infinite retry by accident:** The loop never reaches a real stop condition.
- **Every error is retryable:** Configuration and data errors are treated like rate limits.
- **No observability:** Failures happen, but the run cannot be reconstructed.
- **Unsafe logs:** Raw prompts, client text, or secrets are dumped into logs.
- **False progress:** Repeated identical failures are counted as forward movement.
- **No degraded path:** The only outcomes are full success or confusing crash.

---

## Portfolio Evidence

Save:

- the loop failure policy
- the loop control module
- one example log entry or redacted trace of a failed run

This shows that you can build agent control flow that behaves like a system instead of a demo.

---

## References

- Python Logging HOWTO: https://docs.python.org/3/howto/logging.html
- OpenTelemetry Concepts: https://opentelemetry.io/docs/concepts/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
