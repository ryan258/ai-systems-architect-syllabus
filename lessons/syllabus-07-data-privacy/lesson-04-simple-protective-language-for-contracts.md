# Lesson 07.04: Simple Protective Language for Contracts

**Parent syllabus:** [Syllabus 07: Data Privacy and Client Safety](../../syllabus-07-data-privacy.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Data handling clause pack and security questionnaire response sheet

---

## Outcome

By the end of this lesson, you can draft plain-English client-facing language that sets realistic data handling boundaries without pretending to be a lawyer, auditor, or compliance officer.

You will produce:

- a data handling clause pack
- a short security questionnaire response sheet
- a list of statements you will not make

---

## Why This Matters

Privacy problems often start in promises, not code.

Common failures:

- A consultant says a workflow is "HIPAA compliant" without authority or evidence.
- A contract says almost nothing about approved tools, retention, deletion, or incident notice.
- The client assumes outputs, prompts, and derived notes belong to them, while the consultant assumes something else.
- A security questionnaire answer overstates controls because the consultant wants to keep the deal moving.
- SOC 2 is treated like a slogan instead of a specific report about controls.
- The agreement does not say what happens if the client sends restricted data unexpectedly.

The contract is part of the system boundary. It should reduce ambiguity, not create it.

This lesson is not legal advice. Use counsel to review any production agreement language.

---

## Best-Practice Principles

1. **Say what you will do and what you will not do.**  
   A useful clause defines boundaries, not only aspirations.

2. **Name the approved data path.**  
   If a workflow depends on specific tools, storage locations, or review rules, write that down.

3. **Do not overstate compliance.**  
   Do not claim certification, attestation, or legal status you do not actually have.

4. **Define output ownership and data handling separately.**  
   Ownership of deliverables does not automatically answer who may store, reuse, or delete underlying data.

5. **Include retention, deletion, and incident notice expectations.**  
   These are operational rules, not optional niceties.

6. **Prepare short, truthful questionnaire answers.**  
   Enterprise buyers often want concise, consistent responses. Write them before the pressure starts.

7. **Keep a refusal list.**  
   Know which claims you will not make, such as "we certify HIPAA compliance" or "we have SOC 2" when that is not true.

---

## Concepts

### Data Handling Clause

Contract language that explains how client data may be used, stored, shared, retained, and deleted.

### Output Ownership Clause

Language that defines who owns deliverables, drafts, templates, or system artifacts created during the engagement.

### Security Questionnaire Response Sheet

A short internal document with accurate, reusable answers to common client questions.

Examples:

- Where is client data stored?
- Which vendors are used?
- How is access controlled?
- What happens if there is an incident?
- Do you hold a SOC 2 report?

### Overstatement Risk

The risk created when you describe controls or compliance status more strongly than the facts support.

Examples:

- claiming a certification you do not hold
- implying HIPAA compliance without the right contracts and controls
- using "enterprise-grade security" with no defined meaning

### Refusal List

A short list of statements you will not make because they are inaccurate or unsafe.

---

## Data Handling Clause Pack Template

Use this structure:

```markdown
# Data Handling Clause Pack: [Engagement Type]

## 1. Approved Use of Client Data

Plain-English description of what the consultant may do with client data.

## 2. Prohibited Use of Client Data

What the consultant will not do.

## 3. Tools and Vendors

Which categories of tools may be used, and which require client approval first.

## 4. Storage, Logging, and Access

How working data is stored, logged, and access-controlled.

## 5. Retention and Deletion

How long working copies may exist and what triggers deletion.

## 6. Incident Notice

How the consultant will notify the client if a material data handling problem is discovered.

## 7. Output Ownership

Who owns the engagement deliverables and under what conditions.

## 8. Client Responsibilities

What the client must provide or approve.

## 9. Security Questionnaire Notes

Short answers to common IT or procurement questions.

## 10. Statements We Will Not Make

- ...
```

---

## Walkthrough

Use the pediatric therapy clinic example again.

The consultant needs language that is useful and honest.

### Step 1 - Write the approved-use boundary

Weak clause:

> Consultant may use AI tools as needed.

Better clause:

> Consultant may use approved tools and workflows to analyze, summarize, and transform client-provided information only for the purpose of performing the agreed services. Consultant will not use client data for unrelated product development, model training, or marketing unless separately agreed in writing.

This is plain language. It sets the boundary.

### Step 2 - Write the prohibited-use clause

Example:

> Consultant will not place passwords, access credentials, full payment card details, or other clearly restricted secrets into AI tools. Consultant will not use non-approved tools or vendors for data covered by the agreed restrictions. Consultant will not share client data outside the engagement team except as authorized in writing or required by law.

### Step 3 - Add retention and deletion language

Example:

> Consultant will keep working copies of client data only as long as reasonably necessary to perform the services, troubleshoot issues, meet agreed documentation obligations, or comply with law. Unless otherwise agreed in writing, consultant will delete temporary working copies after completion of the engagement and will not retain raw client records in personal archives.

This answers an operational question the client will care about later.

### Step 4 - Add incident notice language

Example:

> If consultant becomes aware of a material unauthorized access, disclosure, or handling error involving client data used for the engagement, consultant will notify the client promptly, share the known facts available at that time, and cooperate in reasonable containment and follow-up steps.

This is more useful than vague reassurance.

### Step 5 - Write truthful questionnaire answers

Example response sheet:

```markdown
## Security Questionnaire Notes

- Do you have a SOC 2 report?
  - Answer: Provide the accurate status only. If none, say none. Do not imply otherwise.

- Do you process regulated health information?
  - Answer: Only if the agreed workflow requires it and the necessary approvals and contracts are in place.

- Which AI vendors do you use?
  - Answer: List only the approved vendors relevant to the engagement.

- How do you handle retention?
  - Answer: State the actual working-retention and deletion rule used for the engagement.

- What is your incident response approach?
  - Answer: State the actual notice and containment process; do not promise legal conclusions.
```

### Step 6 - Keep a refusal list

For this kind of work, a refusal list may include:

- "We certify HIPAA compliance."
- "We are SOC 2 certified."
- "No client data ever leaves our systems," unless that is actually true.
- "The workflow is fully compliant with the EU AI Act," unless qualified counsel and controls support that claim.

The rule is:

> Do not let sales pressure create factual debt.

---

## Practice

Create a data handling clause pack and security questionnaire response sheet for one consulting engagement.

Include:

1. approved use of client data
2. prohibited use of client data
3. tools and vendor approval language
4. storage, logging, and access language
5. retention and deletion language
6. incident notice language
7. output ownership language
8. client responsibilities
9. five short questionnaire answers
10. five statements you will not make

Keep the clause pack in plain English. If a clause sounds like marketing or fake legal certainty, rewrite it.

---

## Review Checklist

Your clause pack is acceptable when:

- It defines approved and prohibited use clearly.
- Vendor approval is addressed explicitly.
- Retention and deletion are covered.
- Incident notice language exists.
- Output ownership is addressed separately from data handling.
- Questionnaire answers are short and truthful.
- The document avoids claims you cannot support.
- The refusal list is concrete.
- A client or counsel could review it without decoding jargon.

---

## Common Failure Modes

- **Compliance theater:** The language promises legal or audit status the consultant cannot prove.
- **No prohibited uses:** The contract says what work is allowed but not what is off-limits.
- **No retention rule:** Working copies accumulate by habit.
- **Questionnaire improvisation:** Answers change based on who asks.
- **SOC 2 confusion:** A report about controls is described like a universal permission slip.
- **No refusal list:** Pressure leads to inaccurate promises.

---

## Portfolio Evidence

Save:

- The data handling clause pack
- The security questionnaire response sheet
- The refusal list

This shows that you can set safe client boundaries before procurement and legal pressure arrive.

---

## References

- HHS, Business Associates: https://www.hhs.gov/hipaa/for-professionals/privacy/guidance/business-associates/index.html
- AICPA, SOC 2 - SOC for Service Organizations: Trust Services Criteria: https://www.aicpa-cima.com/topic/audit-assurance/audit-and-assurance-greater-than-soc-2
- NIST Privacy Framework: https://www.nist.gov/privacy-framework
- European Commission, AI Act: https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai
