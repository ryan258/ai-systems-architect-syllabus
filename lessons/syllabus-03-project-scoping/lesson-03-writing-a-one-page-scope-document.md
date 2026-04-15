# Lesson 03.03: Writing a One-Page Scope Document

**Parent syllabus:** [Syllabus 03: Project Scoping](../../syllabus-03-project-scoping.md)  
**Estimated time:** 2-3 hours  
**Artifact:** One-page AI project scope document

---

## Outcome

By the end of this lesson, you can turn discovery notes into a clear one-page scope document for an AI workflow project.

You will produce a scope document that names deliverables, exclusions, timeline, revision policy, assumptions, dependencies, and acceptance criteria.

---

## Why This Matters

Discovery without a written scope still leaves too much room for interpretation.

Common failures:

- The client remembers the conversation differently.
- The deliverable sounds impressive but cannot be verified.
- The project includes hidden setup, cleanup, support, and training work.
- Revisions become unlimited.
- Timeline slips because the client did not provide access or feedback.
- The scope does not mention data handling or human review.

A scope document is not a full legal contract. It is the operating agreement for the project. It should be plain enough for the client and precise enough to protect the work.

---

## Best-Practice Principles

1. **Use the client's language for the problem.**  
   The scope should reflect what the client is trying to improve, not just what you plan to build.

2. **Make deliverables concrete.**  
   Name the files, workflows, prompts, diagrams, demos, or handoff docs the client will receive.

3. **Limit phase one.**  
   A smaller complete phase beats a broad vague promise.

4. **Include exclusions.**  
   Exclusions are part of professional clarity.

5. **Tie timeline to dependencies.**  
   Your timeline should assume timely client access, feedback, and decisions.

6. **Define revision rules.**  
   Revisions should improve the agreed deliverable, not create new deliverables.

7. **Add AI-specific boundaries.**  
   Mention review gates, data limits, production status, model limitations, and maintenance assumptions.

---

## Concepts

### Scope Document

A short written agreement that defines what a project includes and excludes.

It should answer:

- What problem are we solving?
- What will be delivered?
- What is not included?
- What does done mean?
- What does the client need to provide?
- What happens if something changes?

### Deliverable

The concrete output the client receives.

Examples:

- Prompt library
- Workflow handoff guide
- Prototype app
- Architecture diagram
- Eval checklist
- Loom-style demo recording
- Data inventory
- Runbook

### Revision

A change to improve an agreed deliverable within the original scope.

A revision is not:

- A new integration
- A new user role
- A new workflow
- Production hardening
- Ongoing support
- Training for additional teams

### Assumption

Something the scope depends on being true.

Example:

> Client will provide up to 20 anonymized sample documents before the project start date.

---

## One-Page Scope Template

Use this as a practical starting point:

```markdown
# Project Scope: [Project Name]

## 1. Project Goal

In plain language, what problem will phase one solve?

## 2. Phase-One Deliverables

This project includes:

- Deliverable 1:
- Deliverable 2:
- Deliverable 3:

## 3. Not Included

This project does not include:

- ...

## 4. Inputs and Client Responsibilities

The client will provide:

- ...

## 5. Timeline

Estimated timeline:

- Start:
- Draft delivery:
- Review window:
- Final delivery:

Timeline assumes timely client access, feedback, and decisions.

## 6. Review and Revision Policy

Includes [number] revision cycle(s) focused on the agreed deliverables.

Requests outside this scope will be handled as a change request or future phase.

## 7. AI, Data, and Review Boundaries

- Data used:
- Data excluded:
- Human review required before:
- Production status:
- Maintenance included:

## 8. Acceptance Criteria

The project is complete when:

- ...

## 9. Change Triggers

The scope must be revisited if:

- ...
```

---

## Walkthrough

Project:

> AI workflow for drafting sales follow-up emails from call notes.

Weak deliverable:

> Build an AI follow-up assistant.

Better deliverables:

- One prompt chain that converts structured call notes into a draft follow-up email.
- One review checklist for accuracy, tone, missing facts, and unsupported promises.
- One operator handoff guide explaining inputs, steps, review points, and failure handling.
- One demo recording showing the workflow on anonymized sample notes.

Weak exclusion:

> Integrations are not included.

Better exclusions:

- No CRM integration in phase one.
- No automatic email sending.
- No live customer data processing.
- No deliverability setup.
- No ongoing prompt monitoring after handoff.
- No support for more than one sales follow-up template.

Weak acceptance criteria:

> The workflow is done when the client likes it.

Better acceptance criteria:

> The project is complete when the workflow can take an approved sample call note, produce a draft follow-up email using the agreed template, flag missing facts, and be run by the client's operator using the handoff guide without live assistance.

---

## Practice

Write a one-page scope document for one project:

1. AI support ticket reply drafter
2. AI proposal drafting workflow
3. AI meeting notes to project plan workflow
4. AI internal knowledge base search prototype
5. Your own real project

Keep it under one page if possible.

Required sections:

- Project goal
- Deliverables
- Not included
- Client responsibilities
- Timeline
- Revision policy
- AI/data/review boundaries
- Acceptance criteria
- Change triggers

---

## Review Checklist

Your scope document is acceptable when:

- A non-technical client can understand it.
- Each deliverable is concrete.
- Exclusions are specific enough to prevent assumptions.
- Client responsibilities are named.
- Timeline depends on client access and feedback.
- Revision policy is clear.
- AI data and review boundaries are explicit.
- Acceptance criteria prove completion.
- Change triggers are named.

---

## Common Failure Modes

- **Deliverables are nouns, not artifacts:** "AI assistant" is not enough.
- **No exclusions:** The client fills the blank space with expectations.
- **No client responsibilities:** Delays look like your fault.
- **Unlimited revision language:** The project cannot close cleanly.
- **No production boundary:** A prototype is mistaken for a deployed system.
- **No human review boundary:** The client assumes automation can act independently.
- **Too long:** The scope becomes too dense to use.

---

## Portfolio Evidence

Save:

- One-page scope document
- Before-and-after examples of vague versus precise scope language
- Revision policy
- Acceptance criteria

This becomes a reusable client-facing template.

---

## References

- PlainLanguage.gov guidelines: https://www.plainlanguage.gov/guidelines/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
