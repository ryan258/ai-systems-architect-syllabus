# Syllabus 14: Reliability and Observability
**Status:** Enterprise Gap - Knowing When the System Is Healthy
**Format:** One chat session per unit, self-paced

---

## What This Is

Production systems fail in quiet ways. AI systems fail in even quieter ways because they can return plausible output while being wrong, slow, expensive, or unsafe. This syllabus teaches you how to define reliability, instrument the system, respond to incidents, and keep the system understandable on bad days.

---

## Unit 1 - Reliability Targets for AI Systems

**Goal:** Define what "working" means before you try to monitor it.

**Topics:**
- The difference between uptime, correctness, latency, and usefulness
- Service Level Indicators (SLIs) for AI systems
- Service Level Objectives (SLOs) that are realistic for client work
- Error budgets in plain language
- How reliability goals change for internal tools vs. client-facing systems

**Session starter:**
> "Help me define reliability targets for an AI system. What are useful SLIs and SLOs for latency, error rate, cost, output quality, and human handoff? Keep it practical for client-facing AI workflows."

---

## Unit 2 - Logs, Metrics, and Traces

**Goal:** Instrument an AI system so failures can be reconstructed without guesswork.

**Topics:**
- The difference between logs, metrics, and traces
- Structured logging for AI requests, model calls, retrieval, tool calls, and handoffs
- Trace IDs that connect a user request to every downstream step
- Metrics for latency, error rate, retries, cost, and token usage
- What not to log when client data is sensitive

**Session starter:**
> "Teach me logs, metrics, and traces for an AI workflow. Show me what to log for a request, model call, retrieval step, tool call, and human handoff without exposing sensitive data."

---

## Unit 3 - Dashboards and Alerts That Do Not Create Noise

**Goal:** Build monitoring that tells you what matters without overwhelming you.

**Topics:**
- The minimum useful production dashboard
- Alert thresholds for error rate, latency, cost spikes, and failed jobs
- Separating urgent alerts from weekly review items
- Detecting silent degradation in output quality
- Low-energy alert design: clear cause, impact, and next action

**Session starter:**
> "Help me design a minimum useful dashboard and alert system for a deployed AI workflow. I need alerts for latency, errors, cost spikes, and failed jobs, but I do not want noise. Make it low-energy friendly."

---

## Unit 4 - Incidents, Runbooks, and Postmortems

**Goal:** Know what to do when a production AI system breaks.

**Topics:**
- What counts as an incident
- Writing a runbook that tells you exactly what to check first
- Rollback, disable, degrade, or handoff: choosing the right response
- Client communication during an incident
- Writing a blameless postmortem that improves the system

**Session starter:**
> "Help me write an incident response runbook for an AI system. Include what to check first, how to decide whether to roll back or disable the system, what to tell the client, and how to write a useful postmortem."

---

## Unit 5 - Load Testing and Release Safety

**Goal:** Verify the system can handle real traffic before clients depend on it.

**Topics:**
- Basic load testing for FastAPI and background jobs
- Testing worst-case latency when the model or vector database is slow
- Canary releases and phased rollout in plain language
- Rollback plans for AI workflow changes
- Proving a release is safe enough to ship

**Session starter:**
> "Teach me practical load testing and release safety for an AI workflow API. I want to test latency, model slowness, vector database slowness, and rollback plans without overbuilding."

---

## Practice Project

Choose one deployed or deployable AI workflow. Define three SLIs and one SLO. Add structured logs, request IDs, and basic metrics. Write a one-page runbook for the most likely failure and a release checklist that includes rollback instructions.
