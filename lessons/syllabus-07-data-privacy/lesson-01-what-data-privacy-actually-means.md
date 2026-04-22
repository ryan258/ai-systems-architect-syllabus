# Lesson 07.01: What Data Privacy Actually Means

**Parent syllabus:** [Syllabus 07: Data Privacy and Client Safety](../../syllabus-07-data-privacy.md)  
**Estimated time:** 2 hours  
**Artifact:** Client data classification and handling map

---

## Outcome

By the end of this lesson, you can look at a workflow and identify what kinds of client data it touches, why that matters, and what handling rules belong around it before any build work begins.

You will produce a client data classification and handling map that records:

- data categories
- sensitivity level
- system purpose
- approved uses
- prohibited uses
- storage and logging rules
- retention and deletion rules
- approval or escalation points

---

## Why This Matters

Most privacy failures start before the first API call.

Common failures:

- A consultant says "it is just a CSV" without identifying that it contains health information, child data, or internal business secrets.
- A workflow stores raw client files because retention was never discussed.
- A team logs prompts and outputs for debugging without deciding what should never be logged.
- A model provider is chosen before anyone asks whether the client approved that vendor.
- Derived artifacts such as summaries, embeddings, notes, or screenshots are treated as harmless even though they still contain sensitive information.
- A client assumes you understand their privacy obligations, while you assume they will tell you what matters.

Privacy is not a legal department topic that appears later. It is a system design input.

This lesson is not legal advice. It is operating discipline that helps you know when to stop, narrow scope, or get qualified review.

---

## Best-Practice Principles

1. **Classify data before building the workflow.**  
   If you do not know what kinds of data the system touches, you cannot decide where it may go.

2. **Treat purpose as a boundary.**  
   Data should be used only for the workflow purpose the client approved, not for convenience or future speculation.

3. **Minimize what enters the system.**  
   Send the smallest amount of information needed for the task. If the system can work with a summary, do not send the raw record.

4. **Map storage and logging early.**  
   Privacy failures often happen in traces, screenshots, exports, and debug logs rather than in the primary data store.

5. **Assume derived artifacts can still be sensitive.**  
   Summaries, embeddings, model outputs, and reviewer notes may still carry personal, health, or confidential business information.

6. **Separate approved paths from prohibited paths.**  
   "We are careful" is not enough. The system needs explicit rules for what is allowed, redacted, quarantined, or escalated.

7. **Make the map maintainable.**  
   A low-energy operator should be able to read the artifact and know what data the workflow may touch, where it may go, and what requires approval.

---

## Concepts

### Personal Data

Information related to an identified or identifiable person.

Examples:

- full name
- email address
- phone number
- account number tied to a person
- address

The exact legal definition varies by jurisdiction, but the operational question is simple:

> Could this reasonably identify or be tied back to a person?

### Sensitive Data

Data that creates higher risk if mishandled.

Examples:

- health information
- financial account details
- government identifiers
- child data
- biometric data
- confidential employee information

### Protected Health Information (PHI)

HHS defines PHI as protected health information under the HIPAA Privacy Rule.

Operationally, if you are working for a HIPAA covered entity or business associate and the workflow touches identifiable health information, you should assume the system design needs extra review.

### Confidential Business Information

Non-public client information that can create contractual, commercial, or reputational harm if exposed.

Examples:

- pricing sheets
- customer lists
- product plans
- incident reports
- acquisition plans

### Data Minimization

Using only the data necessary for the task.

Examples:

- sending a ticket summary instead of the full thread
- masking account numbers
- storing record IDs rather than raw records

### Data Lifecycle

What happens to data from intake through deletion.

Typical stages:

- receipt
- processing
- storage
- logging
- review
- export
- retention
- deletion

### Handling Map

A working artifact that records what data the workflow touches and the rules around it.

This is the privacy equivalent of an architecture diagram or operating worksheet.

---

## Client Data Classification and Handling Map Template

Use this structure:

```markdown
# Client Data Classification and Handling Map: [Workflow Name]

## Workflow Purpose

What job is the workflow supposed to do?

## Data Categories

| Category | Example fields | Sensitivity | Why the workflow needs it | Approved path | Prohibited path |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

## Storage and Logging Rules

- What may be stored:
- What may be logged:
- What must never be logged:
- What must be redacted or masked:

## Retention and Deletion Rules

- Working retention:
- Long-term retention:
- Deletion trigger:
- Deletion owner:

## Approvals and Escalations

- What requires approval before processing:
- What requires approval before external sharing:
- What requires professional review:

## Open Questions

- ...

## Decision

Proceed, proceed with narrower scope, wait for approvals, or stop.
```

---

## Walkthrough

Use this example:

> A pediatric therapy clinic wants an AI workflow that summarizes intake forms, drafts staff-facing follow-up notes, and prepares a parent email for human review.

Assume the clinic is a HIPAA covered entity and the intake forms include information about children.

### Step 1 - Name the data, not just the file

Weak description:

> Intake spreadsheet and notes.

Better description:

- child and parent names
- contact details
- scheduling preferences
- diagnoses or care concerns
- insurance information
- staff notes

The privacy map starts getting useful only when the categories are specific.

### Step 2 - Separate the categories by sensitivity

For this workflow:

| Category | Sensitivity | Why it matters |
| --- | --- | --- |
| Parent contact information | Medium | personal data, external communication risk |
| Child name and appointment details | High | child data plus medical context |
| Diagnoses and care concerns | High | health information |
| Insurance details | High | financial and health-adjacent information |
| Internal staffing notes | Medium or high | operationally sensitive, may contain health context |

This classification changes the design immediately:

- parent email drafts need strict review
- raw intake data should not appear in logs
- generic public AI tools are out unless the path is explicitly approved

### Step 3 - Write the purpose boundary

Approved purposes:

- summarize intake facts for clinic staff
- draft a parent follow-up email for review
- route scheduling questions to the right staff member

Not approved:

- store raw intake forms for future model training
- reuse the data to build marketing content
- paste records into unapproved AI workspaces
- expose diagnoses in monitoring dashboards

The review question is:

> What exactly did the client approve this system to do with this data?

### Step 4 - Map storage and logging

Weak rule:

> We will log enough to debug.

Better rule:

- store internal record IDs and workflow status
- log model name, step name, trace ID, and parser result
- never log raw diagnoses, insurance numbers, or full parent emails
- redact names in screenshots or sample cases

This is where observability and privacy meet. You need enough information to reconstruct failures without turning logs into a shadow database.

### Step 5 - Decide the first scope limit

Given the data involved, a responsible first decision may be:

> Do not process insurance details in phase one. Limit the workflow to scheduling and staff-facing summaries until the approved vendor path, retention rule, and human review design are confirmed.

That is privacy architecture, not delay.

### Step 6 - Draft the map

Example:

```markdown
# Client Data Classification and Handling Map: Pediatric Therapy Intake Follow-Up

## Workflow Purpose

Summarize intake information for clinic staff and draft parent follow-up emails for human review.

## Data Categories

| Category | Example fields | Sensitivity | Why the workflow needs it | Approved path | Prohibited path |
| --- | --- | --- | --- | --- | --- |
| Parent contact information | Name, email, phone | Medium | Draft follow-up communication | Approved clinic workflow only | Public AI chat tools |
| Child identity and schedule | Child name, appointment slot | High | Scheduling summary | Approved clinic workflow only | Logs and screenshots |
| Diagnoses and care concerns | Intake note text | High | Staff-only summary | Approved clinic workflow only | Parent-facing draft body unless approved |
| Insurance details | Carrier, member ID | High | Not needed for phase one | Excluded from workflow | Any model prompt |

## Storage and Logging Rules

- What may be stored: internal record ID, workflow status, approval outcome
- What may be logged: trace ID, step name, model path, latency, error code
- What must never be logged: diagnoses, full intake text, insurance numbers
- What must be redacted or masked: names in sample cases and screenshots

## Retention and Deletion Rules

- Working retention: only as long as needed to complete the task
- Long-term retention: none outside approved clinic systems
- Deletion trigger: task complete or client request
- Deletion owner: clinic operations owner

## Approvals and Escalations

- What requires approval before processing: any use of new vendor tools
- What requires approval before external sharing: any parent-facing email draft
- What requires professional review: HIPAA, child data, and retention questions

## Open Questions

- Is a business associate agreement required for the selected vendors?
- Which clinic role approves retention?

## Decision

Proceed only with narrowed phase-one scope and approved vendor path.
```

---

## Practice

Pick one real or realistic workflow.

Create a client data classification and handling map with:

1. workflow purpose
2. at least five data categories
3. sensitivity level for each category
4. approved and prohibited paths
5. storage and logging rules
6. retention and deletion rules
7. approval and escalation points
8. final proceed, narrow, wait, or stop decision

At least one category must cover:

- personal data
- sensitive or regulated data
- confidential business information

---

## Review Checklist

Your handling map is acceptable when:

- The workflow purpose is specific.
- Data categories are named clearly, not hidden inside vague file labels.
- At least one prohibited path is explicit.
- Logging rules say what must never be logged.
- Retention and deletion are defined.
- Derived artifacts are treated as data, not ignored.
- At least one approval or escalation point exists.
- The final decision narrows or pauses the work if key facts are missing.
- A future maintainer could understand the data boundary quickly.

---

## Common Failure Modes

- **File-name thinking:** The artifact says "PDFs" or "CSVs" instead of naming the actual data.
- **Purpose drift:** Data is used for tasks the client never approved.
- **Log leakage:** Sensitive fields are excluded from storage but still appear in traces, screenshots, or support notes.
- **Derived-data blind spot:** Summaries and embeddings are treated as harmless.
- **No decision:** The map records risk but does not change scope or design.
- **No owner:** Retention and deletion rules exist on paper only.

---

## Portfolio Evidence

Save:

- The client data classification and handling map
- One short note explaining the narrowest safe first scope
- One sanitized version with client names and identifiers removed

This shows that you can treat privacy as architecture, not cleanup.

---

## References

- NIST Privacy Framework: https://www.nist.gov/privacy-framework
- HHS, What is PHI?: https://www.hhs.gov/answers/hipaa/what-is-phi/index.html
- FTC, Children's Online Privacy Protection Rule: https://www.ftc.gov/legal-library/browse/rules/childrens-online-privacy-protection-rule-coppa
- European Commission, AI Act: https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai
