# Lesson 10.01: What an API Is When You Are the Builder

**Parent syllabus:** [Syllabus 10: API Design with FastAPI](../../syllabus-10-api-design.md)  
**Estimated time:** 2-3 hours  
**Artifact:** API boundary brief and endpoint inventory

---

## Outcome

By the end of this lesson, you can take an internal workflow and define the external API boundary that turns it into a product surface another system can call safely.

You will produce:

- an API boundary brief
- an endpoint inventory
- a first pass at sync versus async decisions

---

## Why This Matters

Calling APIs and designing them are different jobs.

Common failures:

- A script is exposed directly with its internal steps visible to the client.
- The API leaks tool-specific details instead of offering a stable business action.
- Long-running work is forced into a synchronous request because the builder never defined a real boundary.
- Inputs, outputs, and side effects are unclear, so the client integration becomes brittle.
- The API works for one caller but cannot support audit, auth, or versioning later.
- Error behavior reflects internal stack structure instead of a client contract.

An API is not just "a way to run the code remotely." It is a public interface with responsibilities.

---

## Best-Practice Principles

1. **Design the API around the caller's job, not your internal implementation.**  
   The client should call "draft a reply" or "submit a review job," not "run step three of my prompt chain."

2. **Define the boundary before the framework.**  
   FastAPI is an implementation tool. The boundary, resource names, and side effects are design work.

3. **Keep the external surface smaller than the internal workflow.**  
   Many internal steps can stay private while one stable endpoint represents the business action.

4. **Name side effects explicitly.**  
   If the API writes to a CRM, queues a job, or emits a webhook, that must be part of the contract.

5. **Decide early whether the interaction is synchronous or asynchronous.**  
   Latency, reliability, and operator experience all depend on this.

6. **Assume the caller is not sitting next to you.**  
   The contract must be understandable without your memory, your terminal, or your source code open.

7. **Treat the API boundary as a governance surface.**  
   Ownership, auth, rate limits, logging, and versioning start at the boundary.

---

## Concepts

### API Boundary

The line between your system and everything outside it.

It defines:

- who can call the service
- what inputs are accepted
- what outputs are guaranteed
- what side effects can happen
- what errors can be returned

### Resource

The thing the API exposes conceptually.

Examples:

- draft request
- review job
- classification result
- webhook subscription

### Operation

An action a client can ask the API to perform.

Examples:

- submit document
- get job status
- cancel run

### Synchronous Request

A request where the client waits for the result in the same response.

### Asynchronous Job

A request where the client submits work and receives a job handle or ID for later retrieval.

### Idempotency

The property that the same request can be retried safely without creating duplicate side effects.

This matters when clients retry after timeouts or network failure.

---

## API Boundary Brief Template

Use this structure:

```markdown
# API Boundary Brief: [Workflow Name]

## Workflow Goal

What business action is this API exposing?

## Primary Callers

- ...

## Main Resources

- ...

## Endpoint Inventory

| Endpoint | Method | Purpose | Sync or async | Side effects |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## Required Inputs

- ...

## Guaranteed Outputs

- ...

## Error Classes

- validation error
- auth failure
- rate limit
- not found
- internal processing failure

## Ownership

- Product owner:
- Operator:
- Security approver:
```

Ask these review questions:

- What is the client trying to achieve?
- What should stay private inside the workflow?
- Which operations deserve their own endpoint?
- Which side effects must be visible to the caller?
- Does the workflow need a synchronous response or a job model?

---

## Walkthrough

Use this example:

> A client's CRM should call an API to draft a follow-up email after a sales call summary is saved.

### Step 1 - Name the business action

Weak API description:

> Expose my LLM workflow.

Better API description:

> Accept a call summary and return or queue a draft follow-up email for human review.

The second description gives the caller a stable reason to exist.

### Step 2 - Hide the internal steps

Internal workflow steps might include:

- transcript cleanup
- intent classification
- prompt selection
- draft generation
- tone check

The caller does not need five endpoints for those.

The external surface might only need:

- `POST /follow-up-jobs`
- `GET /follow-up-jobs/{job_id}`

This protects maintainability and allows internal changes without breaking clients.

### Step 3 - Define the side effects

Questions:

- Does this endpoint only return a draft?
- Does it also write to the CRM?
- Does it notify a reviewer?
- Does it queue background work?

If the answer is "sometimes," the contract is weak.

### Step 4 - Decide sync versus async early

If the workflow usually finishes in under a second or two and the response is small, synchronous may be fine.

If the workflow can take many seconds, call external tools, or require review, an async job model is usually better.

This is an architectural decision, not a later optimization.

### Step 5 - Inventory the endpoints

Example:

| Endpoint | Method | Purpose | Sync or async | Side effects |
| --- | --- | --- | --- | --- |
| `/follow-up-jobs` | POST | submit a new draft request | async | queues job |
| `/follow-up-jobs/{job_id}` | GET | fetch current job state and result | sync retrieval | none |
| `/follow-up-jobs/{job_id}/cancel` | POST | request cancellation if still running | sync control | cancels work if possible |

This is already more useful than "build an API around the script."

### Step 6 - Add ownership now

Before writing code, answer:

- Who owns the endpoint contract?
- Who is paged when it fails?
- Who approves auth and data exposure?

That is governance, and it belongs at design time.

---

## Practice

Choose one existing workflow that currently runs as a script or internal tool.

Create:

1. an API boundary brief
2. an endpoint inventory with at least three endpoints
3. a sync versus async decision for each endpoint
4. a list of visible side effects
5. owner and approver roles

Use a real workflow such as:

- support reply drafting
- proposal generation
- internal document triage
- CRM note generation

---

## Review Checklist

Your boundary artifact is acceptable when:

- It describes the business action clearly.
- The public surface is smaller than the internal workflow.
- Endpoints are named around resources or actions the client understands.
- Sync versus async behavior is explicit.
- Side effects are visible.
- Error classes are listed.
- Ownership is named.

---

## Common Failure Modes

- **Script wrapper mentality:** The API mirrors internal implementation details.
- **Too many endpoints:** Every workflow step becomes public unnecessarily.
- **No side-effect disclosure:** Clients do not know what the API writes or triggers.
- **No boundary owner:** Everyone can change the contract informally.
- **Sync by default:** Long-running work is forced into an unsuitable request/response path.
- **No retry thinking:** Duplicate submissions become accidental duplicate work.

---

## Portfolio Evidence

Save:

- the API boundary brief
- the endpoint inventory
- one short note explaining why the main operation is sync or async

This shows that you can turn internal automation into a stable service boundary.

---

## References

- FastAPI First Steps: https://fastapi.tiangolo.com/tutorial/first-steps/
- OpenAPI Specification: https://spec.openapis.org/oas/latest.html
- RFC 9110: HTTP Semantics: https://www.rfc-editor.org/rfc/rfc9110
