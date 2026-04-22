# Lesson 07.07: Human Handoff Design

**Parent syllabus:** [Syllabus 07: Data Privacy and Client Safety](../../syllabus-07-data-privacy.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Human handoff policy and approval queue specification

---

## Outcome

By the end of this lesson, you can design approval gates that pause an AI workflow before major or irreversible actions and present the reviewer with only the information needed to make a safe decision.

You will produce:

- a human handoff policy
- an approval queue specification
- reviewer actions and audit fields

---

## Why This Matters

High-risk automation usually fails at the last mile.

Common failures:

- The model drafts a parent-facing message and the system sends it without review.
- Reviewers see too much text, too little context, or too many options, so they approve carelessly.
- The workflow knows when it is uncertain, but has no mechanism to stop and ask for help.
- Approval actions are not logged, so disputed decisions cannot be reconstructed.
- Handoff exists in theory, but latency, queue ownership, and timeout behavior are undefined.
- The interface works on a good day but collapses under fatigue or brain fog.

If the reviewer cannot approve safely and quickly, the handoff design is weak.

---

## Best-Practice Principles

1. **Put handoff at the risk boundary.**  
   Require approval where the system could disclose sensitive data, take irreversible action, or create major client impact.

2. **Show only the minimum necessary review context.**  
   The reviewer needs the key facts, not the full data exhaust.

3. **Offer clear reviewer choices.**  
   Approve, reject, request changes, or escalate should be explicit and easy to log.

4. **Design for low-energy review.**  
   Short summaries, highlighted risks, clear defaults, and minimal reading reduce rubber-stamping.

5. **Log the handoff event.**  
   Queue entry, reviewer identity, decision, reason code, and timestamp should be reconstructable.

6. **Define timeout and fallback behavior.**  
   If no one reviews the item, the system must not silently proceed.

7. **Use handoff data to improve the workflow.**  
   Rejection reasons and escalations are evaluation and design inputs, not just operational noise.

---

## Concepts

### Human Handoff

A pause in the workflow where the system stops and asks a person to decide or confirm the next action.

### Irreversible Action

An action that cannot be safely undone or that creates real-world impact.

Examples:

- sending a parent-facing message
- exporting records
- changing billing status
- updating a regulated system of record

### Approval Queue

A place where items wait for review with the information needed to decide.

### Reason Code

A short, consistent label explaining why the reviewer approved, rejected, or escalated.

Examples:

- policy mismatch
- missing context
- sensitive disclosure risk
- low confidence

### Audit Event

A structured record of the handoff and decision.

---

## Human Handoff Policy Template

Use this structure:

```markdown
# Human Handoff Policy: [Workflow Name]

## Handoff Triggers

| Triggering event | Why human review is required | Reviewer role | Allowed actions |
| --- | --- | --- | --- |
|  |  |  |  |

## Approval Queue Item

- Item summary:
- Key facts shown:
- Risks shown:
- Source links or IDs:
- Recommended action:

## Reviewer Actions

- Approve
- Reject
- Request revision
- Escalate

## Timeout and Fallback

What happens if no reviewer acts?

## Audit Fields

- Trace ID:
- Reviewer:
- Decision:
- Reason code:
- Timestamp:

## Metrics

- Approval rate:
- Rejection rate:
- Escalation rate:
- Queue wait time:
```

---

## Walkthrough

Use the pediatric therapy clinic workflow.

The system can produce:

- staff-facing intake summary
- parent follow-up email draft
- scheduling recommendation

### Step 1 - Decide where the AI must stop

Good handoff triggers:

- any parent-facing message
- any record export
- any case with missing or conflicting context
- any case involving diagnoses in outward-facing content

No handoff needed:

- low-risk internal note formatting if the output is not leaving the approved staff workflow and the risk is low

The review question is:

> What is the first point where a wrong output could matter to a parent, patient, record, or external system?

### Step 2 - Design the approval card

Weak approval card:

- full raw intake note
- full draft
- dozens of metadata fields
- no highlighted risk

Better approval card:

- one-sentence case summary
- short draft preview
- highlighted risk flags
- source record ID
- clear choices: approve, reject, revise, escalate

This design protects low-energy review.

### Step 3 - Define the allowed actions

Example:

| Triggering event | Why human review is required | Reviewer role | Allowed actions |
| --- | --- | --- | --- |
| Parent-facing email draft | external communication and possible sensitive disclosure | Clinic staff reviewer | Approve, reject, revise, escalate |
| Record export request | potential disclosure of child and health data | Operations owner | Approve, reject, escalate |
| Missing policy or consent context | uncertain safety boundary | Clinic manager | Reject or escalate only |

### Step 4 - Log the audit event

Example fields:

```text
trace_id=trace_h74
queue_item_id=review_221
reviewer_role=clinic_staff
decision=reject
reason_code=sensitive_disclosure_risk
timestamp=2026-04-21T15:08:00Z
```

This helps with observability, incident review, and future evals.

### Step 5 - Define timeout behavior

Weak timeout rule:

> If nobody responds, send it anyway.

Better timeout rule:

> If no reviewer acts within the target window, keep the item pending, notify the owner, and do not release external content automatically.

### Step 6 - Feed the handoff data back into improvement

If rejection reasons repeatedly say:

- missing context
- too much detail
- incorrect disclosure risk

then those are not only reviewer problems. They are input to:

- prompt updates
- context strategy changes
- exclusion rules
- eval cases

---

## Practice

Choose one workflow with at least three decisions that could create external or irreversible impact.

Create a human handoff policy and approval queue specification with:

1. at least five handoff triggers
2. reviewer role for each
3. allowed reviewer actions
4. the approval card contents
5. timeout and fallback behavior
6. audit fields
7. queue metrics
8. rejection or escalation reason codes

At least one trigger must involve:

- external communication
- sensitive data disclosure risk
- missing or conflicting context

---

## Review Checklist

Your handoff policy is acceptable when:

- Handoff triggers sit at real risk boundaries.
- The approval card shows the minimum necessary context.
- Reviewer actions are explicit.
- Timeout behavior does not silently proceed.
- Audit fields are defined.
- Queue metrics are defined.
- Low-energy review is considered directly.
- Rejection reasons can be reused as improvement data.

---

## Common Failure Modes

- **Review theater:** Human approval exists, but the card is too noisy to use well.
- **Silent proceed:** Timeout or queue backlog causes the system to continue anyway.
- **No audit trail:** There is no record of who approved what and why.
- **Wrong reviewer:** The person receiving the handoff lacks authority or context.
- **No feedback loop:** Rejections are not turned into system improvements.
- **One-size review:** Every queue item uses the same card regardless of risk.

---

## Portfolio Evidence

Save:

- The human handoff policy
- The approval queue specification
- One short note explaining how the design supports low-energy review

This shows that you can stop the workflow at the right moment, not after damage is done.

---

## References

- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for Large Language Model Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
