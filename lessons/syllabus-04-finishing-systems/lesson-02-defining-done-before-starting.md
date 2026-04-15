# Lesson 04.02: Defining Done Before Starting

**Parent syllabus:** [Syllabus 04: Finishing Systems](../../syllabus-04-finishing-systems.md)  
**Estimated time:** 2 hours  
**Artifact:** One-sentence done condition and acceptance checklist

---

## Outcome

By the end of this lesson, you can define "done" in concrete, observable terms before starting a project.

You will produce a one-sentence done condition and acceptance checklist for an AI workflow deliverable.

---

## Why This Matters

If done is vague, the project cannot close cleanly.

For AI systems work, vague done conditions create especially sharp risk. A workflow might produce something impressive once but still lack repeatability, review checks, data boundaries, failure handling, and handoff instructions.

Common failures:

- "Done" means "I feel satisfied," which changes with energy and mood.
- "Done" means "the client likes it," but no acceptance criteria exist.
- The deliverable works only on the happy path.
- The system has no stop condition.
- Testing, documentation, and handoff are treated as optional.
- The project keeps absorbing new ideas because no finish line was written.

A done condition turns completion into a testable claim.

---

## Best-Practice Principles

1. **Write done before build.**  
   The finish line should shape the work from the start.

2. **Use observable conditions.**  
   A reviewer should be able to tell whether the condition is met.

3. **Define the user and use case.**  
   Done for whom? Under what conditions?

4. **Include quality gates.**  
   Finished does not mean flawless, but it must meet the agreed standard.

5. **Name exclusions separately.**  
   A done condition should not hide future work inside vague language.

6. **Tie done to handoff.**  
   A client deliverable is not finished until the receiver can use or review it.

7. **Keep it short.**  
   If the done condition cannot fit in one sentence, the project may be too broad.

---

## Concepts

### Done Condition

A one-sentence description of the observable state that means the work is complete.

Formula:

> This is done when [user] can [do useful thing] using [deliverable] under [bounded conditions] with [quality gate].

### Acceptance Checklist

A short checklist used to verify the done condition.

### Quality Gate

A required check before the deliverable can be accepted.

Examples:

- Output follows the agreed format.
- Human review checklist is included.
- No live customer data is required for demo.
- Operator can run the workflow from documentation.
- Known limitations are listed.

### Polished

More refined than necessary for acceptance.

Complete and polished are not the same. Polish is useful only after the acceptance criteria are met.

---

## Walkthrough

Project:

> AI workflow that drafts a project brief from messy discovery notes.

Weak done condition:

> Done when the workflow is good.

This is not observable.

Better done condition:

> This is done when a consultant can paste anonymized discovery notes into the prompt chain and receive a one-page project brief with assumptions, open questions, and out-of-scope items, then verify it using the included review checklist.

### Acceptance checklist

- The prompt chain accepts the agreed input type: anonymized discovery notes.
- The output includes problem, users, deliverables, assumptions, open questions, and exclusions.
- The workflow includes a review checklist.
- The workflow warns when required facts are missing.
- The handoff notes explain when not to use it.
- The demo uses sample data, not live client data.

### What is not required

- Full proposal generation
- Pricing recommendations
- Legal terms
- CRM integration
- Multiple output formats
- Automated client emailing

The done condition makes it easier to cut work without cutting value.

---

## Practice

Choose one project idea.

Write:

1. A weak done condition.
2. A better one-sentence done condition.
3. Five acceptance checklist items.
4. Three things that are not required.
5. One quality gate related to AI output review.
6. One quality gate related to handoff.

Use this template:

```markdown
# Done Definition: [Project Name]

## Weak Done Condition

...

## Strong Done Condition

This is done when...

## Acceptance Checklist

- [ ] ...
- [ ] ...
- [ ] ...

## Not Required for This Version

- ...

## Quality Gates

- AI output review:
- Handoff:

## Completion Decision

Accepted, revise, reduce scope, or defer.
```

---

## Review Checklist

Your done definition is acceptable when:

- It is one sentence.
- It names the user.
- It names the useful action.
- It names the deliverable.
- It includes bounded conditions.
- It includes review or quality criteria.
- The acceptance checklist can be verified.
- The not-required list prevents perfectionism and scope creep.

---

## Common Failure Modes

- **Mood-based done:** The project depends on how you feel about it.
- **Client-vague done:** The client must "like it," but no acceptance criteria exist.
- **Too many use cases:** One done condition tries to cover several projects.
- **No quality gate:** Output exists, but nobody checks whether it is usable.
- **No exclusions:** Future features sneak into the finish line.
- **No handoff test:** You can use it, but nobody else can.

---

## Portfolio Evidence

Save:

- One-sentence done condition
- Acceptance checklist
- Not-required list
- Completion decision

This becomes a reusable finish-line template for future projects.

---

## References

- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- PlainLanguage.gov guidelines: https://www.plainlanguage.gov/guidelines/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
