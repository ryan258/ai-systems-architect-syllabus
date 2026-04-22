# Lesson 16.05: Workflow Orchestration

**Parent syllabus:** [Syllabus 16: Distributed AI Systems](../../syllabus-16-distributed-ai-systems.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Orchestration decision brief and recovery runbook

---

## Outcome

By the end of this lesson, you can decide when a simple queue-and-worker design is enough, when you need a workflow engine, and how to document step tracking, resume behavior, and human checkpoints for a multi-step AI process.

You will produce:

- an orchestration decision brief
- a step and state model
- a recovery and resume runbook

---

## Why This Matters

Distributed AI systems often become fragile when the process has more steps than the original architecture was built to represent.

Common failures:

- A workflow spans multiple services, but no one can see which step failed.
- Human approval exists, but the system cannot pause and resume cleanly.
- Recovery means rerunning everything because there is no checkpointed state.
- A homegrown chain grew from five steps to twenty, and now retries create unpredictable results.
- The chosen framework is powerful, but the team cannot operate or debug it on a low-energy day.
- AI decision flow is modeled, but surrounding side effects and durable state are not.

Orchestration is the control plane for work that has to survive interruption, retry, approval, and audit.

---

## Best-Practice Principles

1. **Choose the simplest orchestration model that preserves required guarantees.**  
   More framework is not automatically more reliability.

2. **Make workflow steps explicit.**  
   Operators should be able to say which step is running, paused, failed, or completed.

3. **Persist enough state to resume safely.**  
   If restart means guessing, the orchestration model is weak.

4. **Model human checkpoints as first-class states.**  
   Approval and review are not exceptions. They are part of the workflow.

5. **Define compensation or rollback behavior for important side effects.**  
   Recovery is more than retry.

6. **Connect traces to workflow state, not only to individual HTTP calls.**  
   Distributed observability should reflect the business process.

7. **Keep maintainability in view.**  
   The orchestration system should still be understandable when the original builder is unavailable.

---

## Concepts

### Orchestration

Coordinating multi-step work so state, retries, pauses, and outcomes are explicit.

### Workflow State

The durable record of where a workflow instance currently stands.

### Checkpoint

A point where the workflow can resume without repeating everything.

### Compensation

The action taken to undo or neutralize a side effect when a later step fails.

### Durable Execution

An execution model where workflow progress is persisted so the system can recover after interruption.

### Human Checkpoint

A step where the system intentionally pauses for review, approval, or correction.

---

## Orchestration Decision Brief Template

Use this structure:

```markdown
# Orchestration Decision Brief: [Workflow Name]

## 1. Workflow Goal

- What the workflow does:
- Why orchestration is needed:

## 2. Step Model

| Step | Type | Durable state needed? | Retryable? | Human checkpoint? |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## 3. Orchestration Options Considered

| Option | Strength | Weakness | Best fit | Not a fit when |
| --- | --- | --- | --- | --- |
| Simple queue plus worker |  |  |  |  |
| Workflow engine |  |  |  |  |
| AI graph or agent framework |  |  |  |  |

## 4. Chosen Approach

- Why this option:
- What state is persisted:
- Resume behavior:
- Compensation rule:

## 5. Recovery Runbook

- Failure detection:
- Resume path:
- Manual override:
- Rollback or compensation path:

## 6. Privacy, Security, Cost, and Observability Notes

- Audit evidence:
- Sensitive state rule:
- Cost guardrail:
- Trace and dashboard requirements:

## 7. Open Questions

- ...
```

Review questions:

- Which steps truly require durable execution?
- Where can the workflow resume without duplicating work?
- Is the chosen tool solving a real control problem or only adding abstraction?

---

## Walkthrough

Use this example:

> A support drafting workflow receives a ticket event, validates it, enqueues work, retrieves policy context, drafts a response, runs policy checks, pauses for human review, and publishes the approved draft to a support system.

### Step 1 - Write the real step model

For this workflow, the steps are not just:

- get ticket
- draft answer

They are closer to:

1. accept event
2. create job
3. retrieve policy context
4. draft response
5. validate structured output
6. run policy checks
7. wait for human review
8. publish approved draft
9. close the workflow

Once the real process is visible, orchestration choice becomes easier.

### Step 2 - Compare orchestration styles

A simple queue plus worker may be enough when:

- steps are short
- there are few human pauses
- resume logic is simple
- retries can be expressed locally

A workflow engine becomes more attractive when:

- workflows run for a long time
- steps need durable checkpoints
- human approval is common
- compensation or resume behavior matters

An AI graph framework is useful when:

- the AI decision flow itself has branching state
- prompt, tool, and model-routing logic are central
- you still pair it with broader system controls where needed

Tool choice should follow control requirements, not trend pressure.

### Step 3 - Be honest about the human checkpoint

For this service, human review is not a side note. It is a major state transition.

The system should know:

- who owns the pending review
- what the timeout or SLA is
- how the workflow resumes after approval
- what happens after rejection

If human review is handled outside the workflow state model, auditability and recovery both suffer.

### Step 4 - Define compensation and resume

If publication to the support system fails after approval:

- can the system retry safely?
- is the publish step idempotent?
- should the workflow pause for an operator?
- what evidence proves the downstream action already happened?

Resume rules are where orchestration earns its keep.

### Step 5 - Keep observability attached to the workflow

Useful fields include:

- workflow ID
- current step
- previous completed step
- attempt number per step
- last failure reason
- human checkpoint age
- total workflow age

That lets the operator reason about the business process, not only individual service logs.

### Step 6 - Keep the chosen system operable

For a small team, an orchestration tool is only a win if:

- someone can debug it without vendor archaeology
- the state model is readable
- the recovery path is documented
- the tool matches the actual workflow complexity

A simple design that is clearly documented usually beats an advanced framework nobody can operate well.

---

## Practice

Choose one multi-step AI workflow and create an **Orchestration Decision Brief** that includes:

1. explicit workflow steps
2. human checkpoints
3. durable state needs
4. options considered
5. chosen orchestration approach
6. resume and recovery rules

Do not choose the tool first. Start with the failure and recovery requirements.

---

## Review Checklist

- Workflow steps are explicit and stateful.
- Human checkpoints are represented as real workflow states.
- The orchestration choice is justified by control needs.
- Durable state and checkpoints are documented.
- Resume and compensation behavior are defined.
- Observability reflects workflow state, not only infrastructure calls.
- Cost, privacy, and security implications are addressed.
- The chosen approach is maintainable for the actual team.

---

## Common Failure Modes

- Treating a long multi-step process as if it were one retried function call.
- Choosing a workflow tool before modeling the process.
- Leaving human review outside the workflow system entirely.
- Restarting failed workflows from the beginning because there are no checkpoints.
- Using an AI graph for agent logic but ignoring surrounding operational state.
- Building orchestration nobody can debug during an incident.

---

## Portfolio Evidence

Keep a sanitized copy of your orchestration decision brief and recovery runbook.

Redact:

- system names
- queue names
- internal step identifiers if sensitive
- operator names
- endpoint details

What remains shows that you can design multi-step AI workflows that survive interruption and review.

---

## References

- [Temporal Documentation](https://docs.temporal.io/)
- [Dagster Documentation](https://docs.dagster.io/)
- [LangGraph Documentation](https://docs.langchain.com/oss/python/langgraph)
- [OpenTelemetry Concepts](https://opentelemetry.io/docs/concepts/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
