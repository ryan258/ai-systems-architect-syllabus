# Lesson 12.05: Production Readiness Checklist

**Parent syllabus:** [Syllabus 12: Production AI Systems](../../syllabus-12-production-ai-systems.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Production readiness checklist and operating cadence

---

## Outcome

By the end of this lesson, you can turn scattered production notes into a short checklist that determines whether a workflow should launch, stay limited, be redesigned, or be held.

You will produce:

- a production readiness checklist
- launch-day checks
- a first-week and weekly operating cadence
- explicit blockers and sign-off rules

---

## Why This Matters

Teams often have the right controls somewhere, but not in one place at launch time.

Common failures:

- Cost limits exist in code, but nobody verified the stop event in staging.
- Monitoring exists, but the alert owner does not know the first response.
- Human handoff is documented, but the backup reviewer is never assigned.
- Security and privacy controls are assumed because the design looks careful.
- Launch happens because "nothing seems obviously wrong" instead of because checks passed.
- The team uses one launch checklist once and never defines the weekly review cadence.
- A blocker is known, but no one labels it as a blocker, so the system ships anyway.

Readiness checklists are not administrative overhead. They are the last compression step before real exposure.

---

## Best-Practice Principles

1. **The checklist should change the launch decision.**  
   If a failed item does not block or constrain rollout, the checklist is ceremonial.

2. **Keep the checklist short enough to use.**  
   A strong checklist is deliberate and finite.

3. **Require evidence, not confidence.**  
   "We think it works" is weaker than "the stop event fired in staging."

4. **Separate pre-launch, launch-day, and weekly operating checks.**  
   They answer different operational questions.

5. **Include owners and fallback owners.**  
   A production check with no owner is a hidden gap.

6. **Review cost, observability, security, privacy, evaluation, handoff, and governance together.**  
   Production failures usually cross categories.

7. **Use the same checklist after launch.**  
   Weekly use is how the checklist stays real.

---

## Concepts

### Readiness Gate

A condition that must be met before the next rollout step.

Examples:

- launch blocked
- limited rollout only
- safe to expand

### Evidence Line

The concrete proof that a checklist item has passed.

Examples:

- staging trace showing stop event
- alert fired in test and reached owner
- approval queue route exercised with timeout

### Blocker

A missing control or unproven behavior that prevents safe launch or expansion.

### Operating Cadence

The recurring review cycle after launch.

Examples:

- launch-day verification
- first-week daily review
- weekly operating review

### Sign-Off Rule

The named owner or approver required before the service launches or expands.

---

## Production Readiness Checklist Template

Use this structure:

```markdown
# Production Readiness Checklist: [Workflow Name]

## System Summary

- Workflow goal:
- Users affected:
- External actions:
- Current rollout stage:

## Pre-Launch Checks

| Area | Requirement | Evidence | Owner | Status |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## Launch-Day Checks

- ...

## First-Week Review

- Daily checks:
- Sampling plan:
- Escalation rule:

## Weekly Operating Review

- Cost review:
- Drift review:
- Queue review:
- Incident review:

## Blockers

- ...

## Decision and Sign-Off

- Decision:
- Technical owner:
- Business or client approver:
- Next review date:
```

Suggested checklist areas:

- cost controls
- observability
- safety boundary and prompt injection defenses
- privacy and logging rules
- approval queue and human handoff
- evaluation evidence
- rollback or halt path
- ownership and approvals

---

## Walkthrough

Use this example:

> A support drafting API is ready for limited rollout to one support queue, with mandatory human approval before anything customer-facing is sent.

### Step 1 - Gather the production artifacts

Before writing the checklist, gather:

- cost architecture worksheet
- observability spec
- safety-boundary spec
- approval queue operating policy
- latest evaluation or staging evidence

The checklist should point to this evidence. It should not replace it.

### Step 2 - Write pre-launch checks that can fail

Weak check:

> Monitoring is configured.

Better check:

> A test alert reached the named owner, and the owner knows the first-response steps.

Weak check:

> Human review exists.

Better check:

> The customer-facing queue path, timeout rule, and backup reviewer were exercised once in staging.

Evidence turns belief into operations.

### Step 3 - Separate launch-day checks

Launch-day checks are narrower:

- correct configuration version deployed
- alert destinations verified
- queue owner on duty
- rollback or disable path confirmed
- first sampling window scheduled

These are not the same as design review questions.

### Step 4 - Define the first-week review

For this service, a useful first-week cadence might be:

- daily sample of 10 approved drafts
- daily review of stop events and alert volume
- daily review of queue age and rejection reasons
- hold expansion if rejection rate or fallback usage exceeds baseline

This is where limited rollout becomes a real operating mode.

### Step 5 - Name blockers honestly

Examples of blockers:

- no verified daily spend stop
- no quiet-failure review loop
- no backup reviewer for the approval queue
- no attack-test result for retrieved-content injection

Known gaps that do not change the launch decision are notes. Known gaps that do change the launch decision are blockers.

### Step 6 - End with a decision

Possible outcomes:

- hold
- limited rollout only
- expand to next cohort
- redesign before wider use

The checklist earns its place only when it affects what happens next.

---

## Practice

Choose one workflow that is at prototype, staging, or limited-rollout stage.

Create a production readiness checklist that includes:

1. system summary
2. at least 12 pre-launch checks
3. at least five launch-day checks
4. a first-week review cadence
5. a weekly operating review
6. explicit blockers
7. a final decision and sign-off section

At least one pre-launch check must require evidence for:

- cost guardrails
- observability or alerting
- security or trust boundaries
- human handoff

---

## Review Checklist

Your readiness artifact is acceptable when:

- Checklist items can pass or fail.
- Evidence is required.
- Owners are named.
- Pre-launch and launch-day checks are separated.
- Weekly operating review is included.
- Blockers are explicit.
- The decision section changes rollout behavior.
- The checklist is short enough to use repeatedly.
- The artifact would still make sense on a low-energy day.

---

## Common Failure Modes

- **Checklist theater:** Items are present, but none can actually block launch.
- **Confidence without evidence:** The team believes controls exist but cannot prove them.
- **One-time use:** The checklist is written for launch and never reused.
- **No owner:** Every item is "team-owned."
- **No operating cadence:** Launch is defined, but post-launch review is not.
- **Blockers hidden as notes:** Important gaps are documented but do not change action.

---

## Portfolio Evidence

Save:

- the production readiness checklist
- the blocker list
- one short note explaining the final decision

This shows that you can convert production controls into a real launch gate instead of a loose checklist.

---

## References

- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- OpenTelemetry Concepts: https://opentelemetry.io/docs/concepts/
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
