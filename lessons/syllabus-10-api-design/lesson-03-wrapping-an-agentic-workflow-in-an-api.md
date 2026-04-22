# Lesson 10.03: Wrapping an Agentic Workflow in an API

**Parent syllabus:** [Syllabus 10: API Design with FastAPI](../../syllabus-10-api-design.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Long-running workflow API contract and transport decision memo

---

## Outcome

By the end of this lesson, you can expose a multi-step AI workflow through an API without pretending long-running, streaming, or bidirectional behavior is all the same thing.

You will produce:

- a long-running workflow API contract
- a polling, webhook, SSE, or WebSocket decision memo
- disconnect, cancellation, and partial-output rules

---

## Why This Matters

Most workflow APIs break at the interaction model, not at the endpoint syntax.

Common failures:

- A long-running job is forced into a synchronous request until clients time out.
- The API returns `200 OK` while the real work has only been queued.
- Webhooks are added without delivery verification or retry rules.
- SSE is used for something that actually requires client-to-server interaction.
- WebSockets are chosen for prestige when one-way streaming over HTTP would be simpler.
- Disconnects happen, but the server keeps working with no cancellation policy.
- Partial output exists, but the contract never says what a client can trust mid-stream.

Workflow transport choice is architecture. If the interaction model is wrong, the rest of the API will feel unstable no matter how clean the code is.

---

## Best-Practice Principles

1. **Choose the interaction model based on runtime behavior.**  
   Duration, directionality, and retry behavior should drive the design.

2. **Treat job submission and job retrieval as separate concerns.**  
   For long-running work, submission is one contract and result retrieval is another.

3. **Use the smallest transport that fits.**  
   Polling, webhooks, SSE, and WebSockets each have costs. Simpler is better when it satisfies the use case.

4. **Make partial and final states explicit.**  
   Clients need to know what "queued," "running," "partial," "failed," and "complete" mean.

5. **Define disconnect and cancellation behavior.**  
   Network interruptions are normal. The server needs a rule for whether work continues, pauses, or stops.

6. **Do not confuse FastAPI background tasks with full job infrastructure.**  
   Same-process background tasks are fine for small follow-up work, not for every heavy workflow.

7. **Keep transport choice connected to reliability, cost, and observability.**  
   Streaming and long-running work should still be monitored, bounded, and debuggable.

---

## Concepts

### Job Submission Endpoint

An endpoint that accepts work and returns a job ID or resource handle.

### Polling

A client repeatedly asks for job state or results.

### Webhook

A callback sent by your API to a client-provided endpoint when work finishes or changes state.

### Server-Sent Events (SSE)

A one-way stream from server to client over HTTP, typically using `text/event-stream`.

### WebSocket

A two-way persistent connection between client and server.

### Cancellation Policy

The rule defining whether queued or in-flight work can be stopped and what response the client receives.

### Partial Output

Intermediate output that may be useful before the whole workflow is complete.

---

## Transport Decision Memo Template

Use this structure:

```markdown
# Transport Decision Memo: [Workflow Name]

## Workflow Runtime Shape

- Expected duration:
- Output size:
- Does the client need partial output?
- Does the client need to send messages after start?

## Candidate Options

| Option | Good for | Main cost | Decision |
| --- | --- | --- | --- |
| Polling |  |  |  |
| Webhook |  |  |  |
| SSE |  |  |  |
| WebSocket |  |  |  |

## Chosen Interaction Model

- Submit endpoint:
- Status endpoint:
- Result endpoint:
- Stream endpoint, if any:

## State Model

- queued
- running
- partial
- completed
- failed
- cancelled

## Disconnect and Cancellation Rules

- ...

## Observability Notes

- trace IDs
- job IDs
- stream disconnect events
- timeout alerts
```

Example contract pattern:

```markdown
POST /review-jobs
-> returns 202 with job_id

GET /review-jobs/{job_id}
-> returns current status and metadata

GET /review-jobs/{job_id}/events
-> optional SSE stream for progress

POST /review-jobs/{job_id}/cancel
-> requests cancellation if still queued or running
```

---

## Walkthrough

Use this example:

> A proposal-review workflow reads uploaded documents, retrieves reference material, drafts a review summary, and streams progress updates to a client dashboard.

### Step 1 - Reject the default synchronous instinct

If the workflow might take 20 to 60 seconds and touch several steps, a plain synchronous request is usually a bad fit.

Symptoms of the bad fit:

- client or proxy timeouts
- no visible progress
- confusing retries
- duplicate work when clients retry

This is where a job model usually earns its keep.

### Step 2 - Separate submission from retrieval

Good baseline:

- `POST /proposal-review-jobs`
- `GET /proposal-review-jobs/{job_id}`

The first endpoint creates work. The second reports state.

This supports reliability, retries, and observability much better than "wait until everything is done."

### Step 3 - Choose the client notification path

Use polling when:

- the client can tolerate periodic checks
- implementation simplicity matters
- output is not truly live

Use webhooks when:

- the client system can receive callbacks
- eventual notification matters more than live token flow

Use SSE when:

- the client needs one-way live progress or streamed model output
- the client does not need to keep sending messages over the same connection

Use WebSockets when:

- the client and server both need to send messages during the same session
- the interface is genuinely interactive

That is the review question. Not "Which option sounds more modern?"

### Step 4 - Define state transitions

Example states:

- `queued`
- `running`
- `partial`
- `completed`
- `failed`
- `cancelled`

If the states are not defined, operators and clients will each invent different meanings.

### Step 5 - Handle disconnects and cancellation

For streaming:

- if the client disconnects, decide whether the workflow keeps running
- if a cancellation request arrives, define which states can still be cancelled
- if partial output was sent, define whether it is authoritative or only advisory

This is especially important for cost and reliability.

### Step 6 - Use FastAPI background tasks carefully

FastAPI's `BackgroundTasks` are useful for small same-process follow-up work after sending a response.

The FastAPI docs explicitly caution that heavier background computation may need bigger tools such as a queue-backed worker system.

Inference:

For long, expensive, or multi-server workflows, design a real job path instead of assuming in-process background execution is enough.

### Step 7 - Make the stream observable

For any long-running or streaming path, log:

- request ID
- job ID
- stream start and end
- disconnect event
- cancellation request
- final state

Streaming without observability becomes hard to debug quickly.

---

## Practice

Choose one workflow that takes longer than a typical short request or that benefits from partial progress.

Create:

1. a transport decision memo
2. a long-running workflow API contract
3. a state model with at least five states
4. disconnect and cancellation rules
5. one rationale for why you chose polling, webhook, SSE, or WebSocket

Use a real example such as:

- document review
- multi-step proposal generation
- CRM enrichment
- chat-style drafting assistant

---

## Review Checklist

Your workflow API artifact is acceptable when:

- The interaction model matches the runtime behavior.
- Submission and result retrieval are separated when needed.
- Polling, webhook, SSE, and WebSocket trade-offs are considered explicitly.
- Job states are defined.
- Disconnect and cancellation rules exist.
- Background tasks are not treated as universal job infrastructure.
- Observability fields are named.

---

## Common Failure Modes

- **Synchronous denial:** Long work is forced into one response.
- **Transport prestige:** WebSockets are chosen where SSE or polling would be simpler.
- **No state model:** Clients cannot tell what the job is doing.
- **No disconnect rule:** The server and client disagree on whether work continues.
- **No cancellation path:** Users retry because they cannot stop stale work.
- **Opaque streaming:** Partial output is sent with no guidance on what it means.

---

## Portfolio Evidence

Save:

- the transport decision memo
- the long-running workflow API contract
- one state-transition summary or diagram

This shows that you can expose real workflows through an API without collapsing into naive request/response assumptions.

---

## References

- FastAPI Background Tasks: https://fastapi.tiangolo.com/tutorial/background-tasks/
- FastAPI Custom Response and StreamingResponse: https://fastapi.tiangolo.com/advanced/custom-response/
- FastAPI WebSockets: https://fastapi.tiangolo.com/advanced/websockets/
- WHATWG HTML Standard, Server-Sent Events: https://html.spec.whatwg.org/dev/server-sent-events.html
- RFC 6455: The WebSocket Protocol: https://www.rfc-editor.org/rfc/rfc6455.html
- RFC 9110: HTTP Semantics: https://www.rfc-editor.org/rfc/rfc9110
