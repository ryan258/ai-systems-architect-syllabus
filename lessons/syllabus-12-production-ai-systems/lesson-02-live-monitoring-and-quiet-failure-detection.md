# Lesson 12.02: Live Monitoring and Quiet Failure Detection

**Parent syllabus:** [Syllabus 12: Production AI Systems](../../syllabus-12-production-ai-systems.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Production observability specification and alert matrix

---

## Outcome

By the end of this lesson, you can instrument a production AI system so latency, cost, routing problems, review-queue issues, and quiet quality failures become visible early enough to act on.

You will produce:

- a production observability specification
- a required-event and required-metric map
- an alert matrix
- a quiet-failure review loop

---

## Why This Matters

Production systems fail across the whole service path, not only inside the model call.

Common failures:

- The API is healthy, but retrieval misses are quietly degrading draft quality.
- Human reviewers are revising more responses, but nobody tracks that as a quality signal.
- Queue wait time climbs, so "human approval" becomes a bottleneck that operators discover too late.
- Fallback routing masks a provider problem while cost doubles.
- Logs capture too little evidence to reconstruct what changed.
- Debugging relies on raw prompts and sensitive records instead of structured event fields.
- Alerting exists, but every threshold fires and nobody trusts it.

Once the workflow is a service, observability has to cover the system boundary, the AI steps inside it, and the human review loop around it.

---

## Best-Practice Principles

1. **Monitor the whole request path.**  
   Intake, retrieval, model calls, policy checks, queue behavior, and approval outcomes all matter.

2. **Treat required events as an architecture decision.**  
   If the service cannot emit the events needed for reconstruction, the design is incomplete.

3. **Track quiet failure signals, not only outages.**  
   Reviewer revision rate, escalation rate, and fallback frequency often show problems before uptime moves.

4. **Keep traces and logs privacy-safe.**  
   Use IDs, versions, and structured fields instead of dumping raw prompts or records.

5. **Write alerts that trigger action.**  
   Every alert should have an owner, threshold, and first response.

6. **Combine automated signals with human sampling.**  
   Some quality drift is visible only when real outputs are reviewed over time.

7. **Make the design small enough to maintain.**  
   A lean signal set with good response rules is stronger than a giant dashboard nobody uses.

---

## Concepts

### Golden Path

The normal end-to-end service flow for one request.

Example:

- request intake
- retrieval
- draft generation
- policy check
- approval queue
- decision recorded

### Required Event

A structured event the system must emit for reconstruction or operations.

Examples:

- request accepted
- retrieval completed
- model call completed
- policy check failed
- queue item created
- reviewer decision recorded

### Quiet Failure

A degradation that does not look like a crash.

Examples:

- revision rate rises
- latency drifts upward
- fallback route usage grows
- queue wait time exceeds reviewer targets

### Signal Chain

The relationship between events, metrics, traces, alerts, and human review.

Healthy observability lets an operator move from:

- alert
- to trace
- to recent events
- to a likely explanation

### Privacy-Safe Debugging

Reconstructing failure with metadata and IDs rather than raw customer content.

### Review Loop

A recurring manual check that samples recent outputs and compares them to the last known baseline.

---

## Production Observability Specification Template

Use this structure:

```markdown
# Production Observability Specification: [Workflow Name]

## Service Goal

What must this service deliver reliably?

## Golden Path

1. ...
2. ...
3. ...

## Required Events

| Event | When emitted | Required fields | Sensitive data rule |
| --- | --- | --- | --- |
|  |  |  |  |

## Metrics

- Intake and queue:
- Latency:
- Cost:
- Quality proxies:
- Routing and fallback:
- Reliability:

## Traces

How are requests reconstructed end to end?

## Alerts

| Alert | Threshold | Owner | First response |
| --- | --- | --- | --- |
|  |  |  |  |

## Quiet-Failure Review Loop

- Daily:
- Weekly:
- Baseline comparison:

## Privacy and Retention Rules

- Redacted fields:
- Retention duration:
- Deletion path:

## First-Response Notes

What should the operator check first when an alert fires?
```

Review questions:

- Can one failed or degraded request be reconstructed from IDs and structured fields?
- Which metrics show quality movement before users complain?
- Which alerts are truly decision-relevant?

---

## Walkthrough

Use this example:

> A support drafting API retrieves approved knowledge, drafts a response, runs policy checks, and posts the result to a human approval queue.

### Step 1 - Map the golden path

The service is not only "call the model."

The actual golden path is:

1. accept request
2. load ticket metadata
3. retrieve policy context
4. draft response
5. run policy check
6. create queue item
7. record reviewer outcome

Monitoring that starts at step 4 is already missing production context.

### Step 2 - Define required events

Minimal required events for this service:

- request accepted
- retrieval completed
- draft generation completed
- policy check result
- queue item created
- reviewer decision recorded
- stop event if cost or safety guardrail fires

Each event should include stable IDs, route choice, version fields, and outcome codes.

### Step 3 - Choose metrics that show quiet failure

Useful metrics here include:

- p50 and p95 latency
- average cost per request
- fallback-route percentage
- policy-check fail rate
- reviewer rejection rate
- queue wait time
- queue age at approval

This is stronger than uptime alone because it measures user-impacting drift.

### Step 4 - Write actionable alerts

Weak alert:

> Quality might be down.

Better alerts:

| Alert | Threshold | Owner | First response |
| --- | --- | --- | --- |
| Draft latency high | p95 above target for 30 minutes | service owner | inspect queue depth, provider latency, and input size growth |
| Fallback spike | fallback route above baseline for one hour | platform owner | compare provider health and recent routing changes |
| Reviewer rejection spike | rejection rate doubles from baseline | workflow owner | sample ten recent traces and compare versions |
| Queue backlog | queue wait time exceeds target | review owner | inspect staffing, trigger mix, and stuck items |

The first response matters as much as the threshold.

### Step 5 - Add a quiet-failure review loop

Not every problem will trigger an alert.

For this service:

- sample 10 recent approved items daily during launch week
- review top rejection reasons weekly
- compare revision rate, queue age, and fallback usage to the last stable week

This protects against slow degradation.

### Step 6 - Protect privacy while debugging

Good observability fields:

- trace ID
- ticket ID
- prompt version
- policy set version
- route name
- latency
- cost estimate
- review outcome

Bad observability shortcut:

> Log the full ticket body and prompt so debugging is easy.

That solves one problem by creating another.

---

## Practice

Choose one workflow that is already live, nearly live, or realistic enough to operate.

Create a production observability specification that includes:

1. a service goal
2. the golden path
3. at least eight required events
4. at least six metrics
5. an end-to-end trace plan
6. at least four alerts with owners and first responses
7. a quiet-failure review loop
8. privacy and retention rules

At least one metric must represent:

- quality
- cost
- latency
- queue or reviewer behavior

---

## Review Checklist

Your observability artifact is acceptable when:

- It covers the whole service path.
- Required events are explicit.
- Traces can reconstruct one request end to end.
- At least one quiet-failure signal exists.
- Alerts have thresholds, owners, and first responses.
- Queue or handoff behavior is observable where relevant.
- Privacy rules are written down.
- The review loop compares current behavior to a baseline.
- The design is realistic for the team that will operate it.

---

## Common Failure Modes

- **Uptime-only monitoring:** The service is available while outputs get worse.
- **No reconstruction path:** Events exist, but a trace cannot be followed across the workflow.
- **Privacy spill:** Raw prompts become the default debugging tool.
- **Alert spam:** Thresholds fire constantly and operators stop trusting them.
- **No human signal:** Reviewer corrections are ignored.
- **No baseline:** Drift is claimed without any comparison point.

---

## Portfolio Evidence

Save:

- the production observability specification
- the alert matrix
- the quiet-failure review loop

This shows that you can observe a live AI service as an operating system, not just as a model call.

---

## References

- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- OpenTelemetry Concepts: https://opentelemetry.io/docs/concepts/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- Python Logging HOWTO: https://docs.python.org/3/howto/logging.html
