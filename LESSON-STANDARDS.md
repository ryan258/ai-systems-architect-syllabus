# Lesson Standards

These standards define the quality bar for every lesson in this curriculum. The goal is not to produce motivational reading. The goal is to create training that turns into systems, artifacts, decisions, and portfolio evidence.

---

## Core Rule

Every lesson must produce at least one artifact.

An artifact can be code, a diagram, an ADR, a runbook, an eval set, a threat model, a checklist, a policy note, a deployment, a case study, or a reusable template.

If the learner cannot show what changed after the lesson, the lesson is incomplete.

---

## Required Lesson Structure

Each lesson should include:

1. **Outcome** - What the learner can do after the lesson.
2. **Why This Matters** - The real-world failure this lesson prevents.
3. **Best-Practice Principles** - The durable rules behind the lesson.
4. **Concepts** - The vocabulary and mental models needed.
5. **Walkthrough** - A concrete example.
6. **Practice** - A task that produces an artifact.
7. **Review Checklist** - How to judge whether the artifact is good.
8. **Common Failure Modes** - What usually goes wrong.
9. **Portfolio Evidence** - What to save or publish.
10. **References** - Primary sources or durable standards when the lesson depends on external best practice.

---

## Best-Practice Baseline

Every technical lesson should be checked against these disciplines:

- **Reliability:** What does "working" mean, and how would we know it stopped working?
- **Security:** What can the system access, and what happens if the model is manipulated?
- **Privacy:** What data enters the system, where does it go, and what should never be logged?
- **Cost:** What can spend money, and where are the hard limits?
- **Observability:** Can a failure be reconstructed from logs, metrics, and traces?
- **Evaluation:** What behavior is being tested before release?
- **Governance:** Who owns the decision, risk, approval, and handoff?
- **Maintainability:** Can the system be understood on a low-energy day?

---

## Source Hierarchy

When grounding a lesson in best practices, prefer:

1. Official standards and frameworks.
2. Official vendor documentation.
3. Widely accepted engineering references.
4. High-quality implementation examples.
5. Personal preference only after the above are satisfied.

Do not let a framework tutorial outrank a system-design principle. Tools change. Failure modes repeat.

---

## Primary Reference Set

Use these as recurring anchors across lessons:

- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- OpenTelemetry observability concepts: https://opentelemetry.io/docs/concepts/
- FastAPI docs: https://fastapi.tiangolo.com/
- Anthropic Claude docs: https://docs.anthropic.com/
- LangGraph docs: https://docs.langchain.com/oss/python/langgraph
- CrewAI docs: https://docs.crewai.com/

---

## Lesson Acceptance Criteria

A lesson is ready when:

- It has a concrete artifact.
- It names the failure modes it protects against.
- It separates concepts from implementation steps.
- It includes a checklist that can be used for review.
- It does not imply that AI output can be trusted without verification.
- It does not teach a tool without teaching the architectural decision behind the tool.
- It is specific enough to act on but general enough to survive framework churn.

---

## Tone and Style

Lessons should be clear, direct, and usable. Avoid hype. Avoid vague claims like "AI will transform everything." Prefer operational language:

- "This system fails if..."
- "Use this pattern when..."
- "Do not use this pattern when..."
- "The review question is..."
- "The artifact is accepted when..."

The learner should finish each lesson with sharper judgment, not just more vocabulary.
