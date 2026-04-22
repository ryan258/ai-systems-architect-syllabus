# Lesson 16.04: State, Caching, and Consistency

**Parent syllabus:** [Syllabus 16: Distributed AI Systems](../../syllabus-16-distributed-ai-systems.md)  
**Estimated time:** 2-3 hours  
**Artifact:** State map and caching policy with consistency rules

---

## Outcome

By the end of this lesson, you can decide what data your distributed AI system stores, where it stores it, what can be cached safely, and how fresh each part of the system actually needs to be.

You will produce:

- a state map
- a caching policy
- a consistency and invalidation rule set

---

## Why This Matters

As soon as an AI workflow becomes distributed, state starts spreading into places that are easy to forget.

Common failures:

- Job state exists partly in the queue, partly in memory, and partly in a database with no clear source of truth.
- Cached retrieval results survive after source documents changed.
- Model outputs are cached without enough context to know whether reuse is safe.
- Sensitive content is copied into caches and long-lived stores without retention or access rules.
- Operators cannot tell whether a result is fresh, stale, or still in progress.
- The system becomes cheap and fast by accident, then wrong in ways nobody can explain.

State design is where correctness, privacy, performance, and maintainability start pulling against each other.

---

## Best-Practice Principles

1. **Name the source of truth for every important state.**  
   If multiple stores can each appear authoritative, operators will lose time and trust.

2. **Separate durable state from ephemeral acceleration.**  
   Queues and caches should not silently become the only place the truth exists.

3. **Cache only when the reuse value outweighs the freshness risk.**  
   Cheap wrong answers are still wrong answers.

4. **Give every cache an invalidation trigger or lifetime rule.**  
   A cache without expiry or invalidation becomes hidden data governance risk.

5. **Store references where possible, not duplicate sensitive content everywhere.**  
   Minimize privacy exposure and retention burden.

6. **Tie consistency choice to business impact.**  
   Some fields must be exact now. Others can be slightly stale if the tradeoff is explicit.

7. **Keep rebuild and recovery in scope.**  
   A state design is weak if nobody can explain how to restore it after failure.

---

## Concepts

### Source of Truth

The system or store that should be treated as authoritative for one type of state.

### Durable State

State that must survive process restarts, retries, and operational incidents.

Examples:

- job status
- human approval status
- final outputs

### Ephemeral State

State that exists to make the system faster or smoother but can be rebuilt.

Examples:

- queue messages
- short-lived cache entries
- temporary worker memory

### Cache Key

The set of inputs that determines whether a cached value is reusable.

### Invalidation Trigger

The event or rule that should make a cached value unsafe to reuse.

### Consistency Requirement

The acceptable freshness or exactness for one type of data.

Examples:

- immediately consistent
- bounded stale
- eventually consistent

---

## State Map and Caching Policy Template

Use this structure:

```markdown
# State Map and Caching Policy: [Workflow Name]

## 1. State Inventory

| State type | Source of truth | Store | Sensitivity | Retention | Consistency need |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

## 2. Cache Policy

| Cache target | Why cache it | Cache key | TTL or invalidation | Unsafe reuse condition |
| --- | --- | --- | --- | --- |
|  |  |  |  |  |

## 3. Queue and Workflow State Rules

- Job state store:
- Worker-local state rule:
- Human approval state:

## 4. Privacy, Security, Cost, and Observability Notes

- Sensitive data exclusion rule:
- Encryption or access rule:
- Cost reason for caching:
- Trace fields for freshness:

## 5. Recovery Notes

- How to rebuild caches:
- How to restore durable state:

## 6. Open Questions

- ...
```

Review questions:

- Which store is authoritative for job progress?
- What cache entries become dangerous when source data changes?
- Can an operator explain whether a result is fresh enough for its purpose?

---

## Walkthrough

Use this example:

> A support drafting workflow accepts jobs through an API, stores job progress, retrieves policy documents, drafts responses, and sends items into a human review queue.

### Step 1 - Map the durable state

For this service, durable state likely includes:

- job status and attempts
- ticket reference and tenant reference
- review queue item status
- final draft metadata
- policy version used for the draft

That data should live in a durable system of record such as a relational database or another explicitly managed state store.

### Step 2 - Identify ephemeral state

Ephemeral state may include:

- queue messages
- worker-local processing state
- temporary retrieval results
- short-lived cache entries

This state may matter for performance, but it should not be the only place critical truth exists.

### Step 3 - Choose what to cache carefully

Possible cache targets:

- retrieval results for a stable policy document set
- expensive tool lookup results
- model-independent enrichment data

Riskier cache targets:

- final user-facing drafts
- data that changes with active policy edits
- content containing high-sensitivity personal details

Caching should reflect business risk, not only technical opportunity.

### Step 4 - Write the invalidation rules

For the support drafting workflow, retrieval cache reuse may become unsafe when:

- the policy corpus changes
- tenant-specific rules change
- the prompt or ranking logic changes
- the ticket state changes materially

If invalidation rules are not explicit, stale context becomes silent model error.

### Step 5 - Make freshness visible

For each important result, the system should be able to show:

- source version
- cache hit or miss
- retrieval timestamp
- policy version used
- job creation time and completion time

That helps operators and reviewers understand whether the output is credible.

### Step 6 - Keep privacy in scope

For this workflow:

- raw ticket content may need stricter retention than job metadata
- caches should avoid storing full transcripts unless necessary
- logs should reference content by ID where possible
- sensitive content should not be copied into multiple stores casually

State sprawl often becomes privacy sprawl.

### Step 7 - Plan for rebuild

A practical policy should say:

- queues can be replayed or repopulated if needed
- caches can be flushed and rebuilt
- durable state must be backed up and recoverable
- operators can verify freshness after restore

Recovery is part of state design, not a separate afterthought.

---

## Practice

Choose one distributed AI workflow and create a **State Map and Caching Policy** that includes:

1. source of truth for each important state type
2. durable versus ephemeral state distinction
3. cache targets and cache keys
4. invalidation rules
5. privacy and retention notes
6. recovery and rebuild notes

Keep it operational. Someone reading it should know where the system remembers things and when cached answers stop being trustworthy.

---

## Review Checklist

- Every important state type has a source of truth.
- Durable and ephemeral state are distinguished.
- Cache targets are justified, not assumed.
- Cache keys and invalidation rules are explicit.
- Sensitive content handling is documented.
- Freshness signals are visible in operations or review.
- Recovery notes explain how to rebuild caches and restore durable state.
- Cost savings from caching do not override correctness and privacy without review.

---

## Common Failure Modes

- Letting the queue become the only place job truth exists.
- Caching retrieval or model outputs without versioning the source context.
- Forgetting to invalidate caches after prompt or policy changes.
- Replicating private content into every store for convenience.
- Treating stale data as acceptable without deciding that on purpose.
- Designing a state model nobody can restore after failure.

---

## Portfolio Evidence

Keep a sanitized copy of your state map and caching policy.

Redact:

- database names
- cache names
- internal retention thresholds if sensitive
- tenant identifiers
- document store paths

What remains demonstrates that you can make distributed AI systems understandable and recoverable instead of only fast.

---

## References

- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Redis Documentation](https://redis.io/docs/latest/)
- [OpenTelemetry Concepts](https://opentelemetry.io/docs/concepts/)
- [Google SRE Workbook: Configuration Design](https://sre.google/workbook/configuration-design/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
