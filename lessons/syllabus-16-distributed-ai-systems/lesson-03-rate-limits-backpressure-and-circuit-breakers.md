# Lesson 16.03: Rate Limits, Backpressure, and Circuit Breakers

**Parent syllabus:** [Syllabus 16: Distributed AI Systems](../../syllabus-16-distributed-ai-systems.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Overload protection policy and circuit breaker plan

---

## Outcome

By the end of this lesson, you can define how a distributed AI service protects itself and its dependencies during traffic spikes, provider slowdowns, and cost-pressure events.

You will produce:

- an overload protection policy
- a rate-limit and backpressure plan
- a circuit breaker and degraded-mode rule set

---

## Why This Matters

Distributed systems rarely fail because one request is too hard. They fail because too many requests arrive while something downstream gets worse.

Common failures:

- A provider slowdown causes the queue to grow until every caller is delayed.
- Retry storms increase load on a dependency that is already failing.
- Monthly model budget is exhausted because the service keeps accepting work during a burst.
- One noisy tenant consumes shared capacity and harms everyone else.
- The system knows it is overloaded, but has no rule for what to reject first.
- Operators see high error rates but cannot tell whether to shed load, open a circuit, or wait.

Overload control is what keeps a bad hour from becoming a bad week.

---

## Best-Practice Principles

1. **Protect the system at multiple layers.**  
   Ingress, queue depth, worker concurrency, provider calls, and side-effect systems each need their own guardrail.

2. **Prefer bounded rejection over uncontrolled collapse.**  
   Failing early and clearly is often safer than letting latency and cost spiral.

3. **Tie overload decisions to business priority.**  
   Not all work has equal value when capacity is constrained.

4. **Use circuit breakers to stop retrying doomed paths.**  
   If a dependency is unhealthy, open the circuit and change behavior intentionally.

5. **Design a degraded mode before you need it.**  
   "Manual review only" or "accept but pause drafting" may be safer than pretending normal behavior still exists.

6. **Make overload signals visible to operators.**  
   Queue age, rejection rate, circuit state, and spend should be observable, not inferred.

7. **Test pressure with realistic spikes.**  
   Quiet success under light load does not prove safe behavior under burst conditions.

---

## Concepts

### Rate Limit

A rule that caps how much work one caller, tenant, or system can submit over time.

### Backpressure

The deliberate slowing or refusal of new work so the system can recover instead of collapsing.

### Load Shedding

Rejecting or deferring lower-priority work when the system is beyond safe capacity.

### Circuit Breaker

A control that stops calls to a failing dependency for a period of time and routes the system into fallback behavior.

### Degraded Mode

A reduced but safer operating mode used during dependency failure or overload.

Examples:

- accept work but do not start drafting yet
- skip low-value enrichment
- route directly to human review

### Queue Age

How long work waits before execution.

For many AI workflows, queue age is a better overload signal than raw queue length.

---

## Overload Protection Policy Template

Use this structure:

```markdown
# Overload Protection Policy: [Workflow Name]

## 1. Capacity Risks

- Most likely overload source:
- Most expensive dependency:
- Work that can be delayed or rejected first:

## 2. Protection Layers

| Layer | Signal | Threshold | Action | Owner |
| --- | --- | --- | --- | --- |
| Ingress |  |  |  |  |
| Queue |  |  |  |  |
| Worker |  |  |  |  |
| Provider dependency |  |  |  |  |
| Budget or spend |  |  |  |  |

## 3. Circuit Breaker Rules

- Dependency:
- Open condition:
- Fallback behavior:
- Recovery check:

## 4. Degraded Modes

- Mode 1:
- Mode 2:
- Exit condition:

## 5. Privacy, Security, Cost, and Observability Notes

- Sensitive work that must not be deferred casually:
- Cost shutoff rule:
- Required alerts and dashboards:

## 6. Open Questions

- ...
```

Review questions:

- What should be rejected first when the system is under pressure?
- Which signal tells you the queue is unhealthy soon enough to act?
- What happens when the model provider is slow but not fully down?

---

## Walkthrough

Use this example:

> A support drafting system usually handles steady traffic, but product incidents can create large bursts of inbound tickets while the model provider and retrieval store both slow down.

### Step 1 - Protect ingress

For this workflow, ingress controls may include:

- per-tenant rate limits
- authenticated sources only
- a hard cap on accepted work per minute

This prevents one source from flooding shared capacity.

### Step 2 - Watch queue age, not just queue length

Queue length alone can be misleading if job size varies.

More useful signals:

- oldest queued job age
- number of jobs in retry
- worker concurrency saturation
- percentage of work routed to degraded mode

Those signals tell the operator whether the system is falling behind in a way users will notice.

### Step 3 - Define load shedding order

For this service, the system might:

1. reject or defer low-priority tenants first
2. pause optional enrichment
3. reduce concurrency against the provider
4. route new tickets to manual review if the queue age exceeds the safety boundary

That is more defensible than letting the system degrade randomly.

### Step 4 - Add a circuit breaker

If the model provider error rate or timeout rate rises sharply:

- open the circuit
- stop sending new drafting calls for a short interval
- place jobs in a deferred or manual-review path
- probe carefully for recovery

Without that control, retries and new requests can keep pounding the same bad dependency.

### Step 5 - Connect overload to cost

For AI systems, overload and cost often move together.

Examples:

- retry storms increase model spend
- queue backlog can hide the true cost of accepted work
- degraded dependency behavior can increase token use or fallback calls

Your overload policy should include a spend-based safety trigger, not only technical error signals.

### Step 6 - Tell operators what degraded mode means

For the support drafting workflow, degraded mode might mean:

- accept jobs
- skip the draft generation step
- create a manual review item with retrieved context only

That keeps service moving while protecting quality and cost.

### Step 7 - Make pressure visible in one place

Useful dashboard fields:

- accepted versus rejected requests
- queue age
- retry volume
- circuit breaker state
- provider latency and error rate
- cost per completed unit during the incident window

If the operator needs four screens to understand pressure, the policy is not finished.

---

## Practice

Choose one distributed AI workflow and create an **Overload Protection Policy** that includes:

1. ingress rate limits
2. queue and worker pressure signals
3. circuit breaker rules for one critical dependency
4. degraded-mode behavior
5. cost-triggered safety actions
6. core observability and operator signals

Write it as if someone else may need to operate the system under stress.

---

## Review Checklist

- Protection exists at more than one layer.
- Overload signals are measurable and operator-visible.
- Rate limits protect shared capacity fairly.
- Queue age or another user-impacting signal is included.
- Circuit breaker behavior is defined for a critical dependency.
- Degraded modes are explicit and reversible.
- Cost guards are included alongside technical signals.
- Alerts and dashboards support real operational action.

---

## Common Failure Modes

- Letting the queue grow without any explicit load-shedding rule.
- Using retries as the only response to provider trouble.
- Measuring request count without measuring queue age or backlog.
- Forgetting that overload can be a budget incident, not only a latency incident.
- Rejecting work randomly instead of by policy.
- Opening a circuit but not defining fallback behavior.

---

## Portfolio Evidence

Keep a sanitized copy of your overload policy and circuit breaker plan.

Redact:

- tenant names
- exact provider limits
- internal thresholds if sensitive
- queue names
- alert routing details

What remains demonstrates that you can design AI services to degrade intentionally rather than collapse unpredictably.

---

## References

- [Google SRE Book: Addressing Cascading Failures](https://sre.google/sre-book/addressing-cascading-failures/)
- [Google SRE: Handling Overload](https://sre.google/resources/book-update/handling-overload/)
- [OpenTelemetry Concepts](https://opentelemetry.io/docs/concepts/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
