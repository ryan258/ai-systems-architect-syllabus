# Lesson 12.04: Human Handoff Systems and Approval Queues

**Parent syllabus:** [Syllabus 12: Production AI Systems](../../syllabus-12-production-ai-systems.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Approval queue operating policy and escalation matrix

---

## Outcome

By the end of this lesson, you can run human review as an operational control instead of a vague promise by defining triggers, reviewer roles, response targets, timeout behavior, and audit fields for a production approval queue.

You will produce:

- an approval queue operating policy
- a handoff trigger matrix
- escalation and timeout rules
- audit fields and queue metrics

---

## Why This Matters

Saying "a human reviews it" is not the same as having a working handoff system.

Common failures:

- The workflow knows when it should pause, but the queue has no clear owner.
- Reviewers receive too much context, too little context, or the wrong context.
- High-risk items and low-risk items look identical in the queue.
- Timeout behavior is undefined, so items sit forever or the system proceeds unsafely.
- Queue backlog makes approval a bottleneck, but nobody measures it.
- Audit logs record final approval but not the triggering reason or rejected alternatives.
- Rejections and escalations are never turned into workflow improvements.

Production handoff is an operating system: trigger, route, review, decide, record, improve.

---

## Best-Practice Principles

1. **Put handoff at the real risk boundary.**  
   Require review where the service could disclose sensitive data, take an irreversible action, or create material client impact.

2. **Route to the right reviewer, not just any reviewer.**  
   Authority and context matter as much as availability.

3. **Design the queue card for low-energy review.**  
   Show the minimum necessary facts, the main risk, and clear actions.

4. **Define response targets and timeout behavior.**  
   A queue without aging rules is only hidden latency.

5. **Log the whole decision, not only the final click.**  
   Trigger reason, reviewer role, decision, timestamp, and reason code should all be reconstructable.

6. **Separate approval from escalation.**  
   Some cases need a higher authority or specialist review, not a faster decision.

7. **Use queue data as product feedback.**  
   Rejections, revisions, and escalations are evidence about system quality and boundary design.

---

## Concepts

### Handoff Trigger

A condition that forces the workflow to stop and request human review.

Examples:

- customer-facing message
- record export
- billing action above a threshold
- conflicting policy context
- low-confidence or policy-check failure

### Approval Route

The reviewer role and queue path assigned to a trigger type.

### Response Target

The expected time window for reviewer action.

Examples:

- 15 minutes for urgent account lockout response
- 4 hours for routine outbound communication
- same business day for data export review

### Timeout Rule

The defined behavior when the response target is missed.

Examples:

- do not proceed automatically
- notify queue owner
- escalate to backup reviewer

### Reason Code

A short label explaining why the reviewer approved, rejected, revised, or escalated the item.

### Queue Metric

A measurement that shows whether human review is functioning.

Examples:

- queue wait time
- backlog age
- rejection rate
- escalation rate

---

## Approval Queue Operating Policy Template

Use this structure:

```markdown
# Approval Queue Operating Policy: [Workflow Name]

## Handoff Triggers

| Trigger | Risk | Reviewer role | Allowed actions | Target response time |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## Queue Card Contents

- Item summary:
- Recommended action:
- Key supporting facts:
- Risk flags:
- Source IDs or links:

## Timeout and Escalation

- Primary timeout:
- Backup reviewer:
- Final fallback:

## Audit Fields

- Trace ID:
- Queue item ID:
- Trigger reason:
- Reviewer:
- Decision:
- Reason code:
- Timestamp:

## Metrics

- Queue wait time:
- Backlog age:
- Approval rate:
- Revision rate:
- Escalation rate:

## Improvement Loop

- Weekly review:
- Top reason codes:
- Workflow changes triggered by review data:
```

Review questions:

- Who owns each trigger type?
- What happens if nobody acts?
- What reviewer information is truly necessary?

---

## Walkthrough

Use this example:

> A support drafting service creates customer-facing reply drafts and occasionally recommends billing credits or escalation to a specialist team.

### Step 1 - Define the triggers

Good handoff triggers for this service:

- any customer-facing message
- any credit or billing adjustment recommendation
- any draft with conflicting policy signals
- any request involving sensitive account data
- any draft generated under fallback routing after repeated failures

The main question is:

> Where would a wrong output become expensive, irreversible, or trust-damaging?

### Step 2 - Route to the correct reviewer

Not every trigger belongs to the same queue.

Example routing:

| Trigger | Reviewer role |
| --- | --- |
| Customer-facing message | support reviewer |
| Billing credit above threshold | operations lead |
| Sensitive data disclosure risk | privacy or compliance owner |
| Conflicting policy context | senior support lead |

This is a governance choice, not only a UI choice.

### Step 3 - Design the queue card

Weak queue card:

- full conversation transcript
- every retrieved chunk
- dozens of metadata fields
- no recommendation

Better queue card:

- one-sentence case summary
- draft preview
- highlighted risk flags
- source IDs
- recommended action
- explicit buttons: approve, reject, revise, escalate

Low-energy review should be possible without losing control.

### Step 4 - Define timeout behavior

Weak rule:

> If nobody acts, keep waiting and hope someone notices.

Better rule:

- notify the queue owner when the response target is missed
- escalate to a backup reviewer after the second threshold
- never send external communication automatically because the queue aged out

Timeout behavior is part of system safety.

### Step 5 - Record the full decision

Useful audit fields:

```text
trace_id=trace_2191
queue_item_id=review_884
trigger_reason=customer_facing_message
reviewer_role=support_reviewer
decision=revise
reason_code=policy_tone_mismatch
timestamp=2026-04-21T15:08:00Z
```

That is enough to study queue health later without storing unnecessary sensitive content.

### Step 6 - Reuse queue data for improvement

If the top reasons for revision are:

- missing context
- policy mismatch
- overconfident phrasing

then the queue is showing product defects, not only reviewer preference. That data should feed:

- prompt revisions
- retrieval changes
- policy-check updates
- release gate changes

---

## Practice

Choose one workflow with at least three actions or outputs that could create real user or client impact.

Create an approval queue operating policy that includes:

1. at least five handoff triggers
2. reviewer role for each trigger
3. allowed actions for each route
4. queue card contents
5. timeout and escalation rules
6. audit fields
7. at least five queue metrics
8. a weekly improvement loop

At least one trigger must involve:

- external communication
- money or billing impact
- sensitive data disclosure risk

---

## Review Checklist

Your handoff artifact is acceptable when:

- Triggers sit at real risk boundaries.
- The right reviewer role is named for each trigger.
- The queue card shows the minimum necessary context.
- Timeout behavior never silently proceeds.
- Escalation paths are explicit.
- Audit fields reconstruct the decision.
- Queue metrics show backlog and review quality.
- Review data feeds back into system improvement.
- The design works on a low-energy day.

---

## Common Failure Modes

- **Review theater:** Human review exists, but the queue card is too noisy to use safely.
- **No owner:** Items wait in a shared queue with unclear responsibility.
- **Silent timeout:** The system proceeds or the backlog grows with no explicit rule.
- **Wrong reviewer:** The person assigned lacks authority or context.
- **No audit trail:** The trigger and reason code disappear once the decision is made.
- **No feedback loop:** Queue outcomes never change the workflow design.

---

## Portfolio Evidence

Save:

- the approval queue operating policy
- the handoff trigger matrix
- one short note explaining how timeout and escalation work

This shows that you can make human review operationally real instead of relying on good intentions.

---

## References

- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
