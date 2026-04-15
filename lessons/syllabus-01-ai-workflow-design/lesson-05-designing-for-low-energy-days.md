# Lesson 01.05: Designing for Low-Energy Days

**Parent syllabus:** [Syllabus 01: AI Workflow Design](../../syllabus-01-ai-workflow-design.md)  
**Estimated time:** 90-120 minutes  
**Artifact:** Low-energy workflow card

---

## Outcome

By the end of this lesson, you can redesign an AI workflow so it still works when your attention, working memory, energy, or executive function is limited.

You will produce a one-page low-energy workflow card.

---

## Why This Matters

A workflow that only works on your best day is not reliable.

Common failures:

- Too many choices are required before starting.
- The workflow depends on memory instead of visible steps.
- The user has to decide what to do with ambiguous AI output.
- The workflow has no pause point.
- Recovery after interruption is unclear.
- The system produces a large output when the user needs a small next action.

Low-energy design is not a softer standard. It is a reliability standard for human-operated systems.

---

## Best-Practice Principles

1. **Reduce decisions at the start.**  
   Use defaults, templates, and one obvious first action.

2. **Make progress visible.**  
   The workflow should show where you are, what is done, and what remains.

3. **Design pause and resume points.**  
   Assume interruption. A good workflow can be restarted without reconstructing context from memory.

4. **Prefer small outputs.**  
   On low-energy days, the best output is often the next decision, not a complete report.

5. **Use checklists for repeated judgment.**  
   Do not make the user re-invent the review process every time.

6. **Separate capture from decision.**  
   Let the workflow collect messy input now and make hard decisions later.

7. **Make failure gentle and clear.**  
   If something goes wrong, the workflow should say what happened, what is safe, and what to do next.

---

## Concepts

### Cognitive Load

The amount of mental effort required to use the workflow.

### Decision Load

The number and difficulty of choices required to move forward.

### Resume Point

A saved state that lets the user return without remembering what happened.

### Minimum Viable Next Action

The smallest useful action that moves the work forward.

### Low-Energy Workflow Card

A one-page operating guide for running a workflow on a difficult day.

---

## Walkthrough

Use this fragile workflow:

> "When I have a client call transcript, I ask AI to summarize it, identify next steps, draft a proposal, check the risks, and help me decide what to send."

Why it fails on low-energy days:

- Too many tasks are combined.
- The user must remember the right prompt.
- The output may be too long.
- The user must decide whether the proposal is safe.
- There is no pause point.

Better workflow card:

```markdown
# Low-Energy Workflow Card: Client Call to Next Step

## Use When

I have a client call transcript or rough notes and need the next practical action.

## Start Here

Paste the notes into Step 1. Do not clean them first.

## Step 1 - Extract Facts

Ask AI to extract only stated facts:
- Client goal
- Current workflow
- Pain
- Deadline
- Systems mentioned
- Missing information

Stop if more than five key items are "Not stated."

## Step 2 - Choose One Next Action

Ask AI to recommend exactly one:
- Ask clarifying questions
- Draft scope
- Decline project
- Schedule follow-up

## Step 3 - Human Check

Before sending anything, check:
- Does this make a promise?
- Does this mention price?
- Does this touch legal, medical, financial, or sensitive data?
- Am I too tired to notice a bad assumption?

If yes to any, save as draft and review later.

## Resume Note

Save:
- Source notes
- Extracted facts
- Recommended next action
- Open questions
```

What improved:

- The first action is obvious.
- Cleaning is not required.
- The workflow stops when information is missing.
- The output is one next action.
- Human review is built in.
- Resume state is explicit.

---

## Practice

Pick one workflow that you avoid when tired.

Create a low-energy workflow card with:

1. **Use when**
2. **Do not use when**
3. **Start here**
4. **Required inputs**
5. **Default settings**
6. **Step-by-step flow**
7. **Stop conditions**
8. **Human review checklist**
9. **Resume note**
10. **Definition of done**

Then run a bad-day simulation:

- Pretend you have half your normal attention.
- Remove any step that depends on memory.
- Reduce any output that is too long to review.
- Add a stop condition where risk is high.

---

## Review Checklist

Your workflow card is acceptable when:

- The first step is obvious.
- Required inputs are minimal.
- Defaults are visible.
- The workflow can pause and resume.
- There is a definition of done.
- The output is small enough to review.
- Stop conditions are explicit.
- Human review appears before risky action.
- The workflow does not require remembering hidden context.

---

## Common Failure Modes

- **Best-day workflow:** It assumes full focus, patience, and memory.
- **Too many choices:** The user has to design the workflow while running it.
- **No stop condition:** The workflow pushes through missing or risky information.
- **Output overload:** The AI returns more text than the user can safely review.
- **No resume state:** Interruption means starting over.
- **No definition of done:** The work expands until energy runs out.

---

## Portfolio Evidence

Save:

- The low-energy workflow card
- Before-and-after workflow notes
- One sample run
- A short note explaining which cognitive load you removed

This is strong evidence of constraint-driven AI workflow design.

---

## References

- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
