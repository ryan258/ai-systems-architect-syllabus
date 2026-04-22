# Lesson 12.01: Cost Architecture and Hard Limits

**Parent syllabus:** [Syllabus 12: Production AI Systems](../../syllabus-12-production-ai-systems.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Cost architecture worksheet and hard-limit policy

---

## Outcome

By the end of this lesson, you can map how a live AI system spends money across the full request path and set hard limits so retries, long inputs, or stuck jobs cannot run without a deliberate stop.

You will produce:

- a cost architecture worksheet
- a per-run and daily budget policy
- hard-stop and alert rules

---

## Why This Matters

Local scripts usually fail one run at a time. Production systems fail through repetition.

Common failures:

- The team estimates only one model call, but the real path includes retrieval, policy checks, retries, and review-queue work.
- A stuck batch replays the same failed items and doubles spend before anyone notices.
- Context growth quietly raises average token usage even though traffic volume looks stable.
- Soft warnings exist in a dashboard, but nothing stops the next expensive call.
- Missing usage metadata is treated as zero cost.
- An emergency override is added during an incident and never removed.
- Finance or client owners ask what the system can spend in a day, and nobody can answer with confidence.

Cost architecture is part of system architecture. If spend is not bounded, the service boundary is weak.

---

## Best-Practice Principles

1. **Price the whole workflow path, not only the main model call.**  
   Retrieval, validation, fallback routes, retries, and queue reprocessing all affect production spend.

2. **Use more than one budget layer.**  
   Per-request, per-run, daily, and manual-approval thresholds solve different failure modes.

3. **Put hard stops at the paid boundary.**  
   Check the budget immediately before the next expensive action, not after it.

4. **Model failure-path cost explicitly.**  
   Retries, malformed output loops, and replayed jobs should be part of the design, not an afterthought.

5. **Treat unknown usage as a risk signal.**  
   If pricing or token evidence is missing, slow down or stop.

6. **Make overrides rare, owned, and temporary.**  
   A cost override without an approver and expiry becomes a hidden permanent state.

7. **Keep the budget policy readable on a low-energy day.**  
   If an operator cannot answer "what can this system spend before it stops?" in a minute, the policy is too vague.

---

## Concepts

### Cost Path

The full set of actions that can create spend for one request or job.

Examples:

- retrieval calls
- one or more model calls
- fallback route to a second provider
- policy-check pass
- queue retries

### Budget Layer

A spend limit at a specific scope.

Typical layers:

- per request
- per run or batch
- daily service budget
- manual approval threshold for unusual jobs

### Failure Amplifier

A condition that multiplies spend beyond the happy path.

Examples:

- repeated schema failures
- long-context input growth
- duplicate event delivery
- replaying an entire batch after a partial error

### Hard Limit

A threshold that must stop or quarantine work.

Examples:

- max estimated spend per run
- max total tokens
- max retries for one item
- max daily spend before manual approval

### Unknown-Usage Rule

The policy for what happens when accurate usage or pricing evidence is missing.

Weak rule:

> Keep going and estimate later.

Better rule:

> Fall back to a conservative estimate, or stop and require operator review.

### Override Window

A temporary exception to normal limits used only with an owner, reason, and expiry.

---

## Cost Architecture Worksheet Template

Use this structure:

```markdown
# Cost Architecture Worksheet: [Workflow Name]

## Workflow Goal

What does the service do, and what is the normal unit of work?

## Paid Steps

| Step | Paid action | Normal usage | Failure-mode usage | Notes |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## Budget Layers

- Per request:
- Per run or batch:
- Daily service budget:
- Manual-approval threshold:

## Failure Amplifiers

- Retries:
- Long inputs:
- Fallback routes:
- Replay or duplicate events:

## Hard Stops

| Trigger | Limit | Stop action | Logged event |
| --- | --- | --- | --- |
|  |  |  |  |

## Alerts

| Alert | Threshold | Owner | Action |
| --- | --- | --- | --- |
|  |  |  |  |

## Unknown-Usage Rule

What happens if cost evidence is missing or incomplete?

## Override Policy

- Who can approve:
- Maximum duration:
- Required reason:
- Removal process:

## Review Cadence

- Daily:
- Weekly:
```

Review questions:

- What does one normal unit of work cost?
- What does one ugly failure-path unit of work cost?
- Which event actually stops spend?
- Who can temporarily raise a limit, and for how long?

---

## Walkthrough

Use this example:

> A support response drafting service receives a ticket, retrieves approved policy context, drafts a reply, runs a policy check, and posts the draft to a human review queue.

### Step 1 - Price the whole path

Weak estimate:

> One draft call per ticket.

Real estimate:

1. retrieve policy context
2. draft response
3. retry once if parsing fails
4. run policy-check prompt
5. write queue item and structured event logs

The lesson is simple: price what the service actually does.

### Step 2 - Separate normal and failure-mode usage

Normal path for one ticket might be:

- retrieval call
- one drafting call
- one policy-check call

Failure path might be:

- retrieval call
- drafting call
- retry draft
- second policy check
- fallback model route

If the worksheet only shows the normal path, it will understate production risk.

### Step 3 - Set more than one budget layer

For this service, reasonable layers might be:

- per request limit to catch unusually long tickets
- per run limit for batch replay or backfill jobs
- daily service limit to prevent repeated incidents from compounding
- manual approval threshold for exceptional export or backfill jobs

Each layer protects against a different mistake.

### Step 4 - Put hard stops at the expensive edge

Weak control:

> Record total cost in the dashboard after the job finishes.

Better control:

- check the request budget before each model call
- stop after the retry cap
- stop if the daily service budget is exhausted
- quarantine duplicate or replayed jobs instead of running them again automatically

Spend should be blocked before the next paid action, not analyzed after it.

### Step 5 - Define unknown-usage behavior

Suppose a provider response is missing token counts.

Weak rule:

> Assume it was small.

Better rule:

- use a conservative estimate if the system can do that reliably
- or stop the run and log `usage_evidence_missing`

Missing usage data is not a bookkeeping detail. It is a control failure.

### Step 6 - Make overrides temporary

If the team needs a higher budget for a one-time backfill:

- name the approver
- state the reason
- set an expiry
- record when the override was removed

Production systems drift when emergency settings become the new baseline.

---

## Practice

Choose one workflow that is already live, close to live, or realistic enough to operate.

Create a cost architecture worksheet and hard-limit policy that includes:

1. the full paid workflow path
2. normal-path and failure-path usage estimates
3. at least three budget layers
4. at least four failure amplifiers
5. at least four hard-stop rules
6. at least three alerts with owners and actions
7. an unknown-usage rule
8. an override policy with approver and expiry

At least one hard stop must cover:

- retries or loops
- long-context or abnormal input growth
- daily service spend

---

## Review Checklist

Your cost architecture artifact is acceptable when:

- It prices the whole workflow path, not only the primary model call.
- It separates normal-path and failure-path usage.
- It has more than one budget layer.
- Hard stops sit at the paid boundary.
- Unknown usage is handled explicitly.
- Duplicate or replay risk is visible.
- Alerts have owners and expected actions.
- Overrides are temporary and governed.
- A future maintainer could explain the budget policy quickly without guessing.

---

## Common Failure Modes

- **Average-only math:** The worksheet prices the happy path and ignores retries, fallbacks, and replay.
- **Soft-limit theater:** Dashboards exist, but nothing stops the next expensive step.
- **Unknown means free:** Missing usage metadata is treated as harmless.
- **No daily cap:** Small failures repeat until they become a large incident.
- **Permanent emergency mode:** An override is added and never removed.
- **No owner:** Cost alerts go to everyone and therefore to nobody.

---

## Portfolio Evidence

Save:

- the cost architecture worksheet
- the hard-stop and alert table
- one redacted note showing how you would handle an override or stop event

This shows that you can design live AI systems with bounded spend instead of hopeful monitoring.

---

## References

- FinOps Framework: https://www.finops.org/framework/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OpenTelemetry Concepts: https://opentelemetry.io/docs/concepts/
- Python `decimal`: https://docs.python.org/3/library/decimal.html
