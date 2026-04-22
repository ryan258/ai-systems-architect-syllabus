# Lesson 12.03: Trust Boundaries and Prompt Injection Defense

**Parent syllabus:** [Syllabus 12: Production AI Systems](../../syllabus-12-production-ai-systems.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Production safety-boundary specification and attack test matrix

---

## Outcome

By the end of this lesson, you can define the trust boundaries of a live AI system and set controls so untrusted inputs, retrieved content, and model outputs cannot directly become high-impact actions.

You will produce:

- a production safety-boundary specification
- a trust-boundary diagram or boundary table
- an attack test matrix

---

## Why This Matters

Prompt injection is only one part of the production safety problem. The bigger question is what the model can do if manipulated.

Common failures:

- User text, retrieved pages, and uploaded files are all treated as instructions with no meaningful separation.
- The model can trigger a tool or outbound action without a review gate.
- Security review focuses on the prompt wording but ignores the permission surface.
- Tool calls are logged after the fact, but blocked attempts are not.
- Indirect prompt injection through retrieved documents is not tested before launch.
- Output validation exists for schema shape but not for unsafe action requests or disclosure attempts.
- The team knows the workflow uses untrusted content, but cannot point to the exact boundaries where controls apply.

The operational question is not "Can the model be manipulated?" The operational question is "What can happen if it is?"

---

## Best-Practice Principles

1. **Treat all external content as untrusted.**  
   Tickets, chat messages, attachments, retrieved web pages, and imported documents all belong on the untrusted side of the boundary.

2. **Separate data from instructions as much as the architecture allows.**  
   Small, step-specific prompts and structured fields are safer than one blended context block.

3. **Make the action boundary explicit.**  
   Reading data, drafting text, sending a message, exporting records, and updating a system of record are different risk classes.

4. **Constrain tools and side effects tightly.**  
   If the model can act, the allowed actions must be narrow, logged, and reviewable.

5. **Validate outputs before they become downstream inputs.**  
   Schema checks, policy checks, and approval gates should sit between model output and external action.

6. **Log security-relevant decisions, not only failures.**  
   Blocked tool attempts, quarantined inputs, approval requirements, and rejection reasons should all be visible.

7. **Test the production boundary with realistic attacks.**  
   Security posture is weak until the system has seen direct injection, indirect injection, and attempted action misuse in testing.

---

## Concepts

### Trust Boundary

The line between content or systems you control and content or systems you do not.

### Action Boundary

The point where the workflow moves from generating or classifying content to taking a real action.

Examples:

- sending a customer message
- writing to a CRM
- exporting records
- approving a payment or credit

### Untrusted Content Class

A category of input that may contain hostile instructions or unsafe data.

Examples:

- user ticket body
- uploaded file
- retrieved article
- third-party webhook payload

### Tool Permission Profile

The list of actions a given workflow step may take.

Examples:

- read-only retrieval
- draft-only generation
- queue-item creation
- no direct external send

### Security Event

A structured event relevant to attack detection or review.

Examples:

- suspicious input quarantined
- blocked tool request
- approval gate triggered
- policy check rejected output

### Attack Test Matrix

A short list of realistic adversarial cases with expected system behavior.

---

## Production Safety-Boundary Specification Template

Use this structure:

```markdown
# Production Safety-Boundary Specification: [Workflow Name]

## Workflow Goal

What is the service supposed to do?

## Untrusted Inputs

- ...

## Protected Assets

- Data stores:
- Secrets:
- External actions:

## Trust Boundaries

| Boundary | What crosses it | Main risk | Control |
| --- | --- | --- | --- |
|  |  |  |  |

## Input Controls

- Filtering:
- Allowed formats:
- Quarantine rules:

## Context Controls

- Instruction/data separation:
- Allowed retrieved content:
- Excluded content:

## Tool and Action Controls

- Allowed tools:
- Disallowed tools:
- Approval-required actions:

## Output Controls

- Validation:
- Rejection rules:
- Human review triggers:

## Audit Events

- ...

## Attack Tests

| Test | Attack type | Expected behavior | Launch blocker if fail? |
| --- | --- | --- | --- |
|  |  |  |  |

## Release Blockers

- ...
```

Review questions:

- Where can hostile content enter?
- What real actions are still possible after model output?
- Which failed attack should stop launch?

---

## Walkthrough

Use this example:

> A support drafting service reads inbound tickets, retrieves approved policy guidance, drafts a response, and places the result in a human review queue.

### Step 1 - Mark the boundaries

Untrusted inputs include:

- customer ticket text
- file attachments
- third-party webhook metadata
- retrieved knowledge articles if the corpus is not tightly controlled

Protected assets include:

- account records
- billing actions
- outbound email or chat send
- internal policy instructions

If these are not listed separately, the boundary is already blurry.

### Step 2 - Separate drafting from acting

For this service, the model may:

- read ticket text
- read approved policy context
- draft a response object
- recommend escalation

It may not:

- send the response directly
- update the billing system
- export customer records
- reveal system instructions

That is a stronger control than any single prompt phrase.

### Step 3 - Add input and context controls

Controls might include:

- reject unsupported file types
- quarantine attachments with malformed extraction results
- use only approved retrieval sources with stable metadata
- place untrusted content in clearly delimited data fields
- exclude hidden system notes from model-visible context unless necessary

The goal is not perfection. The goal is to reduce what manipulated content can influence.

### Step 4 - Define output and action controls

Good controls for this workflow:

- schema validation on the draft object
- policy check before queue creation
- mandatory approval for any customer-facing response
- rejection if the draft attempts data disclosure outside policy

This makes prompt injection a reviewable event instead of an automatic breach path.

### Step 5 - Write attack tests that matter

Minimum tests for this system:

- direct injection in the ticket body
- indirect injection in a retrieved article
- request to reveal hidden instructions
- request to send a message without approval
- request to export account history

Each test should define the expected safe behavior. "Model seemed okay" is not an acceptable result.

### Step 6 - Log security-relevant decisions

Useful audit events:

- suspicious input quarantined
- blocked export attempt
- approval gate required
- policy check rejected output
- attack-test case result

Security work is weak when nothing is recorded until after damage is done.

---

## Practice

Choose one workflow that processes untrusted text, files, or retrieved content.

Create a production safety-boundary specification that includes:

1. the workflow goal
2. at least four untrusted inputs
3. protected assets
4. at least three trust boundaries
5. input controls
6. context controls
7. tool and action controls
8. output controls
9. audit events
10. at least five attack tests

At least one attack test must cover:

- direct prompt injection
- indirect prompt injection
- attempted high-impact action misuse

---

## Review Checklist

Your safety-boundary artifact is acceptable when:

- External content is treated as untrusted by default.
- Trust boundaries are explicit.
- The action boundary is visible.
- Tool permissions are narrow and reviewable.
- Output validation exists before downstream action.
- Approval-required actions are named.
- Security-relevant events are logged.
- Attack tests cover both direct and indirect injection.
- The launch blockers are clear.

---

## Common Failure Modes

- **Prompt-only defense:** The design relies on phrasing instead of permissions and validation.
- **Mixed trust context:** User content and system instructions are blended with no clear control.
- **Action boundary blindness:** Drafting and acting are treated as the same thing.
- **No blocked-attempt logging:** The team sees only successful misuse after the fact.
- **No realistic tests:** The system ships without direct or indirect adversarial cases.
- **Too much tool power:** The model can still take high-impact action directly.

---

## Portfolio Evidence

Save:

- the production safety-boundary specification
- the trust-boundary table or diagram
- the attack test matrix

This shows that you can turn AI security concerns into explicit production controls instead of prompt folklore.

---

## References

- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OpenTelemetry Concepts: https://opentelemetry.io/docs/concepts/
- Anthropic Claude documentation: https://docs.anthropic.com/
