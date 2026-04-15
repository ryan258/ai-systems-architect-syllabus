# Syllabus 18: Advanced Evaluation and Release Gates
**Status:** Enterprise Gap - Proving the System Is Ready to Ship
**Format:** One chat session per unit, self-paced

---

## What This Is

Basic tests catch broken code. AI evaluations catch broken behavior. This syllabus teaches you how to build eval sets, grade outputs, compare model versions, test adversarial cases, and decide when a change is safe enough to release.

---

## Unit 1 - Evaluation Strategy for AI Systems

**Goal:** Decide what must be evaluated and what level of rigor the project deserves.

**Topics:**
- Unit tests, integration tests, and AI evals: what each catches
- Choosing success criteria before looking at outputs
- Golden datasets for expected behavior
- Regression tests for previous failures
- Matching eval rigor to client risk

**Session starter:**
> "Help me design an evaluation strategy for an AI system. Explain unit tests, integration tests, golden datasets, regression tests, and AI evals. Show me how to choose the right rigor for client risk."

---

## Unit 2 - Building Gold Sets and Adversarial Sets

**Goal:** Create test cases that represent normal, edge, and hostile usage.

**Topics:**
- What belongs in a gold set
- Edge cases from real user behavior
- Adversarial examples for prompt injection, ambiguity, and sensitive data
- Keeping eval sets small but meaningful
- Updating evals when production failures teach you something new

**Session starter:**
> "Teach me how to build gold sets and adversarial test sets for AI workflows. I want normal cases, edge cases, injection attempts, sensitive-data cases, and examples based on real failures."

---

## Unit 3 - Grading Outputs and Traces

**Goal:** Measure whether the model and the workflow behaved correctly.

**Topics:**
- Rule-based grading vs. human grading vs. model-assisted grading
- Rubrics for factuality, format, completeness, tone, and safety
- Trace grading for tool calls, retrieval, and decision steps
- Spot-checking model-assisted graders
- Avoiding evals that reward pretty wrong answers

**Session starter:**
> "Help me grade AI outputs and workflow traces. Show me rubrics for factuality, format, completeness, tone, safety, retrieval quality, and tool-call correctness. Include when to use human grading vs. model-assisted grading."

---

## Unit 4 - Model and Prompt Version Comparison

**Goal:** Know whether a new model, prompt, or retrieval change actually improves the system.

**Topics:**
- Comparing model versions on the same eval set
- Prompt versioning and rollback
- Retrieval changes and answer-quality impact
- Cost, latency, and quality trade-offs
- Writing a release note for an AI behavior change

**Session starter:**
> "Teach me how to compare model versions, prompts, and retrieval changes. I want a practical scorecard that includes quality, safety, latency, and cost so I know whether a change is actually better."

---

## Unit 5 - Release Gates and Continuous Evaluation

**Goal:** Prevent risky AI changes from reaching production without review.

**Topics:**
- Defining pass/fail release thresholds
- Blocking deployment when evals regress
- Sampling production outputs for review
- Using client feedback as eval data
- Scheduling periodic re-evaluation after model updates

**Session starter:**
> "Help me design release gates for AI systems. I want pass/fail thresholds, regression blocking, production sampling, client feedback loops, and periodic re-evaluation after model updates."

---

## Practice Project

Pick one AI workflow. Build a 25-case eval set with normal cases, edge cases, previous failure cases, and adversarial cases. Create a grading rubric, run two prompt or model versions against it, and write a release decision explaining which version should ship.
