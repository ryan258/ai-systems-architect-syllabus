# Lesson 14.04: Incidents, Runbooks, and Postmortems

**Parent syllabus:** [Syllabus 14: Reliability and Observability](../../syllabus-14-reliability-and-observability.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Incident response runbook and postmortem template

---

## Outcome

By the end of this lesson, you can define what counts as an incident for an AI system, decide whether to roll back, disable, degrade, or hand off, and document both the first response and the learning path afterward.

You will produce:

- an incident response runbook
- an incident classification matrix
- a postmortem template

---

## Why This Matters

Production systems do not fail politely just because the team has dashboards.

Common failures:

- The team notices a problem but wastes time deciding whether it is really an incident.
- Operators have telemetry but no rule for when to roll back, disable ingress, or accept degraded behavior.
- Client-facing communication is delayed because nobody owns the message.
- The runbook is too long to use under stress.
- Human handoff or review queues are ignored during incidents even when they are part of the failure.
- The postmortem becomes a blame document or a vague summary with no system changes.

Incidents are where reliability turns from design into operator behavior. If the response path is weak, the instrumentation only shows failure faster.

---

## Best-Practice Principles

1. **Define incident classes before you need them.**  
   Severity and response rules should exist before the next outage, not during it.

2. **Make the first-response runbook short.**  
   Operators need a reliable first move more than a complete theory of failure.

3. **Separate rollback, disable, degrade, and handoff decisions.**  
   Each response solves a different class of problem.

4. **Treat client communication as part of the incident path.**  
   Silence is not a neutral operating posture when service risk is visible to others.

5. **Preserve evidence early.**  
   Trace IDs, timestamps, recent revisions, and trigger conditions should be captured before the system changes again.

6. **Write blameless postmortems with concrete follow-up work.**  
   The point is to improve controls, not to narrate regret.

7. **Keep the runbook usable on a bad day.**  
   The operator should know what to check first, second, and third without interpretation debt.

---

## Concepts

### Incident

A reliability event that exceeds the service's accepted operating boundary and requires coordinated response.

### Severity Level

A classification that determines urgency, communication needs, and escalation path.

### Rollback

Returning to a previous known-good revision or configuration.

### Disable

Stopping new work or blocking ingress to prevent further harm.

### Degraded Mode

Operating with reduced functionality to preserve the most important service behavior.

Examples:

- draft-only path with manual review
- no expensive fallback model route
- queue-only intake while processing is paused

### Postmortem

A structured review explaining what happened, why it mattered, how it was mitigated, and what changes are required next.

---

## Incident Response Runbook Template

Use this structure:

```markdown
# Incident Response Runbook: [Workflow Name]

## Incident Classification

| Severity | Example conditions | Response owner | Client communication? |
| --- | --- | --- | --- |
|  |  |  |  |

## First Checks

1. Confirm current user impact.
2. Identify recent deploy or config changes.
3. Pull recent trace IDs and alert context.

## Response Options

- Roll back when:
- Disable ingress when:
- Enter degraded mode when:
- Force human handoff when:

## Evidence to Capture

- Timestamps:
- Triggering alerts:
- Revision or config version:
- Key trace IDs:

## Communication

- Internal escalation:
- Client or stakeholder note:
- Update cadence:
```

Postmortem template:

```markdown
# Postmortem: [Incident Name]

## Summary

- What happened:
- User impact:
- Start and end time:

## Timeline

- ...

## Root Causes and Contributing Factors

- ...

## What Worked

- ...

## What Failed

- ...

## Follow-Up Actions

| Action | Owner | Due date |
| --- | --- | --- |
|  |  |  |
```

Review questions:

- Which conditions count as a severity jump?
- When should the operator disable instead of keep debugging live?
- What evidence must be captured before the system changes again?

---

## Walkthrough

Use this example:

> A support drafting service begins producing slow responses and reviewer rejection spikes after a model-route change.

### Step 1 - Classify the incident

Not every alert is an incident.

For this service, an incident might be:

- sustained request failures
- queue age above the accepted threshold
- reviewer rejection spike with clear customer impact
- unsafe or duplicate side effects

The severity should tell the operator whether to page, escalate, or start client communication.

### Step 2 - Capture evidence first

Before changing too much, capture:

- alert name and start time
- recent revision or config changes
- representative trace IDs
- affected routes or dependencies

If those details are lost during rollback, the postmortem becomes weaker immediately.

### Step 3 - Choose the response shape

For this workflow:

- roll back if the recent model-route change caused the failure
- disable ingress if unsafe outputs or duplicate side effects are occurring
- enter degraded mode if a smaller safe path can preserve service
- force manual handoff if draft quality is not trustworthy

The important point is that each action has a different purpose.

### Step 4 - Keep communication explicit

Internal note:

- what happened
- who owns the response
- what the next update time is

Client note if needed:

- what service behavior is affected
- what mitigation is in place
- when the next update will arrive

Client communication should be concise and factual, not theatrical.

### Step 5 - Write the postmortem for system change

Weak postmortem:

> The system had some issues after a deploy. We will be more careful.

Better postmortem:

- incident summary
- concrete timeline
- technical and operational causes
- what detection and mitigation worked
- what controls or review steps must change

The postmortem should drive action, not memory.

### Step 6 - Feed the follow-up back into reliability work

Possible follow-up actions:

- tighten the release gate
- add a missing alert or trace field
- change degraded-mode rules
- strengthen human-handoff thresholds

Postmortems matter only if they change the system.

---

## Practice

Choose one realistic failure mode for a live or planned AI system.

Create:

1. an incident response runbook
2. an incident classification matrix
3. a postmortem template
4. a short client or stakeholder update note

At least one response rule must cover:

- rollback
- disable ingress or stop new work
- degraded mode or forced human handoff

---

## Review Checklist

Your incident artifact is acceptable when:

- Incident classes are explicit.
- The first-response runbook is short and actionable.
- Rollback, disable, degrade, and handoff rules are separated.
- Evidence capture happens early.
- Communication rules are written down.
- The postmortem template is blameless and action-oriented.
- Follow-up work can be assigned clearly.
- The design is usable under stress.

---

## Common Failure Modes

- **Severity confusion:** Teams argue about whether the problem is "real" while impact continues.
- **Runbook overload:** The response guide is too long to use.
- **One-response mindset:** Everything becomes a rollback or everything becomes debugging.
- **Lost evidence:** Key trace IDs and change context disappear before review.
- **Silent stakeholders:** Clients or owners receive no factual update.
- **Narrative postmortem:** The write-up describes the incident but changes nothing.

---

## Portfolio Evidence

Save:

- the incident response runbook
- the incident classification matrix
- one redacted postmortem template or sample

This shows that you can respond to production failure as an operator, not only detect it as an observer.

---

## References

- Google SRE Book, Postmortem Culture: https://sre.google/sre-book/postmortem-culture/
- Google SRE Workbook: https://sre.google/workbook/table-of-contents/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
