# Syllabus 26: Capstone Sequence
**Status:** Integration Layer - Turning the Curriculum Into Proof
**Format:** One applied build per capstone, self-paced

---

## What This Is

The previous syllabi teach the domains. This syllabus forces them together. Each capstone should produce a working system, an architecture package, an eval package, an operations package, and a case study.

If the work cannot be shown, tested, explained, or handed off, it is not done.

---

## Non-Negotiable Artifacts

Every capstone must produce:

- A problem statement
- A scope document
- A C4 context diagram
- A sequence diagram
- A data-flow diagram
- At least three Architecture Decision Records (ADRs)
- A threat model
- A privacy and compliance note
- A cost model
- An eval plan
- A runbook
- A rollback or disable plan
- A handoff checklist
- A short case study

---

## Capstone 1 - Streamed AI Workflow API

**Goal:** Build a client-ready AI API that feels responsive and can be deployed safely.

**Build:**
- A FastAPI backend that wraps one AI workflow
- API key authentication and rate limiting
- A streaming endpoint using SSE or WebSockets
- Structured request and response models
- Cost limits and timeout handling
- Structured logs and request IDs
- Deployment to a cloud target from Syllabus 13

**Architecture questions:**
- When should the API return immediately, stream tokens, or create a background job?
- What should happen if the user disconnects mid-stream?
- What should be logged without storing sensitive data?
- What is the rollback plan if the streamed endpoint fails?

**Done when:**
You can demo a streamed response, explain the API contract, show logs for one request, and hand a non-technical client a one-page quickstart.

**Session starter:**
> "Help me plan Capstone 1: a streamed AI workflow API. I need the architecture, endpoints, auth, rate limits, SSE or WebSocket streaming, logging, timeout handling, deployment plan, and client quickstart."

---

## Capstone 2 - Production RAG Knowledge System

**Goal:** Build a retrieval system that can answer questions from a real document collection and be maintained over time.

**Build:**
- A document ingestion pipeline
- Text cleaning and chunking
- Embeddings and vector storage
- Query-time retrieval and answer generation
- Source citation or chunk provenance
- Re-indexing logic for changed documents
- A RAG vs. fine-tuning decision memo

**Architecture questions:**
- What knowledge changes often enough that it should stay in retrieval?
- What behavior, tone, or output structure might justify fine-tuning later?
- How will you detect stale, missing, or low-quality retrieval?
- What data should never be indexed?

**Done when:**
You can ingest a real corpus, ask at least 25 evaluation questions, inspect retrieved chunks, and explain why RAG, fine-tuning, or both fit the use case.

**Session starter:**
> "Help me plan Capstone 2: a production RAG knowledge system. I need ingestion, chunking, embeddings, retrieval, citations, re-indexing, eval questions, and a RAG vs. fine-tuning decision memo."

---

## Capstone 3 - Multi-Agent Report System

**Goal:** Build a bounded multi-agent system that divides work across specialists without losing control.

**Build:**
- A researcher agent
- A writer agent
- A reviewer agent
- A policy-checker agent
- Explicit handoffs between agents
- Shared state and private agent context
- Stop conditions, loop limits, and cost budgets
- Per-agent tracing and evals

**Architecture questions:**
- Which steps deserve agents instead of ordinary functions?
- Should the workflow use a LangGraph-style state graph or a CrewAI-style role-based crew?
- What information should each agent see or not see?
- How will you detect hallucination cascades?

**Done when:**
You can run the system, inspect each agent's contribution, prove it stops safely, and compare the graph-based and crew-based designs in an architecture brief.

**Session starter:**
> "Help me plan Capstone 3: a multi-agent report system with researcher, writer, reviewer, and policy-checker agents. I need orchestration design, context rules, stop conditions, tracing, evals, and a LangGraph vs. CrewAI comparison."

---

## Capstone 4 - Enterprise Integration and Rollout

**Goal:** Design an AI system that fits into a real organization's existing tools and workflows.

**Build:**
- Stakeholder map
- Requirements brief
- Integration map for Slack, email, CRM, Google Workspace, Notion, Airtable, or an internal API
- Before-and-after workflow map
- Pilot rollout plan
- User quickstart
- Operator runbook
- Support and escalation process

**Architecture questions:**
- Who owns the workflow before and after AI is introduced?
- What systems does the AI read from, write to, or trigger?
- What approval gates are needed?
- How will adoption be measured without creating surveillance problems?

**Done when:**
You can explain the system to a user, an operator, a buyer, and a security reviewer, each in their own language.

**Session starter:**
> "Help me plan Capstone 4: an enterprise AI integration and rollout. I need stakeholder discovery, requirements, integration map, before-and-after workflow, pilot plan, training materials, support process, and handoff package."

---

## Capstone 5 - Full AI Systems Architecture Package

**Goal:** Produce the kind of package a top-tier AI systems architect could use to win, build, review, and hand off a serious client project.

**Build:**
- A full architecture brief for one complete AI system
- C4 diagrams, sequence diagram, and data-flow diagram
- ADRs for model choice, data architecture, deployment, orchestration, and security
- SLOs and observability plan
- Threat model and least-privilege access plan
- Risk register, model card, and data card
- Eval suite and release gates
- Cost model and pricing rationale
- Incident runbook and postmortem template
- Final handoff checklist
- Public case study version with sensitive details removed

**Architecture questions:**
- What are the main trade-offs and why did you choose this design?
- What could fail quietly?
- What should never be automated?
- What is the smallest reliable version of the system?
- What evidence proves the system is ready to ship?

**Done when:**
Another technical person can review the package, understand the system, challenge the decisions, and run the system without you explaining it live.

**Session starter:**
> "Help me plan Capstone 5: a full AI systems architecture package. I need diagrams, ADRs, SLOs, threat model, risk register, eval suite, release gates, cost model, runbook, handoff checklist, and public case study."

---

## Final Portfolio Review

**Goal:** Turn the capstones into evidence that you can operate at an AI systems architect level.

**Review checklist:**
- Does each project solve a real business or workflow problem?
- Can you explain the architecture in five minutes?
- Can you show the working system?
- Can you show what you tested?
- Can you show what you decided not to automate?
- Can you show how the system fails safely?
- Can someone else operate it from the runbook?
- Does the case study explain the result without overselling?

**Session starter:**
> "Review my AI systems architect portfolio. Evaluate my capstones for architecture quality, production readiness, security, eval rigor, governance, business clarity, and client handoff quality. Tell me what is missing before I show this to clients."
