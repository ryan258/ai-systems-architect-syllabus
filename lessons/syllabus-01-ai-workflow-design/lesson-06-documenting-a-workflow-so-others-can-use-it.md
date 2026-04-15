# Lesson 01.06: Documenting a Workflow So Others Can Use It

**Parent syllabus:** [Syllabus 01: AI Workflow Design](../../syllabus-01-ai-workflow-design.md)  
**Estimated time:** 2 hours  
**Artifact:** Workflow handoff document

---

## Outcome

By the end of this lesson, you can document an AI workflow clearly enough that another person can run it, review it, and know when to stop.

You will produce a one-page workflow handoff document.

---

## Why This Matters

Undocumented workflows are personal habits, not systems.

Common failures:

- The workflow only works because you remember the missing steps.
- Another person cannot tell which prompt to use.
- Inputs are unclear.
- Expected output is unclear.
- Failure behavior is missing.
- There is no owner, review point, or definition of done.

Documentation is not paperwork after the real work. For AI workflows, documentation is part of the system interface.

---

## Best-Practice Principles

1. **Document the run path, not every thought.**  
   The user needs what to do, what to check, and when to stop.

2. **Start with purpose and boundaries.**  
   Explain what the workflow does and what it does not do.

3. **List inputs and outputs explicitly.**  
   Ambiguous inputs create ambiguous outputs.

4. **Include prompts or prompt links.**  
   A workflow handoff must include the actual reusable prompts or where to find them.

5. **Document review points.**  
   Show where a human must inspect, approve, revise, or reject.

6. **Name failure behavior.**  
   The user should know what to do when the model output is missing, wrong, malformed, unsafe, or too long.

7. **Keep it short enough to use.**  
   If the handoff is too long, it will not be read during real work.

---

## Concepts

### Workflow Handoff Document

A plain-English guide that lets another person run the workflow.

### Operator

The person running or maintaining the workflow.

### Review Point

A required check before continuing.

### Definition of Done

The condition that tells the operator the workflow is complete.

### Escalation

What the operator does when the workflow cannot safely continue.

---

## Handoff Template

Use this template:

```markdown
# Workflow Handoff: [Workflow Name]

## Purpose

What problem does this workflow solve?

## Use When

When should someone run it?

## Do Not Use When

When is this workflow unsafe, irrelevant, or incomplete?

## Inputs Needed

- ...

## Tools Needed

- ...

## Steps

1. ...
2. ...
3. ...

## Prompts

Link or paste the reusable prompts.

## Review Points

- Check:
- Stop if:

## Expected Output

What should the final output look like?

## Failure Handling

If this happens, do this.

## Definition of Done

The workflow is done when...

## Owner

Who maintains this workflow?
```

---

## Walkthrough

Use this example:

> A workflow turns messy client notes into a project-scope draft.

```markdown
# Workflow Handoff: Client Notes to Scope Draft

## Purpose

Turn messy client discovery notes into a first project-scope draft with assumptions and open questions.

## Use When

Use after a discovery call when notes are available but the scope is not yet clear.

## Do Not Use When

- The client has not described a real problem.
- The notes include legal, medical, financial, or regulated-data decisions.
- You are too tired to review assumptions before sending anything.

## Inputs Needed

- Raw call notes or transcript
- Client name
- Known deadline
- Any existing constraints

## Tools Needed

- AI assistant
- Scope prompt chain
- Project notes folder

## Steps

1. Run the fact-extraction prompt.
2. Review missing information.
3. Run the scope-risk prompt.
4. If high-severity risks appear, stop and ask clarifying questions.
5. Run the scope-draft prompt.
6. Review assumptions before sending or saving.

## Prompts

- Prompt 1: Extract stated facts
- Prompt 2: Identify scope risks
- Prompt 3: Draft one-page scope

## Review Points

- Stop if more than five major facts are missing.
- Stop if the draft includes pricing, legal claims, or unsupported promises.
- Stop if the model invents systems, deadlines, or commitments.

## Expected Output

A one-page scope draft with:
- Problem
- Proposed workflow
- In scope
- Out of scope
- Assumptions
- Open questions

## Failure Handling

If output is too vague, rerun Step 1 with stricter extraction.
If assumptions are unsupported, delete them or mark them as open questions.
If the scope is too broad, split into Phase 1 and Later.

## Definition of Done

The workflow is done when the scope draft can be reviewed by a human without needing the original transcript open.

## Owner

Ryan owns and updates this workflow.
```

---

## Practice

Pick one workflow you have built or want to build.

Write a one-page workflow handoff document using the template.

Then run this test:

1. Pretend someone else must run it tomorrow.
2. Remove anything that depends on your memory.
3. Add missing prompts, review points, and stop conditions.
4. Cut anything that does not help the operator run the workflow.

---

## Review Checklist

Your handoff document is acceptable when:

- Purpose is clear.
- Use and do-not-use cases are clear.
- Required inputs are listed.
- Required tools are listed.
- Steps are ordered.
- Prompts are included or linked.
- Review points are explicit.
- Failure handling is included.
- Definition of done is clear.
- Owner is named.
- The document fits on one page or close to it.

---

## Common Failure Modes

- **Memory dependency:** Important steps are implied but not written.
- **Prompt missing:** The document describes prompts without giving them.
- **No do-not-use case:** The operator cannot tell when the workflow is unsafe.
- **No review points:** The workflow has no quality gate.
- **No failure handling:** The operator does not know what to do when output is bad.
- **Too much context:** The handoff becomes a manual nobody will read.

---

## Portfolio Evidence

Save:

- The workflow handoff document
- The reusable prompts
- One sample output
- A short case note explaining how documentation made the workflow reusable

This is the artifact that turns a personal AI habit into something client-ready.

---

## References

- Anthropic prompt engineering docs: https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview
- OpenTelemetry concepts: https://opentelemetry.io/docs/concepts/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
