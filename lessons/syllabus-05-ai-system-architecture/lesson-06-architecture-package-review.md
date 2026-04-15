# Lesson 05.06: Architecture Package Review

**Parent syllabus:** [Syllabus 05: AI System Architecture](../../syllabus-05-ai-system-architecture.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Complete AI system architecture package

---

## Outcome

By the end of this lesson, you can assemble the core architecture artifacts for one AI system into a reviewer-ready package.

You will produce a complete AI system architecture package that includes:

- One-page architecture brief
- Diagram pack
- Three ADRs
- Capacity, latency, and cost worksheet
- Pre-build architecture review decision
- Open questions and required follow-up

---

## Why This Matters

Architecture artifacts are only useful when they work together.

An architecture brief without diagrams is hard to review. Diagrams without ADRs do not explain why decisions were made. ADRs without cost, latency, reliability, and risk context are incomplete. A review packet without a decision does not move the project forward.

Common failures:

- Artifacts contradict each other.
- Diagrams show components that the brief never mentions.
- ADRs describe decisions that do not appear in the architecture.
- Cost and latency estimates do not match the proposed runtime path.
- Risks are listed but not connected to gates or follow-up work.
- The package is too large for a client, reviewer, or future maintainer to use.

The architecture package is the bridge between "I have a system idea" and "this system is ready to build, simplify, prototype, or reject."

---

## Best-Practice Principles

1. **Make every artifact answer a review question.**  
   If an artifact does not support a decision, review, handoff, or risk discussion, cut or simplify it.

2. **Keep the package coherent.**  
   The same system name, scope, assumptions, non-goals, data boundaries, and human handoffs should appear across artifacts.

3. **Trace decisions to evidence.**  
   ADRs should connect to diagrams, capacity estimates, risk reviews, eval needs, or client constraints.

4. **Use a short executive summary.**  
   A busy stakeholder should understand the system, risk posture, and decision in one page.

5. **Separate facts, assumptions, and open questions.**  
   Do not let uncertainty hide inside confident architecture language.

6. **End with a build decision.**  
   The package should recommend build, simplify, prototype, defer, or reject.

7. **Keep a sanitized version.**  
   Architecture packages become portfolio evidence when sensitive data, client names, credentials, and proprietary details are removed.

---

## Concepts

### Architecture Package

A small bundle of documents that explains what the system is, how it works, why major decisions were made, what risks exist, and what should happen next.

### Traceability

The ability to follow a decision back to the context that justified it.

Example:

> ADR: Use background jobs  
> Evidence: latency budget shows model path can take 20 seconds  
> Diagram: sequence diagram shows webhook returns 202 and worker processes job  
> Review gate: queue depth and failed jobs must be observable

### Executive Summary

A concise explanation for a stakeholder who will not read every artifact.

It should include:

- Problem
- Proposed system
- Main decision
- Main risks
- Recommended next step

### Package Consistency

The discipline of checking that artifacts do not disagree.

Examples:

- If the diagram shows raw document storage, the privacy review must address retention.
- If the ADR says human approval is required, the sequence diagram should show the approval step.
- If the cost budget assumes background jobs, the container diagram should show a queue or worker.

---

## Walkthrough

Use this example:

> A support response drafting and review system processes support tickets, generates draft replies, checks policy, and posts drafts to Slack for human review.

### Step 1 - Assemble the artifacts

Required package:

- Architecture brief
- Context diagram
- Container diagram
- Sequence diagram with happy path and failure path
- Data-flow diagram with trust boundaries
- ADR for human approval before customer-facing replies
- ADR for background jobs
- ADR for storing metadata instead of raw ticket text
- Capacity, latency, and cost worksheet
- Pre-build architecture review decision

### Step 2 - Check consistency

| Claim | Where it should appear |
| --- | --- |
| Human approval required | Brief, sequence diagram, ADR, review packet |
| Raw ticket text not retained | Brief, data-flow diagram, ADR, privacy review |
| Background jobs used | Container diagram, sequence diagram, ADR, latency budget |
| Draft delayed status exists | Sequence failure path, reliability review, observability review |
| Cost hard stop exists | Brief, cost worksheet, review packet |

### Step 3 - Write the executive summary

```markdown
# Architecture Package Summary: Support Response Drafting and Review System

## Problem

Support agents spend too much time drafting first replies, but customer-facing messages require policy accuracy and human accountability.

## Proposed System

The system accepts ticket events, enqueues a draft job, uses an AI model to draft a response with policy checks, and posts the draft to Slack for human review. It does not send customer-facing replies directly.

## Main Decision

Build a background-job-based drafting workflow with mandatory human approval before any customer-facing action.

## Main Risks

- Prompt injection in ticket content
- Incorrect policy claims
- Slack or model-provider outages
- Sensitive customer data retention
- Reviewer rubber-stamping
- Token cost spikes on long tickets

## Recommendation

Build a limited phase-one system for one ticket category, with evals, observability, cost limits, and a runbook required before production rollout.
```

### Step 4 - Decide what happens next

Weak ending:

> Looks good.

Better ending:

> Build a smaller version for one ticket category. Required before launch: 30-ticket eval set, queue-depth alert, failed-job alert, approval audit log, token budget hard stop, and operator runbook.

---

## Package Template

```markdown
# AI System Architecture Package: [System Name]

## 1. Executive Summary

- Problem:
- Proposed system:
- Main decision:
- Main risks:
- Recommended next step:

## 2. Architecture Brief

Link or paste the one-page brief.

## 3. Diagram Pack

- Context diagram:
- Container diagram:
- Sequence diagram:
- Failure-path diagram:
- Data-flow diagram:

## 4. ADRs

- ADR 001:
- ADR 002:
- ADR 003:

## 5. Capacity, Latency, and Cost Worksheet

- Expected volume:
- Target latency:
- Monthly cost range:
- Bottlenecks:
- Hard limits:

## 6. Risk and Review Summary

- Reliability:
- Security:
- Privacy:
- Observability:
- Evaluation:
- Human handoff:

## 7. Assumptions

- ...

## 8. Open Questions

- ...

## 9. Required Follow-Up

- ...

## 10. Decision

Build, build smaller, prototype first, defer, or reject.

## 11. Sanitized Portfolio Version

What must be removed or anonymized before sharing publicly?
```

---

## Practice

Choose one AI workflow or system idea.

Assemble an architecture package with:

1. One-page architecture brief
2. Four or five diagrams
3. Three ADRs
4. Capacity, latency, and cost worksheet
5. Architecture review decision
6. Executive summary
7. Open questions
8. Required follow-up
9. Sanitization notes for portfolio use

Then do a consistency pass:

- Does every ADR appear in at least one diagram or review section?
- Does every data store have a retention or deletion story?
- Does every high-impact action have a human approval rule?
- Does every slow path have a latency or background processing decision?
- Does every production risk have a gate, mitigation, or follow-up artifact?

---

## Review Checklist

Your architecture package is acceptable when:

- It has an executive summary.
- The brief, diagrams, ADRs, budget, and review decision describe the same system.
- Major decisions are traceable to constraints or evidence.
- Data movement and trust boundaries are visible.
- Human approval points are explicit.
- Cost, latency, and capacity estimates are included.
- Observability and evaluation needs are named.
- Open questions are not hidden.
- The final decision is clear.
- A sanitized portfolio version can be created without leaking sensitive details.

---

## Common Failure Modes

- **Artifact pile:** Documents exist, but they do not connect to each other.
- **Contradictory scope:** One artifact implies production deployment while another says prototype.
- **Invisible assumptions:** The package sounds certain even though key facts are unknown.
- **Decisionless package:** The artifacts are thorough but do not recommend what to do next.
- **No review path:** The package cannot be used by security, operations, or client stakeholders.
- **Portfolio leak:** The public version includes client data, internal architecture, credentials, or proprietary details.

---

## Portfolio Evidence

Save:

- Complete architecture package
- Executive summary
- Final build/simplify/prototype/defer/reject decision
- Sanitized public version

This becomes one of the strongest artifacts in the curriculum because it shows full-system thinking before implementation.

---

## References

- C4 model official site: https://c4model.com/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- OpenTelemetry concepts: https://opentelemetry.io/docs/concepts/
