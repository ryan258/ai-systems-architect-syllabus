# Lesson 15.06: Infrastructure as Code and CI/CD Review Package

**Parent syllabus:** [Syllabus 15: Infrastructure as Code and CI/CD](../../syllabus-15-infrastructure-as-code-and-cicd.md)  
**Estimated time:** 3 hours  
**Artifact:** Complete infrastructure as code and CI/CD review package

---

## Outcome

By the end of this lesson, you can assemble the module artifacts into a reviewer-ready package that shows whether one AI system is prepared for repeatable deployment and controlled release.

You will produce a complete infrastructure as code and CI/CD review package that includes:

- environment matrix and promotion flow
- Terraform stack sketch and state policy
- CI workflow contract and required checks
- delivery pipeline brief and rollback playbook
- config, secrets, and drift control plan
- final readiness decision

---

## Why This Matters

Individual operational documents are useful. A coherent package is what supports a real go or no-go decision.

Common failures:

- Environment rules say one artifact is promoted forward, but the delivery process rebuilds before production.
- Terraform manages the runtime, but config and secret ownership remain unclear.
- CI proves code quality, but release gates ignore AI-specific regression and cost signals.
- Rollback depends on the previous version still being deployable, but the promotion flow never preserves that path.
- Drift policy exists on paper, but the release decision never checks whether drift is present.
- Good documents exist separately, but nobody can judge overall readiness quickly.

This package is the bridge between "we wrote the docs" and "we can justify depending on this release process."

---

## Best-Practice Principles

1. **Make every artifact answer a release-readiness question.**  
   If a section does not help judge repeatability, safety, or maintainability, simplify it.

2. **Keep the package internally consistent.**  
   Promotion rules, infrastructure code, CI gates, rollback posture, and config controls should not contradict each other.

3. **Trace release risk to specific controls.**  
   Privacy, security, reliability, cost, and governance concerns should map to a documented safeguard.

4. **Separate facts, assumptions, blockers, and change triggers.**  
   Review quality depends on clear uncertainty.

5. **End with a decision.**  
   The package should support staging only, limited rollout, expand, or hold.

6. **Preserve operator usefulness over documentation volume.**  
   Short, connected artifacts beat a bloated package nobody uses.

7. **Keep a sanitized portfolio version.**  
   Strong delivery discipline is valuable evidence after private details are removed.

---

## Concepts

### IaC and CI/CD Review Package

A compact set of artifacts used to judge whether infrastructure and release flow are repeatable, reviewable, and recoverable.

### Release Readiness Decision

A deliberate judgment about whether the current system should remain in staging, enter limited rollout, expand, or hold.

### Delivery Blocker

A missing control or unresolved mismatch that prevents safe promotion.

Examples:

- state backend not protected
- required CI checks not enforced
- rollback path untested
- secret rotation ownership unclear

### Change Trigger

A future change that should reopen the package.

Examples:

- new cloud provider or runtime
- new deployment topology
- new external side effect
- major config or secret model change

---

## Infrastructure as Code and CI/CD Review Package Template

Use this structure:

```markdown
# Infrastructure as Code and CI/CD Review Package: [Workflow Name]

## 1. Executive Summary

- Workflow goal:
- Main delivery risks:
- Current recommendation:

## 2. Environment and Promotion Design

- Environments:
- Promotion path:
- Release authority:
- Demo isolation rule:

## 3. Infrastructure as Code Posture

- Managed resources:
- State policy:
- Plan and apply controls:
- Drift policy:

## 4. CI Controls

- Required checks:
- Mock-model testing rule:
- Secret handling rule:
- Branch protection:

## 5. Delivery and Rollback Controls

- Staging gates:
- Production gates:
- Rollout shape:
- Rollback triggers:

## 6. Config, Secrets, and Recovery

- Source-of-truth map:
- Rotation posture:
- Recovery notes:

## 7. Privacy, Security, Cost, Observability, and Governance Notes

- Privacy controls:
- Security controls:
- Cost controls:
- Observability requirements:
- Owners and approvers:

## 8. Open Questions and Blockers

- ...

## 9. Decision

Staging only, limited rollout, expand, or hold.

## 10. Sanitized Portfolio Version

What must be removed before public sharing?
```

---

## Walkthrough

Use this example:

> A support drafting service now has cloud deployment, reliability instrumentation, Terraform sketches, CI rules, and a defined release path. The team needs one review artifact before broader rollout.

### Step 1 - Gather the module artifacts

Package contents:

- environment matrix and promotion flow from Lesson 15.01
- Terraform stack sketch and state policy from Lesson 15.02
- CI workflow contract and required checks from Lesson 15.03
- delivery pipeline brief and rollback playbook from Lesson 15.04
- config and secrets control plan from Lesson 15.05

The package should let a reviewer understand repeatability and risk without reading every repository file.

### Step 2 - Check internal consistency

Examples of required alignment:

| Claim | Where it should appear |
| --- | --- |
| one artifact is promoted to production | environment policy, delivery brief, rollback plan |
| production release depends on required CI checks | CI policy, promotion flow, readiness decision |
| stateful changes need explicit handling | Terraform posture, delivery brief, rollback notes |
| secrets are referenced at runtime, not stored in code | IaC posture, config plan, CI policy |
| drift can block release | drift policy, delivery gates, decision section |

If those claims appear in only one artifact, the package is weak.

### Step 3 - Write the executive summary

Example:

```markdown
# Infrastructure as Code and CI/CD Review Package: Support Drafting Service

## 1. Executive Summary

- Workflow goal: deploy and operate a support drafting API through a repeatable path that protects client data, cost budgets, and release safety.
- Main delivery risks: config drift, secret handling mistakes, stateful release changes, and unreviewed infrastructure edits.
- Current recommendation: limited rollout only, with plan review enforced, required CI checks active, rollback drill completed, and secret rotation ownership documented.
```

The summary should expose the operating pressure quickly.

### Step 4 - Turn the package into a decision

Weak ending:

> The deployment setup looks reasonably mature.

Better ending:

> Limited rollout only. Do not expand until production branch protection enforces the required checks, weekly drift review is running, and the rollback playbook has been exercised against the current release path.

The package should change what happens next.

### Step 5 - Name the change triggers

For this service, reopen the package if:

- a new runtime or cloud provider is introduced
- deployment topology changes materially
- production approval is automated end to end
- new stateful dependencies are added
- secret rotation or config management tooling changes

That is how the package stays useful instead of becoming a one-time document.

---

## Practice

Choose one AI system that is deployed or likely to deploy soon.

Assemble a complete **Infrastructure as Code and CI/CD Review Package** with:

1. executive summary
2. environment and promotion design
3. infrastructure as code posture
4. CI controls
5. delivery and rollback controls
6. config, secrets, and recovery notes
7. privacy, security, cost, observability, and governance notes
8. open questions and blockers
9. final decision

End with one of these decisions:

- staging only
- limited rollout
- expand
- hold

---

## Review Checklist

- The package includes every core module artifact.
- Promotion flow, IaC posture, and rollback path agree with each other.
- CI requirements are connected to release gates.
- Config and secret controls match the infrastructure design.
- Drift posture is part of release judgment.
- Privacy, security, cost, observability, and governance notes are present.
- Open blockers are specific enough to act on.
- The package ends with a clear decision.

---

## Common Failure Modes

- Treating separate documents as complete without checking for contradictions.
- Forgetting that rollback depends on preserved artifacts and compatible state.
- Documenting CI and delivery separately without connecting them to merge rules.
- Leaving config ownership vague even though infrastructure code is well organized.
- Calling the system repeatable while drift is unmanaged.
- Ending the review with a description instead of a decision.

---

## Portfolio Evidence

Keep a sanitized copy of your final review package.

Redact:

- client names
- project IDs
- environment names if sensitive
- secret references
- internal URLs and thresholds

What remains is strong evidence that you can design repeatable delivery and infrastructure controls for real AI systems.

---

## References

- [Terraform Language Documentation](https://developer.hashicorp.com/terraform/language)
- [GitHub Actions Documentation](https://docs.github.com/actions)
- [GitHub Actions Workflow Syntax](https://docs.github.com/actions/using-workflows/workflow-syntax-for-github-actions)
- [Google SRE Workbook - Canarying Releases](https://sre.google/workbook/canarying-releases/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
