# Syllabus 11: Advanced Python for AI Workflow Architecture
**Status:** Hidden Gap — Not Basics, But Workflow-Specific Patterns
**Format:** One chat session per unit, self-paced

---

## What This Is

You are a full-stack developer with deep Python expertise. This is not a beginner course. The gap here is specific patterns that come up repeatedly in AI workflow architecture — agentic loops, async handling, structured output parsing, and building internal tools that survive low-energy days.

---

## Unit 1 — Building Reliable Agentic Loops

**Goal:** Write Python loops that handle AI agent workflows without silent failures.

**Topics:**
- Retry logic with exponential backoff for API calls
- How to detect when an agent is stuck or looping
- Logging decisions made by an agent for later review
- Graceful degradation when a step fails

**Session starter:**
> "Help me build a reliable agentic loop in Python. I want retry logic, failure detection, decision logging, and graceful degradation. Show me the patterns I should always use when an AI agent controls flow."

---

## Unit 2 — Async Python for Parallel AI Calls

**Goal:** Run multiple AI calls concurrently without blocking.

**Topics:**
- asyncio basics for AI API calls
- Running parallel Claude API calls with asyncio.gather
- Rate limit handling in async contexts
- When async adds complexity without value

**Session starter:**
> "Show me how to use Python asyncio to run multiple Claude API calls in parallel. Include rate limit handling. When does async actually help vs. add unnecessary complexity for AI workflow scripts?"

---

## Unit 3 — Parsing and Validating Structured AI Output

**Goal:** Reliably extract structured data from AI responses without brittle string parsing.

**Topics:**
- Using Pydantic for output validation
- Prompting strategies that produce clean JSON
- Handling partial or malformed AI output gracefully
- When to use instructor library vs. rolling your own

**Session starter:**
> "Help me build a robust system for parsing structured output from Claude API calls. I want Pydantic validation, clean JSON prompting strategies, and graceful handling of malformed output. What libraries and patterns should I use?"

---

## Unit 4 — Building Internal CLI Tools for Your Own Workflows

**Goal:** Wrap your AI workflows in simple command-line tools you can invoke quickly.

**Topics:**
- Click or Typer for building CLI interfaces
- How to design a CLI for low-energy day use (minimal required flags)
- Packaging a CLI tool so it works from anywhere in your terminal
- How to add it to your dotfiles or daily context system

**Session starter:**
> "Help me build a simple CLI tool using Typer that wraps one of my AI workflows. I want it designed for low-energy day use — minimal flags, smart defaults. Show me how to package it so I can invoke it from anywhere in my terminal."

---

## Unit 5 — Cost Controls and Shutoff Logic

**Goal:** Make sure a stuck loop or bad input can never run up a large bill without warning.

**Topics:**
- Tracking cumulative token usage inside an agentic loop
- Setting hard limits that stop execution when a threshold is hit
- Automatic shutoff for repeated errors or unexpected loop counts
- Logging cost per run so you can spot drift over time

**Session starter:**
> "Help me add cost guardrails to a Python agentic loop. I want cumulative token tracking, a hard stop when a limit is hit, and automatic shutoff for error loops. Show me the implementation pattern."

---

## Unit 6 — Testing AI Workflow Scripts

**Goal:** Write tests that catch breakage without requiring full API calls every time.

**Topics:**
- Mocking API calls in pytest so tests run fast and free
- What to test: input handling, output parsing, error paths
- How to write a regression test when a workflow starts producing bad output
- Lightweight CI for solo projects

**Session starter:**
> "Teach me how to write pytest tests for Python scripts that call the Claude API. I want to mock the API calls so tests are fast and do not cost money. What should I always test? How do I write a regression test when output quality drops?"

---

## Practice Project

Pick one existing Python script from your workflow library. Add retry logic, structured output validation, and at least three pytest tests with mocked API calls.
