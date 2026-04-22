# Lesson 06.07: Live Monitoring After Deployment

**Parent syllabus:** [Syllabus 06: How AI Models Work](../../syllabus-06-how-ai-works.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Monitoring specification and operator runbook

---

## Outcome

By the end of this lesson, you can monitor an AI workflow after launch so quiet quality failures, cost spikes, and routing drift become visible before a client reports them.

You will produce:

- a monitoring specification
- key logs, metrics, and traces
- alert rules
- drift checks
- an operator runbook

---

## Why This Matters

Pre-launch testing and post-launch observation solve different problems.

Common failures:

- The workflow passes an eval set but degrades in real traffic.
- Latency climbs slowly over time because context is growing.
- Human reviewers reject more drafts, but nobody tracks that as a quality signal.
- Costs spike after a routing or prompt change and the alert arrives too late.
- Logs store too much sensitive data or too little evidence to reconstruct failures.
- A quiet model regression is visible in operator comments for days before anyone acts.

If nobody is watching the right signals, the workflow can fail politely for a long time.

---

## Best-Practice Principles

1. **Monitor user impact, not only infrastructure health.**  
   Queue depth matters, but so do rejection rate, escalation rate, schema failure rate, and time to useful output.

2. **Treat structured logs as a design artifact.**  
   If a failure cannot be reconstructed from trace IDs, step status, route choice, and model metadata, the logging design is weak.

3. **Protect privacy inside observability.**  
   Do not solve debugging by dumping full prompts or sensitive records into logs.

4. **Track drift through proxies and review loops.**  
   Monitor changes in approval rate, escalation rate, error class, and sampled human review outcomes.

5. **Alert on decision-relevant thresholds.**  
   Alerts should trigger action, not noise.

6. **Connect monitoring to the eval plan.**  
   The behaviors that matter before launch should still matter after launch.

7. **Write the operator runbook while the system still makes sense.**  
   A monitor without a response plan is only an expensive notification system.

---

## Concepts

### Structured Log

A machine-readable event record for each important workflow step.

Typical fields:

- trace ID
- workflow step
- route choice
- model
- latency
- token use or cost
- parser result
- approval outcome
- error reason

### Quality Proxy Metric

A measurable signal that correlates with actual workflow quality.

Examples:

- human approval rate
- revision rate
- policy-check fail rate
- escalation rate
- customer reopen rate

### Drift

A meaningful change in system behavior over time.

Examples:

- more frequent reviewer corrections
- more schema failures after a model update
- more fallback routes after local runtime degradation
- longer prompts causing slower output

### Alert Threshold

A boundary that triggers operator attention.

Examples:

- p95 latency above target for 30 minutes
- review rejection rate doubles
- per-ticket cost exceeds limit
- queue depth remains above threshold

### Runbook

A short operating guide that explains what to check and what to do when a monitor fires.

---

## Monitoring Specification Template

Use this structure:

```markdown
# Monitoring Specification: [Workflow Name]

## Service Goal

What must this workflow deliver reliably?

## Key Workflow Steps

- Step 1:
- Step 2:
- Step 3:

## Logs

What structured events must be recorded?

## Metrics

- Latency:
- Cost:
- Quality proxies:
- Routing:
- Reliability:

## Traces

How will requests be reconstructed end to end?

## Alerts

| Alert | Threshold | Owner | Action |
| --- | --- | --- | --- |
|  |  |  |  |

## Drift Checks

What weekly or daily checks detect quality movement over time?

## Privacy and Retention Rules

What is redacted, retained, and deleted?

## Runbook

What does the operator do first, second, and third when an alert fires?
```

---

## Walkthrough

Use the support response drafting and review system.

### Step 1 - Monitor the whole path

Key steps:

1. ticket intake
2. policy retrieval
3. draft generation
4. policy check
5. human review

Weak monitoring:

> API uptime and CPU usage

Better monitoring:

- time from ticket intake to draft availability
- draft rejection rate by reviewers
- policy-check fail rate
- queue depth
- cost per ticket
- local vs cloud route usage

### Step 2 - Define structured logs

Example fields:

```text
trace_id
ticket_id
workflow_step
selected_model_path
model_name
prompt_version
schema_version
latency_ms
estimated_cost_usd
policy_ids_used
approval_outcome
error_class
```

This supports reconstruction without storing raw customer content.

### Step 3 - Add quality proxy metrics

For this workflow:

- reviewer approval rate
- reviewer revision rate
- escalation rate on ambiguous tickets
- policy-check fail rate
- draft delayed status rate

If revision rate rises sharply after a model or prompt change, that is operationally important even if the service is technically up.

### Step 4 - Write alerts with action

Example alerts:

| Alert | Threshold | Owner | Action |
| --- | --- | --- | --- |
| Draft latency high | p95 above 30 seconds for 30 minutes | Operator | check queue depth, provider latency, and context growth |
| Reviewer rejection spike | rejection rate above 20 percent for one hour | Workflow owner | sample ten recent cases and compare to last release baseline |
| Cost spike | average cost per ticket above hard limit | Product owner | inspect route mix and long-context growth |
| Policy-check failures | fail rate doubles from baseline | Workflow owner | review recent policy retrieval inputs and hold rollout expansion |

### Step 5 - Add drift checks that humans can actually run

Weekly drift review:

- sample 10 reviewed tickets
- compare approval and revision rates to baseline
- inspect top rejection reasons
- compare route distribution and prompt version changes

This is lightweight and useful.

### Step 6 - Write the runbook

Example:

```markdown
## Runbook

1. Identify whether the alert is quality, latency, cost, or routing related.
2. Pull the recent trace IDs and compare the current version to the last known good version.
3. Check queue depth, route mix, model availability, and schema failure rate.
4. Sample recent human-reviewed cases to confirm whether user impact is real.
5. If the alert involves customer-facing risk, hold rollout expansion or revert the recent change.
```

---

## Practice

Choose one workflow that is already live, close to live, or realistic enough to operate.

Create a monitoring specification and operator runbook with:

1. service goal
2. key workflow steps
3. required structured logs
4. at least six metrics
5. end-to-end trace plan
6. at least four alerts
7. at least two drift checks
8. privacy and retention rules
9. first-response runbook

At least one metric must represent:

- quality
- cost
- latency
- route behavior or fallback behavior

---

## Review Checklist

Your monitoring spec is acceptable when:

- It monitors workflow outcomes, not only infrastructure.
- Logs are structured and reconstructable.
- Sensitive data handling is explicit.
- At least one quality proxy metric exists.
- At least one cost metric exists.
- Alerts have owners and actions.
- Drift checks compare current behavior to a baseline.
- Monitoring is connected to the eval plan or release gate.
- The runbook explains what to do when an alert fires.
- The design is realistic for a small team to maintain.

---

## Common Failure Modes

- **Infra-only monitoring:** The workflow is up, but the outputs are getting worse.
- **Privacy leaks in logs:** Sensitive prompts become the debugging strategy.
- **No action thresholds:** Alerts exist but no one knows when to respond.
- **No baseline:** Drift is claimed without comparison.
- **No human signal:** Reviewer rejection or rework is ignored.
- **No runbook:** Alerts create confusion instead of response.

---

## Portfolio Evidence

Save:

- The monitoring specification
- The alert table
- The operator runbook

This shows you can observe AI workflows after launch, not just build them.

---

## References

- OpenTelemetry concepts: https://opentelemetry.io/docs/concepts/
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
