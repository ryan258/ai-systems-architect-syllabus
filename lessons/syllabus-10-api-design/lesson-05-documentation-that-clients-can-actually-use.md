# Lesson 10.05: Documentation That Clients Can Actually Use

**Parent syllabus:** [Syllabus 10: API Design with FastAPI](../../syllabus-10-api-design.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Client-facing API docs pack and versioning note

---

## Outcome

By the end of this lesson, you can produce API documentation that works for both developers and non-technical stakeholders instead of assuming generated docs are enough.

You will produce:

- a client-facing API docs pack
- a plain-English quickstart
- a versioning and change note

---

## Why This Matters

An undocumented API is private whether or not it is deployed.

Common failures:

- The FastAPI docs exist, but the client still cannot tell where to start.
- Endpoint descriptions explain implementation details instead of business use.
- Generated docs expose technical schema but not the integration path.
- Versioning is undefined, so "small changes" break clients unexpectedly.
- Tags, summaries, and examples are inconsistent across endpoints.
- Non-technical reviewers have no plain-English explanation of what the API does.

Generated docs are necessary. They are not sufficient.

---

## Best-Practice Principles

1. **Use generated docs as the technical base, not the whole documentation strategy.**  
   Swagger UI and ReDoc are excellent references, not complete onboarding.

2. **Explain the business action first.**  
   A client should know what problem the endpoint solves before reading schema details.

3. **Provide one happy-path quickstart.**  
   The fastest route from zero to first successful call should be obvious.

4. **Document versioning and change expectations early.**  
   Clients need to know how breaking changes are signaled and how long old behavior remains supported.

5. **Use tags, summaries, and descriptions consistently.**  
   FastAPI metadata is part of your product surface.

6. **Keep examples realistic and safe.**  
   Sample payloads should be plausible without exposing private or regulated data.

7. **Write for multiple readers deliberately.**  
   Developers need schema and examples. Sponsors and operators need quickstart, scope, and limitations.

---

## Concepts

### OpenAPI Schema

The machine-readable description of your API used for docs, client generation, and tooling.

### Swagger UI

FastAPI's default interactive documentation UI at `/docs`.

### ReDoc

An alternative documentation UI that FastAPI exposes by default at `/redoc`.

### Plain-English Quickstart

A short guide explaining how to authenticate, call the main endpoint, and interpret the response without assuming deep API knowledge.

### Versioning Note

A short statement describing:

- current API version
- where version appears
- what counts as breaking
- how changes are communicated

### Deprecation Notice

A warning that an endpoint or behavior is being phased out.

---

## Client-Facing Docs Pack Template

Use this structure:

```markdown
# API Docs Pack: [API Name]

## 1. What This API Does

One short paragraph in plain English.

## 2. Who Should Use It

- ...

## 3. Main Workflow

1. Authenticate
2. Submit work
3. Check result or receive updates

## 4. Fast Start

- Base URL:
- Auth method:
- First request:
- Example response:

## 5. Endpoint Reference

Link to Swagger UI / ReDoc / OpenAPI schema.

## 6. Important Limits

- Rate limits:
- Data restrictions:
- Timeout expectations:
- Human review notes:

## 7. Versioning

- Current version:
- Breaking-change policy:
- Contact path:
```

FastAPI metadata worth defining:

- title
- summary
- description
- version
- tags and tag descriptions
- contact and terms, if relevant

---

## Walkthrough

Use this example:

> A workflow API accepts support tickets, drafts replies, and returns review-ready output objects.

### Step 1 - Start with the plain-English summary

Weak summary:

> AI workflow API for text processing.

Better summary:

> This API accepts support-ticket content and returns a draft reply object that is intended for human review before customer use.

The second summary tells the client what the service does and what it does not do.

### Step 2 - Use FastAPI's generated docs well

FastAPI already gives you:

- an OpenAPI schema
- Swagger UI
- ReDoc

That is a strong baseline.

Now improve it with:

- clear endpoint summaries
- tag descriptions
- meaningful field names
- example payloads

FastAPI metadata is not decorative. It shapes usability.

### Step 3 - Write the quickstart

A good quickstart answers:

- where to send requests
- how to authenticate
- which endpoint to call first
- what success looks like
- where to look when the work is async

This is what non-technical stakeholders and integration partners need most.

### Step 4 - Add practical limits

Document:

- rate limits
- maximum payload size
- whether output is draft-only
- whether human review is required
- what data should not be sent

If the docs hide operational limits, the integration will fail in production instead of in planning.

### Step 5 - Define versioning clearly

At minimum say:

- current API version
- whether the version is in the URL, header, or docs metadata
- what counts as a breaking change
- how long old versions are supported

If the versioning policy is "we'll see," clients will treat every release as risky.

### Step 6 - Keep examples safe

Do not use:

- real client names
- regulated data
- secrets
- live internal IDs

Examples should educate, not leak.

---

## Practice

Choose one API from this syllabus and create a client-facing docs pack for it.

Include:

1. plain-English summary
2. main workflow quickstart
3. authentication instructions
4. one example request and response
5. endpoint reference links or placeholders
6. important limits
7. versioning note

Minimum requirements:

- one summary for developers
- one summary suitable for non-technical stakeholders
- one first-call example
- one statement about breaking changes

---

## Review Checklist

Your documentation artifact is acceptable when:

- The API purpose is explained in plain English.
- Generated docs are complemented by a quickstart.
- Endpoint descriptions are understandable.
- Limits and boundaries are documented.
- Versioning is explicit.
- Example payloads are realistic and sanitized.
- A new client could make a first successful call from the docs.

---

## Common Failure Modes

- **Generated docs only:** Schema exists, onboarding does not.
- **No business framing:** The docs tell how to call the API, not why.
- **No first-call path:** The reader still does not know where to start.
- **Hidden limits:** Rate limits, review requirements, or data exclusions are omitted.
- **Version ambiguity:** Clients cannot tell how changes will be handled.
- **Unsafe examples:** Documentation becomes a source of sensitive-data leakage.

---

## Portfolio Evidence

Save:

- the client-facing docs pack
- the quickstart guide
- one screenshot or excerpt of the documented FastAPI docs metadata

This shows that you can make an API usable by real clients, not just technically reachable.

---

## References

- FastAPI First Steps: https://fastapi.tiangolo.com/tutorial/first-steps/
- FastAPI Metadata and Docs URLs: https://fastapi.tiangolo.com/tutorial/metadata/
- OpenAPI Specification: https://spec.openapis.org/oas/latest.html
