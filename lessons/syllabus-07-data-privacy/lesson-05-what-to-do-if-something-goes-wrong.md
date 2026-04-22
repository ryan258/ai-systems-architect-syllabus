# Lesson 07.05: What to Do If Something Goes Wrong

**Parent syllabus:** [Syllabus 07: Data Privacy and Client Safety](../../syllabus-07-data-privacy.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Data incident response packet

---

## Outcome

By the end of this lesson, you can respond to a client data handling mistake in a controlled, useful way instead of freezing, improvising, or making the situation worse.

You will produce a data incident response packet that includes:

- incident summary
- containment steps
- evidence log
- notification draft
- escalation matrix
- follow-up actions

---

## Why This Matters

The worst time to invent an incident process is during the incident.

Common failures:

- The consultant notices the mistake, hesitates, and loses time that should be spent on containment.
- Someone deletes evidence before the client or operator can reconstruct what happened.
- The first client message contains speculation, promises, or legal conclusions nobody can support.
- The workflow keeps running after a bad data path is discovered.
- Health data or child data may be involved, but no one knows who should assess reporting obligations.
- The team "fixes" the system but never records the root cause or prevention steps.

Good incident response is not calm because the event is small. It is calm because the steps are already written down.

This lesson is not legal advice. Use it to know when to notify, contain, document, and escalate to qualified counsel or compliance professionals.

---

## Best-Practice Principles

1. **Contain first.**  
   Stop the unsafe data path before you optimize the explanation.

2. **Preserve evidence.**  
   Save timestamps, trace IDs, tool names, screenshots, and access details needed to reconstruct the event.

3. **Notify with known facts only.**  
   Explain what is known, what is not yet known, and what you are doing next. Do not guess at legal outcomes.

4. **Pause risky automation.**  
   If the workflow could repeat the same mistake, disable or narrow it immediately.

5. **Route regulated questions to the right owner.**  
   If PHI, child data, enterprise customer commitments, or jurisdiction-specific rules may be involved, escalate early.

6. **Record the timeline.**  
   A written event log reduces confusion and supports later review.

7. **Turn the incident into a control change.**  
   A response is incomplete if the workflow remains able to fail the same way tomorrow.

---

## Concepts

### Security Incident

An event that may affect confidentiality, integrity, or availability.

Examples:

- unauthorized access
- wrong-recipient disclosure
- exposed credentials
- unsafe prompt logging

### Privacy Incident

An event involving inappropriate handling, disclosure, or retention of personal or sensitive data.

Examples:

- uploading client records to a non-approved tool
- storing raw records longer than agreed
- exposing identifying information in a dashboard

### Breach Assessment

The process of deciding whether the incident triggered legal, contractual, or reporting obligations.

You may contribute facts to this assessment, but you should not pretend to be the final legal authority unless that is your role.

### Containment

The steps taken immediately to stop further harm.

Examples:

- disable a workflow step
- rotate exposed credentials
- revoke access
- delete temporary files where appropriate
- stop new uploads

### Evidence Log

A structured record of what happened, when it happened, what systems were involved, and what actions were taken.

### First Notice

The initial communication to the client or internal owner.

It should be:

- prompt
- factual
- narrow
- action-oriented

---

## Data Incident Response Packet Template

Use this structure:

```markdown
# Data Incident Response Packet: [Incident Name]

## 1. Incident Summary

- Date and time discovered:
- Reporter:
- Workflow or system:
- Short description:

## 2. Containment Actions

- What was stopped immediately:
- What access was restricted:
- What workflow paths were paused:

## 3. Known Facts

- Data categories involved:
- Systems or tools involved:
- Approximate scope:
- Current uncertainty:

## 4. Evidence Log

- Trace IDs:
- File names or record IDs:
- Relevant screenshots or logs:
- Timeline of actions:

## 5. Notifications and Escalations

- Client contact:
- Internal owner:
- Legal or compliance review needed:
- Other stakeholders:

## 6. Draft First Notice

Short factual message with known facts and next steps.

## 7. Recovery and Follow-Up

- Immediate recovery actions:
- Required control changes:
- Review date:

## 8. Decision Log

- Workflow can resume?
- Conditions for resume:
- Who approves resume:
```

---

## Walkthrough

Use this example:

> While testing the pediatric therapy clinic workflow, a consultant uploads a CSV containing child names, diagnoses, and insurance member IDs into a non-approved AI workspace.

### Step 1 - Contain the problem

Immediate actions:

- stop further uploads
- pause the workflow test
- document which workspace was used
- identify whether sharing, retention, or training settings were involved
- remove local temporary copies if that is part of the approved containment path

Do not keep experimenting while you are still figuring out exposure.

### Step 2 - Preserve evidence

Capture:

- time of upload
- file name or record set
- account used
- workspace or vendor used
- screenshots of settings and event status
- any generated outputs or logs

The evidence log supports the client and any later professional assessment.

### Step 3 - Write a factual first notice

Weak notice:

> We had a breach and everything is probably exposed.

Also weak:

> It is fine now.

Better notice:

> We discovered that a test file containing client intake data was uploaded into a non-approved AI workspace during workflow testing at approximately 2:15 PM. We immediately stopped the test, preserved the relevant records, and are reviewing the exact data categories and tool settings involved. We will send an updated factual summary after we complete the initial assessment and coordinate with your designated contact.

That is prompt, factual, and useful.

### Step 4 - Escalate the right questions

For this case:

- if PHI may be involved, the client's HIPAA decision-makers or counsel may need to assess next steps
- if child data is involved, the client may want privacy or legal review even if COPPA is not the primary issue here
- if enterprise customer commitments exist, the client may need to assess notice requirements under contract
- if the workflow falls into a regulated AI deployment context, the client may need to assess any additional governance or incident obligations

Your role is to route the decision quickly, not improvise legal certainty.

### Step 5 - Decide whether the workflow resumes

Do not resume because the file is deleted locally.

Resume only when:

- containment is complete
- the client has the first factual notice
- the workflow path is narrowed or fixed
- the responsible owner approves restart

Example:

> Resume only with synthetic data and with the insurance fields excluded until the client confirms the approved vendor and review path.

### Step 6 - Turn the incident into a prevention change

Required follow-up might include:

- updating the exclusion matrix
- adding upload guardrails
- restricting account access
- adding a warning banner before tool use
- updating the intake checklist to confirm approved workspaces earlier

If none of those change, the response was incomplete.

---

## Practice

Create a data incident response packet for one plausible workflow mistake.

Your packet must include:

1. incident summary
2. containment actions
3. known facts and uncertainties
4. evidence log
5. client and internal escalation list
6. a first-notice draft
7. recovery and follow-up actions
8. resume or hold decision

Use an incident involving one of these:

- wrong tool
- wrong recipient
- exposed log data
- unsafe prompt or file upload

Add a short line stating when legal, privacy, or compliance review would be required.

---

## Review Checklist

Your incident response packet is acceptable when:

- Containment actions come before explanation.
- Known facts and uncertainties are separated.
- Evidence preservation is explicit.
- The first-notice draft avoids speculation.
- Escalation owners are named.
- The workflow resume decision is clear.
- At least one prevention change is recorded.
- The packet would help a future maintainer understand the event quickly.

---

## Common Failure Modes

- **Panic deletion:** Evidence is destroyed before the event can be understood.
- **Speculative notice:** The first message makes legal claims or guesses.
- **Workflow keeps running:** The same unsafe path stays active during review.
- **No owner:** Nobody knows who decides whether work can resume.
- **No prevention change:** The incident is documented but not used to improve the system.
- **Silence delay:** The client is not notified promptly because the story is not perfect yet.

---

## Portfolio Evidence

Save:

- The data incident response packet
- The first-notice draft
- One short note describing the prevention change triggered by the incident

This shows that you can respond under pressure without making the system less safe.

---

## References

- HHS, Breach Notification Rule: https://www.hhs.gov/hipaa/for-professionals/breach-notification/index.html
- HHS, Business Associates: https://www.hhs.gov/hipaa/for-professionals/privacy/guidance/business-associates/index.html
- NIST Privacy Framework: https://www.nist.gov/privacy-framework
- European Commission, AI Act: https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai
