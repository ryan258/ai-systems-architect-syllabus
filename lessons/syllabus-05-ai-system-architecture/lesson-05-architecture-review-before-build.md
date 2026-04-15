# Lesson 05.05: Architecture Review Before Build

**Parent syllabus:** [Syllabus 05: AI System Architecture](../../syllabus-05-ai-system-architecture.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Pre-build architecture review packet

---

## Outcome

By the end of this lesson, you can review an AI system design before implementation and decide what to build, simplify, defer, or reject.

You will produce a pre-build architecture review packet that includes:

- Problem statement
- Architecture option comparison
- Data-flow review
- Reliability review
- Observability review
- Security and privacy review
- Evaluation and release gate review
- Cost review
- Human handoff review
- Build/defer/reject decision

---

## Why This Matters

Most expensive architecture mistakes are visible before code is written.

Common failures:

- Building a multi-agent system when a deterministic workflow would work.
- Fine-tuning when RAG is the right tool.
- Storing client data because it is convenient, not necessary.
- Skipping human review for high-impact actions.
- Choosing synchronous APIs for long-running workflows.
- Shipping without an eval plan.
- Adding infrastructure that the operator cannot maintain.

A pre-build review is not bureaucracy. It is where you prevent avoidable complexity.

---

## Best-Practice Principles

1. **Review against risks, not aesthetics.**  
   The question is not "Does this architecture look sophisticated?" The question is "Will it work safely under real constraints?"

2. **Compare options explicitly.**  
   A design is weak if it cannot explain why it is better than at least one simpler alternative.

3. **Prefer the smallest reliable system.**  
   Do not use agents, queues, vector databases, streaming, or fine-tuning unless they solve a real requirement.

4. **Make non-goals explicit.**  
   State what the system will not do. This prevents accidental scope creep and unsafe automation.

5. **Check data movement early.**  
   Privacy, security, compliance, and logging decisions are cheaper before implementation.

6. **Require a failure story.**  
   Every design must explain what happens when the model is wrong, a dependency fails, a user submits malicious input, or costs spike.

7. **End with a decision.**  
   Architecture review should produce a clear next step: build, simplify, defer, prototype, or reject.

---

## Concepts

### Architecture Review

A structured review of whether a system design is fit for purpose before implementation.

It should answer:

- What problem are we solving?
- What are the constraints?
- What options did we consider?
- What are the major risks?
- What evidence do we need before production?

### Option Comparison

A short comparison of two or three viable approaches.

Example:

- Option A: simple deterministic workflow
- Option B: single-agent workflow
- Option C: multi-agent workflow

The comparison should include cost, latency, reliability, safety, maintainability, and user experience.

### Gate

A condition that must be satisfied before the system advances.

Examples:

- No production launch before eval pass rate is defined.
- No external tool writes without human approval.
- No client data ingestion before retention rules are approved.
- No deployment before rollback plan exists.

---

## Review Packet Template

Use this structure:

```markdown
# Architecture Review: [System Name]

## 1. Problem Statement

What problem does this system solve?

## 2. Non-Goals

What will this system not do?

## 3. Users, Operators, and Approvers

Who uses it, who runs it, and who approves high-impact actions?

## 4. Proposed Architecture

Short description plus links to diagrams.

## 5. Options Compared

| Option | Pros | Cons | Risks | Cost | Decision |
| --- | --- | --- | --- | --- | --- |
| A | | | | | |
| B | | | | | |
| C | | | | | |

## 6. Data Review

What data enters, where it goes, what is stored, what is logged, and how it is deleted.

## 7. Reliability Review

SLOs, failure modes, fallback paths, runbook needs.

## 8. Observability Review

Logs, metrics, traces, audit events, dashboards, and alerts needed to reconstruct failures.

## 9. Security and Privacy Review

Threats, permissions, prompt injection risks, sensitive data handling.

## 10. Evaluation and Release Gate Review

Eval set, pass/fail criteria, human review criteria, rollback triggers, and launch blockers.

## 11. Cost Review

Per-request estimate, monthly estimate, hard limits, alerts.

## 12. Human Handoff Review

Where the AI must stop and ask for approval.

## 13. Decision

Build, simplify, defer, prototype, or reject.

## 14. Required Follow-Up

ADRs, tests, evals, diagrams, runbooks, or client approvals needed.
```

---

## Walkthrough

Use this example:

> A client wants an AI system that reviews sales calls, scores lead quality, drafts CRM notes, and updates Salesforce.

### Step 1 - State the problem

Weak:

> Use AI to improve sales.

Better:

> Reduce manual CRM note writing after sales calls while preserving manager visibility and preventing unapproved changes to lead status.

### Step 2 - State non-goals

This system will not:

- Change deal stage automatically.
- Send customer-facing messages.
- Score employee performance.
- Store raw call recordings after processing unless explicitly approved.
- Replace manager review for high-value opportunities.

### Step 3 - Compare options

| Option | Pros | Cons | Risks | Decision |
| --- | --- | --- | --- | --- |
| Deterministic transcript extraction plus templates | Fast, cheap, predictable | Weak at summarizing nuance | May miss context | Good first version |
| Single agent drafts notes and lead score | Flexible, easy to build | Harder to evaluate | May overstate confidence | Possible after evals |
| Multi-agent reviewer team | Separation of roles | More latency and cost | Agents may amplify errors | Defer |

### Step 4 - Review data flow

Questions:

- Are call transcripts personal data?
- Are recordings stored or deleted?
- Does the model provider receive full transcripts?
- What appears in logs?
- What goes into Salesforce?
- Can the client delete a transcript and derived notes?

Decision pressure:

> Start with transcript excerpts and derived notes only. Do not store raw recordings unless the client explicitly requires it and approves retention controls.

### Step 5 - Review reliability

Questions:

- What happens if transcript parsing fails?
- What happens if Salesforce is down?
- What happens if the model output is malformed?
- How does a sales rep know a draft is waiting?
- What is the SLO for draft availability?

Decision pressure:

> Queue Salesforce updates and require human approval before write-back.

### Step 6 - Review security and privacy

Questions:

- Can transcript content contain prompt injection?
- Can the model write directly to Salesforce?
- Which Salesforce fields can be updated?
- Are tokens scoped to least privilege?
- What audit log proves who approved the update?

Decision pressure:

> The AI can draft CRM updates but cannot write them without human approval and a scoped integration token.

### Step 7 - Review cost

Questions:

- How many calls per month?
- How long are transcripts?
- Is summarization needed before final drafting?
- What is max cost per call?
- What happens if the monthly budget is near limit?

Decision pressure:

> Put a per-call token budget and require summarization for long transcripts.

### Step 8 - End with a decision

Decision:

> Build a deterministic extraction plus AI drafting system with human approval before Salesforce write-back. Defer multi-agent review until the first eval set shows which failure modes need specialist reviewers.

Required follow-up:

- ADR for human approval before Salesforce writes
- Data-flow diagram
- Eval set with 25 real or realistic sales calls
- Observability plan for trace IDs, failed jobs, approval events, and Salesforce write errors
- Cost budget
- Runbook for failed Salesforce writes

---

## Practice

Choose one AI system idea.

Create a pre-build architecture review packet:

1. Problem statement
2. Non-goals
3. Users, operators, and approvers
4. Proposed architecture summary
5. Two or three architecture options compared
6. Data-flow review
7. Reliability review
8. Observability review
9. Security and privacy review
10. Evaluation and release gate review
11. Cost review
12. Human handoff review
13. Final decision
14. Required follow-up artifacts

End with one of these decisions:

- Build
- Build a smaller version
- Prototype first
- Defer
- Reject

---

## Review Checklist

Your architecture review packet is acceptable when:

- The problem statement is specific.
- Non-goals are explicit.
- At least two options are compared.
- The simplest viable option is considered.
- Data movement and storage are reviewed.
- Prompt injection or adversarial input is considered.
- Logs, metrics, traces, audit events, or alerts are named.
- An eval or release gate is named.
- Cost limits are named.
- Human approval points are named.
- At least five failure modes are reviewed.
- The final decision is clear.
- Follow-up artifacts are listed.

---

## Common Failure Modes

- **Rubber-stamp review:** The review only confirms the favorite design.
- **Complexity bias:** The most sophisticated architecture wins without proving it is needed.
- **No non-goals:** The system quietly expands into unsafe automation.
- **No data review:** Privacy problems are discovered after implementation.
- **No eval gate:** The design has no way to prove behavior before launch.
- **No observability plan:** The operator cannot reconstruct what happened after a bad output, failed job, or disputed approval.
- **No operator story:** Nobody knows how the system will be monitored or repaired.
- **No decision:** The review produces discussion but not direction.

---

## Portfolio Evidence

Save:

- The pre-build architecture review packet
- The option comparison table
- The final build/simplify/defer/reject decision
- One sanitized version that can be shown publicly

This artifact demonstrates senior judgment: knowing what not to build is part of architecture.

---

## References

- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- OpenTelemetry concepts: https://opentelemetry.io/docs/concepts/
- C4 model diagram review checklist: https://c4model.com/diagrams/checklist
