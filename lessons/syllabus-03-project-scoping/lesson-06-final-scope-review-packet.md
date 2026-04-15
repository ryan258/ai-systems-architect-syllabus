# Lesson 03.06: Final Scope Review Packet

**Parent syllabus:** [Syllabus 03: Project Scoping](../../syllabus-03-project-scoping.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Client-ready scope review packet

---

## Outcome

By the end of this lesson, you can review a proposed AI workflow project before acceptance and decide whether to proceed, clarify, re-scope, or decline.

You will produce a client-ready scope review packet that combines discovery notes, scope, exclusions, change rules, and handoff assumptions.

---

## Why This Matters

The dangerous moment in a project is right before you say yes.

At that point, optimism is high. The client is interested. You want momentum. But if the scope is incomplete, the project will become harder to control after work starts.

Common failures:

- You accept a project with unresolved access, data, or approval questions.
- The scope document exists but does not match discovery notes.
- The client has not approved exclusions.
- Production expectations are still ambiguous.
- There is no decision rule for new requests.
- Maintenance and support are implied but not defined.

The final review packet is your pre-flight check before commitment.

---

## Best-Practice Principles

1. **Review before agreement, not after kickoff.**  
   The cheapest time to fix scope is before the project starts.

2. **Compare discovery notes to the scope.**  
   Every important client need should be either included, excluded, deferred, or marked as an open question.

3. **Resolve blockers explicitly.**  
   Do not start work if access, data, approval, or compliance questions are unresolved.

4. **Require client approval of boundaries.**  
   Exclusions should be visible before work begins.

5. **Create a change path.**  
   New requests are easier to handle when the process is agreed in advance.

6. **Check AI risk boundaries.**  
   A scoped AI project should state data limits, human review needs, automation limits, and production status.

7. **End with a decision.**  
   Proceed, clarify, re-scope, paid discovery, or decline.

---

## Concepts

### Scope Review Packet

A compact set of project documents used to decide whether the project is ready to begin.

It includes:

- Discovery summary
- Scope document
- Exclusion list
- Assumptions and dependencies
- Risk and data notes
- Revision and change process
- Open questions
- Proceed or pause decision

### Proceed Decision

A clear decision that the project is ready to start under the written scope.

### Pause Decision

A decision that the project should not start until a blocker is resolved.

### Decline Decision

A decision not to take the project because risk, ambiguity, budget, fit, or expectations are not acceptable.

---

## Scope Review Packet Template

```markdown
# Scope Review Packet: [Project Name]

## 1. Discovery Summary

- Client request:
- Business problem:
- Current workflow:
- Users:
- Operator:
- Approver:

## 2. Recommended Phase One

What should be built or delivered first?

## 3. Deliverables

- ...

## 4. Not Included

- ...

## 5. Client Responsibilities

- Access:
- Data:
- Feedback:
- Decisions:
- Approval:

## 6. AI, Data, and Risk Boundaries

- Data allowed:
- Data excluded:
- Human review required:
- Automation limits:
- Production status:
- Security or compliance concerns:

## 7. Timeline and Communication Cadence

- Start:
- Draft:
- Review window:
- Final:
- Update cadence:
- Primary communication channel:

## 8. Revision and Change Rules

- Included revision cycles:
- What counts as a revision:
- What counts as a scope change:
- Change request process:

## 9. Open Questions

- ...

## 10. Decision

Proceed, clarify, re-scope, paid discovery, or decline.

## 11. Reason

Why is this the right decision?
```

---

## Walkthrough

Scenario:

> A client wants an AI assistant that drafts internal policy answers from company documents.

Discovery revealed:

- The company has 300 policy documents.
- Some documents are outdated.
- Employees may ask HR-related questions.
- The client wants the assistant in Slack eventually.
- Phase one budget is limited.
- No security reviewer has joined the conversation yet.

### Bad proceed decision

> Start building a Slack bot that answers employee questions from all documents.

Why this is weak:

- Document quality is unknown.
- HR content may create compliance risk.
- Slack integration adds access and security work.
- The approval owner is missing.
- Budget does not match production expectations.

### Better scope review decision

> Do not start production build. Recommend paid discovery or a limited prototype using 20 approved, non-sensitive documents. Phase one should produce a data inventory, answer-quality eval set, retrieval prototype, and risk review. Slack integration and employee rollout are excluded until documents, security review, and approval process are resolved.

### Packet decision

Decision:

> Re-scope to a limited prototype.

Reason:

> The original request has unresolved data quality, HR risk, production access, and approval questions. A limited prototype can test usefulness without creating an unsupported internal advice system.

---

## Practice

Create a final scope review packet for one project:

1. AI support reply drafter
2. AI proposal assistant
3. AI meeting notes to project plan workflow
4. AI internal policy Q&A prototype
5. Your own real client idea

Use your discovery notes and one-page scope from earlier lessons.

End with one of these decisions:

- Proceed
- Clarify before starting
- Re-scope to smaller phase one
- Offer paid discovery first
- Decline

Write the reason in one paragraph.

---

## Review Checklist

Your scope review packet is acceptable when:

- It summarizes the discovery findings.
- It matches the proposed scope.
- It names included deliverables.
- It names exclusions.
- It names client responsibilities.
- It includes AI data, review, automation, and production boundaries.
- It defines revision and change rules.
- It lists open questions.
- It ends with a clear decision and reason.
- It would prevent you from starting a bad-fit project too early.

---

## Common Failure Modes

- **Scope and discovery mismatch:** You ignore something important the client said.
- **No open questions:** You pretend uncertainty is resolved.
- **Proceeding with blockers:** Missing data access or approval is treated as a later detail.
- **No AI risk boundary:** The scope does not say what the AI can and cannot do.
- **No decline option:** Every lead becomes a project, even when fit is poor.
- **No written decision:** You start work without a clear reason for the chosen path.

---

## Portfolio Evidence

Save:

- Final scope review packet
- Proceed/pause decision
- Open questions list
- AI risk boundary summary
- Client responsibilities section

This becomes evidence that you can evaluate project fit before accepting implementation work.

---

## References

- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
- PlainLanguage.gov guidelines: https://www.plainlanguage.gov/guidelines/
