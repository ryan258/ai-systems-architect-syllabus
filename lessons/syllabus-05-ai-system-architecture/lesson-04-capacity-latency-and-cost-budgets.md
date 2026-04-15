# Lesson 05.04: Capacity, Latency, and Cost Budgets

**Parent syllabus:** [Syllabus 05: AI System Architecture](../../syllabus-05-ai-system-architecture.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Capacity, latency, and cost budget worksheet

---

## Outcome

By the end of this lesson, you can estimate whether an AI system is realistic before building it.

You will produce a worksheet that estimates:

- Expected users and request volume
- Latency budget by system step
- Cost per request
- Monthly cost range
- Bottlenecks and rate limits
- Degradation plan when the slow or expensive path fails

---

## Why This Matters

AI systems can fail financially and operationally even when the code works.

Common failures:

- A workflow is too slow for the user experience.
- The model provider rate-limits the system during peak use.
- Token costs are acceptable in testing but expensive in production.
- Retrieval is fast alone but slow when combined with model calls and post-processing.
- A synchronous API blocks while a long workflow runs.
- A system has no plan for what happens when the model provider is slow.

Architects do not need perfect estimates. They need estimates good enough to catch bad designs before implementation.

---

## Best-Practice Principles

1. **Budget the whole path, not just the model call.**  
   User experience includes auth, upload, validation, retrieval, model call, tools, post-processing, storage, and response delivery.

2. **Use ranges, not fake precision.**  
   Early estimates should be low, expected, and high. A single number hides risk.

3. **Separate interactive and background work.**  
   If the user is waiting, latency matters immediately. If a worker is processing a job, reliability and progress visibility matter more.

4. **Track tail latency, not only averages.**  
   Average response time hides the slow requests that shape user experience.

5. **Treat provider limits as architecture constraints.**  
   Token limits, rate limits, request limits, and concurrency limits are part of the design.

6. **Put hard brakes on spend.**  
   Every production AI system needs per-request, per-run, per-client, and monthly cost boundaries.

7. **Design graceful degradation.**  
   The system should have a cheaper, slower, smaller, or human-assisted path when the ideal path is unavailable.

---

## Concepts

### Capacity

How much work the system can handle over time.

Examples:

- Requests per minute
- Documents ingested per hour
- Concurrent users
- Jobs waiting in queue
- Model calls per minute

### Latency Budget

The time allowed for each step in a request or workflow.

Example:

- Auth and validation: 100 ms
- Retrieval: 700 ms
- Model call: 8 seconds
- Post-processing: 400 ms
- Response delivery: 300 ms

### Cost Budget

The acceptable cost of running the system.

Examples:

- Cost per request
- Cost per workflow run
- Cost per client per month
- Maximum daily spend
- Cost of retries and failures

### Bottleneck

The part of the system that limits throughput, latency, or cost.

Examples:

- Model provider rate limits
- Vector database latency
- File parsing speed
- Queue worker count
- Human approval availability

### Graceful Degradation

A planned reduced-capability mode when the ideal path is unavailable.

Examples:

- Use a smaller model for low-risk tasks.
- Skip non-essential enrichment.
- Queue work for later.
- Return partial results.
- Ask for human review.
- Disable expensive features when budget is near the limit.

---

## Walkthrough

Use this example:

> A client wants a support response drafting system that processes 500 tickets per day and posts drafts to Slack.

### Step 1 - Estimate volume

```text
Users:
- 12 support agents
- 2 managers

Ticket volume:
- Low: 200 tickets/day
- Expected: 500 tickets/day
- High: 1,500 tickets/day

Peak:
- 30% of tickets arrive during a 2-hour morning window
- Expected peak: 150 tickets / 2 hours = 75 tickets/hour
- High peak: 450 tickets / 2 hours = 225 tickets/hour
```

Review question:

> Can the queue and worker setup handle the high peak without producing an unacceptable backlog?

### Step 2 - Decide interaction mode

This system does not need to respond to the ticketing webhook with a full draft. It can:

1. Accept the webhook.
2. Return `202 Accepted`.
3. Process in the background.
4. Post the draft to Slack.

Architecture decision:

> Use background jobs because the model path is too slow and variable for synchronous webhook processing.

### Step 3 - Build a latency budget

```text
Webhook request path:
- Signature validation: 100 ms
- Payload validation: 100 ms
- Idempotency check: 100 ms
- Enqueue job: 100 ms
- Return 202: 100 ms

Target webhook response: under 500 ms

Background job path:
- Fetch ticket details: 1 sec
- Retrieve related policy snippets: 1 sec
- Model draft: 8-20 sec
- Policy check: 2-5 sec
- Post to Slack: 1 sec

Expected draft availability: 15-30 sec
High-latency fallback: if over 60 sec, post "draft delayed" status
```

Review question:

> Which path is user-facing, and which path can take longer without breaking the integration?

### Step 4 - Build a cost budget

Use estimates until real token measurements exist.

```text
Per ticket expected:
- Input tokens: 4,000
- Output tokens: 700
- Policy check tokens: 1,500
- Retry allowance: 10%

Per ticket cost:
- Low: $0.01
- Expected: $0.03
- High: $0.10

Monthly ticket volume:
- Low: 4,000
- Expected: 10,000
- High: 30,000

Monthly model cost:
- Low: $40
- Expected: $300
- High: $3,000

Hard controls:
- Max ticket length before summarization: 20,000 chars
- Max retries per step: 2
- Max model cost per ticket: $0.25
- Daily client budget alert: $25
- Monthly client hard stop: $750 unless approved
```

Review question:

> What design choice prevents one long or malformed ticket from becoming a runaway cost?

### Step 5 - Find bottlenecks

Likely bottlenecks:

- Model provider rate limit
- Retrieval latency
- Worker concurrency
- Slack API limits
- Human review availability

Mitigations:

- Queue jobs and process with bounded concurrency.
- Add idempotency keys.
- Cache common policy retrieval results.
- Alert on queue depth.
- Escalate when drafts wait too long for review.

### Step 6 - Define graceful degradation

```text
If model provider is slow:
- Queue job and post delayed status after 60 seconds.

If retrieval fails:
- Generate draft with no policy claims and mark "policy context unavailable."

If monthly budget is near limit:
- Disable low-priority auto-drafts and require manual trigger.

If Slack is down:
- Keep job result stored as pending notification and retry later.

If ticket is too long:
- Summarize first or require human triage.
```

---

## Practice

Pick one AI system idea.

Create a capacity, latency, and cost worksheet with these sections:

1. **Users and usage**
   - Number of users
   - Requests or jobs per day
   - Peak usage estimate

2. **Interaction mode**
   - Synchronous response, streaming response, background job, batch job, or human-triggered workflow

3. **Latency budget**
   - Step-by-step time estimate
   - Target response time
   - Slow-path behavior

4. **Cost budget**
   - Estimated tokens or paid operations per request
   - Cost per request
   - Monthly low, expected, and high estimate
   - Hard limits and alerts

5. **Bottlenecks**
   - Provider limits
   - Database or vector search limits
   - Queue or worker limits
   - Human review limits

6. **Graceful degradation**
   - What happens when the expensive, slow, or unavailable path cannot be used

---

## Review Checklist

Your worksheet is acceptable when:

- It uses low, expected, and high estimates.
- It separates user-facing latency from background job duration.
- It includes at least one provider limit.
- It includes per-request and monthly cost estimates.
- It includes hard cost controls.
- It identifies the likely bottleneck.
- It defines what happens when the model provider is slow or unavailable.
- It includes a human fallback for high-risk failure.
- It names what must be measured after launch to replace guesses with real data.

---

## Common Failure Modes

- **Average-only planning:** The estimate ignores slow tail requests.
- **Model-only budget:** Latency and cost ignore retrieval, tools, retries, logging, and post-processing.
- **No peak estimate:** The design handles daily average but fails during bursts.
- **No hard stop:** Cost alerts exist, but nothing stops a runaway workflow.
- **No retry budget:** Retries double or triple spend without being counted.
- **Synchronous by default:** Long model workflows block request/response paths unnecessarily.
- **No degraded mode:** The system fails closed or fails dangerously when a dependency is slow.

---

## Portfolio Evidence

Save:

- Capacity, latency, and cost worksheet
- One ADR that follows from the worksheet
- A one-paragraph explanation of the biggest bottleneck and mitigation

This shows clients you can think about operations before production.

---

## References

- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- OpenTelemetry concepts: https://opentelemetry.io/docs/concepts/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
