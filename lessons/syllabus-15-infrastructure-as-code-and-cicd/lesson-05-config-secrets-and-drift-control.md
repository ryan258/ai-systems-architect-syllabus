# Lesson 15.05: Config, Secrets, and Drift Control

**Parent syllabus:** [Syllabus 15: Infrastructure as Code and CI/CD](../../syllabus-15-infrastructure-as-code-and-cicd.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Config and secrets control plan with drift-recovery checklist

---

## Outcome

By the end of this lesson, you can separate ordinary configuration from secrets, define how settings change across environments, and document how to detect and recover from infrastructure drift without improvisation.

You will produce:

- a config inventory
- a secrets control plan
- a drift-recovery checklist

---

## Why This Matters

Deployment failures often come from settings, not code.

Common failures:

- Environment variables grow without ownership or naming rules.
- Secret rotation happens ad hoc and breaks the live service.
- Config lives partly in code, partly in dashboards, and partly in memory.
- Drift is noticed only when the next deploy behaves strangely.
- Terraform state is backed up, but nobody knows how to recreate the surrounding secret and config structure.
- Sensitive values appear in logs, test fixtures, or incident notes.

Configuration discipline is what turns infrastructure from "it currently works" into "we can keep operating it."

---

## Best-Practice Principles

1. **Classify settings before storing them.**  
   Not every setting is a secret, and not every environment variable belongs in code.

2. **Give every critical setting an owner and a source of truth.**  
   Operators should know where to look and who can approve change.

3. **Rotate secrets through a documented path.**  
   Rotation should be routine, not a crisis maneuver.

4. **Detect drift on a schedule and before important releases.**  
   Silent divergence is an operational risk.

5. **Avoid leaking sensitive data into logs, plans, and artifacts.**  
   Privacy controls should extend into your deployment tooling.

6. **Keep recreate-from-scratch notes realistic.**  
   A recovery document should match the actual system, not an old idealized design.

7. **Tie config governance to reliability and cost.**  
   Wrong timeouts, quotas, or scale settings can be just as harmful as broken code.

---

## Concepts

### Configuration

Non-secret settings that define runtime behavior.

Examples:

- environment name
- feature flag
- retry limit
- queue name

### Secret

A value that must be protected because exposure creates security, privacy, or operational risk.

Examples:

- API keys
- signing secrets
- database passwords
- service credentials

### Source of Truth

The authoritative place where a setting or secret should be defined and reviewed.

### Drift Recovery

The process of detecting unmanaged change and returning the system to a known, documented state.

### Rotation Window

A planned period where old and new secrets or config values may coexist to support safe cutover.

---

## Config and Secrets Control Plan Template

Use this structure:

```markdown
# Config and Secrets Control Plan: [Workflow Name]

## 1. Configuration Inventory

| Setting | Type | Environment scope | Source of truth | Owner | Failure impact |
| --- | --- | --- | --- | --- | --- |
|  | config or secret |  |  |  |  |

## 2. Storage Rules

- Code-config rule:
- Environment variable rule:
- Secret manager rule:
- Forbidden storage locations:

## 3. Rotation and Change Rules

- Rotation cadence:
- Dual-value or cutover rule:
- Validation after rotation:
- Emergency revoke rule:

## 4. Drift Control

- Drift checks run:
- Release blocker drift categories:
- Reconciliation owner:

## 5. Recovery Notes

- How to rebuild config:
- How to restore secret references:
- How to validate recovery:

## 6. Open Questions

- ...
```

Drift-recovery checklist structure:

```markdown
# Drift-Recovery Checklist: [Workflow Name]

1. Confirm the changed resource or setting.
2. Determine whether the source of truth is code, config store, or secret manager.
3. Assess privacy, security, reliability, and cost impact.
4. Decide whether release must be blocked.
5. Reconcile the change back into the approved path.
6. Verify the live system after reconciliation.
7. Record the cause and prevention step.
```

Review questions:

- Which settings are secrets, and which are ordinary configuration?
- Can a secret be rotated without downtime?
- What categories of drift should block production release?

---

## Walkthrough

Use this example:

> A support drafting service uses webhook signing keys, model-provider credentials, queue identifiers, feature flags, and observability settings across staging and production.

### Step 1 - Classify the settings

For this service:

- queue name is config
- feature flag for limited rollout is config
- webhook signing key is a secret
- model API key is a secret
- log level is config
- tracing endpoint may be config, but auth for it may be a secret

Classification reduces confusion and prevents overprotecting everything or underprotecting the values that matter.

### Step 2 - Decide where each category lives

A reasonable rule set:

- stable non-secret defaults live in version-controlled config
- per-environment overrides come from environment-specific config sources
- secrets live in a managed secret store
- Terraform manages references and access, not raw secret values

Forbidden locations might include:

- committed `.env` files with live credentials
- screenshots of dashboards in team docs
- CI artifacts containing secret values

### Step 3 - Write rotation procedures

For the support drafting service, secret rotation should say:

1. create the new secret version
2. update the runtime reference
3. deploy or reload using the approved path
4. verify healthy auth and service behavior
5. retire the previous version after the safety window

This protects availability while reducing long-lived secret risk.

### Step 4 - Detect drift intentionally

For this workflow, drift checks could happen:

- weekly on a schedule
- before each production release
- after emergency manual changes

Release-blocking drift categories might include:

- ingress or auth configuration
- secret access policy
- scaling or cost-related settings
- observability and alert routing

If drift touches those areas, do not normalize it. Reconcile it.

### Step 5 - Make recovery notes concrete

If this service had to be rebuilt:

- Terraform recreates the runtime, IAM, and secret bindings
- secret manager holds the current secret versions
- config store contains environment-specific settings
- smoke tests verify webhook auth, retrieval access, and review-queue delivery

Recovery notes should allow a real rebuild, not just describe architecture at a high level.

### Step 6 - Keep privacy in scope

Configuration and secret systems can leak sensitive data through:

- verbose logs
- support bundles
- debug screenshots
- copied environment files

Your control plan should state what must never appear in shared artifacts or incident chat.

---

## Practice

Choose one deployed or deployable AI service and create a **Config and Secrets Control Plan** that includes:

1. configuration inventory
2. source of truth per setting
3. storage rules
4. rotation steps
5. drift detection and reconciliation rules
6. rebuild and validation notes

Add a short drift-recovery checklist that an operator could follow during a release hold.

---

## Review Checklist

- Settings are classified as config or secret on purpose.
- Every critical value has a source of truth and owner.
- Secret storage avoids code, plans, and CI artifacts where possible.
- Rotation steps are documented and testable.
- Drift checks are scheduled and tied to release practice.
- Recovery notes describe how the live system is recreated.
- Sensitive values are excluded from logs and shared artifacts.
- Reliability, cost, privacy, and governance implications are visible.

---

## Common Failure Modes

- Treating all environment variables as equally safe.
- Rotating secrets without a cutover plan or validation step.
- Storing important config partly in dashboards and forgetting to document it.
- Ignoring drift because the system still appears healthy.
- Allowing debug tooling to expose secret values.
- Writing recovery notes that assume knowledge only one operator has.
- Forgetting that quota, timeout, or concurrency settings are operationally important config.

---

## Portfolio Evidence

Keep a sanitized copy of your config inventory, secret policy, and drift checklist.

Redact:

- secret names and paths
- environment identifiers
- internal hostnames
- exact quotas and limits if sensitive

What remains demonstrates operational maturity around settings, access, and recovery.

---

## References

- [Terraform State](https://developer.hashicorp.com/terraform/language/state)
- [Using Secrets in GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions)
- [Security Hardening for GitHub Actions](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions)
- [Google SRE Workbook - Configuration Design](https://sre.google/workbook/configuration-design/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
