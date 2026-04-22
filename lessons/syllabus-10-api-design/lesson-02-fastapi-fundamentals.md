# Lesson 10.02: FastAPI Fundamentals

**Parent syllabus:** [Syllabus 10: API Design with FastAPI](../../syllabus-10-api-design.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Endpoint contract and local prototype spec

---

## Outcome

By the end of this lesson, you can define a FastAPI endpoint with explicit request and response models, predictable status codes, and a local testing path that proves the contract before deployment.

You will produce:

- an endpoint contract
- request and response models
- a local prototype and smoke-test plan

---

## Why This Matters

A working endpoint is not the same as a reliable API contract.

Common failures:

- The request body is implied by code, not documented by a model.
- The response returns internal fields that should never leave the service.
- Status codes are inconsistent or defaulted thoughtlessly.
- Validation errors and business errors are mixed together.
- The endpoint "works on my machine" but has no repeatable local test path.
- Shared auth or logging code is copied into each route instead of treated as a dependency.

FastAPI is useful because it turns explicit contracts into code and docs quickly. That only works if the contract is actually explicit.

---

## Best-Practice Principles

1. **Model the request and response separately.**  
   Input shape and output shape often differ for security and maintainability reasons.

2. **Use response models to filter output intentionally.**  
   Returning raw internal objects is a common data exposure mistake.

3. **Choose status codes deliberately.**  
   A client should be able to distinguish creation, validation failure, auth failure, and not-found behavior.

4. **Keep path operation code thin.**  
   Validation, auth, and shared setup belong in dependencies or helper layers, not repeated inline.

5. **Raise client-facing errors cleanly.**  
   Return useful error structure without leaking internals or stack details.

6. **Test the contract locally before deployment.**  
   Request shape, response filtering, and error behavior should be verified early.

7. **Optimize for predictable evolution.**  
   Explicit models, tags, and local tests make change safer later.

---

## Concepts

### Path Operation

A FastAPI route handler for an HTTP method and path.

Examples:

- `@app.post("/drafts")`
- `@app.get("/jobs/{job_id}")`

### Request Model

A Pydantic model describing the allowed request body.

### Response Model

A Pydantic model describing the response shape the client is allowed to see.

### Dependency

Reusable setup or validation logic declared with `Depends()`.

Examples:

- auth check
- tenant lookup
- request metadata

### `HTTPException`

FastAPI's way to return a controlled client-facing error.

### Smoke Test

A small local test proving the endpoint responds with the intended contract.

---

## Endpoint Contract Template

Use this structure:

```markdown
# Endpoint Contract: [Endpoint Name]

## Method and Path

- Method:
- Path:

## Purpose

What does this endpoint do?

## Request Model

- Required fields:
- Optional fields:
- Validation rules:

## Response Model

- Returned fields:
- Excluded internal fields:

## Status Codes

- 200 or 201:
- 400 or 422:
- 401 or 403:
- 404:
- 429:
- 500:

## Dependencies

- ...

## Local Test Path

- Run command:
- Example request:
- Expected response:
```

Example FastAPI skeleton:

```python
from fastapi import FastAPI, HTTPException, status
from pydantic import BaseModel

app = FastAPI()


class DraftRequest(BaseModel):
    source_text: str
    audience: str


class DraftResponse(BaseModel):
    draft: str
    needs_review: bool


@app.post("/drafts", response_model=DraftResponse, status_code=status.HTTP_201_CREATED)
async def create_draft(request: DraftRequest) -> DraftResponse:
    if not request.source_text.strip():
        raise HTTPException(status_code=422, detail="source_text must not be empty")

    return DraftResponse(draft="Example draft", needs_review=True)
```

The key idea is not that this code is sophisticated. It is that the contract is visible.

---

## Walkthrough

Use this example:

> A client wants an endpoint that accepts support-ticket text and returns a draft reply object.

### Step 1 - Define the request model

Required fields might be:

- `ticket_id`
- `customer_message`
- `channel`

Optional fields might be:

- `priority`
- `locale`
- `product_area`

If the request body is just an untyped `dict`, the API boundary is already weak.

### Step 2 - Define a separate response model

Return:

- `ticket_id`
- `draft_reply`
- `needs_human_review`

Do not return:

- provider raw output
- internal prompt text
- private routing details
- secret risk flags meant only for operators

FastAPI response models are a security tool as much as a convenience.

### Step 3 - Choose the status codes

Example:

- `201 Created` when a draft object is produced immediately
- `422 Unprocessable Content` for invalid request data
- `401 Unauthorized` when auth is missing or invalid
- `429 Too Many Requests` when limits are hit

Do not make every failure a `500`.

### Step 4 - Keep the route thin

Good route responsibilities:

- accept validated request
- call a service function
- raise clear API errors
- return response model

Bad route responsibilities:

- load config
- validate auth manually
- log raw payloads
- perform all business logic inline

Dependencies and service layers exist to keep this manageable.

### Step 5 - Prove the contract locally

Minimum local checks:

- start the app
- send one valid request
- send one invalid request
- confirm returned fields match the response model
- confirm status codes match expectations

FastAPI's local development speed is only helpful if you actually use it to verify the boundary.

---

## Practice

Create one small FastAPI endpoint around a workflow you already have.

Produce:

1. an endpoint contract
2. a request model
3. a response model
4. status-code rules
5. a local smoke-test plan

Minimum requirements:

- one POST or PUT endpoint
- at least three request fields
- at least two response fields
- one validation error case
- one local test command and example request

---

## Review Checklist

Your endpoint artifact is acceptable when:

- Request and response models are explicit.
- Input and output models are not carelessly reused.
- Response filtering is intentional.
- Status codes are listed and meaningful.
- Shared concerns are identified as dependencies or helpers.
- A local test path exists.
- The endpoint contract is understandable without reading all of the implementation.

---

## Common Failure Modes

- **Untyped boundary:** Request and response shapes are implicit.
- **Model reuse leak:** Input models expose fields that should not be returned.
- **Status code drift:** Everything returns `200` or `500`.
- **Fat route handler:** Business logic, auth, and transport logic are mixed together.
- **No local proof:** The endpoint exists but has not been contract-tested.
- **Docs by accident:** The builder assumes generated docs will fix an unclear contract.

---

## Portfolio Evidence

Save:

- the endpoint contract
- the request and response model definitions
- one local test example with expected output

This shows that you can turn workflow logic into a clear API boundary instead of just exposing code.

---

## References

- FastAPI First Steps: https://fastapi.tiangolo.com/tutorial/first-steps/
- FastAPI Request Body: https://fastapi.tiangolo.com/tutorial/body/
- FastAPI Response Model: https://fastapi.tiangolo.com/tutorial/response-model/
- FastAPI Response Status Code: https://fastapi.tiangolo.com/tutorial/response-status-code/
- FastAPI Handling Errors: https://fastapi.tiangolo.com/tutorial/handling-errors/
- FastAPI Testing: https://fastapi.tiangolo.com/tutorial/testing/
