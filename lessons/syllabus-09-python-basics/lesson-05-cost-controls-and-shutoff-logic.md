# Lesson 09.05: Cost Controls and Shutoff Logic

**Parent syllabus:** [Syllabus 09: Advanced Python for AI Workflow Architecture](../../syllabus-09-python-basics.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Run budget policy and shutoff guardrail module

---

## Outcome

By the end of this lesson, you can add budget tracking and automatic shutoff logic to a Python workflow so one bad input, one stuck loop, or one quiet routing change cannot spend money indefinitely.

You will produce:

- a run budget policy
- a guardrail module
- shutoff and escalation rules

---

## Why This Matters

AI workflow cost problems are usually small mistakes repeated many times.

Common failures:

- The script tracks per-call cost but not whole-run cost.
- A loop hits repeated schema failures and still pays for every retry.
- Missing usage metadata is treated as zero cost.
- A new routing path doubles average spend and nobody notices for weeks.
- The script has warnings but no hard stop.
- Budget logic exists in a notebook or comment, not in running code.

Cost controls are reliability controls. A script that cannot stop itself is not production-ready even for personal use.

---

## Best-Practice Principles

1. **Track the run, not only the request.**  
   Budgets should accumulate across retries, branches, and batch items.

2. **Use both soft warnings and hard stops.**  
   Operators need early signals and absolute guardrails.

3. **Treat missing usage data as uncertain, not free.**  
   Unknown cost should trigger caution, not optimism.

4. **Limit more than spend.**  
   Token count, iteration count, consecutive errors, and elapsed time all matter.

5. **Check the budget before the next expensive action.**  
   Guardrails should sit right before model calls or high-cost branches.

6. **Log the reasons for stopping.**  
   A hard stop without a clear reason is operationally weak.

7. **Make escalation part of the policy.**  
   When the budget is exhausted, the system should know whether to fail, defer, or require human review.

---

## Concepts

### Run Budget

The allowed resource envelope for one workflow run.

Typical fields:

- max estimated cost
- max total tokens
- max loop count
- max elapsed seconds
- max consecutive errors

### Soft Limit

A threshold that warns but does not stop execution yet.

### Hard Limit

A threshold that must stop the workflow.

### Guardrail Checkpoint

The point in the code where the script evaluates whether another expensive step is allowed.

### Shutoff Logic

The code path that ends the run when the budget is no longer acceptable.

### Spend Drift

A change in average cost over time caused by longer prompts, different routing, retries, or input growth.

---

## Run Budget Policy Template

Use this structure:

```markdown
# Run Budget Policy: [Workflow Name]

## Goal

What does this workflow do?

## Soft Limits

- Cost:
- Tokens:
- Elapsed time:

## Hard Limits

- Cost:
- Tokens:
- Loop count:
- Consecutive failures:
- Elapsed time:

## Unknown Usage Rule

What happens if usage or pricing data is missing?

## Stop Action

What happens when a hard limit is hit?

## Escalation

When must a human review the stopped run?

## Required Log Fields

- ...
```

Example guardrail pattern:

```python
from dataclasses import dataclass
from decimal import Decimal


@dataclass
class RunBudget:
    max_cost_usd: Decimal
    max_total_tokens: int
    max_iterations: int
    max_consecutive_errors: int
    total_cost_usd: Decimal = Decimal("0")
    total_tokens: int = 0
    iterations: int = 0
    consecutive_errors: int = 0

    def record_usage(self, cost_usd: Decimal, tokens: int) -> None:
        self.total_cost_usd += cost_usd
        self.total_tokens += tokens

    def should_stop(self) -> bool:
        return any(
            [
                self.total_cost_usd > self.max_cost_usd,
                self.total_tokens > self.max_total_tokens,
                self.iterations > self.max_iterations,
                self.consecutive_errors > self.max_consecutive_errors,
            ]
        )
```

The budget object should be checked immediately before the next paid action.

---

## Walkthrough

Use this example:

> A workflow drafts support replies for a batch of tickets and retries once when output validation fails.

### Step 1 - Define the budget envelope

For one run, you might allow:

- up to 2,000,000 input and output tokens combined
- up to 50 ticket attempts
- up to 3 consecutive failures
- up to $15 estimated total spend

The exact numbers are contextual. The important part is that they exist before the run.

### Step 2 - Put the budget at the workflow boundary

Weak pattern:

> Add a warning inside the provider wrapper and hope someone reads it.

Better pattern:

- initialize a run budget when the workflow starts
- update it after each paid call
- check it before making the next paid call
- stop when the hard limit is exceeded

This turns cost from a passive metric into active control logic.

### Step 3 - Handle missing usage carefully

If the provider response does not include token usage:

- estimate conservatively if you can
- or stop the run if pricing cannot be trusted

Do not set missing usage to zero and keep going. That is how cost incidents become invisible.

### Step 4 - Stop on repeated failure loops

Suppose the workflow:

1. calls the model
2. gets malformed output
3. retries
4. gets malformed output again

Even if the total cost is still under budget, the consecutive failure cap should stop the loop. Cost control and reliability control overlap here.

### Step 5 - Log the stop reason

At stop time, record:

- run ID
- batch ID
- total tokens
- total estimated cost
- current item
- failure count
- stop reason

This is what supports later drift review and governance.

### Step 6 - Decide the fallback action

Possible stop actions:

- stop immediately and return a failed status
- stop the batch and write a retry queue
- escalate the remaining items to manual review

The right answer depends on business risk, not just code convenience.

---

## Practice

Choose one workflow script that calls an LLM or other paid API.

Create:

1. a run budget policy
2. a budget guardrail module
3. one soft-limit warning
4. one hard-stop action
5. one escalation path after shutdown

Minimum requirements:

- total-run cost tracking
- token tracking or equivalent usage tracking
- loop or iteration cap
- consecutive-error cap
- explicit behavior when usage data is missing

---

## Review Checklist

Your budget artifact is acceptable when:

- It tracks the run as a whole.
- It has both warning and stop thresholds.
- Unknown usage is handled explicitly.
- Guardrail checks happen before expensive actions.
- Stop reasons are logged.
- Repeated failure loops trigger shutdown.
- The workflow has a defined escalation path after stop.

---

## Common Failure Modes

- **Per-call blindness:** Costs are visible only one request at a time.
- **Warning without stop:** The script notices overspend and continues anyway.
- **Missing data means zero:** Unknown usage is treated as safe.
- **No loop cap:** Retry logic defeats cost controls.
- **No drift view:** Average spend changes but nobody records it.
- **No operator path:** The script stops, but nobody knows what to do next.

---

## Portfolio Evidence

Save:

- the run budget policy
- the budget guardrail module
- one redacted run summary showing a warning or stop event

This shows that you can keep AI workflow code inside an intentional operating envelope.

---

## References

- Python Logging HOWTO: https://docs.python.org/3/howto/logging.html
- Python `decimal`: https://docs.python.org/3/library/decimal.html
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
