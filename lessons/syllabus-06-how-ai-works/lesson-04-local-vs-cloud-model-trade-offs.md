# Lesson 06.04: Local vs. Cloud Model Trade-Offs

**Parent syllabus:** [Syllabus 06: How AI Models Work](../../syllabus-06-how-ai-works.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Model deployment decision memo

---

## Outcome

By the end of this lesson, you can decide when a task belongs on a local model, a cloud model, or a hybrid path without turning "privacy" or "cost" into slogans.

You will produce a model deployment decision memo that compares:

- task requirements
- capability needs
- latency targets
- privacy constraints
- security risks
- cost boundaries
- observability needs
- operational burden

---

## Why This Matters

Model placement is an architecture decision, not a preference.

Common failures:

- A local model is chosen for customer-facing work it cannot reliably perform.
- A cloud model is used for every task even when a smaller local model would be cheaper and fast enough.
- Teams say "keep it local for privacy" without budgeting hardware, patching, uptime, or operator burden.
- A hybrid architecture is assembled without a routing policy, unified logging, or fallback behavior.
- Sensitive data leaves the local boundary because nobody defined which tasks really need the frontier model.
- Cost comparisons ignore retries, queueing, hardware idle time, and the labor required to run local inference.

The real question is:

> Which model placement gives the system the best risk-adjusted outcome for this task?

---

## Best-Practice Principles

1. **Choose placement per task, not per ideology.**  
   Classification, embeddings, drafting, extraction, reasoning, and moderation may belong on different runtimes.

2. **Capability sets the ceiling.**  
   Privacy savings do not matter if the model cannot do the work reliably enough for the user impact.

3. **Count total operating cost.**  
   Include API spend, retries, tokens, hardware, GPU utilization, maintenance time, and incident burden.

4. **Treat data movement as a design surface.**  
   Decide what can stay local, what can leave the boundary, and what must be redacted or transformed first.

5. **Do not let hybrid architecture hide complexity.**  
   Two model stacks mean two failure surfaces, two evaluation burdens, and often two observability paths.

6. **Design fallback explicitly.**  
   What happens when the local runtime is down, the API provider is slow, or the budget is near limit?

7. **Document the approval logic.**  
   If a task can affect customers, money, privacy, or external systems, write down who approved the placement decision and why.

---

## Concepts

### Local Model

A model run on infrastructure you control directly, such as a workstation, private server, or internal cluster.

Potential advantages:

- lower marginal API cost
- data stays closer to your boundary
- configurable runtimes
- offline or low-connectivity use cases

Potential drawbacks:

- operational burden
- weaker capability on some tasks
- hardware limits
- slower recovery when the host fails

### Cloud Model

A model accessed through a managed API.

Potential advantages:

- stronger frontier capability
- managed availability
- faster access to new model versions
- less infrastructure to operate

Potential drawbacks:

- recurring spend
- external data transfer
- provider dependency
- version drift outside your direct control

### Hybrid Architecture

A system that routes different tasks to different model classes or locations.

Examples:

- local embeddings plus cloud drafting
- local classifier plus cloud escalation
- local redaction pass before cloud reasoning

### Capability Ceiling

The practical upper bound of task quality a given model setup can achieve for the workflow you care about.

This must be measured with evals, not assumed from parameter count or marketing claims.

### Operational Burden

The ongoing work required to keep the model path usable.

Examples:

- host uptime
- GPU memory management
- patching
- model downloads
- version pinning
- capacity planning
- incident handling

---

## Model Deployment Decision Memo

Use this structure:

```markdown
# Model Deployment Decision Memo: [Workflow Name]

## Task

- Task name:
- User impact:
- Accuracy or quality requirement:

## Candidate Placement Options

- Local:
- Cloud:
- Hybrid:

## Evaluation Summary

What evidence do we have for quality on this task?

## Privacy and Security Review

What data leaves the boundary, and what must never leave?

## Latency and Reliability Review

What response time, uptime, and fallback behavior are required?

## Cost Review

- API cost:
- Hardware cost:
- Operator time:
- Hard limits:

## Observability Review

How will route choice, failures, and cost be monitored across placements?

## Decision

Choose local, cloud, or hybrid for this task.

## Conditions and Follow-Up

What must be true for this decision to remain valid?
```

---

## Walkthrough

Use the support response system again.

We will compare three tasks:

1. ticket category classification
2. policy retrieval embeddings
3. customer-facing draft generation

### Step 1 - Do not bundle all tasks together

Weak decision:

> We are a privacy-conscious team, so everything will run locally.

That ignores whether the local model can reliably draft policy-sensitive customer responses.

Better breakdown:

- **Ticket classification:** good local candidate if eval quality is acceptable
- **Embeddings for internal policy retrieval:** strong local candidate
- **Customer-facing draft generation:** likely cloud frontier candidate because quality and edge-case handling matter more

### Step 2 - Evaluate the privacy boundary honestly

Ticket classification may only need:

- subject line
- recent message
- customer tier

That makes local inference attractive if the classifier quality is sufficient.

Draft generation may require:

- summarized ticket facts
- policy evidence
- tone constraints

If this leaves the local boundary for a hosted model, the system should still:

- redact unnecessary personal data
- avoid raw history when a summary is enough
- document retention and logging rules
- define whether vendor training is disabled or contractually controlled

### Step 3 - Count real cost and operations

Local path costs:

- hardware purchase or lease
- operator time
- lower throughput at peak load
- model update handling

Cloud path costs:

- request and token spend
- budget spikes during retries or longer prompts
- provider outages or rate limits

Hybrid path costs:

- all of the above, plus routing and observability complexity

Do not compare "free local" to "paid API." Local is not free.

### Step 4 - Make the decision task by task

Example memo outcome:

```markdown
## Task

- Task name: Customer-facing support draft generation
- User impact: High
- Accuracy or quality requirement: High policy accuracy and clear uncertainty handling

## Candidate Placement Options

- Local: possible but not yet reliable on edge-case policy tickets
- Cloud: strongest current quality
- Hybrid: local redaction plus cloud drafting

## Evaluation Summary

Current eval set shows the frontier cloud model outperforms the local model on policy-exception tickets and ambiguous escalation decisions.

## Privacy and Security Review

Send only summarized ticket facts plus approved policy excerpts. Do not send raw account identifiers or unrelated history.

## Latency and Reliability Review

Draft availability target is under 30 seconds. If the cloud path is unavailable, queue the job and mark delayed. Do not silently downgrade to a weaker local draft for customer-facing use.

## Cost Review

- API cost: acceptable within the monthly support budget
- Hardware cost: not justified for this task alone
- Operator time: lower on cloud path
- Hard limits: per-ticket and monthly spend caps remain required

## Observability Review

Log placement, cost, policy-check result, approval outcome, and fallback events.

## Decision

Use cloud frontier drafting for customer-facing support drafts.

## Conditions and Follow-Up

Revisit when a local model passes the edge-case policy eval set or if privacy requirements tighten.
```

### Step 5 - Define hybrid boundaries carefully

A useful hybrid pattern for this system:

- local redaction or PII masking
- local embeddings for internal policy retrieval
- cloud drafting for customer-facing responses

A risky hybrid pattern:

- local classification
- local first draft
- cloud rescue draft
- cloud policy check
- local final rewrite

That second version is harder to evaluate, harder to monitor, and easier to break.

---

## Practice

Choose one workflow with at least three model-related tasks.

Create a model deployment decision memo that compares local, cloud, and hybrid options for each task.

For each task, include:

1. quality requirement
2. privacy boundary
3. latency target
4. failure impact
5. cost range
6. operational burden
7. observability needs
8. final placement decision

End with:

- one task you would keep local
- one task you would keep cloud
- one task where hybrid is justified or explicitly rejected

---

## Review Checklist

Your model deployment memo is acceptable when:

- It evaluates tasks separately instead of making one blanket decision.
- Quality evidence is mentioned for each important task.
- Privacy review defines what data can and cannot leave the boundary.
- Cost includes hardware or operator burden where relevant.
- Reliability and fallback behavior are explicit.
- Observability covers route choice and failure paths.
- High-impact tasks do not silently downgrade to a weaker model without approval.
- Hybrid complexity is treated as a cost, not a free benefit.
- The final decision includes conditions that would trigger re-evaluation.

---

## Common Failure Modes

- **Ideology over fit:** "local good" or "cloud good" replaces task analysis.
- **Free-local myth:** Hardware, maintenance, and uptime work are ignored.
- **Privacy theater:** Sensitive data still leaves the boundary because prompts were never minimized.
- **Silent downgrade:** A weaker model takes over a high-impact task without notice.
- **Hybrid sprawl:** Multiple model paths are added without routing rules or unified monitoring.
- **No re-evaluation trigger:** The choice stays frozen even when the model landscape or business constraints change.

---

## Portfolio Evidence

Save:

- The model deployment decision memo
- One comparison table across tasks
- One short note explaining where hybrid adds value and where it adds risk

This shows you can place models as an architect, not a fan.

---

## References

- Anthropic docs: https://docs.anthropic.com/
- Ollama: https://ollama.com/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
