# Lesson 15.01: Environments and Release Flow

**Parent syllabus:** [Syllabus 15: Infrastructure as Code and CI/CD](../../syllabus-15-infrastructure-as-code-and-cicd.md)  
**Estimated time:** 2 hours  
**Artifact:** Environment matrix and promotion flow policy

---

## Outcome

By the end of this lesson, you can define the environments your AI system uses, explain what belongs in each one, and write a promotion flow that moves one tested artifact from build to production without chaos.

You will produce:

- an environment matrix
- a promotion flow policy
- a release ownership map

---

## Why This Matters

Cloud deployment only solves where the system runs. It does not solve how changes move safely.

Common failures:

- Local development uses fake settings, while production depends on behavior never exercised before release.
- A demo tenant shares infrastructure with real client traffic.
- Staging exists, but nobody knows what it proves.
- Production is rebuilt by hand instead of receiving the same artifact that passed earlier checks.
- Config differences between environments accumulate until failures appear only after deploy.
- Ownership is vague, so releases happen without a clear approver or rollback decision-maker.

Environment design is the first control surface for reliability, privacy, security, cost, and maintainability.

---

## Best-Practice Principles

1. **Give each environment one clear purpose.**  
   If local, staging, and production all mean "where the app runs," the release path will drift.

2. **Promote the same artifact forward.**  
   Build once, verify it, and deploy that artifact rather than rebuilding per environment.

3. **Keep demos and experiments out of production paths.**  
   Demo pressure causes bad shortcuts unless it has a separate boundary.

4. **Make environment differences explicit.**  
   Runtime scale, secrets, data sources, and alerting rules should be intentional, not accidental.

5. **Use staging to answer specific release questions.**  
   If staging proves nothing, it becomes cost without safety value.

6. **Define release authority before an incident.**  
   Someone should own promotion, hold, and rollback decisions.

7. **Write the smallest environment set that still protects the system.**  
   Too many environments add maintenance cost. Too few hide release risk.

---

## Concepts

### Environment

A bounded runtime context with its own configuration, data policy, and operating purpose.

Common examples:

- local
- development
- staging
- production

### Promotion Flow

The path a release takes from code change to live service.

Example:

`commit -> CI checks -> build artifact -> staging deploy -> manual approval -> production deploy`

### Immutable Artifact

A deployable build output that should not change as it moves across environments.

Examples:

- container image digest
- versioned wheel or package
- pinned deployment bundle

### Environment Parity

The idea that important runtime behavior should match closely enough across environments to make earlier verification meaningful.

### Demo Isolation

Keeping client demos, experiments, or sales previews away from real production systems, data, and operational budgets.

---

## Environment Matrix and Promotion Flow Template

Use this structure:

```markdown
# Environment Matrix and Promotion Flow: [Workflow Name]

## 1. Service Summary

- Workflow goal:
- Main external dependencies:
- Human review or approval steps:

## 2. Environment Definitions

| Environment | Purpose | Data policy | Deploy source | Who can change it | Reliability expectation | Cost guardrail |
| --- | --- | --- | --- | --- | --- | --- |
| Local |  |  |  |  |  |  |
| Development |  |  |  |  |  |  |
| Staging |  |  |  |  |  |  |
| Production |  |  |  |  |  |  |

## 3. Promotion Flow

1. Code is merged only after:
2. Artifact is built by:
3. Staging proves:
4. Production approval requires:
5. Rollback authority belongs to:

## 4. Environment-Specific Differences

| Category | Local | Development | Staging | Production |
| --- | --- | --- | --- | --- |
| Config source |  |  |  |  |
| Secrets source |  |  |  |  |
| Data source |  |  |  |  |
| Monitoring |  |  |  |  |
| Scale target |  |  |  |  |

## 5. Release Rules

- Production never receives:
- Demo traffic is handled by:
- Emergency rollback path:
- Change freeze or hold conditions:

## 6. Open Questions

- ...
```

Review questions:

- What does staging prove that local cannot?
- Which environments handle real client data?
- Can one artifact move through the whole release path unchanged?

---

## Walkthrough

Use this example:

> A support drafting FastAPI service receives ticket events, retrieves policy context, drafts a response, and places the draft into a human review queue.

### Step 1 - Define the minimum useful environment set

For a small service, a practical set might be:

- **Local:** developer laptop with mocked model calls and fake tickets
- **Development:** shared integration environment for basic webhook, auth, and storage checks
- **Staging:** production-like runtime with sanitized or synthetic data, full deployment path, and alerting validation
- **Production:** real client traffic with strict release and rollback control

You do not need every possible environment. You need enough separation to prevent risky mixing.

### Step 2 - State what staging is for

Staging should answer release questions such as:

- Does the exact container start with the real runtime configuration pattern?
- Do webhook auth, secret injection, and dependency access work?
- Does the release produce expected traces, logs, and health checks?
- Do rollback steps actually revert the service?

If staging cannot answer those questions, it is just an expensive duplicate.

### Step 3 - Name the environment differences

For this service:

- local uses mocked model responses and local files
- development uses shared test services and lower-cost quotas
- staging uses sanitized data and production-like secret delivery
- production uses live data, full observability, and tighter approval rules

The goal is controlled difference, not perfect sameness.

### Step 4 - Write the promotion flow

A practical promotion path could be:

1. Merge after CI passes and required reviewers approve.
2. Build one container image and tag it with a commit SHA.
3. Deploy that image to staging through automation.
4. Run smoke checks, release verification, and limited operational review.
5. Require human approval before production.
6. Deploy the same image digest to production.
7. Watch defined release metrics and rollback if triggers fire.

That is simple enough to use and strong enough to prevent a large class of release mistakes.

### Step 5 - Separate demos from production

For this workflow, demos should not point at production queues, production secrets, or live customer tickets.

Safer options:

- a demo environment with fake or heavily sanitized data
- recorded request fixtures
- a staging tenant with explicit limits and no production side effects

This is a privacy and governance control, not just a convenience.

### Step 6 - Assign ownership

For a solo architect, ownership may still be split:

- release operator
- production approver
- incident responder
- secret owner

One person can hold multiple roles, but the responsibilities should still be named.

---

## Practice

Choose one AI workflow that is already deployed or likely to be deployed soon.

Produce an **Environment Matrix and Promotion Flow** that includes:

1. four environments or a justified smaller set
2. data policy per environment
3. config and secret source per environment
4. artifact promotion path
5. release approval and rollback ownership
6. a rule for demo isolation

Keep the artifact short enough that an operator could actually use it during a release review.

---

## Review Checklist

- Each environment has a distinct purpose.
- Real client data handling is explicit.
- Staging proves concrete release questions.
- One artifact can move from build to production.
- Demo or experimental traffic is isolated from production.
- Promotion, hold, and rollback authority are named.
- Cost, observability, and privacy differences are documented.

---

## Common Failure Modes

- Creating a staging environment that is never used for release evidence.
- Rebuilding the artifact per environment and assuming the result is equivalent.
- Letting demo systems share production secrets or data.
- Treating configuration drift as normal instead of as a release risk.
- Using production as the first place webhook auth or queue behavior is tested.
- Adding too many environments for a small team to maintain well.

---

## Portfolio Evidence

Keep a sanitized copy of your environment matrix and promotion flow.

Redact:

- client names
- endpoint URLs
- secret paths
- tenant identifiers
- exact production scale values

What remains is strong evidence that you can design release boundaries, not just deploy code.

---

## References

- [Google SRE Workbook - Configuration Design](https://sre.google/workbook/configuration-design/)
- [Google SRE Workbook - Canarying Releases](https://sre.google/workbook/canarying-releases/)
- [FastAPI Deployment Concepts](https://fastapi.tiangolo.com/deployment/concepts/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
