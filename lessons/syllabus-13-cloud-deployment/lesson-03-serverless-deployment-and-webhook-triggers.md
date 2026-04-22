# Lesson 13.03: Serverless Deployment and Webhook Triggers

**Parent syllabus:** [Syllabus 13: Cloud Deployment](../../syllabus-13-cloud-deployment.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Serverless deployment decision memo and webhook contract

---

## Outcome

By the end of this lesson, you can choose a small cloud deployment model for an AI workflow, compare Cloud Run and Lambda at the architecture level, and define a webhook trigger contract that is safe to expose.

You will produce:

- a serverless deployment decision memo
- a webhook contract
- secrets and trigger-handling rules

---

## Why This Matters

The first cloud deployment often fails at the trigger boundary, not inside the model call.

Common failures:

- The team picks a platform before describing the workflow's runtime shape.
- A webhook is exposed publicly without signature verification or idempotency rules.
- The workflow depends on local process state, so retries or multiple instances create inconsistent behavior.
- Platform-specific details leak into the design before the service boundary is clear.
- Secrets are copied into environment settings with no rotation or ownership rule.
- The service returns success too early or too late, so callers retry unpredictably.

Serverless deployment is useful because it compresses infrastructure overhead. It still needs clear rules for ingress, cost, state, and failure handling.

---

## Best-Practice Principles

1. **Choose the platform after describing the runtime shape.**  
   Request pattern, duration, state needs, and HTTP expectations should drive the decision.

2. **Treat the webhook boundary like a public API surface.**  
   Verification, replay handling, rate considerations, and observability all belong here.

3. **Keep the service stateless between requests.**  
   Request-driven compute works best when durable state lives outside the instance.

4. **Separate trigger acceptance from background consequence.**  
   Decide what must happen inside the request window and what may happen asynchronously.

5. **Use managed secrets, not copied credentials.**  
   Secrets need a source of truth, ownership, and rotation path.

6. **Define duplicate and retry behavior.**  
   Webhook senders retry. The deployment must know what to do with repeated deliveries.

7. **Prefer the smallest deployment model that fits.**  
   Simpler ingress and runtime choices are easier to secure and maintain.

---

## Concepts

### Serverless Runtime

Managed compute that runs in response to requests or events without you maintaining a traditional always-on server.

### Request-Driven Service

A service that consumes resources only when triggered by inbound requests or events.

### Cloud Run Fit

Often a strong fit when you already have a containerized HTTP service and want a small path from local container to deployed service.

### Lambda Fit

Often a strong fit when the workflow is function-shaped, event-driven, or already aligned with AWS event sources.

### Webhook Contract

The set of rules for inbound webhook handling.

Typical fields:

- endpoint path
- authentication or signature verification
- expected status codes
- idempotency rules
- retry behavior

### Idempotency

The ability to handle the same event more than once without creating duplicate side effects.

---

## Serverless Deployment Decision Memo Template

Use this structure:

```markdown
# Serverless Deployment Decision Memo: [Workflow Name]

## Workflow Runtime Shape

- Primary trigger:
- Average request duration:
- Peak request duration:
- HTTP required?
- Durable state required?

## Candidate Options

| Option | Strong fit when | Main tradeoff | Decision |
| --- | --- | --- | --- |
| Cloud Run |  |  |  |
| Lambda |  |  |  |

## Chosen Deployment Path

- Runtime:
- Ingress:
- Secret source:
- Durable state:
- Log path:

## Webhook Contract

- Endpoint:
- Verification rule:
- Duplicate-delivery rule:
- Success status:
- Failure status:
- Timeout rule:

## Cost and Scaling Notes

- Idle behavior:
- Burst behavior:
- Guardrails:

## Rollback or Disable Path

- ...
```

Review questions:

- Does the workflow look more like an HTTP service or a small event function?
- What happens if the same webhook is delivered twice?
- Where do secrets and durable state actually live?

---

## Walkthrough

Use this example:

> A support drafting workflow receives ticket-created webhooks, retrieves approved policy context, drafts a review-only response, and writes the result to an external queue.

### Step 1 - Describe the runtime shape

For this workflow:

- trigger is HTTP webhook
- request duration may be several seconds because of retrieval and model calls
- durable review state lives outside the service
- local process memory should not be trusted as durable state

That already narrows the platform choice more than feature checklists do.

### Step 2 - Compare Cloud Run and Lambda at the architecture level

For this workflow:

- Cloud Run is attractive if the service already exists as a containerized HTTP app
- Lambda is attractive if the team wants a more function-shaped event model or deeper AWS event integrations

The important lesson is not which brand wins. The important lesson is whether the workflow already behaves like a containerized web service or a single event handler.

### Step 3 - Write the webhook contract

Weak contract:

> Expose `/webhook` and return `200` if the code does not crash.

Better contract:

- require signature or shared-secret verification
- record event ID and trace ID
- reject obviously malformed payloads
- handle duplicate delivery idempotently
- define when the service returns success versus retryable failure

The webhook boundary should be readable before the platform is configured.

### Step 4 - Decide what happens inside the request

For some workflows, it is acceptable to do the whole job inside the request.

For others, it is safer to:

- verify the webhook
- persist a job or queue record
- return acknowledgment
- perform heavier work asynchronously

The right answer depends on runtime duration, retry behavior, and side effects.

### Step 5 - Define secret handling

For this deployment, secrets may include:

- webhook verification secret
- model-provider key
- queue or database credentials

Better pattern:

- managed secret source
- named owner
- rotation path
- no secret copied into image or hardcoded in source

### Step 6 - Add rollback and disable behavior

Good first answer:

- disable or reject inbound webhook traffic
- roll back to the previous working revision or function version
- verify no new side effects are created
- keep logs and trace IDs for review

If disable behavior is unclear, the deployment is not yet production-safe.

---

## Practice

Choose one workflow you want to trigger from an external system.

Create:

1. a serverless deployment decision memo
2. a webhook contract
3. a secret-handling rule set
4. an idempotency rule for duplicate deliveries
5. a rollback or disable path

Minimum requirements:

- compare Cloud Run and Lambda explicitly
- include one verification rule for inbound webhook requests
- include one rule for retryable failure
- include one durable-state decision

---

## Review Checklist

Your serverless deployment artifact is acceptable when:

- The runtime shape is described before the platform choice.
- Cloud Run and Lambda are compared at the architecture level.
- The webhook contract is explicit.
- Signature verification or equivalent ingress protection exists.
- Duplicate-delivery handling is defined.
- Secret sources and owners are named.
- Durable state is externalized.
- Rollback or disable behavior is documented.

---

## Common Failure Modes

- **Platform-first thinking:** The team argues about vendors before describing the workload.
- **Webhook by optimism:** Public ingress exists with no verification or replay handling.
- **Stateful request handler:** Correctness depends on one warm instance remembering prior work.
- **Secret sprawl:** Credentials are copied into deployment settings with no rotation path.
- **Ack ambiguity:** The service returns success without defining what was actually accepted.
- **No disable path:** The inbound trigger cannot be stopped cleanly during an incident.

---

## Portfolio Evidence

Save:

- the serverless deployment decision memo
- the webhook contract
- one short note explaining how duplicate webhook delivery is handled

This shows that you can choose and expose a cloud trigger path deliberately instead of only getting code to run somewhere.

---

## References

- Google Cloud Run documentation: https://cloud.google.com/run/docs
- AWS Lambda Developer Guide: https://docs.aws.amazon.com/lambda/latest/dg/welcome.html
- Google Cloud Secret Manager documentation: https://cloud.google.com/secret-manager/docs
- AWS Secrets Manager documentation: https://docs.aws.amazon.com/secretsmanager/
