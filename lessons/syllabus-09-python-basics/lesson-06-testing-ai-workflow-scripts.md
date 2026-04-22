# Lesson 09.06: Testing AI Workflow Scripts

**Parent syllabus:** [Syllabus 09: Advanced Python for AI Workflow Architecture](../../syllabus-09-python-basics.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Pytest regression harness and test matrix

---

## Outcome

By the end of this lesson, you can test an AI workflow script without making live API calls on every run and without giving up coverage on parsing, control flow, error handling, or regressions.

You will produce:

- a test matrix
- a pytest regression harness
- mocked provider-call tests

---

## Why This Matters

Workflow code breaks in ordinary ways long before the model itself is the problem.

Common failures:

- Tests call the live provider and become slow, flaky, and expensive.
- Only the happy path is tested.
- Parsing logic changes and nobody notices until production output fails.
- A real failure happens once, but no regression test is added.
- Network access is still possible in tests, so accidental live calls slip through.
- The script becomes harder to change because there is no safe refactor path.

Testing workflow scripts is not about proving the model is perfect. It is about proving the code behaves predictably around the model boundary.

---

## Best-Practice Principles

1. **Test your code, not the provider's uptime.**  
   Most tests should mock or fake the provider boundary.

2. **Create seams around external calls.**  
   A thin wrapper around API requests makes mocking and failure injection much easier.

3. **Test failure paths intentionally.**  
   Validation errors, timeouts, retries, and budget shutdowns deserve direct coverage.

4. **Turn real incidents into regression tests.**  
   Every meaningful production failure is test material.

5. **Keep tests deterministic.**  
   The same inputs should produce the same assertions every run.

6. **Block accidental network usage in tests.**  
   Free tests are faster, safer, and more repeatable.

7. **Make the smallest CI gate meaningful.**  
   Even solo projects should have a basic test command that protects the main workflow path.

---

## Concepts

### Test Seam

The boundary where external behavior can be replaced with a fake, stub, or mock.

### Mocked Provider Call

A test double that returns controlled outputs instead of calling the real API.

### Regression Fixture

A saved example of a real failure case used to prevent recurrence.

### Deterministic Test

A test that does not depend on timing luck, external network state, or changing model behavior.

### Offline Harness

A test setup that validates workflow logic without paid requests.

### CI Gate

A minimal automated test run that must pass before shipping or merging.

---

## Test Matrix Template

Use this structure:

```markdown
# Test Matrix: [Workflow Name]

| Test name | What it covers | Fake input | Expected result |
| --- | --- | --- | --- |
|  |  |  |  |
```

Example pytest pattern:

```python
import pytest

from workflow.runner import run_ticket


class FakeClient:
    def __init__(self, response_text: str):
        self.response_text = response_text

    def generate(self, *_args, **_kwargs):
        return {"text": self.response_text, "usage": {"total_tokens": 500}}


def test_run_ticket_returns_validated_reply():
    client = FakeClient(
        '{"ticket_id":"t-1","summary":"Password reset","draft_reply":"Try reset link.","needs_human_review":true,"citations":["kb-12"]}'
    )

    result = run_ticket(ticket_id="t-1", client=client)

    assert result.ticket_id == "t-1"
    assert result.needs_human_review is True


def test_run_ticket_rejects_invalid_output():
    client = FakeClient('{"ticket_id":"t-1","summary":"Missing fields"}')

    with pytest.raises(Exception):
        run_ticket(ticket_id="t-1", client=client)
```

The goal is not to simulate every provider detail. The goal is to control the boundary and exercise your code.

---

## Walkthrough

Use this example:

> A workflow script reads a support ticket, calls a model, validates JSON output, and applies budget and retry rules.

### Step 1 - Identify the seams

Good seams:

- provider client wrapper
- parser function
- budget object
- loop runner

Bad seam:

> the entire script as one giant `main()` function

If there is nowhere to inject a fake client, testing will stay expensive.

### Step 2 - Cover the high-value paths

At minimum, test:

- valid output path
- malformed output path
- retry or no-retry branch
- budget stop path
- CLI or input-validation path

This is better than 20 tests that all prove only that success can happen.

### Step 3 - Use fake responses for the provider boundary

Your fake should be able to return:

- valid JSON
- malformed JSON
- timeout or transient failure
- usage metadata

That lets you test both reliability and cost logic without paying for test runs.

### Step 4 - Add one regression test from a real failure

Example incident:

- model returned `citations` as a string instead of a list

Add a regression fixture:

- input ticket example
- fake response payload
- expected rejection or normalization path

Now the bug has memory.

### Step 5 - Block accidental network usage

For tests, remove or patch the network call path so live requests fail immediately.

This protects:

- cost
- speed
- privacy
- confidence in the test suite

### Step 6 - Define the smallest useful CI gate

For a solo workflow project, the gate can be simple:

- run the fast pytest set
- require pass before merge or release

Small is fine. Missing is not.

---

## Practice

Choose one workflow script and create a pytest harness for it.

Minimum requirements:

1. one happy-path test with a fake provider response
2. one invalid-output test
3. one retry, timeout, or budget-stop test
4. one regression test from a real or plausible failure
5. one rule that prevents live network calls during normal tests

Also write a short test matrix explaining what each test protects against.

---

## Review Checklist

Your testing artifact is acceptable when:

- Live provider calls are not required for the main test suite.
- The provider boundary can be mocked or faked.
- Failure paths are covered directly.
- At least one regression test exists.
- Tests are deterministic.
- Network usage is blocked or tightly controlled.
- The suite is small enough to run regularly.

---

## Common Failure Modes

- **Live-call testing:** Every test run costs money and depends on network behavior.
- **Happy-path only:** Failures still break production first.
- **No seam design:** External calls are welded into the business logic.
- **No regression memory:** The same bug can happen twice.
- **Flaky timing assumptions:** Tests pass or fail based on sleep behavior or race conditions.
- **No CI habit:** Tests exist but do not protect release decisions.

---

## Portfolio Evidence

Save:

- the test matrix
- the pytest file or test folder
- one regression case tied to a real failure pattern

This shows that you can make workflow code safer to change without relying on expensive live test runs.

---

## References

- pytest documentation: https://docs.pytest.org/en/stable/contents.html
- pytest monkeypatch fixture: https://docs.pytest.org/en/stable/how-to/monkeypatch.html
- Python `unittest.mock`: https://docs.python.org/3/library/unittest.mock.html
