# AI Systems Architect Syllabus

This repo is a self-paced curriculum for becoming an AI systems architect: someone who can design, build, deploy, evaluate, secure, explain, and hand off AI systems that other people can depend on.

The individual syllabi are not meant to be read as a flat list. They are a progression. Each phase should produce working artifacts, not just notes.

---

## How to Use This Curriculum

Treat a syllabus as complete only when you have produced something observable:

- A working script, API, workflow, deployment, or integration
- A design artifact such as a diagram, ADR, runbook, risk register, or eval plan
- A short case study explaining the problem, decision, implementation, and result
- A reusable template you can apply to future client work

If a module does not create an artifact, it is only reading.

---

## Continuing Lesson Development in a New Chat

When opening a new chat to continue building lessons, use a prompt that gives the assistant the repo, the continuity rules, and the exact target syllabus. The important instruction is to inspect the existing completed lessons first, then continue in the same structure and quality bar.

Use this reusable prompt:

```text
We're continuing my AI Systems Architect curriculum in:

/Users/ryanjohnson/Projects/ai-systems-architect-syllabus

Please continue the lesson development in the same style and quality bar as the previous completed modules.

Before editing, inspect:
1. LESSON-STANDARDS.md
2. lessons/README.md
3. The target syllabus file
4. The completed lessons for the syllabi already built in the repo so the new lessons stay continuous in tone, structure, artifact quality, and sequencing.

Target for this phase:
Build out lessons/[TARGET-FOLDER] from [TARGET-SYLLABUS-FILE].

Requirements:
- Use the existing lesson structure:
  - Outcome
  - Why This Matters
  - Best-Practice Principles
  - Concepts
  - Walkthrough
  - Practice
  - Review Checklist
  - Common Failure Modes
  - Portfolio Evidence
  - References
- Every lesson must produce a concrete artifact.
- Keep the curriculum practical, grounded, and artifact-driven.
- Preserve the current style: direct, operational, no hype, no vague motivational content.
- Weave in reliability, security, privacy, cost, observability, evaluation, governance, and maintainability where relevant.
- Update lessons/README.md so the new lessons appear in the correct sequence.
- Run consistency checks afterward: file presence, artifact lines, required headings, balanced code fences, ASCII cleanliness, and line counts.
- If you find a gap in the syllabus, add a final integration/review lesson rather than leaving the module as disconnected units.

Please implement the files directly and summarize what changed.
```

---

## Phase 1 - Personal Operating System and Scoping

**Goal:** Build the way you work, scope, and finish before scaling what you build.

- [Syllabus 01: AI Workflow Design](syllabus-01-ai-workflow-design.md)
- [Syllabus 02: Constraint-Driven Design](syllabus-02-constraint-driven-design.md)
- [Syllabus 03: Project Scoping](syllabus-03-project-scoping.md)
- [Syllabus 04: Finishing Systems](syllabus-04-finishing-systems.md)

**Phase artifact:** One documented AI workflow, one scope document, and one completed minimum viable deliverable.

---

## Phase 2 - Architecture, Model, Data, and Implementation Foundations

**Goal:** Learn the architecture frame early, then understand how models behave, how client data creates risk, and how to implement reliable AI workflow code.

- [Syllabus 05: AI System Architecture](syllabus-05-ai-system-architecture.md)
- [Syllabus 06: How AI Models Work](syllabus-06-how-ai-works.md)
- [Syllabus 07: Data Privacy and Client Safety](syllabus-07-data-privacy.md)
- [Syllabus 08: Version Control](syllabus-08-version-control.md)
- [Syllabus 09: Advanced Python for AI Workflow Architecture](syllabus-09-python-basics.md)

**Phase artifact:** One architecture brief, one Python workflow with structured output validation, mocked tests, and a privacy checklist.

---

## Phase 3 - Production Builder Core

**Goal:** Move from local scripts to one controlled, deployable AI service.

- [Syllabus 10: API Design with FastAPI](syllabus-10-api-design.md)
- [Syllabus 11: Data Engineering and Pipelines](syllabus-11-data-engineering.md)
- [Syllabus 12: Production AI Systems](syllabus-12-production-ai-systems.md)
- [Syllabus 13: Cloud Deployment](syllabus-13-cloud-deployment.md)

**Phase artifact:** One AI API or retrieval workflow with production controls, a cloud deployment boundary, a runbook, and either a RAG pipeline or a documented RAG vs. fine-tuning decision.

---

## Phase 4 - Operations, Delivery, Security, Evals, and Governance

**Goal:** Turn that deployed service into something dependable, repeatable, and safe to scale.

- [Syllabus 14: Reliability and Observability](syllabus-14-reliability-and-observability.md)
- [Syllabus 15: Infrastructure as Code and CI/CD](syllabus-15-infrastructure-as-code-and-cicd.md)
- [Syllabus 16: Distributed AI Systems](syllabus-16-distributed-ai-systems.md)
- [Syllabus 17: AI Security Architecture](syllabus-17-ai-security-architecture.md)
- [Syllabus 18: Advanced Evaluation and Release Gates](syllabus-18-advanced-evaluation-and-release-gates.md)
- [Syllabus 19: AI Governance and Risk](syllabus-19-ai-governance-and-risk.md)

**Phase artifact:** One production-readiness package with SLOs, traces/logs/metrics, CI workflow, deployment gates, rollback rules, distributed-runtime controls, threat model, eval suite, release gate, risk register, and incident runbook.

---

## Phase 5 - Enterprise, Multi-Agent, Business, and Portfolio Layer

**Goal:** Design AI systems that fit organizations, then package the work so clients can understand and buy it.

- [Syllabus 20: Enterprise Integration and Change Management](syllabus-20-enterprise-integration-and-change-management.md)
- [Syllabus 21: Multi-Agent Systems and Orchestration](syllabus-21-multi-agent-systems-and-orchestration.md)
- [Syllabus 22: Creative Production Systems](syllabus-22-creative-production-systems.md)
- [Syllabus 23: How to Show Your Work](syllabus-23-show-your-work.md)
- [Syllabus 24: Positioning](syllabus-24-positioning.md)
- [Syllabus 25: Business Foundations](syllabus-25-business-foundations.md)

**Phase artifact:** One enterprise-ready AI system design with a rollout plan, training materials, handoff checklist, multi-agent architecture option, case study, positioning statement, and service offer.

---

## Capstone Sequence

After the five phases, move into the applied capstone track:

- [Syllabus 26: Capstone Sequence](syllabus-26-capstone-sequence.md)

The capstone is where the curriculum becomes proof. Each capstone should be built, documented, reviewed, and turned into a portfolio artifact.

---

## Portfolio Standard

By the end of this curriculum, your portfolio should show:

- At least three working AI systems
- At least one deployed API
- At least one RAG system
- At least one multi-agent system
- At least one enterprise integration plan
- At least one public architecture brief
- At least one eval suite
- At least one runbook
- At least one risk register
- At least three case studies

The target is not to know every framework. The target is to be trusted with AI systems that carry business, security, cost, reliability, and user-experience consequences.
