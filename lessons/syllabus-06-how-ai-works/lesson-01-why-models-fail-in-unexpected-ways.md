# Lesson 06.01: Why Models Fail in Unexpected Ways

**Parent syllabus:** [Syllabus 06: How AI Models Work](../../syllabus-06-how-ai-works.md)  
**Estimated time:** 2 hours  
**Artifact:** Model failure analysis register

---

## Outcome

By the end of this lesson, you can diagnose model failures as system failures with different root causes instead of calling everything a prompt issue.

You will produce a model failure analysis register that records:

- Real or realistic failure examples
- Failure class
- Suspected root cause
- Evidence
- Mitigation
- Eval case or monitoring follow-up

---

## Why This Matters

Most AI teams waste time fixing the wrong layer.

Common failures:

- A model invents policy details, and the team keeps rewriting the prompt instead of improving retrieval and review.
- A workflow fails on rare edge cases because the training distribution did not prepare the model for that task shape.
- A team says the model is "misaligned" when the real issue is capability limits or missing context.
- RAG is added because answers are wrong, even though the task really needs calculation, tool use, or human approval.
- A model looks confident, so operators assume the answer is grounded.
- A high-impact workflow ships without a written record of what failure looks like.

If you cannot classify the failure, you cannot choose the right fix.

---

## Best-Practice Principles

1. **Classify the failure before changing the system.**  
   Separate prompt weakness, missing data, capability limits, alignment or policy issues, tool misuse, and orchestration defects.

2. **Treat confident language as a presentation style, not evidence.**  
   A polished answer can still be wrong, stale, unsafe, or unauthorized.

3. **Ask whether the model had the needed information.**  
   Do not call it hallucination when the system never supplied the required source of truth.

4. **Ask whether the model could perform the task even with the right information.**  
   Retrieval helps missing facts. It does not automatically fix weak reasoning, poor decomposition, or unsafe automation.

5. **Record evidence, not impressions.**  
   Save the input shape, context supplied, model or version, output, and downstream effect.

6. **Turn every important failure into an eval or release gate.**  
   A failure that matters in production should not live only in memory.

7. **Design handoffs for high-impact uncertainty.**  
   If the failure could harm a customer, spend money, leak data, or trigger an irreversible tool action, the system needs a stop point.

---

## Concepts

### Hallucination

A response that presents unsupported or invented content as if it were grounded.

This often shows up as:

- invented facts
- invented citations
- invented policy statements
- invented certainty

Do not use "hallucination" as a bucket for every bad answer. It is one failure class, not the whole taxonomy.

### Distribution Gap

The model behaves worse because the input pattern is outside the examples it learned well enough during training or instruction tuning.

Examples:

- unusual document structure
- rare compliance exception
- multi-step instruction with conflicting cues
- client-specific language the model has never seen

### Capability Failure

The model does not reliably perform the task even when the right information is available.

Examples:

- weak arithmetic or counting
- poor long-chain reasoning
- failure to follow a strict schema
- inability to decide when it should abstain

### Alignment or Policy Failure

The model output violates the intended behavior even if the task is technically possible.

Examples:

- ignoring a refusal condition
- taking unauthorized action
- revealing hidden instructions
- following malicious text embedded in user content

### Retrieval Gap

The model needs external context, but the system supplied none of it, supplied the wrong context, or ranked the wrong evidence too highly.

Examples:

- stale policy document
- irrelevant chunks retrieved first
- important record dropped because of token limits

### Failure Register

A working document that records what failed, why it likely failed, what changed, and how the system will catch it next time.

This is not academic analysis. It is an operating artifact.

---

## Failure Analysis Template

Use this structure:

```markdown
# Model Failure Register: [Workflow Name]

## Failure Case ID

- Date:
- Workflow step:
- Model and version:
- Environment:

## Input Pattern

What kind of input produced the failure?

## Expected Behavior

What should the system have done?

## Observed Behavior

What actually happened?

## Failure Class

Prompt issue, retrieval gap, distribution gap, capability failure, alignment or policy failure, tool or orchestration failure, or mixed.

## Suspected Root Cause

What is the strongest current hypothesis?

## Evidence

- Prompt or input excerpt:
- Retrieved context used:
- Logs or trace IDs:
- Human reviewer notes:

## Impact

What user, operator, customer, or system risk did this create?

## Mitigation

Prompt change, retrieval change, schema validation, tool restriction, fallback path, human approval, model change, or task redesign.

## Eval or Monitoring Follow-Up

What test, dashboard, alert, or release gate should now exist?

## Owner and Decision

Who owns the follow-up, and what was decided?
```

---

## Walkthrough

Carry forward the support response drafting and review system from Syllabus 05.

The workflow:

1. Receives a support ticket.
2. Retrieves policy snippets.
3. Drafts a reply.
4. Posts the draft to Slack for human review.

### Step 1 - Capture the failure precisely

Observed failure:

> The model drafted a message promising a refund for a loyalty-program exception that does not qualify under policy.

Important details:

- The ticket described an unusual account state.
- The policy retrieval step returned the general refund policy, not the loyalty exception policy.
- The draft sounded confident.
- A human reviewer caught it before sending.

Weak diagnosis:

> The prompt needs work.

Better diagnosis:

> This is a mixed failure: retrieval gap plus distribution gap. The workflow missed the exception policy, and the task involves a rare policy edge case the model handles unreliably.

### Step 2 - Separate "missing facts" from "weak reasoning"

Ask two questions:

1. Did the model receive the right authority source?
2. If yes, could it still perform the task correctly?

In this case:

- The system did not retrieve the correct exception policy.
- Even with the general policy, the model should have expressed uncertainty instead of issuing a refund promise.

That means two fixes may be needed:

- improve retrieval precision for policy exceptions
- require abstention or escalation language when the evidence is partial

### Step 3 - Decide whether RAG helps

RAG helps when the main issue is missing or stale knowledge.

RAG does not solve:

- irreversible tool permissions
- weak approval design
- poor schema enforcement
- tasks that need calculation or deterministic logic
- low-quality source documents

For this support system:

- RAG helps fetch the correct policy excerpt.
- RAG does not remove the need for human review on customer-facing policy decisions.

### Step 4 - Turn the failure into an operating change

Update the register like this:

```markdown
# Model Failure Register: Support Response Drafting and Review System

## Failure Case ID

- Date: 2026-04-21
- Workflow step: Draft generation
- Model and version: Frontier drafting model
- Environment: Staging replay

## Input Pattern

Support ticket asking for a refund under a loyalty-program exception.

## Expected Behavior

Draft a response that cites the correct policy or escalates if policy context is incomplete.

## Observed Behavior

Draft promised a refund that policy does not authorize.

## Failure Class

Mixed: retrieval gap and distribution gap

## Suspected Root Cause

Retriever ranked the general refund policy above the exception policy. The model also failed to abstain when evidence was incomplete.

## Evidence

- Retrieved policy IDs: policy-refund-general-v3, policy-account-updates-v2
- Missing policy ID: policy-loyalty-exceptions-v1
- Human reviewer note: "Draft overcommits. Missing loyalty exception logic."
- Trace ID: trace_8f2d

## Impact

Would have created an unauthorized customer-facing promise and possible revenue loss.

## Mitigation

- Add exception-policy retrieval rules for loyalty terms.
- Require the draft to mark policy uncertainty.
- Keep mandatory human approval for refund language.

## Eval or Monitoring Follow-Up

- Add three loyalty-exception cases to the regression set.
- Alert when approval rejection reason includes "policy mismatch."

## Owner and Decision

Owner: Workflow architect
Decision: Do not relax approval requirements. Improve retrieval and add evals before rollout expansion.
```

### Step 5 - Use the register to choose the next move

Possible actions:

- **Prompt change only:** too weak here
- **Retrieval fix:** required
- **Model change:** maybe, but only after measuring whether the current model still fails with correct policy context
- **Human handoff:** already required and should remain
- **Eval case:** required
- **Monitoring change:** required

The right question is:

> What changed in the system after we learned from this failure?

If the answer is "nothing but a prompt tweak," the lesson was not applied.

---

## Practice

Pick one AI workflow you already built, designed, or want to build.

Create a model failure analysis register with at least five cases:

1. One missing-information failure
2. One reasoning or capability failure
3. One security or prompt-injection-related failure
4. One cost or latency-related failure
5. One failure where human review prevented damage

For each case, record:

- input pattern
- expected behavior
- observed behavior
- failure class
- root-cause hypothesis
- mitigation
- eval or monitoring follow-up

End the register with:

- one case where RAG clearly helps
- one case where RAG does not solve the real problem

---

## Review Checklist

Your failure register is acceptable when:

- It contains at least five concrete failure cases.
- At least three different failure classes appear.
- Each case includes evidence, not only opinion.
- At least one case distinguishes retrieval failure from capability failure.
- At least one case distinguishes policy or alignment failure from prompt weakness.
- At least one mitigation changes the system outside the prompt.
- At least one eval follow-up is defined.
- At least one monitoring or alert follow-up is defined.
- At least one human handoff or approval point is justified.
- The register names who owns the next action.

---

## Common Failure Modes

- **Prompt-only thinking:** Every failure is treated as wording instead of system design.
- **No evidence:** The team debates from memory instead of logs, traces, or saved examples.
- **RAG as a reflex:** Retrieval is added even when the real issue is authorization, reasoning, or workflow design.
- **No abstention path:** The model is expected to guess instead of stopping when evidence is incomplete.
- **No feedback loop:** A costly failure happens once, gets fixed informally, and is never turned into an eval or alert.
- **No owner:** The failure is documented, but nobody is accountable for the change.

---

## Portfolio Evidence

Save:

- The model failure analysis register
- One short note on the two highest-risk failure classes
- One example showing how a failure changed the system design

This shows that you can debug AI behavior as an architect, not only as a prompt writer.

---

## References

- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
- TruthfulQA: Measuring How Models Mimic Human Falsehoods: https://arxiv.org/abs/2109.07958
- Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks: https://arxiv.org/abs/2005.11401
