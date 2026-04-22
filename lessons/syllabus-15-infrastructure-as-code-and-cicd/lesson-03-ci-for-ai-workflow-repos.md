# Lesson 15.03: CI for AI Workflow Repos

**Parent syllabus:** [Syllabus 15: Infrastructure as Code and CI/CD](../../syllabus-15-infrastructure-as-code-and-cicd.md)  
**Estimated time:** 2-3 hours  
**Artifact:** CI workflow contract and required-checks policy

---

## Outcome

By the end of this lesson, you can define a practical CI workflow for an AI repo, decide which checks are required before merge, and keep the pipeline fast by mocking model calls and guarding secrets.

You will produce:

- a CI workflow contract
- a required-checks policy
- a mock-model testing rule

---

## Why This Matters

Without CI, every merge is a trust exercise.

Common failures:

- Tests are only run on one laptop with one local environment.
- Model calls hit live providers in CI, making validation slow, flaky, and expensive.
- Lint, type, and unit checks exist, but none are mandatory before merge.
- CI holds long-lived secrets even though most checks should not need them.
- Pull requests change prompts, schemas, or evaluation logic without any regression check.
- Action versions float, creating an avoidable supply-chain risk.

For AI systems, CI is not just code hygiene. It is the first release gate for structure, reliability, security, and cost control.

---

## Best-Practice Principles

1. **Keep CI cheap enough to run on every meaningful change.**  
   Fast feedback beats a perfect pipeline that everyone avoids.

2. **Mock external model calls by default.**  
   CI should validate your code, contracts, and control flow without paying provider cost on every run.

3. **Separate required checks from optional diagnostics.**  
   Merge rules should stay small, clear, and enforceable.

4. **Minimize secret use in CI.**  
   Most lint, unit, and contract tests should run with no production credentials.

5. **Pin workflow dependencies.**  
   Actions, Python versions, and key tools should be deliberate.

6. **Treat schema and prompt changes like product changes.**  
   If structured output or routing logic changes, CI should exercise the affected contract.

7. **Make failures interpretable.**  
   A small number of well-named jobs is easier to operate than a long, vague pipeline.

---

## Concepts

### Continuous Integration

Automated validation that runs on code changes before they are merged or released.

### Required Check

A CI result that must pass before the repository allows merge.

### Mocked Model Call

A test double that simulates provider behavior without making a real network request or spending production budget.

### Workflow Contract

The agreed set of jobs, triggers, inputs, and pass conditions that define what CI is responsible for proving.

### Branch Protection

Repository rules that enforce review and required checks before merge.

---

## CI Workflow Contract Template

Use this structure:

```markdown
# CI Workflow Contract: [Repository Name]

## 1. CI Goals

- What CI must catch:
- What CI will not prove:

## 2. Trigger Rules

- Pull request:
- Push to main:
- Manual run:

## 3. Required Jobs

| Job | Purpose | Uses secrets? | Expected duration | Merge blocker if fail? |
| --- | --- | --- | --- | --- |
| lint |  |  |  |  |
| typecheck |  |  |  |  |
| tests |  |  |  |  |

## 4. Mocking and Test Data Rules

- Model-call policy:
- Fixture policy:
- Sanitized data rule:

## 5. Secret and Dependency Rules

- CI secrets allowed:
- Forbidden secrets:
- Action pinning rule:

## 6. Required Checks Policy

- Merge requires:
- Optional diagnostics:
- Hotfix exception rule:

## 7. Open Questions

- ...
```

Example workflow skeleton:

```yaml
name: ci

on:
  pull_request:
  push:
    branches: [main]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install -r requirements-dev.txt
      - run: ruff check .

  typecheck:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install -r requirements-dev.txt
      - run: mypy src

  tests:
    runs-on: ubuntu-latest
    env:
      MODEL_MODE: mock
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install -r requirements-dev.txt
      - run: pytest -q
```

Review questions:

- Which checks should block merge every time?
- Where can model providers be mocked without losing important coverage?
- Does CI need any secret for the common pull-request path?

---

## Walkthrough

Use this example:

> A FastAPI support drafting repo contains request parsing, retrieval wiring, structured output validation, and release scripts.

### Step 1 - Define what CI is supposed to prove

For this repo, CI should prove:

- formatting and lint rules pass
- types are consistent in critical paths
- unit and contract tests pass
- mocked model routes behave as expected
- packaging or infrastructure files pass basic validation

CI does not need to prove:

- real-provider output quality on every pull request
- full-scale load behavior
- production secret rotation

Those belong elsewhere in the delivery system.

### Step 2 - Keep model calls mocked by default

For AI workflow repos, the standard CI path should avoid live model use.

Practical rules:

- default tests run with `MODEL_MODE=mock`
- fixtures represent both valid and malformed model output
- contract tests cover parser, validator, and retry behavior
- any live-provider checks run separately and intentionally

This protects cost, speed, and determinism.

### Step 3 - Decide the required checks

A compact required set might be:

- `lint`
- `typecheck`
- `tests`

Optional checks might include:

- dependency audit
- preview deployment
- heavier evaluation batch

If everything is required, people will pressure the pipeline to become weak or skipped.

### Step 4 - Handle secrets conservatively

For this workflow:

- pull-request CI should not require production credentials
- preview or deployment jobs should use environment-scoped secrets
- secrets should not be printed, echoed, or stored in artifacts
- forked pull requests should not gain privileged tokens

CI security posture matters because the pipeline often has broad repo or cloud reach.

### Step 5 - Pin workflow dependencies

For GitHub Actions:

- use current maintained actions
- pin versions deliberately
- review action permissions
- limit token scope where possible

This is low-effort hardening with real value.

### Step 6 - Add AI-specific regression coverage

For the support drafting repo, CI should also watch for:

- schema changes that break structured output parsing
- prompt or routing edits that invalidate expected fallback behavior
- test fixtures that no longer represent approved edge cases

That is how CI stays relevant to AI systems rather than becoming generic Python hygiene only.

---

## Practice

Choose one AI repo and write a **CI Workflow Contract** that includes:

1. CI goals and limits
2. trigger rules
3. required jobs
4. mocking and fixture policy
5. secret handling rules
6. branch protection or required-check settings

Add a short workflow skeleton or job list that another engineer could implement without guessing.

---

## Review Checklist

- CI goals are explicit.
- Required checks are small and enforceable.
- Model calls are mocked by default.
- Test fixtures cover malformed and edge-case outputs.
- Pull-request CI avoids unnecessary secrets.
- Workflow dependencies are pinned and reviewed.
- Failure output is clear enough to diagnose quickly.
- AI-specific contract risks are covered, not just generic linting.

---

## Common Failure Modes

- Using live model APIs in every CI run.
- Treating all checks as optional and calling the repo "covered."
- Mixing deployment jobs into pull-request validation without clear boundaries.
- Depending on production-like secrets for ordinary tests.
- Letting action permissions default to overly broad values.
- Adding slow, flaky jobs until developers stop trusting CI.
- Forgetting to test structured output contracts after prompt or schema changes.

---

## Portfolio Evidence

Keep a sanitized CI workflow contract and required-checks policy.

Redact:

- internal repository names
- secret identifiers
- exact branch names if needed
- internal environment names

What remains demonstrates that you can design CI for AI systems with cost, security, and maintainability in mind.

---

## References

- [GitHub Actions Documentation](https://docs.github.com/actions)
- [GitHub Actions Workflow Syntax](https://docs.github.com/actions/using-workflows/workflow-syntax-for-github-actions)
- [Using Secrets in GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions)
- [Security Hardening for GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)
- [OpenTelemetry Concepts](https://opentelemetry.io/docs/concepts/)
