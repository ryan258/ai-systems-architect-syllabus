# Lesson 07.02: What to Ask Before Touching Client Data

**Parent syllabus:** [Syllabus 07: Data Privacy and Client Safety](../../syllabus-07-data-privacy.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Pre-engagement client data intake checklist and decision log

---

## Outcome

By the end of this lesson, you can run a structured pre-engagement privacy and security intake before you touch client data, connect a new tool, or promise a workflow design.

You will produce:

- a pre-engagement client data intake checklist
- a decision log
- a proceed, narrow, wait, or stop recommendation

---

## Why This Matters

Bad privacy work often begins with unclear assumptions, not malice.

Common failures:

- The consultant starts prototyping before confirming whether the client approved any AI vendor path.
- A client says "we are HIPAA" or "we are SOC 2" and nobody asks what that means for the actual workflow.
- A workflow touches PHI or child data before anyone confirms the right contacts, contracts, or approvals.
- The person asking for the workflow cannot answer security questionnaire questions accurately, but the project starts anyway.
- Ownership of outputs, deletion, and access control are left vague until after data is already moving.
- The client expects the consultant to know their obligations, and the consultant expects the client to warn them.

The first privacy control is asking better questions before any data enters the system.

---

## Best-Practice Principles

1. **Run intake before prototype work.**  
   Do not "just test with a sample file" until you know what the sample contains and who approved the path.

2. **Ask who can answer accurately.**  
   Security and compliance answers should come from the right client contact, not whoever scheduled the call.

3. **Separate workflow goals from data permissions.**  
   A useful idea is not automatically an authorized data flow.

4. **Check for regulated or contract-heavy contexts early.**  
   HIPAA, child data, enterprise customer commitments, and jurisdiction-specific rules should change the design conversation immediately.

5. **Record unknowns as blockers, not footnotes.**  
   If you do not know whether a business associate agreement, vendor approval, or retention rule is required, that is a real status condition.

6. **End intake with a decision.**  
   The output should say proceed, narrow scope, wait for approvals, or stop.

7. **Keep the checklist reusable.**  
   A strong intake artifact should work again on the next client, with only small edits.

---

## Concepts

### Pre-Engagement Intake

A structured review performed before client data enters your tools or workflow.

It should answer:

- what data the workflow touches
- who owns approvals
- what tools are allowed
- what contracts or commitments matter
- who handles incidents

### Covered Entity and Business Associate

Under HIPAA, the Rules apply to covered entities and business associates.

Operationally:

- if the client is a covered entity or business associate
- and the workflow may use or disclose PHI
- and a vendor or consultant will handle that PHI on the client's behalf

then the design may need written agreements and narrower handling rules before the work starts.

### Security Questionnaire

A set of questions a client or enterprise procurement team uses to understand vendor controls.

You do not need to bluff your way through it. You need accurate answers and clear boundaries.

### Vendor Approval

The client's explicit permission to use a given platform, model provider, storage system, or integration path.

If approval is unclear, treat the vendor as unapproved.

### Decision Log

A short record of:

- known facts
- open questions
- blockers
- owners
- final recommendation

This turns discovery into an operating artifact instead of memory.

---

## Pre-Engagement Client Data Intake Template

Use this structure:

```markdown
# Pre-Engagement Client Data Intake: [Client or Workflow Name]

## Workflow Request

What does the client want the system to do?

## Data Questions

- What kinds of data are involved?
- Does the workflow involve personal, health, child, financial, or confidential business data?
- Does the workflow need raw records, or can it use summaries or metadata?

## Client Context

- Is the client a regulated organization or working under regulated customer commitments?
- Which jurisdictions matter?
- Who can answer privacy, security, and contract questions accurately?

## Tool and Vendor Questions

- Which tools are already approved?
- Which tools are explicitly prohibited?
- Is a new vendor approval required?
- Is a written agreement required before use?

## Storage, Logging, and Access Questions

- Where may data be stored?
- What may be logged?
- Who may access the workflow?
- What is the deletion rule?

## Incident and Escalation Questions

- Who is the first client contact if something goes wrong?
- Who approves scope changes?
- Who signs off on higher-risk automation?

## Open Questions and Blockers

- ...

## Decision Log

- Recommendation:
- Why:
- Next owner:
- Next review date:
```

---

## Walkthrough

Use the pediatric therapy clinic workflow:

> Summarize intake forms, draft staff-facing follow-up notes, and prepare parent email drafts for review.

### Step 1 - Confirm who can answer the real questions

The clinic owner may know the workflow pain.

They may not know:

- whether a business associate agreement is required
- which vendors are approved
- who owns the retention policy
- who answers security questionnaires for enterprise referrals

So the intake should identify:

- workflow sponsor
- operations owner
- privacy or compliance contact
- security or IT contact

If these roles are missing, the project is not ready for data handling decisions.

### Step 2 - Ask the data questions that change scope

Useful intake questions:

- Does the workflow touch identifiable health information?
- Does it include information about children?
- Does it include insurance, billing, or payment data?
- Can phase one work with a narrower subset such as scheduling information only?
- Does the workflow need raw documents or only extracted fields?

These questions are not paperwork. They shape the design.

### Step 3 - Ask the vendor and agreement questions directly

For this clinic:

- Which model providers are approved, if any?
- Can clinic data leave the primary system boundary?
- Does the chosen path require a business associate agreement before PHI is processed?
- Are staff already using an approved secure environment for summaries and drafts?

If the answer to the agreement or approval question is unclear, the decision log should not say "proceed anyway."

### Step 4 - Record the blockers

Example blockers:

- Business associate agreement status unknown
- No approved retention period for derived summaries
- Parent-facing email drafts need a reviewer role and audit trail
- Insurance details are in the input files, but phase one does not need them

The design change is obvious:

> Narrow phase one to staff-only summaries using approved tools while the client confirms the agreement and approval path for any system that touches PHI.

### Step 5 - End with a decision

Example decision log:

```markdown
## Decision Log

- Recommendation: Wait for approvals, then proceed with narrower phase-one scope
- Why:
  - PHI may be involved
  - the approved vendor path is not yet confirmed
  - parent-facing content requires a reviewer and audit trail
- Next owner: Clinic operations lead to confirm vendor approval and agreement path
- Next review date: After legal or compliance contact responds
```

Weak ending:

> Looks promising.

Better ending:

> Do not ingest intake files until the client confirms the approved vendor path, retention rule, and who signs the business associate agreement if one is required.

---

## Practice

Choose one real or realistic client workflow.

Create a pre-engagement client data intake checklist and decision log that includes:

1. workflow request
2. data questions
3. client context
4. vendor and tool questions
5. storage, logging, and access questions
6. incident and escalation contacts
7. open questions and blockers
8. final recommendation

Include at least:

- five data questions
- five tool or approval questions
- three escalation or incident contacts

End with one of these recommendations:

- Proceed
- Proceed with narrower scope
- Wait for approvals
- Stop

---

## Review Checklist

Your intake checklist is acceptable when:

- It identifies who can answer privacy and security questions accurately.
- It asks what kinds of data are involved, not only what files exist.
- It records vendor approval status explicitly.
- It records whether written agreements or additional review may be needed.
- It includes storage, logging, access, and deletion questions.
- It names incident and escalation contacts.
- Unknowns are treated as blockers where appropriate.
- It ends with a clear decision.
- The artifact can be reused on the next client.

---

## Common Failure Modes

- **Prototype first:** Real data enters the workflow before approval questions are answered.
- **Wrong contact:** Sales or operations answers security questions they do not own.
- **Framework name-dropping:** HIPAA, SOC 2, or the EU AI Act are mentioned without tying them to the actual workflow.
- **No blocker logic:** Missing approvals are recorded but do not change the recommendation.
- **No scope narrowing:** The workflow is not reduced even when the data path is still unclear.
- **No written decision:** Discovery happens in conversation only and cannot guide the next step.

---

## Portfolio Evidence

Save:

- The pre-engagement client data intake checklist
- The decision log
- One short note explaining the main blocker and the recommended narrower scope

This shows that you know how to stop unsafe work before it starts.

---

## References

- HHS, Covered Entities and Business Associates: https://www.hhs.gov/hipaa/for-professionals/covered-entities/index.html
- HHS, Business Associates: https://www.hhs.gov/hipaa/for-professionals/privacy/guidance/business-associates/index.html
- NIST Privacy Framework: https://www.nist.gov/privacy-framework
- AICPA, SOC 2 - SOC for Service Organizations: Trust Services Criteria: https://www.aicpa-cima.com/topic/audit-assurance/audit-and-assurance-greater-than-soc-2
- European Commission, AI Act: https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai
