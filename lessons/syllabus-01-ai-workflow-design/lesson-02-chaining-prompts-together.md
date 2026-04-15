# Lesson 01.02: Chaining Prompts Together

**Parent syllabus:** [Syllabus 01: AI Workflow Design](../../syllabus-01-ai-workflow-design.md)  
**Estimated time:** 2 hours  
**Artifact:** Three-step prompt chain map

---

## Outcome

By the end of this lesson, you can design a multi-step AI workflow where each step has a clear input, output, validation rule, and failure path.

You will produce a three-step prompt chain that turns messy input into a reviewed final output.

---

## Why This Matters

One giant prompt often hides multiple jobs inside one model call.

Common failures:

- The model tries to analyze, transform, write, and review in one pass.
- Bad output from step one quietly contaminates step two.
- The workflow has no checkpoint where a human can catch errors.
- The next prompt expects structured data but receives prose.
- The model repeats earlier mistakes because the chain passes too much context.
- Nobody knows which step failed.

Prompt chaining makes work inspectable. Each step should do one job well enough to be reviewed.

---

## Best-Practice Principles

1. **One step, one responsibility.**  
   Split extraction, analysis, drafting, review, and formatting when they require different judgment.

2. **Make every handoff explicit.**  
   Define exactly what step A passes to step B.

3. **Validate before continuing.**  
   Do not let malformed, empty, unsafe, or low-confidence output flow downstream.

4. **Keep context narrow.**  
   Pass only what the next step needs. Extra context increases cost and confusion.

5. **Add checkpoints where risk increases.**  
   Human review belongs before irreversible, client-facing, costly, or high-impact steps.

6. **Log step results.**  
   For serious workflows, you need enough traceability to know which step failed.

7. **Prefer deterministic steps when possible.**  
   Do not use a model for formatting, routing, validation, or calculation if ordinary code would be safer.

---

## Concepts

### Prompt Chain

A sequence of prompts where the output of one step becomes input to the next.

### Handoff Contract

The required shape and meaning of the data passed between steps.

### Checkpoint

A place where output is inspected before the workflow continues.

### Failure Path

What happens when a step produces invalid, unsafe, incomplete, or low-confidence output.

### Trace

A record of what happened in each step: input summary, output, model used, timestamp, cost, validation result, and failure reason.

---

## Walkthrough

Use this example:

> Turn a rough client discovery transcript into a scoped project brief.

### Bad design

```text
Read this transcript and write a complete project scope.
```

Problems:

- Extraction, interpretation, prioritization, and writing are collapsed.
- Missing information may be invented.
- The final scope may hide uncertainty.
- There is no review checkpoint.

### Better chain

Step 1: Extract facts.

```text
Extract only explicit facts from the transcript.

Return:
- Client goals
- Current workflow
- Pain points
- Constraints
- Deadlines
- Systems mentioned
- Missing information

Do not infer. If not stated, write "Not stated."
```

Step 2: Identify scope risks.

```text
Using the extracted facts, identify scope risks.

For each risk, return:
- Risk
- Evidence from extracted facts
- Clarifying question
- Severity: low, medium, high
```

Step 3: Draft scope brief.

```text
Using the extracted facts and scope risks, draft a one-page project scope.

Include:
- Problem
- Proposed workflow
- In scope
- Out of scope
- Assumptions
- Open questions
- Approval needed before build
```

### Handoff map

```text
Transcript
  -> Step 1 extracts facts
  -> Validate: every fact has transcript support
  -> Step 2 identifies risks
  -> Validate: every risk has evidence and question
  -> Step 3 drafts scope
  -> Human reviews before sending to client
```

### Failure paths

- If Step 1 returns too many assumptions: rerun with stricter extraction.
- If Step 2 finds high-severity risks: stop and ask clarifying questions.
- If Step 3 omits open questions: reject and regenerate from validated inputs.

---

## Practice

Choose one messy-to-useful workflow.

Examples:

- Meeting transcript to project scope
- Client notes to proposal
- Research dump to briefing memo
- Bug report to triage summary
- Personal notes to publishable outline

Create a three-step prompt chain:

1. **Step name**
2. **Step purpose**
3. **Input**
4. **Prompt**
5. **Output contract**
6. **Validation rule**
7. **Failure path**
8. **What passes to the next step**

Then draw the chain as a simple flow.

---

## Review Checklist

Your prompt chain is acceptable when:

- Each step has one main responsibility.
- Each step has a defined input and output.
- The handoff between steps is explicit.
- At least one validation rule exists between steps.
- At least one failure path stops the workflow.
- Human review appears before any client-facing or irreversible output.
- The final step uses validated intermediate outputs instead of raw messy context alone.
- Another person could run the chain from the document.

---

## Common Failure Modes

- **One giant prompt in disguise:** The chain has steps, but each step still does everything.
- **No handoff contract:** The next step receives whatever prose the previous step produced.
- **No validation:** Broken output moves downstream.
- **Context flooding:** Every step receives all prior context whether it needs it or not.
- **No stop condition:** The workflow keeps going when required information is missing.
- **Review too late:** Human review happens only after the final output is polished.

---

## Portfolio Evidence

Save:

- The chain map
- The three prompts
- A sample run with intermediate outputs
- A short note explaining where validation and human review happen

This demonstrates that you can design AI workflows as inspectable systems, not one-shot prompts.

---

## References

- Anthropic prompt engineering docs: https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview
- OpenAI prompt engineering guide: https://platform.openai.com/docs/guides/prompt-engineering
- OpenTelemetry concepts for traces: https://opentelemetry.io/docs/concepts/
