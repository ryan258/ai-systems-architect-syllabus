# Lesson 03.02: Discovery Questions Before Starting

**Parent syllabus:** [Syllabus 03: Project Scoping](../../syllabus-03-project-scoping.md)  
**Estimated time:** 2 hours  
**Artifact:** Discovery question bank and call notes template

---

## Outcome

By the end of this lesson, you can run a structured discovery conversation before agreeing to an AI workflow project.

You will produce a reusable discovery question bank and call notes template.

---

## Why This Matters

Bad projects often look attractive at the beginning because the real work is hidden.

Common failures:

- The client describes a symptom, not the actual problem.
- You agree before knowing who must approve the work.
- The client says "our data" without explaining format, quality, access, or sensitivity.
- You do not know what tools the workflow must fit into.
- You miss the person who will operate the system after handoff.
- You price a prototype while the client expects production support.

Discovery is where you find the truth before your calendar and reputation are committed.

---

## Best-Practice Principles

1. **Start with the business problem.**  
   Do not begin with model choice, prompts, agents, or tools.

2. **Ask what done means.**  
   The client's definition of success is the anchor for scope.

3. **Find the current workflow.**  
   AI should fit into a real process, not an imagined clean-room process.

4. **Identify users, operators, owners, and approvers.**  
   These are often different people.

5. **Classify data early.**  
   Ask what data enters the workflow, where it comes from, and whether it is sensitive.

6. **Find constraints before designing.**  
   Budget, timeline, tools, compliance, user energy, approval cycles, and access limits shape the solution.

7. **End with open questions.**  
   A good discovery call should reveal what must be confirmed before scope is final.

---

## Concepts

### Discovery

The structured process of learning enough to decide whether a project is viable, valuable, and bounded.

Discovery is not free implementation. It should produce clarity, not hidden labor.

### Stakeholder

Anyone affected by the project or able to block it.

Stakeholder types:

- Buyer
- Daily user
- Operator
- Data owner
- Security or IT reviewer
- Legal or compliance reviewer
- Executive approver

### Workflow Reality

The actual way work happens today, including shortcuts, exceptions, delays, and manual fixes.

### Constraint

A limit that must shape the project.

Examples:

- "The team only works in Slack."
- "No customer data can leave approved systems."
- "The first version must be demo-ready in two weeks."
- "No one on the team can maintain custom infrastructure."

---

## Walkthrough

Client request:

> We need an AI assistant to help our team write proposals faster.

Weak discovery:

- What kind of assistant do you want?
- What model do you prefer?
- How many proposals do you write?

Better discovery:

### Problem questions

- What part of proposal writing is slowest today?
- What mistakes happen repeatedly?
- What does a successful proposal draft look like?
- What should the AI never decide on its own?

### Current workflow questions

- Where do proposal inputs come from?
- Who writes the first draft now?
- Who reviews it?
- Where does the final proposal live?
- What tools must the workflow integrate with or avoid?

### Data questions

- What source material will the workflow use?
- Does it include customer data, pricing, contracts, or confidential strategy?
- Can examples be anonymized?
- What should never be pasted into an AI tool?

### Scope questions

- Is phase one a prototype, internal workflow, or production system?
- How many proposal types are included?
- How many review cycles are expected?
- What is explicitly not part of phase one?

### Ownership questions

- Who decides whether the draft is good enough?
- Who maintains the prompts after handoff?
- Who approves changes if the scope shifts?
- Who signs off on completion?

---

## Discovery Call Notes Template

Use this structure during or after the call:

```markdown
# Discovery Notes: [Client / Project]

## Client Request

What did the client ask for in their own words?

## Business Problem

What pain, cost, delay, or risk are they trying to reduce?

## Current Workflow

How does the work happen today?

## Users and Stakeholders

- Buyer:
- Daily users:
- Operator:
- Approver:
- Data owner:
- Security or compliance reviewer:

## Inputs and Data

- Source data:
- Sensitive data:
- Data access needed:
- Data that must not be used:

## Desired Output

What should the system or workflow produce?

## Definition of Done

What would make the client say phase one is complete?

## In-Scope Candidates

- ...

## Out-of-Scope Candidates

- ...

## Constraints

- Timeline:
- Budget:
- Tools:
- Compliance:
- Maintenance:
- Review:

## Open Questions

- ...

## Recommended Next Step

Decline, clarify, paid discovery, prototype, or scoped build.
```

---

## Practice

Build your discovery question bank.

Create at least five questions in each category:

1. Business problem
2. Current workflow
3. Users and stakeholders
4. Data and privacy
5. Deliverables and done
6. Exclusions
7. Timeline and decision process
8. Maintenance and handoff

Then pick one imagined AI workflow project and complete the call notes template.

---

## Review Checklist

Your discovery materials are acceptable when:

- They start with the client's problem, not your preferred solution.
- They uncover current workflow reality.
- They identify users, operators, owners, and approvers.
- They ask about data sensitivity and access.
- They force a definition of done.
- They reveal what is not included.
- They end with open questions and a recommended next step.
- They are concise enough to use during a real call.

---

## Common Failure Modes

- **Tool-first discovery:** You ask about models before understanding the workflow.
- **No stakeholder map:** You scope with the buyer but ignore the operator or approver.
- **Data vagueness:** You accept "we have docs" without asking where, how many, what format, and how sensitive.
- **Skipping exclusions:** You leave the hard boundary for later.
- **Free consulting drift:** The discovery call becomes solution design without a paid next step.
- **No decision owner:** You cannot close the scope because no one owns approval.

---

## Portfolio Evidence

Save:

- Discovery question bank
- Discovery notes template
- One completed sample discovery note
- List of qualification red flags

This becomes your repeatable front door for paid work.

---

## References

- PlainLanguage.gov guidelines: https://www.plainlanguage.gov/guidelines/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
