# Syllabus 08: How AI Models Work — Advanced Practitioner Level
**Status:** Hidden Gap — Deepen What You Already Know
**Format:** One chat session per unit, self-paced

---

## What This Is

You already build complex AI workflows, test local models, and run hybrid architectures. This syllabus covers the deeper mechanics — the things that help you debug hard failures, optimize performance, and make better architectural decisions.

---

## Unit 1 — Why Models Fail in Unexpected Ways

**Goal:** Understand the deeper reasons behind model failure beyond bad prompts.

**Topics:**
- Hallucination mechanics — why models confabulate confidently
- How training data distribution affects reliability on edge cases
- The difference between capability failure and alignment failure
- When retrieval-augmented generation (RAG) helps vs. when it does not

**Session starter:**
> "Go deep on why large language models fail in unexpected ways. Not surface-level prompt issues — I want to understand hallucination mechanics, distribution gaps, and when RAG actually solves the problem vs. when it does not."

---

## Unit 2 — Context Window Architecture Decisions

**Goal:** Make smart decisions about how to structure context in complex workflows.

**Topics:**
- How attention degrades across long contexts (the lost-in-the-middle problem)
- Strategies: chunking, summarization, sliding window, hierarchical retrieval
- How to structure a context window for a multi-step agentic workflow
- Trade-offs between one large context vs. multiple smaller calls

**Session starter:**
> "Explain the lost-in-the-middle problem and how it affects agentic AI workflows. What strategies should I use when designing workflows that require large amounts of context? Walk me through the trade-offs."

---

## Unit 3 — Temperature, Sampling, and Output Control

**Goal:** Go beyond basic temperature settings to fully control model output behavior.

**Topics:**
- Temperature vs. top-p vs. top-k — what each actually controls
- How to use low temperature for structured output and high for ideation
- Structured output formats (JSON mode, XML constraints)
- Repetition penalties and frequency penalties

**Session starter:**
> "Explain temperature, top-p, and top-k sampling in depth. How do they interact? When should I use structured output constraints? I want to understand how to precisely control output behavior across different use cases."

---

## Unit 4 — Local vs. Cloud Model Trade-offs

**Goal:** Make informed architectural decisions about when to use Ollama locally vs. Claude via API.

**Topics:**
- Latency, cost, privacy, and capability trade-offs
- Which tasks suit local models vs. frontier models
- Using local models for embeddings and Claude for reasoning
- When hybrid architecture creates risk vs. value

**Session starter:**
> "Help me think through the architectural trade-offs of local models via Ollama vs. Claude API for different tasks in an AI workflow. When does hybrid make sense? What are the failure modes I should design around?"

---

## Unit 5 — Evaluation — How to Know If Your Workflow Actually Works

**Goal:** Build lightweight evaluation methods for your AI workflows.

**Topics:**
- Why vibes-based testing is not enough for client work
- Simple evals: gold-set testing, regression testing, output grading
- How to detect when a workflow degrades after a model update
- Building a feedback loop into deployed workflows

**Session starter:**
> "Teach me practical evaluation methods for AI workflows. I do not need academic rigor — I need to know when my workflow is actually working vs. when it is quietly failing. What is a lightweight eval approach I can build into my process?"

---

## Unit 6 — Multi-Model Routing and Small Language Models

**Goal:** Design systems that send each task to the right model — not everything to the most expensive one.

**Topics:**
- What Small Language Models (SLMs) are — fast, cheap models like Llama 3 8B that run locally via Ollama
- The "right model, right task" framework: simple tasks to small models, complex reasoning to frontier models
- Building a classifier that reads an incoming prompt and routes it to the correct model
- Cost and latency trade-offs across a multi-model architecture
- How this connects to your existing cost architecture work in Syllabus 13

**Session starter:**
> "Help me design a multi-model routing system in Python. I want a classifier that looks at an incoming prompt and decides whether to send it to a local Ollama model or the Claude API. Walk me through the routing logic, how to define task categories, and how to measure whether the routing decisions are correct."

---

## Unit 7 — Live Monitoring After Deployment

**Goal:** Know when a deployed workflow is failing quietly — before a client notices.

**Topics:**
- The difference between testing before launch and observing after launch
- Tracking response latency as a quality signal
- Detecting output drift — when answers get quietly worse over time
- Structured logging that lets you reconstruct any failure

**Session starter:**
> "Help me build a lightweight monitoring layer for a deployed AI workflow. I want latency tracking, drift detection, and structured logging. What is the minimum I need to catch quiet failures before a client does?"

---

## Practice Project

Pick one workflow you have built. Define three things that would tell you it is working correctly. Write a simple test for each one.
