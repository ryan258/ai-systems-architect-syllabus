# Lesson 01.04: Connecting Tools Together

**Parent syllabus:** [Syllabus 01: AI Workflow Design](../../syllabus-01-ai-workflow-design.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Tool integration map

---

## Outcome

By the end of this lesson, you can map how an AI workflow connects to external tools, what data moves between them, what permissions are required, and what can fail.

You will produce a tool integration map for one workflow.

---

## Why This Matters

Tool-connected AI is where workflows become useful and risky.

Common failures:

- The AI gets access to more data than it needs.
- Tool results are treated as always correct.
- The workflow writes to external systems without review.
- Nobody knows what happens if a tool is unavailable.
- Sensitive data is copied between tools without retention rules.
- Tool calls are not logged, so failures cannot be reconstructed.

Connecting tools is not just a convenience feature. It is a permissions, reliability, and privacy design problem.

---

## Best-Practice Principles

1. **Map data before wiring tools.**  
   Know what enters, leaves, gets stored, and gets logged.

2. **Use least privilege.**  
   Give the workflow only the access needed for the task.

3. **Separate read from write.**  
   Reading a document is lower risk than editing, deleting, emailing, posting, or updating records.

4. **Require approval before high-impact writes.**  
   Human review belongs before external actions that are public, irreversible, expensive, or client-facing.

5. **Validate tool outputs.**  
   APIs fail, return partial data, change shape, or contain untrusted content.

6. **Plan for tool failure.**  
   Every tool connection needs timeout, retry, fallback, and "what the user sees" behavior.

7. **Log tool calls safely.**  
   Record enough to debug without storing secrets or unnecessary sensitive content.

---

## Concepts

### Tool

An external capability the AI workflow can use.

Examples:

- Notion
- Google Docs
- Slack
- Gmail
- Airtable
- CRM
- Search API
- File system
- Database
- Calendar

### MCP

Model Context Protocol is a standard for connecting AI applications to tools, resources, and prompts through a defined interface.

For this lesson, the important architectural idea is not the implementation detail. It is that tool access should be explicit, scoped, inspectable, and controlled.

### Resource

Data the tool exposes.

Examples:

- A document
- A folder
- A database row
- A Slack channel
- A calendar event

### Action

Something the workflow can do through a tool.

Examples:

- Read a document
- Search a folder
- Create a draft
- Post a message
- Update a CRM field
- Delete a record

### Permission Boundary

The line between allowed and forbidden access.

Example:

> The workflow can read project notes but cannot send email or edit client records without approval.

---

## Walkthrough

Use this example:

> A workflow reads a Google Doc containing meeting notes, summarizes decisions, and posts a draft follow-up message to Slack.

### Step 1 - Name tools and actions

| Tool | Action | Risk |
| --- | --- | --- |
| Google Docs | Read meeting notes | Sensitive client data exposure |
| AI model | Summarize and draft | Hallucination or policy drift |
| Slack | Post draft for review | Wrong audience or sensitive content leak |

### Step 2 - Separate reads from writes

Read actions:

- Read one selected Google Doc.

Write actions:

- Post a draft message to a private Slack review channel.

Not allowed:

- Edit the source Google Doc.
- Send email to the client.
- Post directly to a public Slack channel.
- Read the whole Drive folder.

### Step 3 - Define data movement

```text
Google Doc text
  -> AI workflow
  -> Summary and draft
  -> Slack review channel
  -> Human approval or revision
```

Sensitive data:

- Client names
- Project details
- Decisions
- Possible financial or contract details

Logging rule:

> Log document ID, trace ID, timestamp, workflow status, and error reason. Do not log full document text by default.

### Step 4 - Define failure behavior

| Failure | Behavior |
| --- | --- |
| Google Doc unavailable | Stop and show "source unavailable" |
| AI output missing required sections | Reject output and retry once |
| Slack post fails | Save draft locally and notify operator |
| Sensitive data detected in public-channel target | Stop and require human approval |

### Step 5 - Define approval rule

> The AI may draft a Slack message, but a human must approve before anything is posted outside the private review channel.

---

## Practice

Pick one workflow that uses or could use external tools.

Create a tool integration map:

1. **Workflow name**
2. **Tools involved**
3. **Read actions**
4. **Write actions**
5. **Forbidden actions**
6. **Data moved between tools**
7. **Sensitive data involved**
8. **Permissions required**
9. **Human approval points**
10. **Failure behavior**
11. **What gets logged**
12. **What must not be logged**

---

## Review Checklist

Your tool integration map is acceptable when:

- Every tool has a purpose.
- Read and write actions are separated.
- Forbidden actions are explicit.
- Sensitive data movement is described.
- Permissions follow least privilege.
- Human approval appears before high-impact writes.
- At least three tool failure modes are listed.
- Logging is useful but does not store secrets or unnecessary sensitive data.
- Another person could tell what the workflow is allowed to access.

---

## Common Failure Modes

- **Tool sprawl:** The workflow connects to tools because it can, not because it needs to.
- **Overbroad permissions:** The workflow can read or write far beyond the task.
- **Unreviewed writes:** The AI can post, email, edit, or delete without approval.
- **No failure behavior:** Tool outages become confusing user-facing failures.
- **Unsafe logs:** Raw documents, tokens, secrets, or sensitive messages are stored unnecessarily.
- **Trusting tool content:** Retrieved content can contain mistakes, stale data, or prompt injection.

---

## Portfolio Evidence

Save:

- The tool integration map
- A short permission summary
- A sanitized data-flow description
- One approval rule

This becomes the first version of a future enterprise integration artifact.

---

## References

- Model Context Protocol documentation: https://modelcontextprotocol.io/
- Anthropic tool use docs: https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/overview
- OWASP Top 10 for LLM Applications: https://owasp.org/www-project-top-10-for-large-language-model-applications/
- OpenTelemetry concepts: https://opentelemetry.io/docs/concepts/
