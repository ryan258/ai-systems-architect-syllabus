# Lesson 03.01: What Scoping Is and Why It Matters

**Parent syllabus:** [Syllabus 03: Project Scoping](../../syllabus-03-project-scoping.md)  
**Estimated time:** 2 hours  
**Artifact:** Scope creep diagnosis sheet

---

## Outcome

By the end of this lesson, you can explain project scope in plain language, identify early scope creep signals, and separate client value from uncontrolled extra work.

You will produce a scope creep diagnosis sheet for an AI workflow consulting project.

---

## Why This Matters

Scope creep destroys margin quietly.

It usually does not begin with a bad client. It begins with unclear boundaries, vague deliverables, missing decision rules, and a consultant who wants to be helpful.

Common failures:

- The client asks for "one small thing" repeatedly.
- The deliverable was never defined tightly enough to know when work is done.
- The client changes the target user after work begins.
- A demo turns into implementation support.
- "AI workflow" becomes prompts, integrations, training, dashboarding, governance, and maintenance.
- Extra review cycles are treated as included by default.

Project scoping protects both sides. The client gets clarity. You get a boundary around time, energy, risk, and payment.

---

## Best-Practice Principles

1. **Define done before work starts.**  
   A project without acceptance criteria cannot be finished cleanly.

2. **Separate problem, deliverable, and implementation.**  
   The problem is the business need. The deliverable is what you hand over. The implementation is how you create it.

3. **State exclusions explicitly.**  
   "Not included" is not negative. It is how you prevent assumptions from becoming unpaid work.

4. **Name assumptions and dependencies.**  
   If the client must provide access, data, decisions, or feedback, those items belong in scope.

5. **Treat changes as a normal process.**  
   Scope changes should trigger a decision: defer, swap, reprice, or write a new phase.

6. **Use plain language.**  
   A scope document is not useful if only technical people can understand it.

7. **Protect reliability and safety.**  
   In AI projects, unclear scope can create unsafe automation, missing review gates, hidden data handling, or unsupported production promises.

---

## Concepts

### Scope

The agreed boundary of the project.

Scope defines:

- What problem will be addressed
- What deliverables will be produced
- What work is included
- What work is excluded
- What timeline and review process apply
- What conditions must be true for the work to proceed

### Scope Creep

Work expanding beyond the original agreement without a matching change in time, budget, or deliverables.

Scope creep can be caused by:

- Ambiguous deliverables
- Missing exclusions
- Unclear revision limits
- New stakeholders joining late
- Client uncertainty
- Consultant over-accommodation
- AI system complexity being underestimated

### Acceptance Criteria

The conditions that prove a deliverable is complete.

Example:

> The handoff guide is complete when a non-technical operator can run the workflow using the documented inputs, prompts, review checks, and stop conditions.

### Change Trigger

A request that should pause the current agreement and require a decision.

Examples:

- New user group
- New integration
- New data source
- New compliance requirement
- Production deployment request
- Additional revision cycle
- Additional training session

---

## Walkthrough

Scenario:

> A client asks you to build an AI workflow that turns customer support tickets into draft replies.

Weak scope:

> Build an AI support workflow for the team.

This is too broad. It does not say what will be delivered, what systems are involved, who reviews output, whether the workflow sends messages automatically, how many ticket types are included, or what happens after handoff.

Stronger scope:

> Create a prototype workflow that takes one exported support ticket category, drafts a suggested reply, flags missing information, and produces a human-review checklist. The workflow will be delivered as reusable prompts, a one-page operator guide, and a demo recording. It will not connect directly to Zendesk, send customer messages, process live customer data, or include production deployment.

### What changed?

| Scope element | Weak version | Stronger version |
| --- | --- | --- |
| Problem | Vague support improvement | Draft replies for one ticket category |
| Deliverable | "AI workflow" | Prompts, operator guide, demo recording |
| Data boundary | Undefined | One exported ticket category |
| Automation boundary | Undefined | Human review required |
| Exclusions | Missing | No live integration, sending, or deployment |
| Done | Undefined | Prototype and handoff artifacts |

### Diagnosis

The strong version prevents three common failures:

1. The client cannot assume live Zendesk integration is included.
2. The client cannot assume the AI sends replies automatically.
3. You can finish the project without becoming the team's ongoing support engineer.

---

## Practice

Create a scope creep diagnosis sheet for this project:

> A small business wants "an AI assistant that helps us with sales follow-up."

Write:

1. The likely client expectation.
2. Five hidden work items that could appear.
3. Five questions you must ask before agreeing.
4. Three things you would include in phase one.
5. Five things you would explicitly exclude.
6. The acceptance criteria for phase one.
7. Three change triggers that require repricing or a new phase.

Use this format:

```markdown
# Scope Creep Diagnosis: [Project Name]

## Initial Client Request

...

## Hidden Work Items

- ...

## Required Discovery Questions

- ...

## Phase One Includes

- ...

## Phase One Does Not Include

- ...

## Acceptance Criteria

The project is complete when...

## Change Triggers

- ...
```

---

## Review Checklist

Your diagnosis sheet is acceptable when:

- It separates the client request from the actual work required.
- It identifies hidden integrations, data, approvals, and handoff needs.
- It includes exclusions that protect time and risk.
- It defines phase-one acceptance criteria.
- It names change triggers.
- It avoids legalistic language.
- It could be reused during a real discovery call.

---

## Common Failure Modes

- **Confusing helpfulness with scope:** You keep adding value without changing the agreement.
- **No definition of done:** The project keeps moving because completion was never defined.
- **Only listing included work:** Missing exclusions leave the client to infer the rest.
- **Ignoring operational work:** Deployment, training, support, monitoring, and revisions appear late.
- **Assuming AI makes scope simpler:** AI projects often need more boundaries because outputs require review and governance.
- **Letting demos become commitments:** A prototype demo is not a production system.

---

## Portfolio Evidence

Save:

- Scope creep diagnosis sheet
- Before-and-after scope language
- List of change triggers
- Phase-one acceptance criteria

This becomes evidence that you can control project boundaries before implementation begins.

---

## References

- PlainLanguage.gov guidelines: https://www.plainlanguage.gov/guidelines/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- Project Management Institute, requirements management overview: https://www.pmi.org/learning/library/requirements-management-project-success-9356
