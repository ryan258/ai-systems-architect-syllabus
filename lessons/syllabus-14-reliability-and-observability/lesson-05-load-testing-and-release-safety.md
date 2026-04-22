# Lesson 14.05: Load Testing and Release Safety

**Parent syllabus:** [Syllabus 14: Reliability and Observability](../../syllabus-14-reliability-and-observability.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Load test plan and phased release checklist

---

## Outcome

By the end of this lesson, you can define practical load tests and release-safety checks for an AI workflow so latency, backlog, downstream slowness, and rollback behavior are exercised before clients depend on the system.

You will produce:

- a load test plan
- a phased release checklist
- rollback triggers and release gates

---

## Why This Matters

Reliability often fails at the boundary between "works in staging" and "handles real traffic."

Common failures:

- The API is tested with one polite request at a time, but real traffic arrives in bursts.
- Model or retrieval slowness is never simulated, so worst-case latency is unknown.
- Queue backlog and timeout behavior are guessed instead of tested.
- A release goes to all users at once even though rollback confidence is weak.
- The rollback path exists on paper but has never been exercised.
- The team measures throughput but not usefulness, backlog age, or cost under load.

Load testing and release safety do not require massive infrastructure. They require realistic failure pressure and disciplined rollout boundaries.

---

## Best-Practice Principles

1. **Test the workload shape you expect, not only the happy path.**  
   Bursts, retries, slow dependencies, and backlog growth matter more than perfect single-request demos.

2. **Exercise the slowest important dependency.**  
   AI systems often fail when the model, retrieval store, or approval queue slows down rather than stops entirely.

3. **Measure user-impacting outcomes under load.**  
   Completion rate, queue age, latency, and cost are usually more important than raw requests per second.

4. **Use phased rollout as a reliability tool.**  
   Canary or limited rollout reduces blast radius while telemetry proves the release.

5. **Define rollback triggers before the release.**  
   Operators should know which metrics or conditions stop expansion.

6. **Keep the first release gate small and concrete.**  
   A short checklist that is actually used beats a long process no one follows.

7. **Treat release verification as part of observability.**  
   The same metrics and trace fields used in production should be used to judge the release.

---

## Concepts

### Load Profile

The traffic shape or work pattern the system is expected to handle.

Examples:

- steady low volume
- weekday burst traffic
- batch backfill job

### Worst-Case Dependency Test

A test where one critical dependency becomes slower, less available, or more expensive than normal.

### Phased Rollout

A release that expands in stages rather than going to all users at once.

### Canary

A small early slice of traffic or users used to validate a release under real conditions.

### Release Gate

A measurable condition that must pass before rollout continues.

### Rollback Trigger

A specific condition that should return the service to the previous known-good state.

---

## Load Test Plan Template

Use this structure:

```markdown
# Load Test Plan: [Workflow Name]

## Service Goal

What must the system keep doing under load?

## Load Profiles

- Normal traffic:
- Burst traffic:
- Backfill or batch case:

## Test Scenarios

| Scenario | What changes | Expected behavior | Release blocker if fail? |
| --- | --- | --- | --- |
|  |  |  |  |

## Measurements

- Completion rate:
- p95 latency:
- Queue age:
- Cost per completed unit:
- Error rate:

## Release Gates

- ...

## Rollback Triggers

- ...
```

Phased release checklist structure:

```markdown
# Phased Release Checklist: [Workflow Name]

## Stage 1

- Scope:
- Metrics to watch:
- Stop conditions:

## Stage 2

- Scope:
- Metrics to watch:
- Stop conditions:

## Rollback

- How to revert:
- How to verify rollback:
```

Review questions:

- What is the worst dependency slowdown worth simulating?
- Which metric should stop the rollout immediately?
- How small can the first live cohort be?

---

## Walkthrough

Use this example:

> A support drafting API receives ticket events, retrieves policy context, drafts responses, and writes review items for one support queue.

### Step 1 - Define the load profiles

For this service:

- normal traffic might be steady daytime ticket flow
- burst traffic might follow a product outage or promotion
- batch load might come from replay or reprocessing

If only normal traffic is tested, the main operational risk may be missed.

### Step 2 - Simulate slow dependencies

Important scenarios:

- model latency doubles
- retrieval store becomes slower
- queue writes start timing out

The point is not to perfectly emulate the internet. The point is to see whether the service degrades safely.

### Step 3 - Measure meaningful outcomes

Useful release measurements:

- successful completion rate
- p95 request-to-draft latency
- review queue age
- cost per completed request
- retry or fallback frequency

Requests per second alone is not enough if the service becomes too slow, too costly, or too noisy under load.

### Step 4 - Use phased rollout

A safe first release may look like:

- stage 1: one support queue or small tenant set
- stage 2: larger but still bounded cohort
- stage 3: broader exposure only after stable telemetry

Phased rollout gives the reliability signals room to prove themselves.

### Step 5 - Define rollback triggers

Good rollback triggers might include:

- sustained failure or timeout rate above the defined threshold
- p95 latency outside the agreed boundary for the observation window
- queue age or rejection rate exceeding the acceptable range
- cost per completed request breaking the release budget

Rollback triggers should be written before the release, not negotiated after the incident starts.

### Step 6 - Keep the checklist small

For this workflow, a practical release checklist might cover:

- build and config verified
- representative load test completed
- critical dependency slowdown tested
- rollback path confirmed
- stage-1 metrics and stop conditions agreed

That is enough to make the release safer without overbuilding.

---

## Practice

Choose one deployable AI service or API.

Create:

1. a load test plan
2. at least three traffic or work profiles
3. at least four test scenarios
4. a phased release checklist
5. at least three rollback triggers

At least one scenario must simulate:

- model slowness
- retrieval or dependency slowness
- burst or backlog pressure

---

## Review Checklist

Your release-safety artifact is acceptable when:

- The load profiles reflect real traffic shapes.
- Slow dependencies are tested deliberately.
- Measurements focus on user-impacting outcomes.
- Release gates are explicit.
- Rollback triggers are defined before launch.
- The rollout is phased rather than all-at-once by default.
- The checklist is short enough to use.
- The same telemetry used in production supports the release decision.

---

## Common Failure Modes

- **Happy-path load test:** The service is tested only under polite traffic.
- **Throughput-only focus:** Requests per second are measured while latency, queue age, or cost are ignored.
- **No slow-dependency test:** The most likely failure shape is never exercised.
- **Big-bang release:** All traffic is exposed before telemetry proves the change.
- **Rollback ambiguity:** The team agrees rollback is important but never defines when to do it.
- **Checklist bloat:** Release process is too heavy to run consistently.

---

## Portfolio Evidence

Save:

- the load test plan
- the phased release checklist
- one short note explaining the main rollback trigger

This shows that you can prove a release is safe enough to ship instead of hoping real traffic will be the test.

---

## References

- FastAPI Deployment: https://fastapi.tiangolo.com/deployment/
- Locust documentation: https://docs.locust.io/
- Google SRE Workbook: https://sre.google/workbook/table-of-contents/
- OpenTelemetry Concepts: https://opentelemetry.io/docs/concepts/
