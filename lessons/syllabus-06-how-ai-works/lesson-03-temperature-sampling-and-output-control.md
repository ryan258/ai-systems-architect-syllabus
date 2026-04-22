# Lesson 06.03: Temperature, Sampling, and Output Control

**Parent syllabus:** [Syllabus 06: How AI Models Work](../../syllabus-06-how-ai-works.md)  
**Estimated time:** 2 hours  
**Artifact:** Output control policy sheet

---

## Outcome

By the end of this lesson, you can choose generation settings and output constraints based on task risk instead of using one default configuration for everything.

You will produce an output control policy sheet that defines:

- task type
- accepted variability
- sampling settings
- structured output rules
- validation steps
- fallback behavior
- logging requirements

---

## Why This Matters

Generation settings are part of system behavior, not cosmetic tuning.

Common failures:

- A classification task runs with creative settings and becomes unstable.
- A high-risk workflow returns free-form prose when the system needs structured JSON.
- Teams blame the model for malformed output even though no schema or parser guard existed.
- Repetition penalties are turned up to hide a prompt or truncation problem.
- Top-k settings are specified in design docs even though the hosted provider does not expose top-k.
- Operators cannot reproduce a failure because the generation settings were never logged.

If the task is strict, the controls must be strict.

---

## Best-Practice Principles

1. **Choose output behavior per task.**  
   Extraction, classification, summarization, drafting, brainstorming, and tool selection have different tolerance for variability.

2. **Use the weakest randomness that still serves the task.**  
   Lower randomness improves repeatability for structured work. Higher randomness is useful only when variation itself creates value.

3. **Constrain the shape before polishing the words.**  
   If downstream code needs JSON, table rows, labels, or tool arguments, enforce structure first.

4. **Do not rely on unsupported controls.**  
   Some local runtimes expose temperature, top-p, and top-k. Many hosted APIs expose only part of that surface. The policy must match the actual runtime.

5. **Validate model output as if it came from an unreliable external service.**  
   Parsing, schema checks, and business-rule validation are not optional for critical flows.

6. **Log the full output-control profile.**  
   Temperature, top-p, top-k if used, schema version, retry path, and parser result should be reconstructable.

7. **Prefer product-level controls over clever prompt tricks.**  
   Use schemas, validators, stop rules, and human review where appropriate instead of hoping the wording alone is enough.

---

## Concepts

### Temperature

A control that affects how sharply the model favors high-probability next tokens.

Practical effect:

- lower temperature usually gives more repeatable outputs
- higher temperature usually gives more variation

It does not guarantee truth or structure.

### Top-p

Also called nucleus sampling.

The model samples from the smallest set of next-token options whose cumulative probability reaches the threshold.

Practical effect:

- lower top-p narrows the candidate set
- higher top-p allows more variation

### Top-k

Sampling from the top `k` next-token candidates.

This is common in local runtimes and some open-model stacks, but not universal in hosted APIs.

### Structured Output

Constraining the model to a defined shape such as:

- JSON object
- schema-bound response
- XML or tagged format
- enumerated label set

This reduces parser fragility and makes evals easier.

### Post-Generation Validation

Checks performed after generation.

Examples:

- schema validation
- allowed-label validation
- numeric range checks
- required-citation checks
- "must escalate" rule for certain policy terms

---

## Output Control Policy Sheet

Use this structure:

```markdown
# Output Control Policy: [Workflow Name]

## Task

- Task name:
- User impact:
- Allowed variability:

## Runtime

- Provider or local runtime:
- Model:
- Parameters actually supported:

## Sampling Policy

- Temperature:
- Top-p:
- Top-k:
- Frequency or repetition penalty:
- Stop conditions:

## Required Output Shape

What exact structure must the model return?

## Validation

- Schema or parser:
- Business-rule checks:
- Reject conditions:

## Retry or Fallback

What happens if output is malformed, missing fields, or too uncertain?

## Human Review Rule

When must the output stop for approval?

## Logging

What settings, schema version, and parser results are recorded?
```

---

## Walkthrough

Use the same support response system.

We will define control policies for three tasks:

1. ticket category classification
2. response drafting
3. policy-check result

### Step 1 - Match the control profile to the task

Task 1: classify the ticket

Desired behavior:

- stable
- short
- schema-bound
- easy to evaluate

Policy:

- temperature: low
- top-p: default or narrow
- top-k: only if the chosen runtime supports it
- output: one label plus confidence band
- parser: strict enum validation

Task 2: draft the response

Desired behavior:

- natural language
- still constrained by policy and tone
- moderate variability is acceptable

Policy:

- temperature: low to moderate
- structured wrapper for fields such as `summary`, `draft`, `uncertainty_flags`
- human review required before any customer-facing action

Task 3: policy check

Desired behavior:

- deterministic
- citation-based
- must refuse if evidence is incomplete

Policy:

- low randomness
- required fields for `status`, `policy_ids`, `missing_evidence`, `escalate`
- reject outputs without cited policy IDs

### Step 2 - Start with structure, not prose

For the policy-check step, use a shape like:

```json
{
  "status": "pass|fail|needs_review",
  "policy_ids": ["policy-id-1"],
  "missing_evidence": ["loyalty_exception_policy"],
  "escalate": true,
  "notes": "Short explanation"
}
```

This makes it much easier to:

- validate
- route
- evaluate
- alert on failure patterns

If the model returns prose instead of this structure, the system should reject and retry or escalate.

### Step 3 - Do not overuse penalties

If the response repeats itself, ask:

- Is the prompt contradictory?
- Is the context too long?
- Is the schema unclear?
- Is the model getting cut off and retrying poorly?

Do not jump straight to heavy repetition penalties. They can distort outputs and make debugging harder.

### Step 4 - Log the policy that actually ran

Example log line:

```text
trace_id=trace_31c2
workflow_step=policy_check
model=frontier-policy-checker
temperature=0.1
top_p=0.9
top_k=unsupported
schema_version=policy_check_v2
parser_result=success
retry_count=0
```

Without this, you cannot reproduce a failure or compare model versions fairly.

### Step 5 - Write the policy sheet

Example entry:

```markdown
## Task

- Task name: Policy check before human review
- User impact: High
- Allowed variability: Low

## Runtime

- Provider or local runtime: Hosted frontier API
- Model: Policy-check model
- Parameters actually supported: temperature, top-p

## Sampling Policy

- Temperature: 0.1
- Top-p: 0.9
- Top-k: unsupported
- Frequency or repetition penalty: none by default
- Stop conditions: end after valid JSON object

## Required Output Shape

JSON object with status, policy_ids, missing_evidence, escalate, and notes

## Validation

- Schema or parser: strict JSON schema
- Business-rule checks: reject if policy_ids empty and status=pass
- Reject conditions: malformed JSON, missing fields, uncited pass

## Retry or Fallback

Retry once with the same schema reminder, then escalate to human review

## Human Review Rule

Always review when status is fail or needs_review

## Logging

Record generation settings, schema version, parser result, and cited policy IDs
```

---

## Practice

Choose one workflow with at least three different model tasks.

Create an output control policy sheet for each task.

Across the set, include:

1. one classification or extraction task
2. one narrative drafting task
3. one high-risk structured decision task

For each task, define:

- allowed variability
- supported sampling controls
- output format
- validation
- retry path
- human review rule
- logging requirements

End with one paragraph explaining why one global temperature setting would be a weak design for this workflow.

---

## Review Checklist

Your output control policy is acceptable when:

- It distinguishes tasks by risk and required variability.
- It uses only controls the runtime actually supports.
- At least one task uses a structured output format.
- At least one task defines strict schema validation.
- Retry and fallback behavior are explicit.
- Human review rules are defined for high-impact outputs.
- Logging captures the generation profile and parser result.
- The policy explains why lower or higher randomness is justified.
- The policy does not use randomness settings as a substitute for missing validation.

---

## Common Failure Modes

- **One-setting-for-everything:** Classification and brainstorming share the same configuration.
- **Unsupported controls:** Design docs specify top-k or penalties that the provider does not expose.
- **No schema:** Downstream code depends on structure the model was never required to produce.
- **Penalty masking:** Repetition penalties are used to hide prompt or context defects.
- **No logging:** Operators cannot tell which settings caused a regression.
- **No reject path:** Malformed output reaches production logic anyway.

---

## Portfolio Evidence

Save:

- The output control policy sheet set
- One schema example used for a high-risk task
- One short note explaining how the policy changes eval design

This shows you can control model behavior with system design, not only prompt wording.

---

## References

- The Curious Case of Neural Text Degeneration: https://arxiv.org/abs/1904.09751
- JSON Schema: https://json-schema.org/
- Anthropic prompt engineering overview: https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
