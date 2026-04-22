# Lesson 06.06: Multi-Model Routing and Small Language Models

**Parent syllabus:** [Syllabus 06: How AI Models Work](../../syllabus-06-how-ai-works.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Routing policy and routing evaluation scorecard

---

## Outcome

By the end of this lesson, you can route work to the right model class instead of sending every request to the most expensive path.

You will produce:

- a routing policy
- task categories
- fallback rules
- routing metrics
- a routing evaluation scorecard

---

## Why This Matters

Routing can cut cost and latency, but bad routing creates a new failure layer.

Common failures:

- Low-risk tasks are sent to an expensive frontier model by default.
- High-risk tasks are misrouted to a weaker model because the router optimized for cost only.
- Routing rules are vague, so operators cannot explain why a given request took a given path.
- The router is evaluated on its own confidence instead of end-to-end workflow outcomes.
- Teams add a learned classifier too early when simple rules would be easier to maintain.
- The fallback path is undefined when the local model is slow, unavailable, or uncertain.

Routing is only good when the wrong route is rare, visible, and recoverable.

---

## Best-Practice Principles

1. **Start with routing by task class, not model enthusiasm.**  
   Ask what the task is, what happens if it fails, and how much variability is acceptable.

2. **Prefer explicit rules before learned routing.**  
   A small, readable rule set is easier to review than an opaque router model.

3. **Optimize for total system value, not cost alone.**  
   A cheap wrong route can cost more in rework, delay, or risk than an expensive correct route.

4. **Give high-impact tasks a conservative default.**  
   When the route decision is uncertain and failure cost is high, send it to the stronger path or to human review.

5. **Evaluate the router separately and end to end.**  
   The route may look correct on paper and still harm the actual workflow.

6. **Log every route decision.**  
   Record the category, target model, confidence if used, latency, cost, and override or fallback events.

7. **Keep the routing policy maintainable.**  
   If the team cannot understand the router on a low-energy day, the cost savings are not worth it.

---

## Concepts

### Small Language Model

A relatively small model, often run locally or on cheaper infrastructure, used for narrow or lower-risk tasks.

Common uses:

- classification
- extraction
- query rewriting
- simple summarization
- lightweight moderation

### Router

The logic that decides which model path a request should take.

Router styles:

- rule-based
- score threshold
- classifier-based
- staged routing with fallback

### Misroute Cost

The cost of sending a task to the wrong model path.

Examples:

- customer-facing draft quality drops
- human reviewers spend more time correcting outputs
- latency improves but trust falls
- security review misses a risky prompt

### Fallback Path

The path used when the preferred route cannot be used safely.

Examples:

- send to stronger model
- ask user to narrow the request
- queue for later
- require human handling

### Shadow Evaluation

Testing route decisions on labeled or replayed requests without affecting live users.

This is useful before enabling automatic routing in production.

---

## Routing Policy Template

Use this structure:

```markdown
# Routing Policy: [Workflow Name]

## Task Categories

| Category | Example prompt | Risk level | Preferred model path | Fallback path |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## Routing Rules

What simple rules decide the route?

## Escalation Rules

When must the request go to a stronger model or a human?

## Logging

What route metadata will be recorded?

## Evaluation Plan

How will routing quality be measured?

## Overrides

Who can override the route, and how is that tracked?
```

---

## Walkthrough

Use the support response system.

We want to route three kinds of work:

1. ticket categorization
2. query rewriting for policy retrieval
3. customer-facing draft generation

### Step 1 - Start with readable categories

Example categories:

- **Simple triage:** label ticket type and priority
- **Retrieval preparation:** rewrite the search query for policy lookup
- **High-stakes drafting:** produce a customer-facing draft that may mention policy, refunds, or escalation

Likely routing:

- simple triage -> local small model
- retrieval preparation -> local small model
- high-stakes drafting -> cloud frontier model

### Step 2 - Write explicit rules before training a router

Example rules:

- if the output is customer-facing, route to the frontier path
- if the task is classification or query rewriting, route to the local path
- if the ticket contains refund, legal, compliance, or security terms, skip cheap drafting paths
- if local inference latency exceeds the threshold, fall back to the cloud or queue

That may be enough. Do not add a learned routing classifier until the simple rules stop serving the workflow.

### Step 3 - Define misroute recovery

If a local classifier returns low confidence or malformed output:

- retry once if the issue is transient
- otherwise route to the stronger model
- log the override

If a strong model is unavailable:

- queue high-risk work
- do not silently send it to a weaker drafting model if the user impact is high

### Step 4 - Measure the router

Create a small labeled set of prompts and expected routes.

Example scorecard columns:

- input pattern
- expected route
- actual route
- route correct?
- end-to-end result acceptable?
- extra latency
- cost difference

This matters because a route can be "technically correct" and still produce bad workflow outcomes.

### Step 5 - Log route choice as an operating event

Example:

```text
trace_id=trace_r42
workflow_step=route_decision
ticket_id=ticket_842
task_category=high_stakes_drafting
selected_path=cloud_frontier
reason=customer_facing_policy_content
fallback_used=false
local_model_available=true
```

If this data is missing, routing failures become guesswork.

---

## Practice

Choose one workflow that has at least three task categories.

Create a routing policy and a routing evaluation scorecard.

Your routing policy must include:

1. task categories
2. example prompts
3. risk level per category
4. preferred model path
5. fallback path
6. escalation rules
7. logging fields
8. override rules

Your routing scorecard must contain at least twenty labeled examples with:

- expected route
- actual route
- route correctness
- end-to-end outcome note

---

## Review Checklist

Your routing policy is acceptable when:

- The categories are readable and operationally useful.
- The initial routing logic is simple enough to review.
- High-impact tasks have a conservative route or escalation rule.
- Fallback behavior is explicit.
- The scorecard tests route correctness on labeled examples.
- Route decisions are logged with reasons.
- Overrides are visible.
- Cost and latency are measured alongside route correctness.
- The design explains when a learned router would be justified later.

---

## Common Failure Modes

- **Router first, logic later:** A learned classifier is added before the task categories are clear.
- **Cost-only routing:** Cheap paths win even when quality risk is too high.
- **Silent fallback:** A stronger path fails and the system quietly downgrades to a weaker model.
- **No route labels:** The router cannot be evaluated because expected routes were never defined.
- **No logging:** Operators cannot explain why a request took a given path.
- **Unmaintainable rules:** The route logic becomes too complex for future maintenance.

---

## Portfolio Evidence

Save:

- The routing policy
- The routing evaluation scorecard
- One short note describing the most expensive misroute and how your design prevents it

This shows you can apply "right model, right task" with operational discipline.

---

## References

- Ollama: https://ollama.com/
- Anthropic docs: https://docs.anthropic.com/
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
