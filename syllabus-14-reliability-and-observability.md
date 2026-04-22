# Syllabus 14: Reliability and Observability
**Status:** Production Spine - Defining and Defending Service Health
**Format:** One chat session per unit, self-paced

---

## What This Is

Once a workflow is live in the cloud, the next question is how you know it is healthy and what you do when it is not. This syllabus is the reliability layer in the production course path.

It takes a deployed service and turns health, telemetry, alerts, incidents, and release safety into explicit operating decisions. It builds directly on Syllabus 13 and sets up the repeatable delivery work in Syllabus 15.

---

## Unit 1 - Reliability Targets for AI Systems

**Goal:** Define what "working" means before you build dashboards and alerts.

**Topics:**
- The difference between uptime, latency, usefulness, correctness, and queue health
- Service Level Indicators (SLIs) that matter for AI systems
- Service Level Objectives (SLOs) and error budgets in plain language
- How reliability targets change across internal tools, review-only systems, and client-facing services
- Why quality drift and human handoff can be reliability concerns, not just model concerns

**Session starter:**
> "Help me define reliability targets for an AI system. I want practical SLIs and SLOs for latency, error rate, queue health, cost, output quality, and handoff behavior for a client-facing workflow."

---

## Unit 2 - Logs, Metrics, and Traces

**Goal:** Instrument the system so failures can be reconstructed instead of guessed.

**Topics:**
- The difference between logs, metrics, and traces in an AI service
- Structured logging for requests, model calls, retrieval, tool use, retries, and handoffs
- Trace IDs that connect one user request to the rest of the system
- Metrics for latency, error rate, retries, queue age, and spend
- Privacy restrictions on what should never enter telemetry

**Session starter:**
> "Teach me logs, metrics, and traces for an AI system. I want an instrumentation plan that covers requests, model calls, retrieval, retries, and human handoff without leaking sensitive client data."

---

## Unit 3 - Dashboards and Alerts That Do Not Create Noise

**Goal:** Surface the signals that matter without training operators to ignore them.

**Topics:**
- The minimum useful dashboard for one deployed AI service
- Alert thresholds for latency, error rate, queue age, cost spikes, and quiet failure signals
- Separating paging alerts from weekly review signals
- Detecting degradation that looks plausible instead of broken
- Designing low-energy alerts with clear cause, impact, and next action

**Session starter:**
> "Help me design dashboards and alerts for a deployed AI workflow. I need signals for latency, failures, queue health, and cost, but I want a system operators can actually trust instead of mute."

---

## Unit 4 - Incidents, Runbooks, and Postmortems

**Goal:** Respond to failures with a repeatable operating path.

**Topics:**
- What counts as an incident in an AI system
- Runbooks for first checks, disable paths, rollback, degrade mode, and human escalation
- Communication during an incident
- Postmortems that improve the system instead of only documenting the outage
- Making the system operable on low-energy days and under real stress

**Session starter:**
> "Help me build incident response and runbooks for an AI service. I want first checks, rollback or disable decisions, client communication rules, and a postmortem pattern that actually improves reliability."

---

## Unit 5 - Load Testing and Release Safety

**Goal:** Verify the system under realistic pressure before broader rollout.

**Topics:**
- Load testing for APIs, queues, and slow dependencies
- Simulating provider slowness, retrieval slowness, and backlog growth
- Phased rollout, canary behavior, and rollback triggers
- Release gates that include latency, errors, queue health, and cost
- Deciding what is safe enough to expand and what should stay in limited rollout

**Session starter:**
> "Teach me practical load testing and release safety for an AI workflow. I want to test slow dependencies, backlog behavior, and rollback triggers without building an enormous SRE program."

---

## Unit 6 - Reliability and Observability Review Package

**Goal:** Turn the reliability artifacts into one reviewer-ready packet.

**Topics:**
- Assembling SLOs, telemetry design, dashboards, alerts, incidents, and load tests into one package
- Checking that signals, runbooks, and release gates do not contradict each other
- Writing reliability blockers and a final rollout decision
- Naming the later changes that should force a re-review

**Session starter:**
> "Help me assemble a reliability and observability review package for one AI workflow. I want the SLOs, telemetry design, dashboard and alert policy, incident runbook, release-safety plan, and a final rollout recommendation in one packet."

---

## Practice Project

Take one workflow that has already been deployed through Syllabus 13 and turn it into a reliability package that includes:

- a small SLI and SLO worksheet
- a logs, metrics, and traces specification
- a dashboard and alert policy
- an incident runbook and postmortem template
- a load-test and phased-release plan
- a final reliability recommendation with blockers and next steps
