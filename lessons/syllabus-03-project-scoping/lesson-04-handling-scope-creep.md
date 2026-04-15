# Lesson 03.04: Handling Scope Creep When It Happens

**Parent syllabus:** [Syllabus 03: Project Scoping](../../syllabus-03-project-scoping.md)  
**Estimated time:** 2 hours  
**Artifact:** Scope change response playbook

---

## Outcome

By the end of this lesson, you can respond to new client requests without sounding defensive, losing control of the project, or doing unpaid work by accident.

You will produce a scope change response playbook with decision rules and scripts.

---

## Why This Matters

Even good scope documents do not prevent every change.

Clients learn during a project. New stakeholders appear. Demo feedback creates new ideas. Technical constraints surface. In AI projects, people often see a prototype and immediately imagine production deployment, more data sources, more users, and deeper integrations.

Common failures:

- You say yes too quickly.
- You say no too bluntly.
- You do extra work to avoid discomfort.
- You treat every request as a problem instead of a decision.
- You do not distinguish a revision from a new deliverable.
- You never document the agreed change.

Handling scope creep is not about being rigid. It is about making changes explicit.

---

## Best-Practice Principles

1. **Acknowledge the request before deciding.**  
   Clients need to feel heard before they can accept a boundary.

2. **Return to the agreed scope.**  
   Use the scope document as the neutral reference point.

3. **Offer choices.**  
   Good options are usually defer, swap, reprice, or create a new phase.

4. **Do not price in the moment if details are unclear.**  
   Pause, assess impact, and respond in writing.

5. **Protect the current deliverable.**  
   New ideas should not destabilize a nearly complete phase.

6. **Document decisions.**  
   If scope changes, write down what changed, what it replaces, and how timeline or cost changes.

7. **Separate relationship tone from project boundary.**  
   You can be warm and firm at the same time.

---

## Concepts

### Revision

Improvement to an agreed deliverable.

Example:

> Adjust the email draft prompt to use a warmer tone within the agreed sales follow-up workflow.

### Scope Change

A request that changes deliverables, users, integrations, data, timeline, risk, or support obligations.

Example:

> Connect the workflow to HubSpot and send the follow-up automatically.

### Swap

Removing one scoped item and replacing it with another of similar effort and risk.

### New Phase

A separate follow-on project that starts after the current phase is accepted.

### Change Request

A written update that defines the requested change and its effect on cost, timeline, and deliverables.

---

## Walkthrough

Scenario:

> You are building a phase-one AI workflow that drafts sales follow-up emails from structured call notes. The agreed deliverables are a prompt chain, review checklist, operator guide, and demo recording. The scope excludes CRM integration and automatic email sending.

The client sees the first demo and says:

> This is great. Could you also connect it to HubSpot and have it create the follow-up task automatically?

### Step 1 - Acknowledge the value

Do not start with a hard no.

Better:

> That is a useful next step. I can see why connecting it to HubSpot would make the workflow more valuable for the team.

### Step 2 - Return to the scope

Use the written scope as the neutral reference.

> The current phase covers the workflow, prompts, review checklist, handoff guide, and demo. HubSpot integration was excluded from phase one.

### Step 3 - Explain the impact

Name the new work without exaggerating.

> Adding HubSpot would introduce access setup, permissions, field mapping, testing, error handling, and a decision about whether the AI can write into the CRM directly.

### Step 4 - Offer choices

Give the client a path forward.

> We have three options: keep this as a phase-two item, swap out the demo recording and part of the handoff work for a limited integration design, or write a separate change request for HubSpot implementation.

### Step 5 - Require written approval

Do not start new work based on a live conversation.

> I will send the options in writing with timeline and cost impact. Once you approve one, I can proceed.

This response protects the relationship and the project boundary at the same time.

---

## Decision Rules

Use these rules when a client asks for something new:

1. **Is it already in scope?**  
   If yes, handle it as planned work.

2. **Is it a revision to an existing deliverable?**  
   If yes, count it against the revision policy.

3. **Does it add a new deliverable, user group, data source, integration, risk, or support need?**  
   If yes, it is a scope change.

4. **Can it replace something already scoped without increasing risk?**  
   If yes, offer a swap.

5. **Would it distract from finishing phase one?**  
   If yes, defer it to a later phase.

6. **Does it require production reliability, security, compliance, or support beyond the current project?**  
   If yes, scope a separate project.

---

## Script Library

### When the request is a good idea but out of scope

> That is a useful idea, and I agree it may be worth doing. It is outside the phase-one scope we agreed on, so I would treat it as either a change request or a phase-two item. I can outline the impact on timeline and cost before we decide.

### When the client asks for "one small thing"

> It may be small, but I want to check the impact before I commit because it touches the project boundary. Let me compare it against the current scope and come back with whether it fits, needs a swap, or should be handled separately.

### When the request would destabilize the current phase

> I recommend we finish and accept the current phase first. This request changes the shape of the system, and adding it now would make it harder to verify the original deliverable cleanly.

### When the client wants an integration

> The current scope covers the workflow and handoff artifacts, not a live integration. We can add integration work as a separate phase after the workflow is approved, because it introduces access, security, testing, and support requirements.

### When the client asks for extra revisions

> The included revision cycle has been used. I can make additional revisions at the agreed hourly rate or package them into a fixed change request if you want a defined cap.

### When the client asks for production deployment after a prototype

> The prototype shows the workflow pattern, but production deployment is a separate scope. It requires reliability checks, data handling decisions, access controls, monitoring, and a support plan.

---

## Change Request Template

```markdown
# Change Request: [Project Name]

## Requested Change

What is being requested?

## Why It Matters

What client need does this address?

## Impact

- Deliverables:
- Timeline:
- Cost:
- Data/security:
- Testing/review:
- Maintenance/support:

## Options

1. Defer to phase two.
2. Swap for an existing scoped item.
3. Add as paid change request.
4. Decline because it conflicts with the project goal.

## Recommendation

...

## Approval

Work begins only after written approval.
```

---

## Practice

Create your response playbook.

Include:

1. Your decision rules.
2. Six scripts from the categories above.
3. A change request template.
4. Three example client requests and how you would classify each one.
5. Your rule for when work pauses until approval.

Use an AI workflow project as the example, such as a support ticket drafter or proposal assistant.

---

## Review Checklist

Your playbook is acceptable when:

- It distinguishes revisions from scope changes.
- It gives the client options.
- It protects the current deliverable.
- It avoids defensive language.
- It requires written approval before extra work begins.
- It accounts for AI-specific risk: integrations, data, automation, review, and production support.
- It can be used without improvising under pressure.

---

## Common Failure Modes

- **Instant yes:** You commit before assessing impact.
- **Instant no:** You damage trust by rejecting without acknowledging value.
- **No written trail:** Everyone remembers the change differently.
- **Revision confusion:** New deliverables are smuggled in as "feedback."
- **Prototype-to-production jump:** A demo becomes an implied deployment commitment.
- **Unpriced support:** You become the maintainer without naming maintenance scope.

---

## Portfolio Evidence

Save:

- Scope change response playbook
- Script library
- Change request template
- Three classified example requests

This becomes a core client-management asset.

---

## References

- PlainLanguage.gov guidelines: https://www.plainlanguage.gov/guidelines/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
