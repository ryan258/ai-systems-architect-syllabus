# Lesson 01.03: Building Specialist Personas

**Parent syllabus:** [Syllabus 01: AI Workflow Design](../../syllabus-01-ai-workflow-design.md)  
**Estimated time:** 90-120 minutes  
**Artifact:** Specialist persona contract

---

## Outcome

By the end of this lesson, you can design an AI specialist persona that has a specific job, boundaries, output expectations, and escalation behavior.

You will produce a persona contract that can be used in a workflow without letting the model drift into unrelated work.

---

## Why This Matters

Personas are useful only when they clarify responsibility.

Common failures:

- The persona is theatrical but operationally vague.
- The persona answers outside its expertise.
- The persona changes tone or priorities mid-workflow.
- The persona overrules source material.
- Multiple personas duplicate each other.
- The persona acts like a decision-maker when it should be a reviewer or recommender.

A specialist persona should behave more like a scoped role in a process than a character in a story.

---

## Best-Practice Principles

1. **Define responsibility before voice.**  
   Voice matters less than what the persona is allowed to do.

2. **Give the persona a jurisdiction.**  
   Name the topics, inputs, decisions, and outputs it owns.

3. **Name what is out of bounds.**  
   Personas drift when their limits are implied instead of explicit.

4. **Use personas to separate concerns.**  
   A reviewer persona should review. A writer persona should write. A policy checker should check policy.

5. **Avoid false authority.**  
   A persona can recommend, flag, or summarize. It should not make legal, medical, financial, or high-impact decisions without human review.

6. **Include escalation behavior.**  
   The persona needs rules for uncertainty, conflict, missing data, and high-risk output.

7. **Test for drift.**  
   A persona is reliable only if it stays in role across boring, ambiguous, adversarial, and edge-case inputs.

---

## Concepts

### Persona Contract

A reusable definition of a specialist role.

It includes:

- Role
- Purpose
- Inputs
- Owned decisions
- Out-of-scope decisions
- Output contract
- Voice or style
- Escalation rules
- Drift tests

### Role Drift

When a persona starts doing work outside its defined responsibility.

Examples:

- A reviewer rewrites instead of reviewing.
- A writer invents facts instead of using source material.
- A strategist gives legal advice.
- A policy checker creates policy instead of checking against supplied policy.

### Escalation Rule

A rule that tells the persona when to stop, flag uncertainty, or send the decision to a human.

---

## Walkthrough

Use this weak persona:

```text
You are an expert consultant. Be strategic and help me improve this proposal.
```

Problems:

- "Expert consultant" is too broad.
- "Improve" is undefined.
- The persona may rewrite, scope, price, advise, or invent.
- There is no output contract.
- There are no limits or escalation rules.

Better persona contract:

```text
Role: Proposal Risk Reviewer

Purpose:
Review a draft AI workflow proposal and identify risks before it is sent to a client.

Inputs:
- Draft proposal
- Known client constraints
- Current scope

You own:
- Identifying unclear scope
- Identifying unsupported claims
- Identifying missing assumptions
- Identifying promises that are too broad
- Suggesting clarifying questions

You do not own:
- Rewriting the full proposal
- Setting price
- Making legal claims
- Inventing capabilities not in the proposal
- Approving the proposal for delivery

Output format:
## Highest-Risk Issues
- Issue:
- Evidence:
- Why it matters:
- Suggested revision or question:

## Missing Information
- ...

## Send / Revise / Stop
Return one of: Send, Revise, Stop.
Explain why.

Escalation:
If the proposal includes legal, medical, financial, employment, or regulated-data claims, mark Stop and request human review.
```

What changed:

- The role is narrow.
- Ownership is explicit.
- Out-of-scope behavior is explicit.
- Output supports review.
- The persona cannot silently approve high-risk work.

---

## Practice

Create one specialist persona contract for a workflow you use.

Choose a role:

- Researcher
- Writer
- Reviewer
- Scope checker
- Policy checker
- Client-readiness reviewer
- Security reviewer
- Low-energy workflow assistant

Write:

1. **Role name**
2. **Purpose**
3. **Inputs**
4. **Responsibilities**
5. **Out-of-scope behavior**
6. **Output contract**
7. **Voice or style**
8. **Escalation rules**
9. **Three drift tests**

Drift tests should include:

- A normal input
- An ambiguous input
- An adversarial or distracting input

---

## Review Checklist

Your persona contract is acceptable when:

- The role is narrow enough to test.
- Responsibilities are explicit.
- Out-of-scope behavior is explicit.
- The output format is defined.
- The persona has escalation rules.
- It does not claim authority it should not have.
- It includes at least three drift tests.
- Another person could use the persona in a workflow without extra explanation.

---

## Common Failure Modes

- **Character instead of role:** The persona has a vibe but no operational responsibility.
- **Too much authority:** The persona makes decisions that need human review.
- **No boundaries:** The persona writes, reviews, scopes, and advises all at once.
- **No escalation:** The persona answers even when the responsible behavior is to stop.
- **No tests:** The persona works on the happy path but fails under ambiguity.
- **Duplicate agents:** Multiple personas do the same job with different names.

---

## Portfolio Evidence

Save:

- The persona contract
- Three drift-test inputs and outputs
- A short note explaining why this role is separate from the rest of the workflow

This becomes useful later for multi-agent design, where role boundaries matter even more.

---

## References

- Anthropic prompt engineering docs: https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
