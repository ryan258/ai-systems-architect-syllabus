# Syllabus 17: AI System Architecture
**Status:** Enterprise Gap - The Design Layer Above the Workflow
**Format:** One chat session per unit, self-paced

---

## What This Is

You already know how to build AI workflows. This syllabus teaches you how to design the whole system around the workflow: boundaries, dependencies, data movement, failure modes, trade-offs, and the documents that let other people understand why the system exists.

---

## Unit 1 - What an AI Systems Architect Actually Designs

**Goal:** Shift from "I built a workflow" to "I designed a system other people can depend on."

**Topics:**
- The difference between a workflow, an application, a platform, and a system
- Where the model fits inside the larger architecture
- Identifying users, clients, operators, data sources, and external services
- Drawing the boundary between what you own and what you integrate with
- Why architecture is mostly trade-offs, not perfect answers

**Session starter:**
> "Help me understand what an AI systems architect actually designs. I already know how to build AI workflows. Show me the layer above that: system boundaries, dependencies, data flows, users, operators, and trade-offs."

---

## Unit 2 - Architecture Diagrams That Explain the System

**Goal:** Create diagrams that make an AI system legible to both technical and non-technical stakeholders.

**Topics:**
- C4 diagrams: context, containers, components, and code
- Sequence diagrams for multi-step AI workflows
- Data-flow diagrams for privacy and security review
- Dependency maps for APIs, models, databases, tools, and human handoffs
- How to keep diagrams simple enough to maintain

**Session starter:**
> "Teach me how to diagram an AI system using C4 diagrams, sequence diagrams, and data-flow diagrams. Use one of my AI workflows as the example. I want diagrams that explain the system without becoming decorative."

---

## Unit 3 - Architecture Decision Records

**Goal:** Document major technical decisions so future you and future clients understand why the system was built this way.

**Topics:**
- What an Architecture Decision Record (ADR) is
- When a decision deserves an ADR
- Writing the context, decision, alternatives, consequences, and reversal plan
- Recording model, cloud, database, and security decisions
- Making ADRs useful without turning them into busywork

**Session starter:**
> "Help me write Architecture Decision Records for AI systems. Show me the format, when to use one, and how to document decisions like local model vs. cloud model, vector database vs. Postgres, or queue vs. direct API call."

---

## Unit 4 - Capacity, Latency, and Cost Budgets

**Goal:** Estimate whether the system can handle real use before it goes live.

**Topics:**
- Basic capacity planning for AI systems
- Latency budgets across API calls, retrieval, model calls, and post-processing
- Cost budgets per request, per client, and per month
- Bottleneck identification before launch
- Designing graceful degradation when the expensive or slow path is unavailable

**Session starter:**
> "Teach me capacity planning for an AI system. I want to estimate latency, cost per request, monthly cost, bottlenecks, and failure points before launch. Keep it practical for a solo architect."

---

## Unit 5 - Architecture Review Before Build

**Goal:** Review an AI system design before writing code so expensive mistakes are caught early.

**Topics:**
- The questions every architecture review should answer
- Checking data flow, security, reliability, cost, and human handoff
- Comparing two or three architecture options
- Writing a trade-off table that clients can understand
- Deciding what to build now, defer, simplify, or reject

**Session starter:**
> "Run an architecture review on an AI system idea with me. Ask the questions an experienced architect would ask before build: data flow, security, reliability, cost, handoff, and trade-offs. Help me decide what to simplify."

---

## Practice Project

Pick one AI workflow you have already built. Create a one-page architecture brief with a C4 context diagram, a sequence diagram, a data-flow diagram, three ADRs, a latency budget, a cost budget, and a trade-off table comparing two possible designs.
