# Lesson 10.04: Security and Authentication

**Parent syllabus:** [Syllabus 10: API Design with FastAPI](../../syllabus-10-api-design.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Authentication and request-protection policy

---

## Outcome

By the end of this lesson, you can define who may call your API, how requests are authenticated and authorized, what limits apply, and what is safe to log.

You will produce:

- an authentication and request-protection policy
- an auth mechanism choice
- logging, rate-limit, and TLS rules

---

## Why This Matters

Once an API exists, it becomes a live attack surface.

Common failures:

- API keys are used where user authentication or scoped tokens are actually required.
- Authentication is present, but object-level authorization is missing.
- Rate limiting is discussed but not enforced anywhere meaningful.
- Sensitive request bodies, prompts, or tokens are written into logs.
- HTTPS is assumed to exist because the endpoint is public.
- One client gets a broad key with permissions it does not need.

Security for workflow APIs is not just "add a header check." It is request admission, authorization, transport protection, auditability, and abuse resistance.

---

## Best-Practice Principles

1. **Choose auth based on caller type.**  
   Machine-to-machine clients, internal tools, and end-user-facing apps often need different auth models.

2. **Separate authentication from authorization.**  
   Knowing who called the API is not enough. The service must still decide what that caller is allowed to access or do.

3. **Keep credentials scoped and revocable.**  
   Broad permanent secrets create unnecessary blast radius.

4. **Rate limit at a layer that actually protects the service.**  
   Abuse and cost spikes should be constrained before they overwhelm the workflow.

5. **Require TLS and protect secrets in transit.**  
   Public or partner-facing APIs should never depend on plaintext transport.

6. **Log request metadata, not sensitive content by default.**  
   You need reconstructable audits without turning observability into a privacy incident.

7. **Treat external integrations as untrusted input sources too.**  
   APIs that call other APIs must still validate and constrain what they receive.

---

## Concepts

### Authentication

How the API identifies the caller.

Examples:

- API key
- bearer token
- OAuth2 access token

### Authorization

The rules deciding what the identified caller may do.

Examples:

- allowed tenant
- allowed endpoint set
- allowed object IDs
- allowed write actions

### API Key

A credential suitable for identifying an API client in many machine-to-machine scenarios.

It is not a substitute for full end-user auth.

### Rate Limiting

A control that restricts how much traffic or work a caller can trigger over time.

### Audit Log

A structured record of request metadata, auth result, decision, and outcome.

### TLS

Transport security protecting the API request and response in transit.

---

## Authentication and Request-Protection Policy Template

Use this structure:

```markdown
# Authentication and Request-Protection Policy: [API Name]

## Primary Caller Types

- ...

## Chosen Auth Mechanism

- API key
- Bearer token
- OAuth2
- Internal network trust only, if applicable

## Authorization Rules

- Which clients can call which endpoints?
- Which objects or tenants can they access?
- Which write actions require stronger approval?

## Rate Limits

- Per client:
- Per endpoint:
- Burst behavior:
- Hard-stop behavior:

## TLS and Secret Handling

- TLS required:
- Secret rotation:
- Secret storage:

## Logging Rules

- Log:
- Do not log:

## Incident Signals

- auth failure spike
- rate-limit spike
- suspicious object access attempts
```

Example FastAPI auth surface:

```python
from fastapi import Depends, FastAPI, HTTPException, status
from fastapi.security import APIKeyHeader

app = FastAPI()
api_key_header = APIKeyHeader(name="x-api-key")


async def require_api_key(api_key: str = Depends(api_key_header)) -> str:
    if api_key != "expected-key":
        raise HTTPException(status_code=status.HTTP_401_UNAUTHORIZED, detail="invalid api key")
    return api_key


@app.get("/jobs")
async def read_jobs(_api_key: str = Depends(require_api_key)):
    return {"jobs": []}
```

This is only the entry point. Real design still needs authorization, rotation, rate limits, and safe logs.

---

## Walkthrough

Use this example:

> A client's CRM will call your API to submit support-ticket drafting jobs and later retrieve results.

### Step 1 - Match auth to the caller

If the caller is a server-to-server CRM integration, an API key or scoped bearer token may be reasonable.

If the caller is a browser frontend acting on behalf of end users, user auth and delegated tokens may be more appropriate.

The first security question is:

> Who is really calling this API?

### Step 2 - Add authorization, not just authentication

Even after the client is identified, the API should still check:

- which tenant the client belongs to
- which job IDs it may read
- whether it may submit, retrieve, or cancel work

This is where broken object-level authorization often appears.

### Step 3 - Decide where rate limits live

For workflow APIs, rate limiting is both a security control and a cost control.

Inference from OWASP API4:

Requests can burn compute, model calls, and downstream service usage. So rate limits should be defined in terms the service can survive, not just in terms of HTTP politeness.

In many deployments, the strongest limit will sit at an API gateway, reverse proxy, or middleware layer rather than inside the route function itself.

### Step 4 - Protect transport and secrets

Rules:

- require HTTPS
- never send auth secrets in URLs
- keep secrets in secure config, not source files
- rotate credentials on a schedule and on incident

If the API will be called from a browser app on another origin, define allowed origins intentionally rather than opening the service broadly.

### Step 5 - Design safe request logging

Good log fields:

- request ID
- client ID
- route
- response status
- latency
- auth result
- rate-limit result

Usually avoid logging by default:

- raw request bodies
- tokens
- prompts
- client-identifying content

This is where observability and privacy need to cooperate.

### Step 6 - Plan incident signals

Watch for:

- invalid credential spikes
- repeated access to many object IDs
- bursty high-cost endpoint traffic
- suspicious cancellation or retry patterns

If the API is secure only when nobody looks closely, it is not secure.

---

## Practice

Choose one API design from this syllabus and create an authentication and request-protection policy for it.

Include:

1. primary caller types
2. chosen auth mechanism and why
3. authorization rules
4. rate-limit rules
5. TLS and secret-handling rules
6. safe logging rules
7. at least three incident signals

Minimum requirements:

- one explicit rule about object or tenant access
- one explicit rule about what not to log
- one limit tied to resource cost or abuse risk

---

## Review Checklist

Your security artifact is acceptable when:

- Auth choice matches the caller type.
- Authentication and authorization are separated.
- Object or tenant access rules are defined.
- Rate limits are explicit.
- TLS and secret handling are defined.
- Safe logging rules exist.
- Abuse or incident signals are named.

---

## Common Failure Modes

- **Header check only:** Authentication exists but authorization does not.
- **API key misuse:** One broad key is used for everything, including user identity.
- **No rate-limit reality:** Limits are too weak, too vague, or enforced too late.
- **Unsafe logs:** Tokens or sensitive request content end up in logs.
- **Transport assumptions:** HTTPS is treated as someone else's problem.
- **No tenant discipline:** One client can discover another client's resources.

---

## Portfolio Evidence

Save:

- the authentication and request-protection policy
- one auth decision note explaining why the chosen mechanism fits the caller
- one redacted request-log example showing safe fields

This shows that you can protect an API as an operating surface, not just add credentials to it.

---

## References

- FastAPI Security Tools: https://fastapi.tiangolo.com/reference/security/
- FastAPI Security - First Steps: https://fastapi.tiangolo.com/tutorial/security/first-steps/
- RFC 9110: HTTP Semantics: https://www.rfc-editor.org/rfc/rfc9110
- OWASP API Security Top 10 - 2023: https://owasp.org/API-Security/editions/2023/en/0x11-t10/
- OWASP API1:2023 Broken Object Level Authorization: https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/
- OWASP API2:2023 Broken Authentication: https://owasp.org/API-Security/editions/2023/en/0xa2-broken-authentication/
- OWASP API4:2023 Unrestricted Resource Consumption: https://owasp.org/API-Security/editions/2023/en/0xa4-unrestricted-resource-consumption/
- OWASP API10:2023 Unsafe Consumption of APIs: https://owasp.org/API-Security/editions/2023/en/0xaa-unsafe-consumption-of-apis/
