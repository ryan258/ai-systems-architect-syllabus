# Lesson 06.02: Context Window Architecture Decisions

**Parent syllabus:** [Syllabus 06: How AI Models Work](../../syllabus-06-how-ai-works.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Context strategy worksheet

---

## Outcome

By the end of this lesson, you can design context on purpose instead of stuffing more text into a larger window and hoping quality holds.

You will produce a context strategy worksheet that defines:

- task-specific context needs
- what must be included
- what must be summarized
- what must be retrieved just in time
- what should never be sent
- how the system behaves when context limits are hit

---

## Why This Matters

Long context creates a new class of architecture mistakes.

Common failures:

- Important instructions are buried in the middle of a long prompt and ignored.
- The system sends too much history, increasing cost without improving quality.
- Sensitive data is included because nobody defined what the model actually needs.
- Retrieval returns many chunks, but the most important evidence is not positioned well enough to influence the answer.
- One giant call is used when two smaller calls would be cheaper, safer, and easier to debug.
- Operators cannot explain why a given answer used one source and ignored another.

Context is not free. It affects quality, cost, latency, privacy, and maintainability.

---

## Best-Practice Principles

1. **Design context per task, not per workflow.**  
   Different steps need different information. A routing classifier, a policy checker, and a customer-facing draft should not all receive the same context bundle.

2. **Protect the top and bottom of the prompt.**  
   Primacy and recency matter. High-priority instructions and decisive evidence should not be buried.

3. **Send authority, not clutter.**  
   Prefer the smallest set of trustworthy context that lets the model do the job.

4. **Summarize history before it becomes noise.**  
   If old context still matters, compress it into a durable summary instead of dragging the full transcript forever.

5. **Treat context limits as security and privacy boundaries.**  
   Define what data may enter the prompt, what must be redacted, and what must stay in internal systems.

6. **Log context decisions without logging sensitive content.**  
   Record source IDs, chunk counts, truncation reasons, and token budgets so failures can be reconstructed.

7. **Prefer multi-step context architecture when review questions differ.**  
   One large call is not automatically better than retrieval plus a second focused synthesis call.

---

## Concepts

### Context Window

The amount of text or multimodal input the model can process in a single request.

The practical question is not only "How large is the window?" It is:

> What information still influences the output at the point where the model needs it?

### Lost in the Middle

A common long-context failure pattern where information placed deep in the middle of the prompt has less influence than information near the beginning or end.

This matters for:

- long instructions
- long chat histories
- many retrieved chunks
- combined system prompts plus tools plus documents

### Context Contract

A written rule set for what each workflow step is allowed to receive.

Example:

- classification step gets ticket title, first message, and customer tier
- policy-check step gets draft plus policy excerpts only
- drafting step gets ticket summary, selected evidence, and output schema

### Hierarchical Retrieval

A strategy where the system retrieves or summarizes in stages instead of sending raw source material all at once.

Example:

1. retrieve candidate policies
2. rerank or summarize
3. send only the top evidence to the drafting step

### Context Degradation Plan

A defined behavior for what happens when context is too large, too expensive, or too sensitive.

Examples:

- summarize first
- route to background processing
- ask the user to narrow the request
- stop and require human triage

---

## Context Strategy Worksheet

Use this structure:

```markdown
# Context Strategy Worksheet: [Workflow Name]

## Step Name

- Goal of this step:
- Model used:
- Latency target:

## Required Inputs

What information must be present for the step to work?

## Allowed Context Sources

- User input
- Retrieved documents
- Prior workflow outputs
- Metadata
- Tool outputs

## Do Not Include

What should never be sent to this step?

## Context Budget

- Max tokens or characters:
- Preferred budget:
- Reserved budget for output:

## Priority Order

What should appear first, last, and never in the middle if it is critical?

## Summarization Rules

When do we summarize instead of sending raw history?

## Retrieval Rules

How many chunks, from which sources, with what ranking logic?

## Truncation or Overflow Plan

What happens when the budget is exceeded?

## Observability

What source IDs, counts, timings, and truncation reasons will be logged?

## Privacy and Security Notes

What must be redacted, masked, or held back?
```

---

## Walkthrough

Use the support response drafting and review system again.

We will design context for three steps:

1. ticket triage
2. response drafting
3. policy check

### Step 1 - Do not reuse the same context everywhere

Weak design:

> Send the full ticket thread, customer history, all retrieved policies, all previous drafts, and all Slack comments to every model call.

Problems:

- expensive
- hard to debug
- privacy exposure grows
- important evidence gets buried

Better design:

- **Triage step:** ticket title, latest customer message, customer tier, and ticket metadata
- **Draft step:** ticket summary, key recent facts, top policy excerpts, and output format
- **Policy-check step:** draft plus authoritative policy excerpts only

Each step gets the context it needs for its review question.

### Step 2 - Protect placement of critical context

For the drafting step:

1. system instruction and non-negotiable constraints
2. brief task summary
3. selected ticket facts
4. top policy excerpts
5. output schema and review flags

Do not bury the policy excerpts between low-value history blocks.

Review question:

> If the model ignored one thing, what is the one thing that would cause the biggest failure?

Place that information where the model is more likely to use it.

### Step 3 - Summarize before history turns into noise

Suppose the ticket thread is 30 messages long.

Do not keep appending raw history forever.

Instead:

- create a running summary with named facts
- preserve the last two raw customer turns
- keep source links or message IDs for auditability

Example summary:

```text
Ticket summary:
- Customer reports duplicate charge from April 16.
- Prior agent already offered troubleshooting steps.
- Customer is on a business tier contract.
- Customer requested refund and account audit.
- No confirmed fraud determination yet.
```

This keeps decisive facts visible while reducing prompt bloat.

### Step 4 - Decide when one large call becomes two smaller calls

For complex tickets, use a staged design:

1. retrieve and summarize the relevant policy evidence
2. draft the response using only the summarized case facts plus top policy evidence

This is often better than one giant call because it:

- lowers noise
- improves observability
- reduces privacy exposure
- makes debugging easier

It also creates a clearer place to enforce cost limits and human review.

### Step 5 - Make context decisions observable

Log decisions like this:

```text
trace_id=trace_91ab
workflow_step=draft_response
ticket_id=ticket_1042
retrieved_policy_ids=policy-refund-general-v3,policy-loyalty-exceptions-v1
chunk_count=3
history_mode=summarized
context_chars=14820
overflow_action=none
redaction_profile=customer_pii_masked
```

This lets you reconstruct why the model saw what it saw without storing the sensitive prompt body in logs.

---

## Practice

Choose one workflow with at least three distinct model or model-adjacent steps.

Create a context strategy worksheet for each step.

Each worksheet must define:

1. step goal
2. required inputs
3. allowed sources
4. prohibited data
5. context budget
6. ordering strategy
7. summarization rules
8. retrieval rules
9. overflow plan
10. observability and privacy notes

End with one architecture decision:

- keep one large call
- split into multiple smaller calls
- add summarization before model invocation
- add human triage before context assembly

---

## Review Checklist

Your context strategy is acceptable when:

- Each workflow step has its own context contract.
- Critical instructions or evidence are not buried.
- The strategy defines what is excluded, not only what is included.
- At least one summarization rule exists.
- At least one overflow or truncation behavior exists.
- Retrieved sources are limited by purpose, not "more is better."
- Privacy-sensitive data handling is explicit.
- Logging records source IDs, counts, or truncation reasons without dumping raw sensitive content.
- The strategy explains when a second call is better than a larger first call.
- The final design can be understood on a low-energy maintenance day.

---

## Common Failure Modes

- **Context stuffing:** Every available document is sent because the window is large enough.
- **No step boundaries:** The same context bundle is reused for classification, drafting, and policy review.
- **Middle burial:** The most important evidence sits in the least influential position.
- **Silent truncation:** The system drops context without telling operators what was removed.
- **No privacy contract:** Sensitive data enters prompts by default.
- **No observability:** Nobody can tell which evidence the answer actually had access to.

---

## Portfolio Evidence

Save:

- The context strategy worksheet set
- One diagram or note showing the staged context flow
- One short explanation of why you chose one large call or multiple smaller calls

This shows you can design context as architecture, not just prompt length.

---

## References

- Attention Is All You Need: https://arxiv.org/abs/1706.03762
- Lost in the Middle: How Language Models Use Long Contexts: https://arxiv.org/abs/2307.03172
- Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks: https://arxiv.org/abs/2005.11401
- OpenTelemetry concepts: https://opentelemetry.io/docs/concepts/
