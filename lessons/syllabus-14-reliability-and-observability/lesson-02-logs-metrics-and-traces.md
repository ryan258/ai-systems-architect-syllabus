# Lesson 14.02: Logs, Metrics, and Traces

**Parent syllabus:** [Syllabus 14: Reliability and Observability](../../syllabus-14-reliability-and-observability.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Observability instrumentation specification and trace field catalog

---

## Outcome

By the end of this lesson, you can define the core telemetry model for an AI system so requests, model calls, retrieval steps, tool use, and human handoffs can be reconstructed consistently without exposing sensitive data.

You will produce:

- an observability instrumentation specification
- a trace field catalog
- log, metric, and retention rules

---

## Why This Matters

Reliability work breaks down when telemetry exists but cannot be joined into one story.

Common failures:

- Logs, metrics, and traces are collected by different teams with different IDs and naming rules.
- Model calls are visible, but retrieval and tool steps are not.
- Human handoff events are left out of the trace entirely.
- Metrics explode in cardinality because user IDs or raw prompts were turned into labels.
- Operators can see a failure spike but cannot identify which version, route, or dependency changed.
- Sensitive client content is logged because the schema was never designed.
- Retention is accidental, so the system either stores too much or loses the evidence too early.

Observability is not "turn on logging." It is a data model for operational truth.

---

## Best-Practice Principles

1. **Design telemetry as one system.**  
   Logs, metrics, and traces should share identifiers and naming conventions.

2. **Trace the full workflow path.**  
   Request intake, retrieval, model steps, tool calls, queue events, and human handoffs all belong in the reliability story.

3. **Use structured logs and controlled fields.**  
   Free-form logs are hard to query, compare, and redact safely.

4. **Protect metrics from bad label design.**  
   High-cardinality labels can make reliability tooling noisy, slow, and expensive.

5. **Redact by design, not by apology.**  
   Decide what never enters logs before instrumentation code is written.

6. **Keep telemetry semantics stable across revisions.**  
   If field names and meanings drift every release, trend analysis becomes weak.

7. **Make the schema understandable to operators.**  
   A future maintainer should know what each event and field means without reverse-engineering the code.

---

## Concepts

### Structured Log

A machine-readable event record with stable fields.

### Metric

An aggregated numerical signal used to track system behavior over time.

### Trace

An end-to-end record connecting one request or job to the steps it triggered.

### Span

A timed segment inside a trace representing one operation.

Examples:

- request validation
- retrieval call
- model generation
- policy check
- queue write

### Correlation ID

A stable identifier used to connect logs, metrics context, and traces for the same request or job.

### Metric Cardinality

The number of distinct label combinations a metric produces.

Bad cardinality examples:

- raw user ID as a label
- full document ID as a label
- unique prompt text as a label

### Retention Rule

The policy defining how long telemetry is stored and what is redacted or deleted.

---

## Observability Instrumentation Specification Template

Use this structure:

```markdown
# Observability Instrumentation Specification: [Workflow Name]

## Request and Job Boundaries

- Request ID:
- Trace ID:
- Job ID, if any:

## Required Spans

- Request intake:
- Retrieval:
- Model call:
- Tool call:
- Human handoff:

## Structured Logs

| Event | Required fields | Redacted fields | Notes |
| --- | --- | --- | --- |
|  |  |  |  |

## Metrics

| Metric | Type | Labels allowed | Labels forbidden |
| --- | --- | --- | --- |
|  |  |  |  |

## Trace Field Catalog

| Field | Meaning | Source | Privacy rule |
| --- | --- | --- | --- |
|  |  |  |  |

## Retention and Sampling

- Trace retention:
- Log retention:
- Metric resolution:
- Sampling notes:
```

Review questions:

- Can one request be reconstructed across all major steps?
- Which fields are safe as metric labels?
- What data should never appear in telemetry?

---

## Walkthrough

Use this example:

> A support drafting service accepts requests from a CRM, retrieves policy content, drafts a reply, runs a policy check, and writes a review item to a queue.

### Step 1 - Define the shared identifiers

At minimum, this service needs:

- request ID
- trace ID
- job or queue item ID

If logs use one ID format and traces use another with no mapping, reconstruction becomes guesswork.

### Step 2 - Map the required spans

Useful spans for this workflow:

- HTTP request validation
- CRM payload normalization
- policy retrieval
- draft generation
- policy check
- review-queue write
- reviewer decision intake, if available

Tracing only the main model call is not enough for real reliability work.

### Step 3 - Design structured logs

Good log fields:

- trace ID
- request ID
- route or workflow name
- model or retrieval route
- version identifiers
- latency
- outcome code
- error class

Bad log shortcut:

> Store the raw ticket, prompt, and generated draft to make debugging easier.

That creates privacy, cost, and governance problems immediately.

### Step 4 - Keep metrics label-safe

Useful metric labels might include:

- route name
- model family
- environment
- status class

Dangerous labels:

- raw user ID
- ticket ID
- document ID
- full exception message

Metric cardinality is an observability reliability issue of its own.

### Step 5 - Write retention rules

For this service:

- metrics may need longer retention for trend review
- traces may need shorter retention but enough time for incident analysis
- structured logs should retain event evidence without storing raw client content

Retention should match debugging needs and privacy boundaries, not default settings.

### Step 6 - Make the schema stable

If one release calls the field `review_result` and the next calls it `approval_status` with a different meaning, dashboards and alerts will drift silently.

Telemetry changes deserve version discipline just like API or schema changes do.

---

## Practice

Choose one AI workflow that has at least three runtime steps.

Create:

1. an observability instrumentation specification
2. a trace field catalog
3. structured log rules for at least five events
4. at least six metrics
5. retention and redaction rules

At least one event or span must cover:

- a model call
- a retrieval or tool step
- a human handoff or queue action

---

## Review Checklist

Your instrumentation artifact is acceptable when:

- Shared identifiers connect logs, metrics context, and traces.
- Major workflow steps are represented as spans or events.
- Structured logs use stable fields.
- Metric labels avoid unsafe cardinality.
- Redaction rules are explicit.
- Retention rules are written down.
- The schema is understandable to operators.
- A future release could preserve the same telemetry meaning.

---

## Common Failure Modes

- **Telemetry silos:** Logs, metrics, and traces cannot be connected.
- **Model-only tracing:** Retrieval, tools, or handoffs disappear from the story.
- **Cardinality explosion:** Metrics become noisy or expensive because labels are uncontrolled.
- **Privacy leak by schema:** Sensitive inputs end up in logs because the fields were never constrained.
- **Field drift:** Telemetry names and meanings change across releases.
- **Retention by accident:** Evidence disappears too soon or is kept too long.

---

## Portfolio Evidence

Save:

- the observability instrumentation specification
- the trace field catalog
- one short note explaining the metric-label rules

This shows that you can design telemetry as an operating system rather than an afterthought.

---

## References

- OpenTelemetry Concepts: https://opentelemetry.io/docs/concepts/
- OpenTelemetry Semantic Conventions: https://opentelemetry.io/docs/specs/semconv/
- Python Logging HOWTO: https://docs.python.org/3/howto/logging.html
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
