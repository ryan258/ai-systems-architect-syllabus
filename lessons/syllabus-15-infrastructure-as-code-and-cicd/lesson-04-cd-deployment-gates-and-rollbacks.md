# Lesson 15.04: CD, Deployment Gates, and Rollbacks

**Parent syllabus:** [Syllabus 15: Infrastructure as Code and CI/CD](../../syllabus-15-infrastructure-as-code-and-cicd.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Delivery pipeline brief and rollback playbook

---

## Outcome

By the end of this lesson, you can map a continuous delivery path for an AI system, define measurable deployment gates, and write rollback rules that are concrete enough to use under pressure.

You will produce:

- a delivery pipeline brief
- deployment gates for staging and production
- a rollback playbook

---

## Why This Matters

CI only proves the code is fit to continue. Delivery design decides whether release risk is controlled.

Common failures:

- Every successful merge goes straight to production with no useful pause for judgment.
- Rollback exists as a hope, not as a defined path.
- Database or vector index changes are deployed as if they were stateless code.
- Release gates watch uptime but ignore quality, queue health, or cost regression.
- Operators cannot tell whether to hold, continue, or revert when metrics move.
- The deployment checklist is so long that nobody uses it during a real release.

Continuous delivery is the discipline of turning technical readiness into a safe release decision.

---

## Best-Practice Principles

1. **Separate build success from production readiness.**  
   Passing CI does not mean the release should go live.

2. **Define deployment gates in measurable terms.**  
   Metrics, smoke tests, and approval steps should be stated before the release.

3. **Make rollback the first recovery path.**  
   When a release is bad, reverting to known-good behavior is usually safer than improvising a fix.

4. **Treat state changes as special risk.**  
   Database migrations, vector index rebuilds, and queue schema changes need explicit handling.

5. **Use limited rollout to reduce blast radius.**  
   Small early exposure creates evidence before broad release.

6. **Keep the release checklist short and live.**  
   A usable checklist is an operational tool. A huge checklist becomes decoration.

7. **Include quality, privacy, and cost gates where they matter.**  
   AI systems can fail by becoming wrong, expensive, or overexposed even while staying "up."

---

## Concepts

### Continuous Delivery

A release process where code is always close to deployable, but production release still requires defined gates and approval.

### Continuous Deployment

A release process where passing automation deploys changes all the way to production automatically.

### Deployment Gate

A condition that must pass before the release moves to the next stage.

Examples:

- smoke test passes
- error rate stays inside threshold
- quality review sample passes
- human approver signs off

### Rollback Playbook

A written set of steps and triggers for returning to a known-good version.

### Compatibility Window

A period where old and new versions, schemas, or indexes must coexist safely during rollout or rollback.

---

## Delivery Pipeline Brief Template

Use this structure:

```markdown
# Delivery Pipeline Brief: [Workflow Name]

## 1. Release Path

1. Merge to main:
2. Build artifact:
3. Staging deploy:
4. Staging verification:
5. Production approval:
6. Production rollout:

## 2. Deployment Gates

| Stage | Gate | Evidence | Owner | Block release if fail? |
| --- | --- | --- | --- | --- |
| Staging |  |  |  |  |
| Production |  |  |  |  |

## 3. State Change Rules

- Database migrations:
- Vector index or retrieval changes:
- Background jobs:

## 4. Rollout Shape

- Full deploy or phased:
- First live cohort:
- Observation window:

## 5. Rollback Playbook

- Rollback triggers:
- Revert action:
- Verification after rollback:
- Communication rule:

## 6. Open Questions

- ...
```

Review questions:

- What conditions should stop rollout immediately?
- Which release changes require a compatibility window?
- Can the operator explain rollback in one minute?

---

## Walkthrough

Use this example:

> A support drafting service is ready to move from staging to a limited production rollout for one support queue.

### Step 1 - Map the delivery stages

A practical path could be:

1. Merge after CI passes.
2. Build one container artifact.
3. Deploy to staging automatically.
4. Run smoke tests, webhook verification, and a small quality spot check.
5. Require human production approval.
6. Release to one queue first.
7. Observe metrics for a defined window.
8. Expand only if gates stay green.

That is continuous delivery. The release is automated where possible, but judgment still has a place.

### Step 2 - Define useful gates

For this service, useful staging gates might include:

- service starts and health checks pass
- webhook verification works
- retrieval dependency is reachable
- draft payload schema is valid
- sample review queue items look correct

Useful production gates might include:

- latency and error rate remain within policy
- reviewer rejection rate does not spike
- queue age stays bounded
- cost per completed draft stays inside budget

Those gates combine technical reliability with AI-specific quality and cost signals.

### Step 3 - Treat stateful changes carefully

Risky release categories:

- database schema changes
- vector index schema changes
- retrieval corpus rebuilds
- queue contract changes

For these, the playbook should state:

- whether backward compatibility is required
- whether rollout must pause until migration completes
- whether rollback leaves the system in a safe state

Stateless rollback logic is not enough when state changes with the release.

### Step 4 - Write rollback triggers before release

For this workflow, rollback triggers might include:

- sustained elevated error rate
- p95 latency outside the observation boundary
- sudden reviewer rejection surge
- queue backlog or timeout growth
- cost spike outside the release budget

If rollback triggers are vague, operators will hesitate at the worst possible time.

### Step 5 - Keep the playbook operational

A usable rollback playbook might say:

1. Stop rollout expansion.
2. Route traffic to the previous stable revision.
3. Confirm health checks and key business metrics recover.
4. Pause any incompatible migration step.
5. Notify named stakeholders.
6. Open an incident review item before retrying release.

That is concrete enough to use at 2 AM.

### Step 6 - Include manual approval where it earns its keep

For small client systems, manual production approval is often the right tradeoff.

It gives space to answer:

- did staging actually prove what mattered?
- are cost and support staff ready for the release?
- is the rollback path still available?

Automation should remove toil, not remove judgment where judgment is still necessary.

---

## Practice

Choose one deployable AI system and write a **Delivery Pipeline Brief** that includes:

1. release stages
2. staging gates
3. production gates
4. treatment of stateful changes
5. rollout shape and observation window
6. rollback triggers and actions

End with a short statement of whether your system is ready for staging only, limited rollout, or broader release.

---

## Review Checklist

- Release stages are clear.
- Gates are measurable, not vague.
- Staging and production prove different things on purpose.
- Stateful changes have explicit compatibility rules.
- Rollout starts with a bounded cohort where possible.
- Rollback triggers are defined before release.
- Rollback verification steps are documented.
- Quality, privacy, observability, and cost are part of release judgment.

---

## Common Failure Modes

- Treating successful CI as permission for full production rollout.
- Forgetting that index or schema changes can outlive a rollback.
- Using only uptime checks as release gates.
- Writing rollback steps that depend on human memory instead of documentation.
- Releasing to all traffic when limited rollout would have been easy.
- Leaving production approval ownership vague.
- Building a long release checklist that is never used live.

---

## Portfolio Evidence

Keep a sanitized copy of your delivery pipeline brief and rollback playbook.

Redact:

- customer names
- service URLs
- release windows
- operator identities
- internal metric thresholds if they are sensitive

What remains shows that you can design safe delivery and recovery for live AI systems.

---

## References

- [Google SRE Workbook - Canarying Releases](https://sre.google/workbook/canarying-releases/)
- [Google SRE Book - Service Level Objectives](https://sre.google/sre-book/service-level-objectives/)
- [GitHub Actions Documentation](https://docs.github.com/actions)
- [OpenTelemetry Concepts](https://opentelemetry.io/docs/concepts/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
