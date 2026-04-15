# Lesson 04.06: Delivery, Acceptance, and Closure

**Parent syllabus:** [Syllabus 04: Finishing Systems](../../syllabus-04-finishing-systems.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Delivery and closure packet

---

## Outcome

By the end of this lesson, you can close a project in a way that makes acceptance, handoff, limitations, and next steps explicit.

You will produce a delivery and closure packet for an AI workflow deliverable.

---

## Why This Matters

A project can be technically complete and still not be finished.

Client-facing work needs delivery, acceptance, and closure. Without those steps, the project can drift into unpaid support, ambiguous revisions, unclear ownership, or silent dissatisfaction.

Common failures:

- The deliverable is sent without a summary.
- The client does not know what to review.
- Known limitations are hidden or scattered.
- Acceptance is implied but not confirmed.
- Handoff instructions are missing.
- Future work is discussed but not separated from the current project.
- Support expectations are undefined.

Closure is part of the system. It is how the work stops being dependent on you.

---

## Best-Practice Principles

1. **Deliver with context.**  
   Explain what is included, what changed, and how to review it.

2. **Ask for explicit acceptance.**  
   Do not rely on silence as approval.

3. **Include the acceptance checklist.**  
   The client should know how to judge whether the deliverable meets scope.

4. **Name known limitations.**  
   Honest limitations reduce misuse and support friction.

5. **Document ownership.**  
   State who owns operation, maintenance, and future changes.

6. **Separate current closure from future phases.**  
   Future work should become a new scope, not a hidden tail.

7. **Archive evidence.**  
   Save the final files, delivery message, acceptance response, and lessons learned.

---

## Concepts

### Delivery Packet

The client-facing package that contains final artifacts and review instructions.

### Acceptance

The client's explicit confirmation that the scoped deliverable is complete.

### Handoff

The information needed for the client or operator to use, review, or maintain the deliverable.

### Known Limitations

Named boundaries that prevent the deliverable from being used outside its intended scope.

### Closure Note

A short message that confirms what was delivered, what is not included, what happens next, and how future work should be handled.

---

## Walkthrough

Project:

> AI workflow that drafts support reply suggestions from one category of exported tickets.

Deliverables:

- Prompt chain
- Example input and output
- Review checklist
- Operator handoff guide
- Demo recording

Weak delivery note:

> Here are the files. Let me know what you think.

This creates ambiguity. The client does not know what to review, what is final, or what happens next.

Stronger delivery note:

> Attached is the final phase-one support reply workflow package. It includes the prompt chain, example input and output, operator guide, review checklist, and demo recording. Please review against the acceptance checklist below by Friday. This phase does not include Zendesk integration, automatic sending, live customer data processing, or ongoing monitoring. If accepted, I will close phase one and can scope integration as a separate phase.

### Acceptance checklist

- The workflow supports the agreed ticket category.
- The prompt chain produces a draft reply and missing-information flags.
- The review checklist identifies accuracy, tone, policy, and unsupported claims.
- The operator guide explains inputs, steps, stop conditions, and limitations.
- The demo uses approved sample data.

### Closure decision

If accepted:

> Phase one closed. Save artifacts, acceptance note, and future-phase ideas.

If not accepted:

> Compare feedback to scope. If feedback is within the revision policy, revise. If outside scope, create a change request.

---

## Delivery and Closure Packet Template

```markdown
# Delivery and Closure Packet: [Project Name]

## Delivery Summary

What is being delivered?

## Final Artifacts

- Artifact:
- Location:
- Purpose:

## Scope Confirmed

What scope does this satisfy?

## Acceptance Checklist

- [ ] ...

## Known Limitations

- ...

## Not Included

- ...

## Handoff Notes

- How to use:
- When not to use:
- Human review required:
- Owner:
- Maintenance:

## Review Deadline

Please review by:

## Acceptance Request

Please confirm whether this satisfies the agreed scope or send consolidated revision notes by the review deadline.

## Future Phase Candidates

- ...

## Closure Log

- Delivered:
- Accepted:
- Final files saved:
- Lessons learned:
```

---

## Practice

Create a delivery and closure packet for one project you have completed or could complete.

Include:

1. Delivery summary.
2. Final artifact list.
3. Scope confirmed.
4. Acceptance checklist.
5. Known limitations.
6. Not-included list.
7. Handoff notes.
8. Review deadline.
9. Acceptance request.
10. Future phase candidates.
11. Closure log.

Then write the client-facing delivery note in 150 words or less.

---

## Review Checklist

Your delivery packet is acceptable when:

- It explains what is being delivered.
- It links or lists final artifacts.
- It ties delivery back to scope.
- It includes an acceptance checklist.
- It names known limitations.
- It repeats what is not included.
- It includes handoff notes.
- It asks for explicit acceptance.
- It separates future phases from current closure.
- It creates a final archive record.

---

## Common Failure Modes

- **Sending files without context:** The client does not know how to review.
- **No acceptance request:** The project lingers.
- **Hidden limitations:** The client misuses the workflow later.
- **Future work blended into delivery:** Phase two quietly starts before phase one closes.
- **No owner named:** Maintenance responsibility is assumed.
- **No archive:** You cannot prove what was delivered or accepted.

---

## Portfolio Evidence

Save:

- Delivery and closure packet
- Client-facing delivery note
- Acceptance checklist
- Final artifact links or file paths
- Lessons learned note

This becomes evidence that you can finish client work professionally, not just build interesting artifacts.

---

## References

- PlainLanguage.gov guidelines: https://www.plainlanguage.gov/guidelines/
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
