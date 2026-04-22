# Lesson 14.03: Dashboards and Alerts That Do Not Create Noise

**Parent syllabus:** [Syllabus 14: Reliability and Observability](../../syllabus-14-reliability-and-observability.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Dashboard specification and alert routing policy

---

## Outcome

By the end of this lesson, you can design a minimum useful dashboard and alert policy for an AI system so urgent failures are visible, quieter degradation is reviewed appropriately, and operators are not overwhelmed.

You will produce:

- a dashboard specification
- an alert routing policy
- urgent-versus-review signal rules

---

## Why This Matters

Observability fails when every graph is visible and every threshold is noisy.

Common failures:

- The dashboard shows dozens of panels but no obvious answer to "is the service healthy right now?"
- Every alert goes to the same person regardless of type or impact.
- Urgent failures and slow drift use the same alert path.
- The service pages on single-request noise but misses multi-hour degradation.
- Alerts show symptoms without cause, impact, or next action.
- Quality drift is visible only in weekly review, while infrastructure noise pages constantly.
- Teams stop trusting alerts because thresholds were never tied to a real operator decision.

Good alerting reduces confusion. Bad alerting creates more of it.

---

## Best-Practice Principles

1. **Build the dashboard around operator questions.**  
   The first screen should answer whether the service is healthy, degraded, expensive, or unsafe.

2. **Separate paging alerts from review items.**  
   Not every problem deserves immediate interruption.

3. **Alert on sustained conditions, not random spikes.**  
   Reliability alerts should reflect meaningful user impact or operating risk.

4. **Pair every alert with an owner and a first move.**  
   Cause, impact, and next action matter more than perfect prose.

5. **Keep quality drift visible without over-paging.**  
   Some signals belong in daily or weekly review rather than real-time alerting.

6. **Route alerts by domain of action.**  
   Platform, workflow, review operations, and product owners often need different signals.

7. **Prefer a small dashboard that stays used.**  
   One sharp view is stronger than a pile of unlabeled charts.

---

## Concepts

### Primary Dashboard

A single view operators can check first to understand current service state.

### Alert Tier

The urgency class assigned to a signal.

Examples:

- page now
- investigate during business hours
- weekly review only

### Sustained Threshold

A threshold that requires the condition to persist for some time before alerting.

### Alert Routing Policy

The rule that decides who gets which alert and by what channel.

### Review Signal

A non-paging indicator used in daily or weekly reliability review.

Examples:

- slow rise in reviewer revision rate
- gradual cost growth
- route-mix drift

### Cause-Impact-Action Format

A low-energy alert format that says what happened, why it matters, and what to do first.

---

## Dashboard Specification Template

Use this structure:

```markdown
# Dashboard Specification: [Workflow Name]

## Primary Questions

1. Is the service healthy right now?
2. Are users waiting too long?
3. Are outputs becoming less useful?
4. Is spend or retry behavior abnormal?

## Dashboard Sections

- Service state:
- Latency and completion:
- Quality and handoff:
- Cost and routing:
- Dependency health:

## Key Panels

| Panel | Why it matters | Refresh cadence | Owner |
| --- | --- | --- | --- |
|  |  |  |  |

## Alert Routing Policy

| Signal | Tier | Threshold | Owner | Channel | First action |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

## Review-Only Signals

- Daily:
- Weekly:
```

Review questions:

- What should an operator look at first?
- Which alerts truly deserve interruption?
- Which signals are better handled in review loops?

---

## Walkthrough

Use this example:

> A support drafting service processes inbound ticket events, drafts replies, and routes them to a human review queue.

### Step 1 - Write the operator questions

For this service, the first questions are:

1. Are requests completing?
2. Is draft latency still within the expected window?
3. Is the review queue backing up?
4. Did cost or fallback behavior change sharply?

These questions should shape the dashboard layout directly.

### Step 2 - Keep the primary dashboard small

A useful first dashboard might contain:

- request success and failure rate
- p95 request-to-draft latency
- review queue age
- reviewer rejection rate
- cost per completed request
- fallback route percentage

That is enough to support a first pass. Everything else can live deeper.

### Step 3 - Separate alert tiers

Example tiers:

- page now: sustained request failure spike or unsafe backlog condition
- business-hours response: elevated cost or review-queue drift
- weekly review: slow changes in rejection rate or route mix

This prevents reviewer drift from paging at 2 AM while still keeping it visible.

### Step 4 - Route by action owner

Example:

| Signal | Owner |
| --- | --- |
| ingress failure spike | platform owner |
| reviewer backlog | review operations owner |
| cost spike | workflow or product owner |
| quality drift | workflow owner |

Routing by ownership is what makes alerts actionable.

### Step 5 - Use cause-impact-action wording

Weak alert:

> Latency threshold exceeded.

Better alert:

> Draft latency has stayed above the target for 30 minutes. Reviewers are waiting longer than normal. Check queue depth, recent model-route changes, and dependency latency first.

Low-energy alert design matters because most alerts happen when attention is already limited.

### Step 6 - Keep quiet degradation in review loops

Some issues should not page immediately:

- gradual rejection-rate increase
- route-distribution drift
- steady cost creep

Those belong in daily or weekly reliability review, where trend context is visible.

---

## Practice

Choose one live or planned AI system.

Create:

1. a dashboard specification
2. at least six key panels
3. an alert routing policy
4. at least three alert tiers
5. a list of daily and weekly review signals

At least one alert must cover:

- sustained failure or completion problems
- latency or backlog
- cost or fallback behavior

---

## Review Checklist

Your dashboard and alert artifact is acceptable when:

- The dashboard starts from operator questions.
- The primary view is small enough to scan quickly.
- Alerts are separated by urgency tier.
- Thresholds are sustained, not spike-driven by default.
- Owners and first actions are named.
- Quality drift is visible even when it is non-paging.
- Review-only signals are defined.
- The design avoids alert fatigue.

---

## Common Failure Modes

- **Wall-of-charts dashboard:** Everything is visible, but nothing is prioritized.
- **One-tier alerting:** Every signal pages or nothing does.
- **Ownerless alerts:** Signals arrive with no clear responder.
- **Spike obsession:** Short noise creates repeated interruptions.
- **No quality path:** Reliability dashboards ignore output usefulness.
- **Review blindness:** Slow degradation is never examined because it was not page-worthy.

---

## Portfolio Evidence

Save:

- the dashboard specification
- the alert routing policy
- one short note explaining why certain signals are review-only instead of paging

This shows that you can make monitoring usable rather than loud.

---

## References

- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- OpenTelemetry Concepts: https://opentelemetry.io/docs/concepts/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
