# Lesson 05.01: What an AI Systems Architect Actually Designs

**Parent syllabus:** [Syllabus 05: AI System Architecture](../../syllabus-05-ai-system-architecture.md)  
**Estimated time:** 90-120 minutes  
**Artifact:** One-page AI system architecture brief

---

## Outcome

By the end of this lesson, you can look at an AI workflow idea and identify the actual system around it: users, operators, model calls, data sources, tools, permissions, risks, costs, failure modes, and handoff points.

You will produce a one-page architecture brief that explains the system before any implementation work begins.

---

## Why This Matters

A workflow can succeed in a notebook and still fail as a system.

Common failures:

- The model works, but the data source is stale.
- The prompt works, but the workflow has no owner when it breaks.
- The API works, but it logs sensitive client data.
- The agent works, but a tool call can take an irreversible action without approval.
- The prototype works, but nobody knows what "working" means after launch.
- The system works once, but cannot be restarted, tested, observed, or handed off.

An AI systems architect designs the whole operating environment, not just the prompt or agent.

---

## Best-Practice Principles

1. **Start with the system boundary.**  
   Define what the system owns, what it depends on, and what it must not touch.

2. **Treat the model as one component, not the architecture.**  
   The model is usually surrounded by data retrieval, validation, orchestration, storage, logging, security, user experience, and human review.

3. **Name the humans in the system.**  
   Every AI system has users, operators, approvers, owners, and people affected by mistakes.

4. **Design for failure before success.**  
   Ask how the system fails when the model is wrong, the tool is down, the input is malicious, the cost spikes, or the client asks for evidence.

5. **Make risk visible early.**  
   Use the NIST AI RMF idea of mapping, measuring, and managing risk as part of design, not after deployment.

6. **Assume inputs can be adversarial.**  
   OWASP's LLM guidance treats prompt injection and excessive agency as core application risks. Do not design systems where the model can blindly obey user-supplied content.

7. **Define observable success.**  
   Reliability work starts by deciding what users need from the service. If you cannot define success, you cannot monitor degradation.

---

## Concepts

### Workflow

A sequence of steps that accomplishes a task.

Example: "Summarize a client document, extract risks, and draft an email."

### System

The workflow plus everything required to run it safely and repeatedly.

Example: API, auth, file upload, document storage, model call, extraction logic, human review, logs, cost limits, deletion policy, and support process.

### Boundary

The line between what the system controls and what it depends on.

Example: Your system may control the API and workflow logic, but depend on Claude, Google Drive, Slack, a vector database, and a client's approval process.

### Actor

Anyone or anything that interacts with the system.

Examples: end user, client admin, operator, model provider, webhook source, attacker, reviewer, compliance stakeholder.

### Failure Mode

A way the system can fail.

Examples: wrong answer, late answer, leaked data, runaway cost, duplicate action, blocked dependency, unsafe tool call, unreviewed recommendation.

### Architectural Decision

A decision that changes system behavior, risk, cost, or maintainability.

Examples: REST vs. streaming, RAG vs. fine-tuning, synchronous request vs. background job, local model vs. cloud model, direct tool call vs. human approval.

---

## Walkthrough

Use this example:

> A client wants an AI workflow that reads support tickets, drafts responses, and posts them into Slack for review.

### Step 1 - Name the real system

Weak name:

> Support ticket AI.

Better name:

> Support Response Drafting and Review System.

The better name reveals that the system drafts responses and requires review. It does not imply autonomous customer support.

### Step 2 - Identify actors

- Customer support agent
- Support manager
- Client admin
- Slack workspace
- Ticketing system
- AI model provider
- Operator who handles incidents
- Customer whose data appears in tickets
- Security reviewer

### Step 3 - Draw the boundary in words

The system owns:

- Ticket ingestion
- Prompt and response drafting logic
- Safety checks
- Slack review message
- Logs and metrics
- Cost limits
- Runbook

The system depends on:

- Ticketing API
- Slack API
- AI model API
- Client permissions
- Human reviewer availability

The system must not:

- Send replies directly to customers without approval
- Store unnecessary customer data in logs
- Use ticket content for model training unless explicitly approved
- Continue posting drafts if policy checks fail

### Step 4 - Identify the model's role

The model should:

- Summarize the ticket
- Draft a response
- Flag uncertainty
- Suggest escalation when confidence is low

The model should not:

- Decide refunds
- Promise policy exceptions
- Send the response
- Override the human reviewer

### Step 5 - Identify failure modes

- Ticket content contains prompt injection.
- Model invents a policy.
- Slack is down.
- Ticketing API rate-limits requests.
- The same ticket is processed twice.
- Sensitive customer data appears in logs.
- The human reviewer approves too quickly without reading.
- Model costs spike because tickets are too long.

### Step 6 - Name first architectural decisions

- Use human approval before customer-facing action.
- Use a background job if ticket processing takes more than a few seconds.
- Store ticket IDs and metadata, not full raw ticket text, unless required.
- Add a policy-checking step before Slack review.
- Use idempotency keys so the same ticket is not processed twice.
- Log trace IDs, timestamps, status, model, token count, and failure reason.

---

## Practice

Pick one AI workflow idea from your own work.

Create a one-page architecture brief with these sections:

1. **System name**
2. **Business or workflow problem**
3. **Primary users**
4. **Operators and approvers**
5. **Data sources**
6. **Model responsibilities**
7. **Things the model must not do**
8. **External dependencies**
9. **Failure modes**
10. **Human handoff points**
11. **Cost controls**
12. **Privacy constraints**
13. **First three architecture decisions**

Keep it to one page. If it cannot fit, the system is probably not scoped tightly enough yet.

---

## Review Checklist

Your architecture brief is acceptable when:

- The system name is specific enough to reveal its responsibility.
- The model is listed as a component, not the whole system.
- At least three human actors are named.
- At least five failure modes are listed.
- At least one privacy constraint is listed.
- At least one cost control is listed.
- At least one human handoff point is listed.
- At least three architecture decisions are written as trade-offs.
- The brief makes clear what the system will not do.

---

## Common Failure Modes

- **Model-centered design:** The brief talks only about prompts and ignores data, users, tools, and operations.
- **No owner:** Nobody is named as responsible when the system fails.
- **No boundary:** The system is allowed to touch too many tools or data sources.
- **No handoff:** The AI can take high-impact actions without review.
- **No observable success:** There is no way to tell whether the system is working after launch.
- **No cost brake:** The design has no token, request, or monthly spending limit.
- **No deletion story:** The design does not say how client data is removed.

---

## Portfolio Evidence

Save:

- The one-page architecture brief
- A short paragraph explaining the most important design trade-off
- A sanitized version that can be shown publicly

This becomes the seed for a future case study.

---

## References

- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- OpenTelemetry concepts: https://opentelemetry.io/docs/concepts/
