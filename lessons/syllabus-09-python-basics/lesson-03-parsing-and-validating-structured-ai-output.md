# Lesson 09.03: Parsing and Validating Structured AI Output

**Parent syllabus:** [Syllabus 09: Advanced Python for AI Workflow Architecture](../../syllabus-09-python-basics.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Output schema contract and parser fallback module

---

## Outcome

By the end of this lesson, you can define a schema for AI output, validate it reliably in Python, and decide what happens when the model returns malformed, partial, or policy-breaking data.

You will produce:

- an output schema contract
- a parser or validator module
- a fallback plan for invalid output

---

## Why This Matters

Most AI workflow bugs do not come from "bad intelligence." They come from weak boundaries.

Common failures:

- The code splits strings and hopes the model keeps the same format forever.
- A JSON response parses, but still violates business rules.
- Extra keys or missing fields pass silently into downstream code.
- A malformed response causes a crash instead of a controlled rejection.
- The prompt contract changes, but the parser contract is never updated.
- The workflow trusts structured output before validation.

String parsing is not a control surface. It is technical debt with good branding.

---

## Best-Practice Principles

1. **Define the schema before writing downstream logic.**  
   Every later step depends on what shape is allowed.

2. **Separate transport validity from business validity.**  
   Valid JSON is not the same thing as an acceptable workflow result.

3. **Validate at the boundary.**  
   Do not pass model output deeper into the system before parsing and checking it.

4. **Prefer explicit rejection over silent coercion.**  
   When the output matters, hidden repair logic becomes hidden risk.

5. **Version the contract.**  
   If prompts or fields change, the schema version should change too.

6. **Capture invalid output safely for debugging.**  
   Keep enough evidence to reproduce the issue without logging sensitive raw data carelessly.

7. **Design the fallback path with the parser.**  
   Invalid output should route to retry, degradation, or human review by design.

---

## Concepts

### Schema Contract

The explicit shape the model output must match.

Examples:

- required fields
- allowed types
- allowed enum values
- optional fields

### Parser Boundary

The line where model output becomes application data.

### Validation Error

A failure to convert raw output into the required structure.

### Business Rule Check

A rule that applies after schema validation.

Examples:

- citations must not be empty if a policy claim exists
- high-risk actions require `needs_human_review=True`

### Fallback Path

What happens when output is invalid.

Examples:

- retry once with stricter instructions
- mark the item for manual review
- reject the record and continue the batch

### Schema Version

A visible version label that ties prompt expectations to parser expectations.

---

## Output Schema Contract Template

Use this structure:

```markdown
# Output Schema Contract: [Workflow Name]

## Schema Version

v1

## Expected Object

What object should the model return?

## Required Fields

- ...

## Optional Fields

- ...

## Allowed Values

- ...

## Business Rule Checks

- ...

## Fallback Path

- Retry when:
- Reject when:
- Escalate when:
```

Example parser pattern:

```python
from pydantic import BaseModel, ValidationError


class DraftReply(BaseModel):
    ticket_id: str
    summary: str
    draft_reply: str
    needs_human_review: bool
    citations: list[str]


def parse_draft(raw_text: str) -> DraftReply:
    try:
        draft = DraftReply.model_validate_json(raw_text)
    except ValidationError as exc:
        raise ParsedOutputError("schema_validation_failed") from exc

    if draft.needs_human_review is False and not draft.citations:
        raise ParsedOutputError("business_rule_failed")

    return draft
```

The key move is simple: parse first, enforce business rules second.

---

## Walkthrough

Use this example:

> A support workflow asks an LLM to produce a JSON object with a summary, a proposed reply, and whether the ticket requires human review.

### Step 1 - Define the target object

Weak prompt requirement:

> Return something structured.

Better requirement:

> Return one JSON object with `ticket_id`, `summary`, `draft_reply`, `needs_human_review`, and `citations`.

This is the contract the parser will defend.

### Step 2 - Write the schema first

For this workflow:

- `ticket_id` must be a string
- `summary` must be a non-empty string
- `draft_reply` must be a string
- `needs_human_review` must be a boolean
- `citations` must be a list of strings

If the team cannot agree on the object shape, they are not ready to automate the next step.

### Step 3 - Parse at the boundary

The parser should run immediately after receiving model output.

Do not:

- store raw output in a database first
- pass half-parsed data to later functions
- let the CLI print "success" before validation

Parsing is a release gate inside the code path.

### Step 4 - Add business rule checks

Schema-valid output can still be wrong.

Example:

- `needs_human_review=False`
- `citations=[]`

That might be structurally valid JSON but operationally unacceptable if the workflow requires citations for unsupported claims.

Business rules belong after schema validation, not mixed into string parsing.

### Step 5 - Decide the fallback path

For malformed output:

- retry once with stricter formatting instructions

For repeated malformed output:

- mark the ticket for human review

For structurally valid but policy-breaking output:

- reject without automatic retry

This distinction matters for reliability and cost.

### Step 6 - Version the schema

If the team later adds `confidence_score`, that is a contract change.

Update:

- the schema version
- the prompt instructions
- the parser
- the tests

Otherwise drift starts quietly and gets discovered only in production.

---

## Practice

Choose one workflow that needs structured AI output.

Create:

1. an output schema contract
2. a parser or validator module
3. at least two business rule checks
4. a fallback plan for malformed output

Minimum requirements:

- one version label
- at least four fields
- one invalid-output retry rule
- one human-review escalation rule

Use a real object shape such as:

- classification decision
- draft reply object
- meeting summary record
- project scope draft

---

## Review Checklist

Your output artifact is acceptable when:

- The target schema is explicit.
- Parsing happens before downstream use.
- Business rule checks are separate from schema validation.
- Invalid output has a defined fallback path.
- The schema has a visible version.
- The design avoids brittle string slicing.
- The parser can fail safely without pretending success.

---

## Common Failure Modes

- **String parsing debt:** The workflow depends on headings or delimiters staying perfect.
- **JSON means trusted:** Valid JSON is accepted without business checks.
- **Schema drift:** Prompt and parser expectations change separately.
- **Silent coercion:** Bad values are repaired quietly and hide real problems.
- **No fallback:** Invalid output crashes the script or leaks downstream.
- **Unbounded retry:** Malformed output triggers repeated paid calls without learning.

---

## Portfolio Evidence

Save:

- the schema contract
- the parser module
- one redacted example of rejected output and the reason it was rejected

This shows that you can turn model output into controlled application data instead of hopeful text handling.

---

## References

- Pydantic Models: https://docs.pydantic.dev/latest/concepts/models/
- Python `json`: https://docs.python.org/3/library/json.html
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
