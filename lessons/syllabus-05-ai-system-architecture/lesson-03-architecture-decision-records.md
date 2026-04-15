# Lesson 05.03: Architecture Decision Records

**Parent syllabus:** [Syllabus 05: AI System Architecture](../../syllabus-05-ai-system-architecture.md)  
**Estimated time:** 2 hours  
**Artifact:** Three Architecture Decision Records

---

## Outcome

By the end of this lesson, you can write lightweight Architecture Decision Records (ADRs) that explain why an AI system is designed the way it is.

You will produce three ADRs for one AI system:

- One model or orchestration decision
- One data or storage decision
- One security, privacy, or human-handoff decision

---

## Why This Matters

AI systems create fast-moving decisions that are easy to forget:

- Why did we use RAG instead of fine-tuning?
- Why does this workflow require human approval?
- Why do we stream responses instead of using background jobs?
- Why is ticket text not stored in the database?
- Why did we choose one model provider over another?
- Why is the agent not allowed to call this tool directly?

If decisions are not recorded, future work becomes guesswork. Teams re-open settled debates, reverse decisions without understanding the original constraints, and lose the context needed to operate the system safely.

ADRs protect decision memory.

---

## Best-Practice Principles

1. **Record decisions when they are made.**  
   Do not wait six months and reconstruct the story from memory.

2. **Write one ADR per decision.**  
   Do not bundle model choice, database choice, auth choice, and deployment choice into one document.

3. **Document the why, not just the what.**  
   The implementation can be found in code. The ADR exists to preserve context, constraints, alternatives, and consequences.

4. **Include rejected options.**  
   The rejected options often matter more than the chosen option when circumstances change later.

5. **Name consequences honestly.**  
   Every architecture decision has trade-offs. An ADR that only lists positives is marketing, not engineering.

6. **Supersede instead of deleting.**  
   If context changes, write a new ADR and mark the old one as superseded. Keep the decision history.

7. **Connect decisions to artifacts.**  
   Link the ADR to diagrams, code, evals, runbooks, risks, or issues that it affects.

---

## Concepts

### Architecture Decision

A decision that meaningfully affects system qualities such as reliability, privacy, security, cost, latency, maintainability, or user experience.

Examples:

- Use a queue for long-running model jobs.
- Require human approval before external writes.
- Use RAG instead of fine-tuning.
- Store only metadata, not raw client documents.
- Use SSE for token streaming.
- Use Postgres plus pgvector instead of a managed vector database.

### Decision Status

Common ADR statuses:

- **Proposed:** Under discussion.
- **Accepted:** Chosen and active.
- **Rejected:** Considered and not chosen.
- **Deprecated:** Still present but no longer recommended.
- **Superseded:** Replaced by a newer ADR.

### Consequence

Something that becomes easier, harder, safer, riskier, cheaper, slower, or more expensive because of the decision.

Good consequences are specific:

- "Reduces duplicate ticket processing because retries use idempotency keys."
- "Adds worker and queue operational complexity."
- "Makes direct customer reply slower because human approval is required."

Weak consequences are vague:

- "This is better."
- "This is scalable."
- "This improves quality."

---

## ADR Template

Use this template:

```markdown
# ADR-001: [Short Decision Title]

## Status

Proposed | Accepted | Rejected | Deprecated | Superseded by ADR-XXX

## Context

What problem, constraints, risks, and forces made this decision necessary?

## Decision

We will...

## Alternatives Considered

- Option A:
- Option B:
- Option C:

## Consequences

Positive:

- ...

Negative:

- ...

Risks and follow-up work:

- ...
```

Keep most ADRs to one or two pages. If it grows larger, link to supporting research instead of turning the ADR into a report.

---

## Walkthrough

Use this example:

> A support response drafting system reads support tickets, drafts replies, checks policy, and posts drafts to Slack for human approval.

### ADR 001 - Require Human Approval Before Customer-Facing Response

```markdown
# ADR-001: Require Human Approval Before Customer-Facing Response

## Status

Accepted

## Context

The system drafts responses to customer support tickets. Drafts may include policy statements, refund language, account instructions, or sensitive customer context. The model can produce plausible but incorrect or unauthorized responses.

The client wants speed improvements, but does not want the AI to send messages directly to customers without review.

## Decision

We will post AI-generated support drafts to Slack for human review. The system will not send customer-facing replies directly.

## Alternatives Considered

- Fully autonomous replies: rejected because policy and customer-impact risk is too high.
- Human review only for low-confidence drafts: rejected for the first release because confidence scoring is not yet validated.
- Human review for all drafts: accepted for the first release.

## Consequences

Positive:

- Prevents the model from directly sending incorrect or unauthorized customer messages.
- Creates a clear accountability point.
- Gives reviewers a chance to catch policy and tone issues.

Negative:

- Slower than fully automated replies.
- Reviewers may rubber-stamp drafts if the interface is too noisy.
- The system needs Slack approval tracking and escalation paths.

Risks and follow-up work:

- Design approval UI so the reviewer sees the ticket summary, draft, uncertainty, and policy flags.
- Add evals to determine whether some low-risk ticket categories can be semi-automated later.
```

### ADR 002 - Use Background Jobs for Draft Generation

```markdown
# ADR-002: Use Background Jobs for Draft Generation

## Status

Accepted

## Context

Draft generation may require ticket parsing, retrieval, model calls, policy checks, and Slack posting. These steps can take longer than a normal webhook request should wait.

Ticketing systems may retry webhooks if responses are slow or fail.

## Decision

We will accept incoming ticket webhooks quickly, enqueue a background job, and return a 202 Accepted response.

## Alternatives Considered

- Synchronous processing in the webhook request: rejected because model latency could trigger retries and duplicate work.
- Scheduled batch processing: rejected because support teams need timely drafts.
- Queue plus worker: accepted because it separates intake from slow AI work.

## Consequences

Positive:

- Webhook handling stays fast.
- Retries can be controlled with idempotency keys.
- Workers can be scaled independently.

Negative:

- Adds queue and worker operations.
- Requires job status tracking.
- Requires clear handling for failed or stuck jobs.

Risks and follow-up work:

- Define timeout and retry policy.
- Add alerts for failed jobs and queue depth.
```

### ADR 003 - Store Ticket Metadata, Not Raw Ticket Text

```markdown
# ADR-003: Store Ticket Metadata, Not Raw Ticket Text

## Status

Accepted

## Context

Support tickets may contain customer personal data, account details, or confidential business information. The system needs enough data to track processing status and debug failures, but does not need to retain full raw ticket text after draft generation.

## Decision

We will store ticket IDs, status, timestamps, trace IDs, model metadata, token counts, and failure reasons. We will not store full raw ticket text unless a future requirement explicitly justifies it.

## Alternatives Considered

- Store full ticket text for debugging: rejected because it increases privacy and breach impact.
- Store no metadata: rejected because operators need traceability.
- Store metadata only: accepted as the minimum useful operational record.

## Consequences

Positive:

- Reduces sensitive data retention.
- Preserves operational traceability.
- Simplifies deletion and incident review.

Negative:

- Some debugging may require going back to the source ticketing system.
- Reproducing exact prompts may be harder unless prompt snapshots are redacted and retained.

Risks and follow-up work:

- Define retention period for metadata.
- Confirm client requirements for audit logs and deletion.
```

---

## Practice

Choose one AI system idea.

Write three ADRs:

1. **Model or orchestration decision**  
   Examples: RAG vs. fine-tuning, single-agent vs. multi-agent, synchronous vs. background job, LangGraph vs. simple workflow.

2. **Data or storage decision**  
   Examples: Postgres vs. vector database, store raw documents vs. metadata only, re-indexing strategy, cache policy.

3. **Security, privacy, or handoff decision**  
   Examples: human approval before external writes, API keys vs. OAuth, least-privilege tool access, no sensitive logs.

For each ADR, include:

- Status
- Context
- Decision
- Alternatives considered
- Positive consequences
- Negative consequences
- Risks and follow-up work

---

## Review Checklist

Your ADRs are acceptable when:

- Each ADR records exactly one decision.
- The decision changes system behavior, risk, cost, latency, security, privacy, or maintainability.
- The context explains why the decision is needed now.
- At least two alternatives are considered.
- Consequences include both positives and negatives.
- The decision is written in active voice: "We will..."
- The ADR avoids implementation detail that belongs in code.
- The ADR can be understood six months later without a meeting transcript.

---

## Common Failure Modes

- **Decision theater:** The ADR justifies a decision that was already made without showing real alternatives.
- **Too broad:** One ADR tries to cover the entire architecture.
- **Too vague:** "Use best model" or "make it secure" is not a decision.
- **No downside:** The ADR hides trade-offs and risks.
- **Implementation dump:** The ADR turns into a setup guide or code walkthrough.
- **No lifecycle:** Deprecated or superseded decisions are deleted instead of recorded.
- **No owner:** The ADR does not say who is affected by the decision or who must review it.

---

## Portfolio Evidence

Save:

- Three ADRs
- One paragraph explaining the hardest trade-off
- A sanitized ADR suitable for a public case study

ADRs are strong portfolio evidence because they show architectural judgment, not just implementation ability.

---

## References

- Michael Nygard ADR format via arc42: https://docs.arc42.org/tips/9-5/
- Microsoft Engineering Playbook ADR example: https://microsoft.github.io/code-with-engineering-playbook/design/design-reviews/decision-log/doc/adr/0001-record-architecture-decisions/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
