# Lesson 07.03: What Not to Put Into AI Tools

**Parent syllabus:** [Syllabus 07: Data Privacy and Client Safety](../../syllabus-07-data-privacy.md)  
**Estimated time:** 2 hours  
**Artifact:** AI tool exclusion and escalation matrix

---

## Outcome

By the end of this lesson, you can decide what information must stay out of general AI tools, what can enter only through an approved path, and what should trigger immediate escalation.

You will produce an AI tool exclusion and escalation matrix that defines:

- data category
- example content
- default rule
- approved exception path
- escalation owner
- logging and deletion rule

---

## Why This Matters

The easiest privacy mistake is often the simplest one: pasting the wrong thing into the wrong tool.

Common failures:

- Passwords, secrets, or access tokens get dropped into a model prompt for troubleshooting.
- A client sends medical or child data, and someone pastes it into a non-approved AI workspace to move faster.
- Internal contracts, pricing sheets, or board notes are treated as harmless because they are not consumer data.
- A team thinks "we are just summarizing it," ignoring that the summary still contains regulated or confidential information.
- Sensitive data is received by mistake and nobody has a stop rule.
- Exclusion rules exist informally, but no reusable artifact tells the team what to do.

If the workflow cannot say what must not enter the tool, it is not ready for client data.

---

## Best-Practice Principles

1. **Use default deny for high-risk data.**  
   Sensitive categories should stay out unless an approved path is explicit.

2. **Separate "never" from "not in this tool."**  
   Some data should never enter general AI tools. Other data may be handled only through approved, contract-backed, and reviewed paths.

3. **Redact before convenience wins.**  
   If a task can be completed with masked identifiers, summaries, or metadata, do that instead of sending raw records.

4. **Treat accidental receipt as an operating case.**  
   The system needs a rule for what happens when a client sends sensitive data by mistake.

5. **Do not ignore confidential business information.**  
   Privacy risk is not limited to regulated personal data. Client secrets still need boundaries.

6. **Map exclusions to escalation owners.**  
   When a high-risk category appears, someone specific should own the next step.

7. **Keep the matrix visible and reusable.**  
   The artifact should be short enough to use during actual work.

---

## Concepts

### Exclusion Rule

A default rule that a data category may not be entered into a given tool or workflow path.

Examples:

- never paste passwords into any AI tool
- never send insurance member IDs to the drafting model
- never store raw diagnosis text in logs

### Approved Exception Path

A documented path where processing may be allowed if certain controls exist.

Examples:

- approved vendor
- written agreement in place
- human review
- masked identifiers
- limited retention

### Escalation

Stopping normal work and routing the case to the right owner.

Examples:

- privacy or compliance contact
- client legal contact
- workflow owner
- security owner

### Confidential Business Data

Non-public information that can hurt the client if exposed, even if it is not regulated personal data.

Examples:

- pricing strategy
- acquisition plans
- customer churn reports
- internal incident postmortems

### Accidental Receipt Protocol

The rule for what to do when a client sends sensitive data unexpectedly.

Usually:

1. stop
2. do not paste it into the model
3. move it only through the approved path
4. notify the right owner

---

## AI Tool Exclusion and Escalation Matrix Template

Use this structure:

```markdown
# AI Tool Exclusion and Escalation Matrix: [Workflow or Practice Name]

| Data category | Example | Default rule | Approved exception path | Escalation owner | Logging and deletion rule |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

## Accidental Receipt Protocol

- Step 1:
- Step 2:
- Step 3:
- Step 4:

## Open Questions

- ...

## Decision

What data categories are fully excluded from this workflow?
```

---

## Walkthrough

Use the pediatric therapy clinic workflow again.

The clinic sends a sample intake export that contains:

- parent and child names
- diagnosis notes
- insurance member IDs
- therapist comments

### Step 1 - Classify the categories fast

For this workflow:

- diagnosis notes: high-risk health information
- insurance member IDs: high-risk financial and health-linked data
- child names: child data and personal data
- therapist comments: potentially sensitive internal notes

The right reaction is not:

> Let me paste it into the model and see if the summary looks good.

The right reaction is:

> Which of these fields are actually needed, and which are excluded from the model path entirely?

### Step 2 - Write default rules

Examples:

- passwords, secrets, and tokens: never
- insurance member IDs: excluded from the model path
- diagnosis notes: only through an approved path and only if the workflow truly requires them
- child identity: minimize, mask, or replace with internal IDs if possible
- therapist comments: no logs, no screenshots, no public AI workspace

### Step 3 - Define the accidental receipt rule

Suppose a client emails a spreadsheet with more data than expected.

Operational rule:

1. stop normal processing
2. do not upload or paste the file into a model
3. confirm the approved handling path
4. notify the client contact or privacy owner if the path is unclear
5. document what happened

That is faster than cleaning up an avoidable incident later.

### Step 4 - Fill the matrix

Example:

```markdown
# AI Tool Exclusion and Escalation Matrix: Pediatric Therapy Intake Workflow

| Data category | Example | Default rule | Approved exception path | Escalation owner | Logging and deletion rule |
| --- | --- | --- | --- | --- | --- |
| Passwords and secrets | Staff credentials, API keys | Never enter any AI tool | None | Security owner | Never log; rotate if exposed |
| Insurance details | Member ID, policy number | Excluded from workflow | None in phase one | Workflow owner | Never log; delete from working copies |
| Diagnoses and care concerns | Intake note text | Do not use in generic AI tools | Only approved clinic workflow with confirmed controls | Privacy or compliance contact | Do not log raw text; delete temporary copies |
| Child personal data | Child name, birth date | Minimize and mask where possible | Approved clinic workflow only | Clinic operations owner | No screenshots or sample-case exports with identifiers |
| Confidential business notes | Therapist staffing notes | Do not use outside approved systems | Only approved internal path | Clinic operations owner | Do not log or share externally |

## Accidental Receipt Protocol

- Step 1: Stop processing
- Step 2: Do not paste or upload the file to a model
- Step 3: Confirm approved path and minimum needed fields
- Step 4: Notify the client owner if the path is unclear
```

### Step 5 - Use the matrix to narrow scope

The practical outcome here may be:

> Phase one can summarize scheduling and contact workflow only. Diagnosis and insurance details stay out of the model path until approvals, controls, and actual need are confirmed.

That is not overcautious. That is good scope control.

---

## Practice

Choose one workflow and create an AI tool exclusion and escalation matrix with at least ten rows.

Include categories for:

1. secrets or credentials
2. regulated or sensitive personal data
3. health data
4. child data
5. confidential business data
6. one category specific to your workflow

For each row, define:

- example data
- default rule
- approved exception path
- escalation owner
- logging and deletion rule

Add an accidental receipt protocol with at least four steps.

---

## Review Checklist

Your exclusion matrix is acceptable when:

- High-risk categories use default deny or explicit exclusion.
- The matrix distinguishes "never" from "approved exception only."
- At least one category is fully excluded from the model path.
- Accidental receipt has a clear stop rule.
- An escalation owner is named for each sensitive category.
- Logging and deletion rules are specific.
- Confidential business data is included, not only personal data.
- The matrix changes scope or workflow behavior in a concrete way.

---

## Common Failure Modes

- **Convenience override:** Sensitive fields are sent because masking feels slower.
- **All-or-nothing thinking:** The team either bans everything or allows everything instead of defining approved exception paths.
- **No accidental receipt rule:** People improvise when unexpected sensitive data arrives.
- **Confidentiality blind spot:** Only regulated personal data is protected; business secrets are ignored.
- **No owner:** The matrix names risks but not who decides the next step.
- **No scope change:** The artifact exists, but excluded categories still leak into the workflow through habit.

---

## Portfolio Evidence

Save:

- The AI tool exclusion and escalation matrix
- The accidental receipt protocol
- One short note explaining which category most changed the workflow scope

This shows that you can say no to unsafe data flows in concrete terms.

---

## References

- HHS, What is PHI?: https://www.hhs.gov/answers/hipaa/what-is-phi/index.html
- FTC, Children's Online Privacy Protection Rule: https://www.ftc.gov/legal-library/browse/rules/childrens-online-privacy-protection-rule-coppa
- NIST Privacy Framework: https://www.nist.gov/privacy-framework
- HHS, Business Associates: https://www.hhs.gov/hipaa/for-professionals/privacy/guidance/business-associates/index.html
