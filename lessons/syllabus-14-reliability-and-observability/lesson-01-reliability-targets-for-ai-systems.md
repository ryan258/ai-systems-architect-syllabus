# Lesson 14.01: Reliability Targets for AI Systems

**Parent syllabus:** [Syllabus 14: Reliability and Observability](../../syllabus-14-reliability-and-observability.md)  
**Estimated time:** 2-3 hours  
**Artifact:** SLI/SLO worksheet and error-budget policy

---

## Outcome

By the end of this lesson, you can define what "working" means for an AI system in measurable terms, including latency, failure rate, quality, cost, and human-handoff behavior.

You will produce:

- an SLI/SLO worksheet
- an error-budget policy
- a reliability boundary for one workflow

---

## Why This Matters

Reliability work fails when the team starts by collecting metrics instead of defining what matters.

Common failures:

- Uptime is treated as the whole reliability story even though the system can stay up while outputs get worse.
- The team chooses one aggressive uptime target with no connection to client expectations or business risk.
- Latency is measured, but handoff backlog and reviewer delay are ignored.
- Cost spikes are treated as finance problems instead of reliability problems.
- Quality failures are left out because they are harder to measure.
- No one knows what amount of failure is acceptable before release expansion should stop.
- Internal tools and client-facing systems are given the same targets without regard to impact.

If "working" is not defined clearly, dashboards and alerts become decoration.

---

## Best-Practice Principles

1. **Define the user-facing service first.**  
   Reliability targets should describe what the user or operator actually experiences, not only what the infrastructure is doing.

2. **Use more than one kind of SLI.**  
   AI systems need a mix of latency, failure, quality, cost, and handoff metrics.

3. **Make the SLO realistic for the service class.**  
   Internal prototypes, limited rollouts, and client-facing systems do not deserve the same reliability target by default.

4. **Keep quality in the reliability conversation.**  
   Plausible but wrong output is a reliability problem when it affects service usefulness.

5. **Define the error budget in plain language.**  
   Operators should know what amount of failure is tolerated before release expansion pauses or rollback becomes appropriate.

6. **State exclusions and dependency assumptions.**  
   A useful reliability target names what is included, what is excluded, and which dependencies shape it.

7. **Make the worksheet reviewable on a low-energy day.**  
   A future maintainer should be able to answer "what counts as healthy?" in a minute.

---

## Concepts

### Reliability Boundary

The specific service behavior you are promising to users or operators.

Examples:

- webhook accepted and review job created
- answer returned within target latency
- human handoff queue stays below a maximum age

### Service Level Indicator (SLI)

A measurable signal used to judge service behavior.

Examples:

- successful request rate
- p95 latency
- approval queue age
- cost per completed request
- reviewer rejection rate

### Service Level Objective (SLO)

A target range for one or more SLIs over a defined period.

### Error Budget

The amount of failure or degradation the service is allowed before the team should slow change, hold rollout, or improve reliability.

### User-Impacting Event

A request or workflow outcome that matters to a real user or operator.

Examples:

- request timed out
- draft arrived too late for review
- cost guardrail stopped a legitimate batch
- reviewer rejected output because it was not usable

### Service Class

The level of operational expectation for a system.

Examples:

- internal personal tool
- internal team workflow
- limited client rollout
- client-facing production service

---

## SLI/SLO Worksheet Template

Use this structure:

```markdown
# SLI/SLO Worksheet: [Workflow Name]

## Service Goal

What must this workflow deliver reliably?

## Service Class

- Internal personal tool / internal team workflow / limited client rollout / client-facing production

## Reliability Boundary

- What counts as a successful unit of work:
- What is excluded:
- Which dependencies matter most:

## Candidate SLIs

| SLI | Why it matters | Measurement window | Included in SLO? |
| --- | --- | --- | --- |
|  |  |  |  |

## Chosen SLOs

| SLO | Target | Window | Owner |
| --- | --- | --- | --- |
|  |  |  |  |

## Error Budget Policy

- What failure is tolerated:
- What actions pause rollout:
- What actions require rollback or redesign:

## Review Cadence

- Weekly review:
- Release review:
```

Review questions:

- What does a successful unit of work look like from the user's perspective?
- Which SLI would matter if the service stayed up but became less useful?
- What change should stop when the error budget is burned?

---

## Walkthrough

Use this example:

> A support drafting service receives tickets, drafts policy-aligned responses, and routes them to a human review queue.

### Step 1 - Define the reliability boundary

Weak boundary:

> The API stays online.

Better boundary:

> The service accepts authenticated ticket events, produces a review-ready draft within the target window, and keeps the approval queue inside an acceptable age range without violating cost guardrails.

That boundary includes usefulness, not only availability.

### Step 2 - Choose candidate SLIs

Useful candidate SLIs for this service:

- successful ticket-to-draft completion rate
- p95 time from ticket receipt to draft availability
- reviewer rejection rate
- approval queue age
- cost per completed ticket

This is stronger than one uptime number because it matches real user impact.

### Step 3 - Set realistic SLOs

For a limited rollout, you might choose:

- completed draft availability within target latency for most tickets
- queue age below a certain threshold during business hours
- cost per ticket below the defined operating budget

The exact numbers depend on the service class. The important part is that they are deliberate and reviewable.

### Step 4 - Write the error budget in plain language

Weak error budget:

> We will try to keep the service reliable.

Better error budget:

- if the main latency or completion SLO is missed materially, stop release expansion
- if reviewer rejection or queue age exceeds the acceptable threshold for the review window, investigate before shipping new changes
- if repeated guardrail stops prevent normal work, redesign rather than widen limits casually

Error budget is a decision rule, not only a graph.

### Step 5 - Separate service classes

An internal tool may tolerate:

- lower uptime
- more manual recovery
- more operator awareness

A client-facing service usually needs:

- tighter response targets
- clearer escalation rules
- more conservative rollout gates

The same reliability target should not be copied across both.

### Step 6 - Keep the worksheet usable

The worksheet should make these answers visible quickly:

- what success means
- what failure matters
- what changes when the budget is burned
- who owns the objective

If those answers require reading five documents, the reliability target is too scattered.

---

## Practice

Choose one deployed or deployable AI workflow.

Create:

1. an SLI/SLO worksheet
2. at least five candidate SLIs
3. at least three chosen SLIs
4. at least one SLO
5. an error-budget policy

At least one SLI must represent:

- latency
- failure or completion rate
- quality or human handoff behavior
- cost

---

## Review Checklist

Your reliability-target artifact is acceptable when:

- The service boundary is explicit.
- Candidate SLIs cover more than uptime.
- At least one quality or handoff metric is included.
- Cost is treated as part of reliable operation.
- SLOs match the service class.
- The error budget changes rollout behavior.
- Exclusions and dependency assumptions are named.
- The worksheet is readable without hidden context.

---

## Common Failure Modes

- **Uptime-only target:** The service is available while outputs or review flow degrade.
- **Fantasy SLO:** The target sounds impressive but does not fit the actual service.
- **No quality signal:** Reliability ignores whether users can use the output.
- **No error budget rule:** Teams track misses but keep shipping normally.
- **Service-class confusion:** Internal and external services are treated as if they deserve the same objective.
- **Hidden boundary:** No one can tell what is actually promised.

---

## Portfolio Evidence

Save:

- the SLI/SLO worksheet
- the error-budget policy
- one short note explaining why the chosen SLO fits the service class

This shows that you can define reliability before you start instrumenting it.

---

## References

- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- Google SRE Workbook: https://sre.google/workbook/table-of-contents/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OpenTelemetry Concepts: https://opentelemetry.io/docs/concepts/
