# Lesson 06.05: Evaluation - How to Know If a Workflow Actually Works

**Parent syllabus:** [Syllabus 06: How AI Models Work](../../syllabus-06-how-ai-works.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Lightweight evaluation plan and starter regression set

---

## Outcome

By the end of this lesson, you can replace vibes-based testing with a lightweight evaluation approach that actually protects a workflow from quiet failure.

You will produce:

- a lightweight evaluation plan
- a starter regression set
- pass or fail rules
- release or rollback triggers

---

## Why This Matters

Most workflow failures are visible before launch and invisible afterward unless someone defines what to measure.

Common failures:

- The workflow "looks good" in demos but fails on edge cases that matter to the client.
- A model update quietly reduces quality and nobody notices until a user complains.
- Teams test only happy paths and skip adversarial, ambiguous, or malformed inputs.
- A workflow ships with no explicit pass threshold.
- Human review catches recurring failure patterns, but that feedback never becomes a test.
- Cost and latency regressions go unmeasured because evals focus only on output quality.

Evaluation is the difference between "I like this result" and "I know when this system is good enough to trust for this job."

---

## Best-Practice Principles

1. **Evaluate the behavior that matters to the workflow.**  
   Do not build generic evals when the real risks are policy accuracy, escalation discipline, latency, cost, or schema compliance.

2. **Start small, but make it real.**  
   A 15-30 case set with strong failure coverage is more useful than a vague promise to build evals later.

3. **Include negative and adversarial cases.**  
   Systems fail at edges, not only on clean inputs.

4. **Version the baseline.**  
   Track model, prompt, schema, retrieval setup, and routing policy so regressions can be compared honestly.

5. **Define pass or fail before the run.**  
   If the threshold is chosen after seeing results, the gate is weak.

6. **Evaluate cost and latency alongside quality.**  
   A more accurate workflow can still be unusable if it is too slow or too expensive.

7. **Connect evals to deployment decisions.**  
   The result should change what happens next: launch, limited rollout, rollback, or redesign.

---

## Concepts

### Eval Case

A single test example with an input, an expected behavior, and a way to judge the result.

The expected behavior can be:

- exact output
- required fields
- rubric-based grade
- citation presence
- escalation decision

### Regression Set

A small set of cases rerun when the system changes.

Good regression sets include:

- previously failing cases
- risky edge cases
- representative normal cases
- security or prompt-injection cases

### Release Gate

A condition that must be satisfied before wider rollout.

Examples:

- schema success rate above 99 percent
- policy accuracy pass rate above 95 percent on high-risk cases
- no increase in rejection rate from human reviewers
- p95 latency below the agreed threshold

### Human Grading

Manual review used when exact matching is not enough.

This is acceptable if the rubric is clear and the sample size is manageable.

### Quiet Failure

A failure that does not crash the workflow but still reduces value or increases risk.

Examples:

- worse tone
- weaker escalation judgment
- slower output
- drift in retrieval quality
- more human rework

---

## Lightweight Evaluation Plan Template

Use this structure:

```markdown
# Lightweight Evaluation Plan: [Workflow Name]

## Workflow Goal

What job must the workflow do well enough to be worth operating?

## Critical Behaviors

- Behavior 1:
- Behavior 2:
- Behavior 3:

## Failure Modes That Matter Most

- ...

## Evaluation Set

| Case ID | Input pattern | Risk level | Expected behavior | Judge method |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## Metrics

- Quality:
- Schema success:
- Latency:
- Cost:
- Human review outcome:

## Pass or Fail Rules

What must be true before release or expansion?

## Version Under Test

- Model:
- Prompt or system instruction version:
- Retrieval setup:
- Output schema version:
- Routing policy version:

## Change Triggers

When must this eval set be rerun?

## Rollback or Escalation Rule

What happens if the workflow misses the threshold?
```

---

## Walkthrough

Use the support response drafting and review system.

We need to know whether it works well enough for a limited rollout.

### Step 1 - Define the behaviors that matter

For this workflow, three behaviors matter most:

1. policy accuracy
2. escalation correctness
3. response usefulness to the reviewer

That is already better than:

> Test whether the draft sounds good.

### Step 2 - Build a small but real case set

Starter set:

- 6 normal support tickets
- 4 policy-exception tickets
- 3 ambiguous tickets that should escalate
- 2 prompt-injection or instruction-conflict tickets

Fifteen cases is enough to begin if the cases are chosen intentionally.

### Step 3 - Define how each behavior is judged

Example rubric:

- **Policy accuracy:** pass if the draft does not invent policy and cites the correct policy path or requests escalation
- **Escalation correctness:** pass if high-risk or ambiguous cases are escalated
- **Reviewer usefulness:** pass if a reviewer can approve or revise quickly because the draft is specific and grounded

Some of these can be judged automatically:

- schema validity
- latency
- token cost

Some may need human grading:

- whether the draft is safe and useful

### Step 4 - Write the release gate

Example gate:

```text
Limited rollout allowed only if:
- policy accuracy passes 14 of 15 cases
- all 3 ambiguous tickets escalate correctly
- schema validity is 100 percent
- p95 draft availability is under 30 seconds
- average per-ticket cost stays under the budget target
```

This is simple, but it changes decisions.

### Step 5 - Turn reviewer feedback into future evals

Suppose reviewers reject drafts because they overstate refund certainty.

Do not keep that only in Slack comments.

Add at least two cases to the regression set that test:

- uncertain refund eligibility
- missing supporting policy evidence

This is how the workflow learns without pretending it is self-improving.

### Step 6 - Document the version under test

Example:

```markdown
## Version Under Test

- Model: frontier-draft-v3
- Prompt or system instruction version: support_draft_prompt_v5
- Retrieval setup: policy_top_k_3_rerank_v2
- Output schema version: support_draft_json_v2
- Routing policy version: routing_policy_v1
```

Without this, "the eval passed" means very little.

---

## Practice

Choose one workflow you care about.

Create a lightweight evaluation plan with:

1. workflow goal
2. three critical behaviors
3. at least fifteen eval cases
4. at least three high-risk or adversarial cases
5. judge method for each case
6. pass or fail rules
7. version under test
8. rerun triggers
9. rollback or escalation rule

At least one metric must cover:

- quality
- latency
- cost
- human review outcome

---

## Review Checklist

Your evaluation plan is acceptable when:

- It names the workflow behaviors that actually matter.
- The case set includes high-risk or adversarial inputs.
- The pass threshold is defined before the run.
- The version under test is recorded.
- At least one metric covers quality.
- At least one metric covers latency or cost.
- At least one metric covers human review or user impact.
- Previously observed failures appear in the regression set.
- A failed eval result changes the rollout decision.
- The plan is small enough to run regularly.

---

## Common Failure Modes

- **Demo-driven confidence:** Only pleasant examples are tested.
- **No gate:** The eval exists, but there is no consequence for failure.
- **No versioning:** Different model or retrieval setups are compared as if they were the same system.
- **Quality-only measurement:** Latency and cost regressions are ignored.
- **Feedback waste:** Human reviewer corrections never become future eval cases.
- **One-time eval:** The test set is never rerun when the system changes.

---

## Portfolio Evidence

Save:

- The lightweight evaluation plan
- The starter regression set
- One short note describing the release gate

This shows that you can prove workflow quality with a practical process.

---

## References

- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- OpenTelemetry concepts: https://opentelemetry.io/docs/concepts/
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
