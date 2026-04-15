# Syllabus 21: Multi-Agent Systems and Orchestration
**Status:** Enterprise Gap - From One Agent to an AI Team
**Format:** One chat session per unit, self-paced

---

## What This Is

Single-agent systems are useful, but enterprise AI is moving toward teams of specialized agents coordinated by an orchestration layer. This syllabus teaches you how to design multi-agent systems that divide work, pass context safely, avoid runaway loops, and stay observable enough to trust in production.

---

## Unit 1 - Why Multi-Agent Systems Matter

**Goal:** Understand when multiple agents are useful and when a single well-designed agent is still better.

**Topics:**
- The shift from "AI as a tool" to "AI as a team"
- Specialist agents: researcher, writer, reviewer, planner, analyst, operator, and policy checker
- When specialization improves quality, context management, and parallel work
- When multi-agent design adds unnecessary cost, latency, and failure modes
- The architecture question: what should be an agent, a tool, a function, or a human decision?

**Session starter:**
> "Teach me when to use a multi-agent system instead of one well-designed agent. I want clear decision rules for when specialization, parallelism, context separation, or review agents are worth the extra complexity."

---

## Unit 2 - Multi-Agent Design Patterns

**Goal:** Learn the core patterns for coordinating specialized agents.

**Topics:**
- Manager-worker: one coordinator delegates subtasks to specialist agents
- Router: classify the request and send it to the right specialist
- Pipeline: research -> draft -> review -> revise -> approve
- Debate and critique: one agent proposes, another challenges, a third decides
- Handoffs: one specialist becomes responsible for the next step
- Parallel agents: multiple specialists work at the same time and a synthesizer combines results

**Session starter:**
> "Walk me through the main multi-agent design patterns: manager-worker, router, pipeline, debate, critique, handoffs, and parallel agents. For each one, show me when it is useful and what can go wrong."

---

## Unit 3 - LangGraph for Controlled Orchestration

**Goal:** Use graph-based orchestration when the system needs explicit state, routing, and recovery.

**Topics:**
- Why graph-based workflows matter for production AI systems
- Nodes, edges, state, conditional routing, and loops
- Durable execution for long-running agent workflows
- Human-in-the-loop checkpoints inside the graph
- Designing stop conditions so agents cannot loop forever
- When LangGraph is better than a simpler chain or role-based crew

**Session starter:**
> "Teach me LangGraph from the perspective of an AI systems architect. Explain nodes, edges, state, conditional routing, durable execution, human-in-the-loop checkpoints, and stop conditions using a research-writer-reviewer workflow."

---

## Unit 4 - CrewAI for Role-Based Agent Teams

**Goal:** Design role-based crews where agents act like a coordinated team with clear responsibilities.

**Topics:**
- Agents, tasks, crews, processes, and flows
- Role design: goal, backstory, tools, constraints, and expected output
- Sequential, hierarchical, and hybrid processes
- Guardrails, callbacks, memory, knowledge, and structured outputs
- When CrewAI fits better than a lower-level graph framework
- Translating a real business process into a crew

**Session starter:**
> "Teach me CrewAI as a role-based multi-agent framework. Show me how to define agents, tasks, crews, processes, flows, guardrails, memory, knowledge, and structured outputs for a realistic enterprise workflow."

---

## Unit 5 - Context, Memory, and Shared State

**Goal:** Control what each agent knows so the system does not drown in context or spread bad assumptions.

**Topics:**
- Context engineering for multi-agent systems
- Private agent context vs. shared state
- What should be passed between agents and what should be hidden
- Memory design: short-term, long-term, task-specific, and client-specific
- Preventing hallucination cascades when agents trust each other too much
- Provenance: tracking which agent produced which claim or decision

**Session starter:**
> "Help me design context, memory, and shared state for a multi-agent AI system. What should each agent see? What should be shared? How do I prevent hallucination cascades and track where each claim came from?"

---

## Unit 6 - Orchestration Safety, Evals, and Observability

**Goal:** Make multi-agent systems measurable, bounded, and debuggable.

**Topics:**
- Stop conditions, loop limits, recursion limits, and cost budgets
- Agent-level evals and whole-system evals
- Trace grading for handoffs, tool calls, routing decisions, and final output
- Monitoring per-agent latency, cost, error rate, and failure reason
- Debugging disagreements between agents
- Choosing release gates for multi-agent changes

**Session starter:**
> "Help me design safety, evals, and observability for a multi-agent AI system. I need loop limits, cost budgets, per-agent tracing, handoff grading, routing evals, and release gates that catch failures before production."

---

## Practice Project

Build a small multi-agent report system. Use a researcher agent, writer agent, reviewer agent, and policy-checker agent. Implement it twice conceptually: once as a LangGraph-style state graph and once as a CrewAI-style role-based crew. Compare the two designs on control, speed, cost, debuggability, safety, and ease of maintenance. Then choose one design and write the architecture brief, eval plan, and production safety checklist.
