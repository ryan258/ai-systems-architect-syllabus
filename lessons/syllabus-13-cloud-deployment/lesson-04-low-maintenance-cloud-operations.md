# Lesson 13.04: Low-Maintenance Cloud Operations

**Parent syllabus:** [Syllabus 13: Cloud Deployment](../../syllabus-13-cloud-deployment.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Service health policy and low-energy operations runbook

---

## Outcome

By the end of this lesson, you can design a small cloud deployment that is understandable, observable, and recoverable on a low-energy day, with a clear health signal, alert path, restart or rollback rule, and source-of-truth configuration.

You will produce:

- a service health policy
- a low-energy operations runbook
- minimum viable alert and rollback rules

---

## Why This Matters

Many first deployments technically work but are too fragile or too confusing to operate.

Common failures:

- The only way to understand the deployment is to click through several dashboards.
- Health checks prove that the process is alive but not that the service can do useful work.
- Alerts exist, but they fire to no one or carry no first-response guidance.
- Config lives partly in code, partly in memory, and partly in a dashboard note.
- The team can deploy a new revision but cannot say how to roll it back safely.
- Restart is treated like a fix even when the real problem is bad config or broken dependencies.

Low-maintenance operations do not mean no maintenance. They mean a small, repeatable, readable operating path.

---

## Best-Practice Principles

1. **Keep one source of truth for deployment intent.**  
   The basic runtime configuration should live in versioned files or a documented deployment definition, not only in click paths.

2. **Use health checks that answer a real service question.**  
   A green process is weaker than a green service boundary.

3. **Alert on conditions that change action.**  
   If the signal does not alter operator behavior, it should not page anyone.

4. **Prefer rollback and disable over improvisation.**  
   During an incident, the first safe move is often to stop new work or restore the last known good revision.

5. **Keep the first-response runbook short.**  
   Operators need three to five checks, not a novel.

6. **Make configuration reviewable.**  
   Environment variables, secrets sources, scaling settings, and ingress rules should be visible without guessing.

7. **Design for the maintainer you will be on a bad day.**  
   The runbook should still work under fatigue, stress, or brain fog.

---

## Concepts

### Service Health Policy

A short document defining what healthy means for the deployed service and how that health is checked.

### Liveness Check

A signal that the process or instance is still running.

### Readiness or Boundary Check

A signal that the service can actually accept and handle useful work.

### Source-of-Truth Configuration

The versioned or explicitly documented place where deployment settings are defined.

### First-Response Runbook

A short list of what to check and what to do when the service alerts or misbehaves.

### Rollback Rule

The condition that tells the operator to return to the previous known-good revision or disable the service.

---

## Service Health Policy Template

Use this structure:

```markdown
# Service Health Policy: [Workflow Name]

## Service Goal

What must the deployment do reliably?

## Health Signals

- Liveness:
- Readiness or boundary check:
- Critical dependency checks:

## Config Source of Truth

- Deployment definition:
- Secret source:
- Scaling settings:
- Ingress settings:

## Alerts

| Alert | Threshold | Owner | First response |
| --- | --- | --- | --- |
|  |  |  |  |

## Rollback and Disable Rules

- Roll back when:
- Disable ingress when:
- Verify recovery by:
```

Low-energy operations runbook structure:

```markdown
# Low-Energy Operations Runbook

## First Checks

1. ...
2. ...
3. ...

## If the service is unhealthy

- ...

## If the latest deployment caused the problem

- ...

## If the issue is ingress or secret related

- ...
```

Review questions:

- What is the smallest signal that shows the service is actually usable?
- Which alert should trigger rollback instead of more debugging?
- Where is the deployment configuration actually defined?

---

## Walkthrough

Use this example:

> A support drafting webhook service is deployed to a small request-driven cloud runtime and writes review-ready jobs to an external queue.

### Step 1 - Define what healthy means

Weak health statement:

> The container is up.

Better health statement:

> The service can accept authenticated webhook requests, reach its required downstream services, and write a review job without violating cost or safety limits.

That is the difference between infrastructure alive and service useful.

### Step 2 - Keep configuration reviewable

For this service, configuration should include:

- public ingress rule
- expected listening port
- required secrets sources
- queue destination
- scaling limits or concurrency expectations

If those values live only in a dashboard, maintenance becomes memory work.

### Step 3 - Write small, actionable alerts

Useful alerts for this service might include:

- sustained non-2xx response spike on webhook ingress
- health endpoint failure beyond a short threshold
- queue-write failure rate above baseline
- sudden cost or fallback-route anomaly from the previous module's controls

Each alert should say who owns it and what the first response is.

### Step 4 - Define rollback versus restart

Weak incident habit:

> Restart first and figure it out later.

Better rule:

- restart only for transient platform or process issues
- roll back if the latest deployment introduced the error
- disable ingress if the service is creating unsafe or duplicate side effects

Restart is a tool. It is not a diagnosis.

### Step 5 - Write the low-energy runbook

For this service, a first-response runbook might be:

1. confirm whether the problem is ingress, service startup, or downstream dependency
2. check whether the latest revision or config change preceded the failure
3. inspect the last known good health signal and recent trace IDs
4. roll back or disable ingress if user-impacting errors continue
5. confirm queue writes or downstream side effects have stopped if the service is disabled

That is enough to act without opening six dashboards in panic.

### Step 6 - Keep the design simple

A good first deployment often has:

- one service
- one health endpoint
- one alert destination
- one rollback path
- one runbook page

Complexity can come later if the workload earns it.

---

## Practice

Choose one workflow you want to operate in the cloud.

Create:

1. a service health policy
2. a low-energy operations runbook
3. at least three alerts with owners and first responses
4. one rollback rule
5. one disable-ingress rule

Minimum requirements:

- one boundary-level health signal
- one source-of-truth configuration note
- one dependency check
- one low-energy first-response sequence

---

## Review Checklist

Your operations artifact is acceptable when:

- Health means more than "process is running."
- Configuration sources are visible.
- Alerts are actionable and owned.
- Rollback and disable rules are explicit.
- The first-response runbook is short and usable.
- The design avoids excessive dashboard-only knowledge.
- The operator can tell when to restart, roll back, or disable ingress.
- The service would still be understandable on a low-energy day.

---

## Common Failure Modes

- **Dashboard dependency:** The only operating knowledge lives in click paths.
- **Liveness-only health:** The service looks up even when useful work is broken.
- **Alert with no action:** Notifications arrive, but nobody knows what to do next.
- **Restart reflex:** Every problem is treated as a restart problem.
- **Config fog:** No one can say which settings are actually in force.
- **No disable rule:** Unsafe behavior continues because stopping the service is unclear.

---

## Portfolio Evidence

Save:

- the service health policy
- the low-energy operations runbook
- one short note explaining the rollback and disable rules

This shows that you can make a deployment operable, not just reachable.

---

## References

- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- OpenTelemetry Concepts: https://opentelemetry.io/docs/concepts/
- Google Cloud Run documentation: https://cloud.google.com/run/docs
- Terraform documentation: https://developer.hashicorp.com/terraform/docs
