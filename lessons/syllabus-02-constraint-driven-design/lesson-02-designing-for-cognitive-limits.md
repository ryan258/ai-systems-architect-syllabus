# Lesson 02.02: Designing for Cognitive Limits

**Parent syllabus:** [Syllabus 02: Constraint-Driven Design](../../syllabus-02-constraint-driven-design.md)  
**Estimated time:** 2 hours  
**Artifact:** Cognitive load audit

---

## Outcome

By the end of this lesson, you can audit an AI workflow for cognitive load and redesign it to reduce memory demands, decision load, ambiguity, and restart friction.

You will produce a cognitive load audit and a revised workflow.

---

## Why This Matters

AI workflows often fail because the human side is overloaded.

Common failures:

- The user must remember which prompt comes next.
- The workflow asks too many questions before producing value.
- Output is too long to review safely.
- The user has to decide whether the AI result is trustworthy with no checklist.
- The workflow requires context reconstruction after interruption.
- The system gives options but no recommended default.

Designing for cognitive limits makes workflows more reliable for everyone, not only people with brain fog or attention challenges.

---

## Best-Practice Principles

1. **Reduce working-memory demands.**  
   Put steps, defaults, and context in the workflow instead of in the user's head.

2. **Minimize first-step friction.**  
   The first action should be obvious and small.

3. **Prefer defaults over configuration.**  
   Ask for a choice only when the answer changes the result.

4. **Chunk information.**  
   Break large outputs into small sections with clear labels.

5. **Show the next action.**  
   Every output should make the next step obvious.

6. **Separate review from generation.**  
   The user should not have to evaluate a final answer without a checklist.

7. **Design for interruption.**  
   The workflow should preserve state so the user can resume without re-reading everything.

---

## Concepts

### Cognitive Load

The mental effort required to use a system.

### Working Memory

The limited amount of information a person can hold while doing a task.

### Decision Load

The number, frequency, and difficulty of choices required by a workflow.

### Chunking

Breaking information into small, labeled pieces.

### Restart Friction

The effort required to resume after interruption.

---

## Walkthrough

Use this workflow:

> Take messy notes, ask AI to summarize them, decide what matters, draft a response, review it, and send it.

### Audit the original workflow

| Friction point | Why it is hard | Redesign |
| --- | --- | --- |
| User must clean notes first | Requires energy before value | Allow messy input |
| User chooses prompt manually | Depends on memory | Provide one start prompt |
| AI returns long summary | Hard to review | Return facts, risks, next action |
| User decides whether output is safe | No review structure | Add checklist |
| No pause point | Interruption means restart | Save resume note |

### Revised workflow

```text
Step 1: Paste messy notes.
Step 2: AI extracts stated facts only.
Step 3: AI identifies missing information.
Step 4: AI recommends one next action.
Step 5: Human uses a four-question review checklist.
Step 6: Save resume note.
```

### Review checklist

```text
Before sending anything:
- Did the AI invent a fact?
- Is a promise being made?
- Is sensitive data included?
- Am I confident enough to send this today?
```

### Resume note

```text
Source:
Status:
Facts extracted:
Open questions:
Recommended next action:
Needs review before:
```

---

## Practice

Pick one AI workflow you use.

Create a cognitive load audit:

1. **Workflow name**
2. **Current steps**
3. **Where memory is required**
4. **Where decisions are required**
5. **Where output is too large**
6. **Where ambiguity appears**
7. **Where interruption would break the flow**
8. **Redesign choices**
9. **New default path**
10. **Review checklist**
11. **Resume note**

Then rewrite the workflow as a numbered low-cognitive-load version.

---

## Review Checklist

Your audit is acceptable when:

- It identifies at least five friction points.
- It names where memory is required.
- It names where decisions are required.
- It reduces the first step.
- It adds or improves defaults.
- It chunks output.
- It includes a review checklist.
- It includes a resume note.
- The revised workflow can be run without remembering hidden context.

---

## Common Failure Modes

- **More instructions, same load:** The redesign explains more but does not reduce effort.
- **No default path:** The user still has to choose how to start.
- **Review overload:** The checklist is too long to use.
- **Output bloat:** The AI returns a report when the user needs a decision.
- **No state:** The workflow cannot survive interruption.
- **Ambiguity moved downstream:** The workflow delays hard decisions instead of making them visible.

---

## Portfolio Evidence

Save:

- Cognitive load audit
- Before-and-after workflow
- Review checklist
- Resume note

This proves you can design workflows around human limits, not just model capability.

---

## References

- W3C Accessibility Fundamentals: https://www.w3.org/WAI/fundamentals/
- PlainLanguage.gov guidelines: https://www.plainlanguage.gov/guidelines/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
