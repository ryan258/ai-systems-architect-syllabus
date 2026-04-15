# Lesson 02.06: Reducing Context Switching Friction

**Parent syllabus:** [Syllabus 02: Constraint-Driven Design](../../syllabus-02-constraint-driven-design.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Context-switching friction audit and integration plan

---

## Outcome

By the end of this lesson, you can redesign an AI workflow so it lives closer to where the user already works.

You will produce a friction audit and an integration plan for Slack, Google Docs, Notion, an IDE, email, or another existing workspace.

---

## Why This Matters

Every extra tool, tab, login, or copy-paste step reduces adoption.

Common failures:

- The AI system requires a new portal nobody opens.
- Users must copy data between tools manually.
- Review happens outside the place where work is decided.
- The workflow breaks because context is scattered.
- The system asks users to change habits before it proves value.
- AI output is generated but never acted on.

Reducing context switching is not just convenience. It is adoption architecture.

---

## Best-Practice Principles

1. **Meet users where work already happens.**  
   Start with the client's existing tools before proposing a new interface.

2. **Move the workflow, not the user.**  
   Bring prompts, approvals, drafts, and summaries into the user's current work surface.

3. **Separate capture, review, and action.**  
   Each may belong in a different tool.

4. **Reduce copy-paste.**  
   Manual transfer creates errors, fatigue, and abandonment.

5. **Keep approval close to decision context.**  
   Review should happen where the user can see enough information to decide safely.

6. **Design notification boundaries.**  
   Do not turn friction reduction into notification noise.

7. **Preserve auditability.**  
   Embedded workflows still need logs, trace IDs, status, and ownership.

---

## Concepts

### Context Switching

Moving attention between tools, tasks, mental models, or workspaces.

### Work Surface

The place where the user already does the work.

Examples:

- Slack channel
- Google Doc
- Notion database
- GitHub pull request
- CRM record
- IDE
- Email inbox

### Embedded Workflow

An AI workflow that appears inside an existing work surface instead of requiring a separate app.

### Integration Pattern

A reusable way to connect workflow steps to tools.

Examples:

- Slash command
- Webhook
- Comment bot
- Sidebar
- Scheduled summary
- Draft creation
- Approval message
- IDE command

---

## Walkthrough

Use this client request:

> A client wants AI to summarize weekly project updates and ask managers to approve blockers.

### Bad design

Build a new dashboard where managers log in, select a project, run the AI summary, and approve blockers.

Why it fails:

- New login
- New habit
- Managers already work in Slack and Google Docs
- Approval is separated from team discussion
- More training required

### Better design

Use existing work surfaces:

| Workflow step | Best work surface | Pattern |
| --- | --- | --- |
| Source updates | Google Docs | Read selected weekly update doc |
| Generate summary | Backend workflow | Scheduled or manual trigger |
| Review blockers | Slack | Approval message in manager channel |
| Store final summary | Google Docs or Notion | Append final approved summary |
| Audit trail | System logs | Trace ID and approval record |

### Friction audit

| Current friction | Impact | Redesign |
| --- | --- | --- |
| Manager must open dashboard | Low adoption | Send Slack approval |
| Copy-paste from Google Docs | Errors and fatigue | Read doc directly |
| No approval record | Accountability gap | Store approver and timestamp |
| Summary appears away from source | Hard to verify | Link to source doc |
| Too many notifications | Alert fatigue | One weekly digest plus urgent blocker alerts |

### Integration plan

```text
Trigger:
- Weekly schedule or Slack command

Read:
- Selected Google Doc update

Process:
- Summarize progress, blockers, decisions, and risks

Review:
- Post draft summary to private Slack manager channel

Approve:
- Manager clicks Approve, Revise, or Escalate

Write:
- Append approved summary to source doc or Notion project page

Log:
- Trace ID, source doc ID, approver, timestamp, status, failure reason
```

---

## Practice

Pick one AI workflow that currently requires tool switching.

Create a context-switching friction audit:

1. **Workflow name**
2. **Current tools**
3. **Where the user starts**
4. **Where the user must switch tools**
5. **Where copy-paste happens**
6. **Where review happens**
7. **Where final action happens**
8. **Where audit trail lives**
9. **Friction points**
10. **Integration pattern for each step**
11. **What should stay manual**
12. **What should not be integrated yet**

Then write a one-page integration plan.

---

## Review Checklist

Your audit and plan are acceptable when:

- The user's current work surface is named.
- Every tool switch is listed.
- Copy-paste steps are listed.
- The plan reduces at least two tool switches.
- Approval happens near decision context.
- The plan avoids notification noise.
- Manual steps are intentionally preserved where needed.
- Logging and audit trail are included.
- The integration is simpler than a new portal unless a portal is clearly justified.

---

## Common Failure Modes

- **Portal reflex:** Building a new app when an embedded workflow would work.
- **Notification spam:** Moving everything into Slack without prioritization.
- **No audit trail:** Embedded work becomes invisible to operators.
- **Unsafe write-back:** The workflow updates systems without approval.
- **Copy-paste dependency:** The user still has to move data manually.
- **Over-integration:** The design connects too many tools too early.
- **No ownership:** Nobody owns failures across tool boundaries.

---

## Portfolio Evidence

Save:

- Friction audit
- Integration plan
- Before-and-after workflow map
- Approval and logging rules

This becomes direct evidence that you can design for adoption, not just functionality.

---

## References

- Model Context Protocol documentation: https://modelcontextprotocol.io/
- OpenTelemetry concepts: https://opentelemetry.io/docs/concepts/
- W3C Accessibility Fundamentals: https://www.w3.org/WAI/fundamentals/
- PlainLanguage.gov guidelines: https://www.plainlanguage.gov/guidelines/
