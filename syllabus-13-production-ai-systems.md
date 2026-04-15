# Syllabus 13: Production AI Systems
**Status:** Hidden Gap — What Separates Builders from Architects
**Format:** One chat session per unit, self-paced

---

## What This Is

Building an AI workflow is one skill. Running one safely in the real world is another. This syllabus covers the four things that protect you, your clients, and your wallet once a system is live.

---

## Unit 1 — Cost Architecture and Hard Limits

**Goal:** Design AI systems so a stuck loop or bad input cannot spend hundreds of dollars without warning.

**Topics:**
- How token costs accumulate in agentic loops
- Setting hard token limits per run and per session
- Automatic shutoff triggers for repeated errors or unexpected loop counts
- Budget alerts before costs become a problem
- Treating cost as a first-class design constraint, not an afterthought

**Session starter:**
> "Help me design cost guardrails for an agentic AI workflow. I want hard token limits per run, automatic shutoffs for error loops, and budget alerting. Treat cost as a core design rule. Show me the patterns and the Python implementation."

---

## Unit 2 — Live Monitoring and Observability

**Goal:** Know when your deployed AI system is failing quietly — before your client notices.

**Topics:**
- What observability means for AI systems (watching real behavior, not just test behavior)
- Tracking response latency over time
- Detecting output quality drift (when answers get quietly worse)
- Logging every API request with enough detail to reconstruct what happened
- Alerting thresholds that notify you without overwhelming you

**Session starter:**
> "Help me build a lightweight observability layer for a deployed AI workflow. I want latency tracking, output quality drift detection, and structured API request logging. What is the minimum I need to know when something is failing quietly?"

---

## Unit 3 — Active AI Security and Prompt Injection Defense

**Goal:** Build AI systems that treat every user input as a potential attack.

**Topics:**
- What prompt injection is — when a bad actor hides instructions inside user input to hijack the AI
- How to separate system instructions from user input safely
- Input filtering and validation before it reaches the model
- Output filtering to catch cases where the model was successfully manipulated
- Designing with the assumption that inputs will be adversarial

**Session starter:**
> "Teach me how to defend against prompt injection in AI workflows. How do I safely separate system instructions from user input? What input and output filters should I build? I want to design every workflow assuming inputs could be adversarial."

---

## Unit 4 — Human Handoff Systems

**Goal:** Design AI workflows that know when to stop and ask a human for help.

**Topics:**
- Identifying the decision points where an AI should never proceed alone
- Building a pause-and-request-approval mechanism
- How to design handoff prompts that are easy to review on a low-energy day
- Logging the handoff reason so you can improve the system over time
- Requiring human approval before any irreversible action

**Session starter:**
> "Help me design a human handoff system for an AI workflow. I need the AI to pause and request approval before major or irreversible actions. The review interface must work on my worst brain fog days — minimal reading, clear choices. Show me the design pattern and a simple implementation."

---

## Unit 5 — Putting It All Together — A Production Checklist

**Goal:** Have a checklist you run through before any AI workflow goes live with a client.

**Topics:**
- Pre-launch: cost limits set, logging configured, security filters in place
- Launch: monitoring active, handoff points tested
- Post-launch: weekly review of logs and cost reports
- Incident response: what to do when something breaks in production

**Session starter:**
> "Help me build a production readiness checklist for AI workflow deployments. I want to cover cost controls, monitoring, security, and human handoffs. Make it a checklist I can run through quickly — designed for low-energy days."

---

## Practice Project

Take one workflow you have already built. Run it through the production checklist from Unit 5. Identify which of the four safety layers is missing. Add the missing layer first.
