# Lesson 13.01: What Changes When a Workflow Leaves Your Laptop

**Parent syllabus:** [Syllabus 13: Cloud Deployment](../../syllabus-13-cloud-deployment.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Deployment boundary brief and runtime responsibility map

---

## Outcome

By the end of this lesson, you can describe what changes when an AI workflow moves from a local script to a deployed service, including runtime expectations, trigger paths, secrets handling, state boundaries, and operator responsibilities.

You will produce:

- a deployment boundary brief
- a runtime responsibility map
- deployment assumptions and exclusions

---

## Why This Matters

A local workflow can depend on your machine, your shell session, your memory, and your presence. A deployed service cannot.

Common failures:

- The workflow only works because the developer's laptop has the right files, credentials, and environment variables.
- A webhook arrives when the machine is asleep or disconnected.
- Secrets live in shell history, `.env` files, or copied dashboard notes with no clear owner.
- Temporary files or local disk state are treated like durable storage.
- The team says "production" when the system still depends on a manual restart.
- Nobody can say what "24/7 availability" actually means for the service.
- The cloud deployment exists, but no one has written down how to disable it safely if it misbehaves.

Moving to the cloud is not only a hosting change. It is a responsibility change.

---

## Best-Practice Principles

1. **Define the deployed service boundary before choosing a platform.**  
   The real questions are what triggers the service, what it stores, what it can access, and what "working" means.

2. **Separate local convenience from runtime requirements.**  
   A deployed workflow should not depend on your shell session, laptop filesystem, or manual supervision.

3. **Write the environment contract explicitly.**  
   Ports, secrets, external services, storage, and startup assumptions should be named before deployment.

4. **Treat trigger paths as production interfaces.**  
   Webhooks, schedules, and manual jobs each create different reliability, security, and observability requirements.

5. **Assume local disk is disposable.**  
   Containers and serverless runtimes may restart, scale out, or disappear without preserving local files.

6. **Define the disable path early.**  
   A service that can be deployed but not safely paused or rolled back is operationally weak.

7. **Keep the first deployment mentally small.**  
   One service, one trigger, one log path, and one runbook beat an elaborate setup no one can maintain.

---

## Concepts

### Deployed Service

A workflow that runs in an environment other than your laptop and can be triggered without you being present.

### Trigger Path

The mechanism that starts work.

Examples:

- HTTP webhook
- scheduled job
- queue message
- manual operator trigger

### Environment Contract

The set of runtime expectations the service requires.

Typical fields:

- port
- secrets
- outbound network dependencies
- storage dependencies
- CPU and memory expectations

### Runtime Responsibility Map

A short description of which system owns which part of operation.

Examples:

- platform starts the process
- secret manager provides credentials
- external database stores durable records
- operator reviews alerts and disables the service when needed

### Stateless Service

A service that does not rely on local process memory or local disk for durable correctness across requests.

### Disable Path

The documented way to stop new work, roll back, or quarantine the service during an incident.

---

## Deployment Boundary Brief Template

Use this structure:

```markdown
# Deployment Boundary Brief: [Workflow Name]

## Workflow Goal

What does this service do?

## Trigger Path

- Primary trigger:
- Secondary triggers:
- Expected request volume:

## Runtime Expectations

- Availability target:
- Max acceptable delay:
- Startup assumptions:

## Environment Contract

- Port:
- Required secrets:
- External services:
- Durable storage:
- Temporary local storage:

## Included Responsibilities

- What the deployed service owns:
- What the platform owns:
- What the operator owns:

## Excluded Assumptions

- What must not depend on a laptop, manual shell session, or local file path:

## Disable and Recovery Path

- How to pause new work:
- How to roll back:
- How to confirm the service is disabled:
```

Runtime responsibility map structure:

```markdown
# Runtime Responsibility Map

| Concern | Primary owner | Backup or dependency | Notes |
| --- | --- | --- | --- |
| Trigger handling |  |  |  |
| Secrets |  |  |  |
| Durable state |  |  |  |
| Alerts |  |  |  |
| Disable path |  |  |  |
```

Review questions:

- What still depends on one person's machine or memory?
- What data survives a restart?
- How would you stop the service safely in five minutes?

---

## Walkthrough

Use this example:

> A support drafting workflow currently runs as a local FastAPI service on a laptop, receives webhook events from a ticketing system, retrieves approved policy context, drafts a response, and places the result in a human review queue.

### Step 1 - Name what changes

On the laptop, the workflow depends on:

- the machine being awake
- the right virtual environment being active
- local secrets being set
- the developer noticing failures

Once deployed, those are no longer acceptable assumptions.

The deployed service now needs:

- a stable trigger path
- durable secret access
- reliable logging
- a defined disable path

### Step 2 - Define the trigger path

For this workflow, the trigger is an inbound webhook from the ticketing system.

That implies:

- the service needs a reachable HTTP endpoint
- the request should be authenticated or signed
- duplicate deliveries are possible
- the service should log request and trace identifiers

Trigger choice is architecture. It changes how the system fails.

### Step 3 - Separate durable and disposable state

Bad assumption:

> The service can write queue state to a local JSON file on disk.

Better assumption:

- local temp files are disposable
- durable queue state lives outside the process
- logs go to the platform log path
- secrets come from a managed secret source, not from the image

Anything that must survive restart should not live only inside the process container.

### Step 4 - Write the environment contract

For this service, the contract might include:

- one inbound HTTP port
- one API key or signing secret for the ticketing webhook
- one model-provider key
- one durable store or queue dependency
- one review destination

Without this contract, "works on my machine" just becomes "works in one cloud revision."

### Step 5 - Define the disable path

Weak answer:

> We can probably just turn it off in the dashboard.

Better answer:

- disable the public trigger or set the service to reject new work
- roll back to the last known good revision if the new revision misbehaves
- verify no new queue items are created
- notify the owner and reviewers

The review question is:

> How do we stop harm, not just stop compute?

### Step 6 - Assign responsibilities

For the support drafting service:

- platform owns process start and runtime scaling
- secret manager owns credential delivery
- external store owns durable queue state
- workflow owner owns rollout and rollback decision
- operator owns alert response and disable confirmation

If these owners are not named, gaps appear during the first incident.

---

## Practice

Choose one workflow you have already built locally.

Create:

1. a deployment boundary brief
2. a runtime responsibility map
3. a list of excluded local assumptions
4. a disable and recovery path

Minimum requirements:

- one primary trigger path
- one secrets boundary
- one durable-state decision
- one documented rollback or disable action

---

## Review Checklist

Your deployment boundary artifact is acceptable when:

- The trigger path is explicit.
- Runtime expectations are named.
- Secrets, storage, and external dependencies are listed.
- Disposable local state is separated from durable state.
- Responsibilities are assigned across platform, service, and operator.
- Excluded laptop-specific assumptions are written down.
- The disable path is clear.
- A future maintainer could explain what the cloud service actually owns.

---

## Common Failure Modes

- **Lift-and-hope deployment:** The workflow is moved to the cloud without redefining assumptions.
- **Laptop leakage:** Paths, local files, or manual shell state remain hidden dependencies.
- **No disable path:** The service can start, but no one knows how to stop it safely.
- **State confusion:** Local disk is treated like durable storage.
- **Secret sprawl:** Credentials are copied into images, notes, or startup scripts carelessly.
- **Unnamed operators:** Everyone assumes someone else will respond.

---

## Portfolio Evidence

Save:

- the deployment boundary brief
- the runtime responsibility map
- one short note explaining the service's disable path

This shows that you can define what a deployed service actually is before you choose where it runs.

---

## References

- FastAPI Deployment: https://fastapi.tiangolo.com/deployment/
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- Google Cloud Run documentation: https://cloud.google.com/run/docs
- AWS Lambda Developer Guide: https://docs.aws.amazon.com/lambda/latest/dg/welcome.html
