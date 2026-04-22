# Syllabus 13: Cloud Deployment
**Status:** Production Spine - Moving the Workflow Off the Laptop
**Format:** One chat session per unit, self-paced

---

## What This Is

Once a workflow has basic production controls, the next step is getting it off your machine and into a runtime you can actually depend on. This syllabus is about the first real cloud boundary in the course path.

It focuses on packaging, runtime assumptions, triggers, low-maintenance operations, and launch verification. It is not yet about full repeatable delivery or distributed runtime complexity. Those come next.

---

## Unit 1 - What Changes When a Workflow Leaves Your Laptop

**Goal:** Understand the runtime boundary you are creating when local code becomes a service.

**Topics:**
- Why "it works on my machine" is not a production argument
- What changes when uptime, ingress, secrets, storage, and logs move into a cloud runtime
- The difference between a local script, a service, and a triggered workload
- What cloud deployment does and does not solve by itself
- How this module builds directly on the controls from Syllabus 12

**Session starter:**
> "Explain what changes when an AI workflow leaves my laptop and becomes a cloud service. I want the real runtime boundary, not just the deployment steps: secrets, ingress, failure modes, state, and operator responsibility."

---

## Unit 2 - Container Packaging and Docker Basics

**Goal:** Package one workflow into a repeatable runtime unit.

**Topics:**
- What a container is and why packaging matters for AI services
- Writing a practical Dockerfile for a Python or FastAPI workflow
- Port, entrypoint, dependency, and secret-injection assumptions
- Building locally before trusting the cloud
- Avoiding oversized or unclear build contexts

**Session starter:**
> "Teach me container packaging and Docker basics for an AI workflow. I want to understand the runtime contract, write a clean Dockerfile, and verify locally before I trust a cloud deployment."

---

## Unit 3 - Serverless Deployment and Webhook Triggers

**Goal:** Choose a low-maintenance cloud runtime and define how work gets into it.

**Topics:**
- What serverless does well for AI APIs and webhook-driven workloads
- Cloud Run, Lambda, and similar patterns in plain language
- Webhook verification, duplicate delivery, and trigger design
- Environment variables, secrets, and runtime config in the cloud
- When a simple serverless service is enough and when it is not

**Session starter:**
> "Help me design a serverless deployment and webhook trigger path for an AI workflow. I want the tradeoffs, not just the steps: runtime choice, webhook verification, duplicate handling, and secrets delivery."

---

## Unit 4 - Low-Maintenance Cloud Operations

**Goal:** Keep one cloud deployment operable without constant dashboard babysitting.

**Topics:**
- Health checks, restart behavior, and minimal operator signals
- Low-energy operations patterns for one small service
- Secret ownership, runtime responsibility, and safe disable paths
- Cost, privacy, and observability considerations at the deployment boundary
- Keeping the first cloud setup small enough to support well

**Session starter:**
> "Help me design low-maintenance cloud operations for one AI service. I want health checks, restart behavior, operator signals, and a clear disable path without building an overcomplicated platform."

---

## Unit 5 - Your First End-to-End Cloud Deployment

**Goal:** Take one controlled workflow from local runtime to cloud runtime with launch evidence.

**Topics:**
- Choosing the first workflow that is actually suitable for deployment
- The path from package to deploy to trigger to smoke test
- Verifying that the service works without relying on your laptop
- Writing a one-page operator runbook for restart, rollback, and first checks
- Deciding what counts as "staging only" versus "ready for limited rollout"

**Session starter:**
> "Walk me through my first end-to-end cloud deployment for an AI workflow. I want packaging, deployment, trigger verification, smoke testing, and a simple operator runbook that I could use under pressure."

---

## Unit 6 - Cloud Deployment Review Package

**Goal:** Combine the deployment artifacts into one reviewer-ready package.

**Topics:**
- Assembling deployment boundary, packaging, trigger design, health policy, and launch verification into one packet
- Checking that runtime assumptions, rollback rules, and secret handling do not contradict each other
- Writing rollout blockers and a final deployment decision
- Naming the later changes that should force a deployment re-review

**Session starter:**
> "Help me assemble a cloud deployment review package for one AI workflow. I want the runtime boundary, packaging spec, trigger design, health policy, launch verification, and a final rollout recommendation in one coherent packet."

---

## Practice Project

Take one workflow that already has the production controls from Syllabus 12 and turn it into a cloud deployment package that includes:

- a deployment boundary brief
- a container packaging specification
- a serverless or webhook trigger decision
- a low-maintenance operations runbook
- a launch verification worksheet
- a final cloud deployment recommendation with blockers and next steps
