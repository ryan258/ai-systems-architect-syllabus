# Syllabus 12: Production AI Systems
**Status:** Production Spine - Operating Controls Before Broader Deployment
**Format:** One chat session per unit, self-paced

---

## What This Is

After API and data-engineering work, the next question is not "can this workflow run?" but "can this workflow be trusted in front of a client?" This syllabus is the first production-operations layer in the course path.

It focuses on the controls that should exist before you worry about broader cloud topology, formal reliability programs, or distributed runtime complexity. The system can still be relatively simple. The operating discipline cannot.

---

## Unit 1 - Cost Architecture and Hard Limits

**Goal:** Put hard financial and operational boundaries around one live AI workflow.

**Topics:**
- How token, model, retrieval, and tool-call costs accumulate in production
- Hard limits per request, per run, per tenant, and per time window
- Automatic shutoff triggers for loops, repeated errors, and unexpected routing
- Budget alerts and spend visibility before the bill becomes the incident
- Treating cost as a production control, not just a finance concern

**Session starter:**
> "Help me design cost guardrails for a production AI workflow. I want hard limits per request and per run, shutoff logic for bad loops, budget alerts, and a policy I could actually operate for a client system."

---

## Unit 2 - Live Monitoring and Quiet Failure Detection

**Goal:** Detect the failures that do not look like obvious crashes.

**Topics:**
- What quiet failure looks like in AI systems: plausible but wrong, slow, expensive, or silently degraded behavior
- Minimum live monitoring for latency, failures, quality drift, queue health, and spend
- Structured logging that supports incident reconstruction without leaking sensitive data
- Thresholds and review signals for early degradation detection
- Designing monitoring that supports the next modules instead of being replaced by them

**Session starter:**
> "Help me design live monitoring and quiet failure detection for an AI workflow. I want the minimum signals that tell me when outputs are drifting, latency is rising, or spend is getting weird before a client reports it."

---

## Unit 3 - Trust Boundaries and Prompt Injection Defense

**Goal:** Treat user input, retrieved content, and tool-facing context as different trust levels.

**Topics:**
- Why prompt injection is a trust-boundary problem, not just a prompt-writing problem
- Separating system instructions, user input, retrieved documents, and tool outputs
- Filtering, validation, and policy checks before the model or tools act on input
- Output restrictions for unsafe tool calls, unsafe claims, or hidden instruction carryover
- Designing for adversarial inputs without pretending the model can self-police perfectly

**Session starter:**
> "Teach me trust boundaries and prompt injection defense for AI systems. I want to separate system instructions from user and retrieved content, control tool access, and stop unsafe behavior before it becomes a production incident."

---

## Unit 4 - Human Handoff Systems and Approval Queues

**Goal:** Design the points where the workflow must pause and ask for human judgment.

**Topics:**
- Identifying decisions the system should not complete alone
- Building approval queues and pause states instead of informal side-channel review
- Designing low-cognitive-load handoff packets for operators
- Logging why the system escalated and what happened next
- Using approval paths as governance, safety, and maintainability controls

**Session starter:**
> "Help me design human handoff systems and approval queues for an AI workflow. I need clear pause rules, low-energy review packets, and a durable record of why the workflow stopped and who approved the next step."

---

## Unit 5 - Production Readiness Checklist

**Goal:** Turn the control decisions into a usable pre-launch review.

**Topics:**
- Pre-launch review of cost limits, monitoring, trust boundaries, and handoff rules
- Launch-day checks for alerts, disable paths, and operator visibility
- Weekly review habits for spend, failures, and escalation reasons
- How to keep the checklist short enough to use during real release pressure
- What should block launch versus what should be tracked as follow-up work

**Session starter:**
> "Help me build a production readiness checklist for an AI workflow. I want a short review that covers cost controls, quiet failure detection, prompt-injection defenses, human handoff, and launch blockers without becoming ceremony."

---

## Unit 6 - Production Operations Review Package

**Goal:** Combine the control artifacts into one reviewer-ready operating package.

**Topics:**
- Assembling cost policy, failure-detection plan, trust boundaries, handoff design, and checklist into one packet
- Checking internal consistency between controls and operating assumptions
- Writing launch blockers, owners, and a final readiness decision
- Naming the future changes that should force re-review

**Session starter:**
> "Help me assemble a production operations review package for one AI workflow. I want the cost controls, monitoring plan, trust-boundary rules, approval design, readiness checklist, and a final launch recommendation in one coherent packet."

---

## Practice Project

Take one AI API, retrieval workflow, or internal automation and turn it into a production operations package that includes:

- a cost architecture and hard-limits policy
- a quiet failure detection and monitoring plan
- a trust-boundary and prompt-injection defense note
- a human handoff and approval-queue design
- a short production readiness checklist
- a final launch recommendation with blockers and next steps
