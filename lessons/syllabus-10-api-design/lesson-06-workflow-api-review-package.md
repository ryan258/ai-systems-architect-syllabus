# Lesson 10.06: Workflow API Review Package

**Parent syllabus:** [Syllabus 10: API Design with FastAPI](../../syllabus-10-api-design.md)  
**Estimated time:** 3 hours  
**Artifact:** Complete workflow API review package

---

## Outcome

By the end of this lesson, you can assemble the core API design artifacts for one workflow into a reviewer-ready package that supports prototype, limited rollout, redesign, or hold.

You will produce a complete workflow API review package that includes:

- API boundary brief
- endpoint contract
- long-running workflow or streaming decision
- security and request-protection policy
- client-facing docs pack
- readiness decision

---

## Why This Matters

API design artifacts are only useful when they work together.

Common failures:

- The endpoint contract says the workflow is synchronous, but the transport memo says it should be a job API.
- The docs quickstart implies a simple client flow, but auth or rate-limit requirements make that impossible.
- The security policy exists, but the boundary brief never states who the real callers are.
- Streaming is supported, but the review package never states what disconnects mean operationally.
- FastAPI docs are polished, but the API is still not ready for client use.
- Good notes exist, but nobody can use them to decide whether to ship or simplify.

This package is the bridge between "we can build an endpoint" and "we can expose a workflow as a maintainable API surface."

---

## Best-Practice Principles

1. **Make every artifact answer a review question.**  
   If a document does not support readiness, safety, or client usability, simplify it.

2. **Keep the API package internally consistent.**  
   Boundary, transport, auth, docs, and limits should reinforce each other.

3. **Trace API design choices to risks.**  
   Transport decisions, auth rules, and versioning should all tie back to runtime behavior and client impact.

4. **Separate facts, assumptions, and release blockers.**  
   This keeps the package usable under review pressure.

5. **End with a rollout decision.**  
   The package should conclude with prototype, limited rollout, redesign, or hold.

6. **Include operator, privacy, and governance notes.**  
   An API is an operating surface, not just a code artifact.

7. **Keep a sanitized version.**  
   Good API design work can become portfolio evidence after names, secrets, and private examples are removed.

---

## Concepts

### Workflow API Package

A compact set of design artifacts used to judge whether one workflow is ready to be exposed as an API.

### Readiness Decision

A deliberate conclusion about what should happen next.

Examples:

- prototype only
- limited rollout
- redesign before client exposure
- hold

### Release Blocker

A missing control or unresolved question that prevents safe rollout.

Examples:

- missing auth policy
- no status endpoint for long-running work
- no rate limit for an expensive endpoint
- no quickstart for client integration

### Change Trigger

A later change that should reopen the package.

Examples:

- new caller type
- new streaming path
- new external write action
- more sensitive data class

---

## Workflow API Review Package Template

Use this structure:

```markdown
# Workflow API Review Package: [API Name]

## 1. Executive Summary

- API goal:
- Main risks:
- Current recommendation:

## 2. Boundary Brief

- Primary callers:
- Main resources:
- Endpoint inventory:

## 3. Endpoint and Model Contract

- Request model:
- Response model:
- Status codes:
- Dependencies:

## 4. Runtime and Transport Design

- Sync or async:
- Polling, webhook, SSE, or WebSocket decision:
- Job states:
- Cancellation rules:

## 5. Security and Request Protection

- Auth mechanism:
- Authorization rules:
- Rate limits:
- TLS:
- Logging rules:

## 6. Client Documentation and Versioning

- Quickstart:
- Generated docs:
- Versioning note:
- Important limits:

## 7. Observability, Privacy, and Governance

- Request IDs and logs:
- Sensitive data handling:
- Owner:
- Reviewer or approver:

## 8. Open Questions and Release Blockers

- ...

## 9. Decision

Prototype, limited rollout, redesign, or hold.

## 10. Sanitized Portfolio Version

What must be removed before public sharing?
```

---

## Walkthrough

Use this example:

> A FastAPI service exposes a support-drafting workflow to a client's CRM, supports async job submission, and provides a plain-English quickstart for the client's integration team.

### Step 1 - Gather the artifacts

Package contents:

- boundary brief from Lesson 10.01
- endpoint contract from Lesson 10.02
- transport memo from Lesson 10.03
- security policy from Lesson 10.04
- docs pack from Lesson 10.05

The package should make rollout review possible without opening every implementation file.

### Step 2 - Check consistency

Examples:

| Claim | Where it should appear |
| --- | --- |
| workflow is async and returns a job ID | boundary brief, endpoint contract, transport memo, quickstart |
| expensive endpoint is rate limited | security policy, docs limits, rollout blockers |
| output is draft-only and requires human review | endpoint contract, docs quickstart, governance notes |
| clients may cancel queued work | transport memo, endpoint inventory, quickstart |
| raw request bodies are not logged | security policy, observability notes, privacy notes |

If those claims appear in only one artifact, the package is weak.

### Step 3 - Write the executive summary

Example:

```markdown
# Workflow API Review Package: Support Draft API

## 1. Executive Summary

- API goal: allow the client's CRM to submit support-ticket drafting jobs and retrieve review-ready output safely.
- Main risks: auth misuse, overspend on burst traffic, ambiguous long-running behavior, and poor client onboarding.
- Current recommendation: limited rollout only, with scoped client auth, job status visibility, request limits, and a signed quickstart before production use.
```

### Step 4 - Turn review into a decision

Weak ending:

> The API looks mostly ready.

Better ending:

> Limited rollout only after 429 behavior is tested, job cancellation rules are documented, and the client quickstart reflects the async polling flow accurately.

The package exists to drive a next action.

### Step 5 - Name the change triggers

Reopen the package if:

- a browser frontend becomes a direct caller
- a new WebSocket path is added
- the API begins writing to an external system automatically
- more sensitive data enters the request body
- versioning policy changes

That is what keeps the package useful after first release.

---

## Practice

Choose one workflow API idea or design from this module.

Assemble a complete workflow API review package with:

1. executive summary
2. boundary brief
3. endpoint and model contract
4. runtime and transport design
5. security and request protection
6. client documentation and versioning
7. observability, privacy, and governance notes
8. open questions and release blockers
9. final rollout decision

End with one of these decisions:

- Prototype only
- Limited rollout
- Redesign before client exposure
- Hold

---

## Review Checklist

Your review package is acceptable when:

- All core artifacts are present.
- The artifacts do not contradict each other.
- Runtime model and endpoint contract align.
- Security rules match the caller type and cost profile.
- Client docs reflect the real integration path.
- Privacy and logging boundaries are visible.
- The package ends with a real rollout decision.
- A future reviewer could continue from this package without guessing.

---

## Common Failure Modes

- **Artifact pile, not package:** Useful documents exist but do not support a decision.
- **No transport coherence:** Async behavior is implied in one place and absent elsewhere.
- **Docs drift:** The quickstart does not match the actual contract.
- **Security isolation:** Auth policy exists separately but does not shape the API design.
- **No blockers:** Missing controls are known but never converted into rollout gates.
- **No change triggers:** The package becomes stale after one release.

---

## Portfolio Evidence

Save:

- the complete workflow API review package
- one redacted endpoint contract and quickstart excerpt
- one short note on the main risk that prevented broader rollout

This shows that you can design and review API surfaces with the same rigor as architecture, privacy, and model operations.

---

## References

- FastAPI First Steps: https://fastapi.tiangolo.com/tutorial/first-steps/
- FastAPI Metadata and Docs URLs: https://fastapi.tiangolo.com/tutorial/metadata/
- FastAPI Security Tools: https://fastapi.tiangolo.com/reference/security/
- OpenAPI Specification: https://spec.openapis.org/oas/latest.html
- OWASP API Security Top 10 - 2023: https://owasp.org/API-Security/editions/2023/en/0x11-t10/
