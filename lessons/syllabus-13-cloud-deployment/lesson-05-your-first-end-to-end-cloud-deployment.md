# Lesson 13.05: Your First End-to-End Cloud Deployment

**Parent syllabus:** [Syllabus 13: Cloud Deployment](../../syllabus-13-cloud-deployment.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Deployment launch worksheet and one-page operator runbook

---

## Outcome

By the end of this lesson, you can take one small AI workflow from local runtime to live cloud deployment with a defined build path, secret setup, webhook or trigger verification, smoke test, and one-page operator runbook.

You will produce:

- a deployment launch worksheet
- deployment verification evidence
- a one-page operator runbook

---

## Why This Matters

The first end-to-end deployment is where the whole chain is tested for real.

Common failures:

- The team chooses the most complicated workflow first and gets blocked everywhere at once.
- The image builds, but the deployed service cannot reach its dependencies.
- Secrets were set in one environment but not another.
- The webhook trigger reaches the service, but the service is not ready for duplicate or malformed events.
- The smoke test proves that the endpoint is reachable, not that the workflow did useful work safely.
- No one writes down how to repeat the deployment or how to recover from a bad one.

An end-to-end deployment is not complete when the URL responds. It is complete when the operating path is understandable and repeatable.

---

## Best-Practice Principles

1. **Choose the smallest workflow worth deploying first.**  
   Start with one bounded workflow that already has clear inputs, outputs, and a human review boundary if needed.

2. **Treat deployment as a sequence of verifiable steps.**  
   Build, configure, deploy, test, and recover should each have evidence.

3. **Smoke test the real behavior, not only the port.**  
   The service should prove it can handle one safe, representative request.

4. **Verify secret and config paths deliberately.**  
   Missing or misnamed runtime settings are common deployment failures.

5. **Write the runbook while the path is fresh.**  
   If you wait until the first outage, you will forget the fragile parts.

6. **Keep rollout scoped.**  
   A first live deployment should be limited enough that rollback is cheap.

7. **End with a repeatable operator path.**  
   Another person, or future you, should be able to redeploy or disable the service without guessing.

---

## Concepts

### Deployment Launch Worksheet

A structured record of the steps and checks required to take a service live.

### Smoke Test

A small, representative test proving the deployed service can accept a real request path and produce the expected high-level behavior.

### Verification Evidence

The proof that a launch step actually passed.

Examples:

- image built successfully
- secret injected and readable
- webhook verification succeeded
- queue write or output handoff succeeded

### Rollout Scope

The limited set of users, queues, or triggers exposed in the first live deployment.

### Operator Runbook

A short guide describing restart, rollback, disable, and first checks.

---

## Deployment Launch Worksheet Template

Use this structure:

```markdown
# Deployment Launch Worksheet: [Workflow Name]

## Chosen Workflow

- Goal:
- Trigger:
- External side effects:
- Initial rollout scope:

## Build and Package Checks

- Dockerfile ready:
- Image built:
- Local container smoke test:

## Cloud Configuration Checks

- Runtime chosen:
- Secrets configured:
- Ingress configured:
- Durable state configured:

## Deployment Verification

- Service started:
- Health endpoint passed:
- Test webhook or trigger delivered:
- Expected downstream effect observed:

## Rollback and Disable

- Previous known-good version:
- Rollback path:
- Disable-ingress path:

## Operator Notes

- Main dashboard or logs path:
- Top failure signs:
- First response:
```

One-page operator runbook structure:

```markdown
# Operator Runbook: [Workflow Name]

## What This Service Does

- ...

## First Checks

1. ...
2. ...
3. ...

## Restart, Rollback, Disable

- Restart when:
- Roll back when:
- Disable ingress when:

## Verification After Recovery

- ...
```

Review questions:

- What evidence proves the service handled one real request path?
- What is the initial rollout scope?
- How would another operator undo the launch safely?

---

## Walkthrough

Use this example:

> A support drafting webhook service is ready to move from local Docker run to a small cloud deployment that receives ticket-created events and creates review-ready jobs.

### Step 1 - Choose the right first workflow

Good first workflow characteristics:

- one inbound trigger
- one clear output
- bounded external side effects
- existing human review boundary

Bad first workflow characteristics:

- many downstream writes
- no rollback boundary
- unclear secret sprawl
- multiple trigger types at once

The first deployment should teach the path, not maximize ambition.

### Step 2 - Verify the build path

Before cloud deployment:

- build the image
- run it locally
- confirm the health or root endpoint works
- confirm the service starts with only the documented runtime variables

This catches packaging mistakes before cloud debugging begins.

### Step 3 - Verify cloud configuration

For this service, check:

- runtime target selected
- webhook secret configured
- model-provider secret configured
- queue destination configured
- ingress limited to the intended trigger path

Cloud deployment problems often come from config mismatch, not application logic.

### Step 4 - Smoke test one real request

Weak smoke test:

> The health endpoint returns 200.

Better smoke test:

- send one safe test webhook
- confirm verification passes
- confirm one review job is created
- confirm logs and trace IDs exist
- confirm no duplicate side effect occurs on controlled replay

The smoke test should prove useful behavior, not just process existence.

### Step 5 - Write the operator runbook

For this service, a one-page runbook should answer:

- what the service does
- how to check whether it is healthy
- when to restart versus roll back
- how to disable ingress
- how to confirm recovery or shutdown

If the runbook is longer than the service deserves, the deployment may already be too complex.

### Step 6 - Keep rollout limited

A safe first rollout might be:

- one support queue
- one internal testing tenant
- one small webhook source set

Limited scope buys learning room without pretending the service is already broad-release ready.

---

## Practice

Choose one real workflow that you could reasonably deploy next.

Create:

1. a deployment launch worksheet
2. build and configuration verification evidence
3. one smoke-test plan
4. one-page operator runbook
5. one limited-rollout scope statement

Minimum requirements:

- one secret configuration check
- one end-to-end trigger test
- one rollback action
- one disable-ingress action

---

## Review Checklist

Your end-to-end deployment artifact is acceptable when:

- The chosen workflow is appropriately small for a first deployment.
- Build, config, deploy, and verification steps are all visible.
- Smoke testing proves useful behavior.
- Secret and ingress checks are explicit.
- Rollback and disable paths are written down.
- The operator runbook is short and actionable.
- Initial rollout scope is limited deliberately.
- Another operator could repeat the deployment path without guessing.

---

## Common Failure Modes

- **Wrong first target:** The team tries to deploy the most complex workflow first.
- **Port-only smoke test:** The service is reachable, but useful work is unproven.
- **Config mismatch:** Secrets or destinations differ between local and cloud environments.
- **No rollback path:** Launch happens with no clear undo.
- **Runbook delay:** The deployment path is known only while the author remembers it.
- **Unlimited first rollout:** The team expands scope before the path is stable.

---

## Portfolio Evidence

Save:

- the deployment launch worksheet
- the one-page operator runbook
- one short note explaining the first smoke test and rollout scope

This shows that you can move a workflow off the laptop and into a repeatable cloud path without hand-waving the last mile.

---

## References

- Docker documentation: https://docs.docker.com/
- FastAPI Deployment: https://fastapi.tiangolo.com/deployment/
- Google Cloud Run documentation: https://cloud.google.com/run/docs
- AWS Lambda Developer Guide: https://docs.aws.amazon.com/lambda/latest/dg/welcome.html
