# Lesson 07.08: Client Data Safety Review Package

**Parent syllabus:** [Syllabus 07: Data Privacy and Client Safety](../../syllabus-07-data-privacy.md)  
**Estimated time:** 3 hours  
**Artifact:** Complete client data safety review package

---

## Outcome

By the end of this lesson, you can assemble the core privacy and client-data safety artifacts for one workflow into a reviewer-ready package that supports approval, scope narrowing, redesign, or deferral.

You will produce a complete client data safety review package that includes:

- client data classification and handling map
- pre-engagement intake checklist and decision log
- AI tool exclusion and escalation matrix
- data handling clause pack
- incident response packet
- prompt injection defense specification
- human handoff policy
- final recommendation

---

## Why This Matters

Privacy controls are weak when they live as disconnected notes.

Common failures:

- The exclusion matrix bans a category, but the contract language still implies broader handling.
- The intake checklist says approvals are missing, but the project proceeds anyway.
- The handoff design assumes a reviewer role that the client never assigned.
- The incident plan does not match what the logging design actually records.
- Prompt injection defenses exist, but the tool permission surface still allows unsafe actions.
- The package has no final decision, so risk is documented but not managed.

The review package is the bridge between "we talked about privacy" and "this workflow has a defensible data-handling posture."

---

## Best-Practice Principles

1. **Make each artifact answer a review question.**  
   If a document does not help approve, narrow, operate, or reject the workflow, simplify it.

2. **Keep the package internally consistent.**  
   Data categories, exclusions, contracts, handoffs, incident paths, and approvals should not contradict each other.

3. **Trace controls to risks.**  
   Every major privacy or disclosure risk should map to a control, handoff, or stop rule.

4. **Separate known facts from open questions.**  
   Hidden uncertainty is what turns privacy work into accidental risk.

5. **End with a decision.**  
   Proceed, proceed with narrower scope, wait for approvals, or stop.

6. **Name approvals and owners.**  
   Governance matters most when the workflow touches sensitive data.

7. **Keep a sanitized version.**  
   Good privacy work can still become portfolio evidence once identifiers and client-sensitive details are removed.

---

## Concepts

### Client Data Safety Package

A compact set of documents that explains what data a workflow touches, what paths are allowed, what is excluded, how incidents are handled, and what approvals are required.

### Traceability

The ability to move from:

- a sensitive data category
- to the exclusion or handling rule
- to the contract language
- to the incident path
- to the final decision

### Approval Decision

A deliberate recommendation about whether the workflow should move forward.

Examples:

- proceed
- proceed with narrower phase-one scope
- wait for approvals
- stop

### Change Trigger

A condition that forces the package to be reviewed again.

Examples:

- new vendor
- new data category
- PHI enters scope
- parent-facing automation expands
- incident occurs

---

## Client Data Safety Review Package Template

Use this structure:

```markdown
# Client Data Safety Review Package: [Workflow Name]

## 1. Executive Summary

- Workflow purpose:
- Main data risks:
- Current recommendation:

## 2. Data Classification and Handling Summary

Key categories, approved paths, and prohibited paths.

## 3. Intake and Approval Status

Known facts, missing approvals, and blockers.

## 4. Exclusion and Escalation Rules

What must stay out of the workflow and who owns exceptions.

## 5. Contract and Questionnaire Notes

Key client-facing data handling language and truthful security answers.

## 6. Incident and Recovery Plan

Containment, notice, and resume conditions.

## 7. Prompt Injection and Handoff Controls

Security boundaries, approval gates, and audit events.

## 8. Open Questions and Change Triggers

What still needs proof and what would force a re-review?

## 9. Decision

Proceed, proceed narrower, wait for approvals, or stop.

## 10. Sanitized Portfolio Version

What must be removed before sharing publicly?
```

---

## Walkthrough

Use the pediatric therapy clinic workflow:

> Summarize intake forms, draft staff-facing notes, and prepare parent-facing follow-up drafts for review.

### Step 1 - Pull the artifacts together

The package should include:

- handling map from Lesson 07.01
- intake checklist from Lesson 07.02
- exclusion matrix from Lesson 07.03
- clause pack from Lesson 07.04
- incident response packet from Lesson 07.05
- prompt injection defense spec from Lesson 07.06
- human handoff policy from Lesson 07.07

The point is not volume. The point is decision-ready privacy evidence.

### Step 2 - Check consistency

Examples:

| Claim | Where it should appear |
| --- | --- |
| Insurance details are excluded from phase one | Handling map, exclusion matrix, workflow scope, incident plan |
| Parent-facing output requires human review | Handoff policy, contract notes, approval queue, final decision |
| Raw diagnoses must not appear in logs | Handling map, exclusion matrix, monitoring notes, incident evidence rules |
| New vendors require approval before use | Intake checklist, contract notes, decision log |

### Step 3 - Write the executive summary

Example:

```markdown
# Client Data Safety Review Package: Pediatric Therapy Intake Follow-Up

## 1. Executive Summary

- Workflow purpose: Create staff summaries and human-reviewed parent follow-up drafts from clinic intake information.
- Main data risks: child data exposure, PHI mishandling, unapproved vendor use, prompt injection through uploaded notes, and accidental external disclosure.
- Current recommendation: proceed only with a narrowed phase-one scope, approved tools, and mandatory human review before any parent-facing output.
```

### Step 4 - End with a real decision

Weak ending:

> We have thought about privacy.

Better ending:

> Wait for approvals before any workflow path touches PHI. In the meantime, a narrow scheduling-only phase may proceed using approved tools, no insurance fields, and human review for all outward-facing content.

### Step 5 - Add change triggers

For this workflow, re-review the package if:

- a new model vendor is proposed
- insurance processing enters scope
- parent-facing automation expands
- a data incident occurs
- retention rules change

That is what keeps the package alive after kickoff.

---

## Practice

Choose one real or realistic workflow and assemble a complete client data safety review package with:

1. executive summary
2. data classification and handling summary
3. intake and approval status
4. exclusion and escalation rules
5. contract and questionnaire notes
6. incident and recovery plan
7. prompt injection and handoff controls
8. open questions and change triggers
9. final decision

End with one of these decisions:

- Proceed
- Proceed with narrower scope
- Wait for approvals
- Stop

---

## Review Checklist

Your client data safety package is acceptable when:

- All core privacy artifacts are present.
- The artifacts do not contradict each other.
- Sensitive data categories map to explicit controls.
- Missing approvals or blockers affect the final decision.
- Human handoff and incident response are included.
- Security and prompt injection controls are included.
- Change triggers are defined.
- The final recommendation is clear.
- A sanitized public version would be possible.

---

## Common Failure Modes

- **Artifact pile, not package:** The documents exist but do not work together.
- **No decision:** Risks are listed, but nothing about scope or approval changes.
- **No traceability:** Sensitive categories are named without linked controls.
- **Hidden blocker:** Missing approvals are buried in notes instead of surfaced in the recommendation.
- **No change triggers:** The package goes stale after kickoff.
- **No owner:** Privacy work is described, but no one approves it.

---

## Portfolio Evidence

Save:

- The complete client data safety review package
- One sanitized version
- One short note explaining the final recommendation

This shows that you can turn privacy concerns into operating controls and scope decisions.

---

## References

- NIST Privacy Framework: https://www.nist.gov/privacy-framework
- HHS, Business Associates: https://www.hhs.gov/hipaa/for-professionals/privacy/guidance/business-associates/index.html
- HHS, Breach Notification Rule: https://www.hhs.gov/hipaa/for-professionals/breach-notification/index.html
- OWASP, LLM Prompt Injection Prevention Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html
- European Commission, AI Act: https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai
