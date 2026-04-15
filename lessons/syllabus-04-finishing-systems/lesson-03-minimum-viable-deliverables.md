# Lesson 04.03: Minimum Viable Deliverables

**Parent syllabus:** [Syllabus 04: Finishing Systems](../../syllabus-04-finishing-systems.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Minimum viable deliverable cutline map

---

## Outcome

By the end of this lesson, you can design a smaller deliverable that still provides real value.

You will produce a minimum viable deliverable cutline map for an AI workflow project.

---

## Why This Matters

A minimum viable deliverable is not half-finished work.

It is the smallest complete version that solves a bounded problem for a real user. The discipline is knowing what to cut without cutting the core value.

Common failures:

- Cutting quality instead of cutting scope.
- Calling an unfinished prototype an MVD.
- Keeping advanced features while skipping documentation.
- Shipping something useful only to the creator.
- Letting guilt drive overbuilding.
- Failing to communicate boundaries honestly to a client.

MVD thinking lets you finish useful work under real constraints.

---

## Best-Practice Principles

1. **Cut breadth before quality.**  
   Support one user, one input, one workflow, or one output before weakening reliability.

2. **Preserve the core job.**  
   The deliverable must still do the main useful thing.

3. **Keep review and handoff.**  
   Documentation, checks, and limitations are part of a viable deliverable.

4. **Name future phases.**  
   Cutting something is easier when it has a clear parking place.

5. **Avoid fake automation.**  
   If a human must review or perform a step, say so.

6. **Communicate limits plainly.**  
   MVD value depends on honest boundaries.

7. **Do not cut safety gates.**  
   AI output review, data handling limits, and stop conditions are not polish.

---

## Concepts

### Minimum Viable Deliverable

The smallest complete deliverable that provides real value to a defined user under defined conditions.

### Cutline

The boundary between what is included now and what is deferred.

### Core Value

The main useful outcome the deliverable must preserve.

Example:

> Help a consultant turn messy discovery notes into a structured scope draft.

### Deferred Feature

Useful work that is intentionally postponed.

Deferred does not mean forgotten. It means not required for the current finish line.

### Non-Negotiable Quality

The quality bar that cannot be cut without making the deliverable unsafe, misleading, or unusable.

---

## Walkthrough

Project:

> AI assistant for creating client proposal drafts.

Original idea:

- Intake form
- Document upload
- RAG over prior proposals
- Brand voice matching
- Proposal drafting
- Pricing suggestions
- Legal terms
- CRM integration
- Approval workflow
- PDF export
- Usage dashboard

This is not a good first deliverable.

### Define the core value

> Turn structured discovery notes into a first proposal outline that a consultant can review.

### Preserve viability

Must keep:

- One supported input format
- One proposal outline template
- Prompt chain
- Review checklist
- Known limitations
- Handoff notes

Can cut:

- RAG over prior proposals
- Brand voice matching
- Pricing suggestions
- Legal terms
- CRM integration
- PDF export
- Dashboard
- Multi-user workflow

### MVD statement

> Phase one delivers a prompt-based proposal outline workflow for one structured input format. It produces a reviewable proposal outline with assumptions, open questions, and missing information flags. It does not generate final client-ready proposals, pricing, legal terms, or CRM updates.

This is smaller, but still complete.

---

## Cutline Map Template

```markdown
# MVD Cutline Map: [Project Name]

## Core User

Who is this for?

## Core Job

What useful thing must it do?

## Minimum Viable Deliverable

What is the smallest complete version?

## Non-Negotiable Quality

- ...

## Included Now

- ...

## Deferred

- ...

## Explicitly Not Included

- ...

## Future Phase Candidates

- Phase two:
- Phase three:

## Client Explanation

How will you explain the smaller version honestly?
```

---

## Practice

Pick one broad AI project idea.

Create a cutline map that:

1. Names the core user.
2. Names the core job.
3. Defines the MVD.
4. Lists non-negotiable quality gates.
5. Lists included work.
6. Lists deferred work.
7. Lists explicitly excluded work.
8. Writes a client explanation in plain language.

Then ask:

> If I cut everything deferred, does the deliverable still create value?

If no, restore the smallest missing part required for usefulness.

---

## Review Checklist

Your cutline map is acceptable when:

- The MVD is complete, not half-built.
- It preserves the core user and core job.
- It cuts breadth before quality.
- It keeps review, handoff, and limitations.
- It names non-negotiable quality gates.
- It separates deferred from excluded.
- It includes a plain-language client explanation.
- Future phases are clear but not smuggled into phase one.

---

## Common Failure Modes

- **Cutting the handoff:** The deliverable works only because you are present.
- **Cutting review gates:** The AI output looks useful but is not checked.
- **Keeping too many users:** One deliverable tries to serve everyone.
- **Feature guilt:** You overbuild because a smaller version feels less impressive.
- **Unclear future phases:** Deferred work keeps pulling attention back into phase one.
- **Calling a demo an MVD:** A demo is not viable unless someone can use or evaluate it.

---

## Portfolio Evidence

Save:

- MVD cutline map
- Core value statement
- Non-negotiable quality list
- Client explanation paragraph

This becomes proof that you can reduce scope without reducing professionalism.

---

## References

- PlainLanguage.gov guidelines: https://www.plainlanguage.gov/guidelines/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
