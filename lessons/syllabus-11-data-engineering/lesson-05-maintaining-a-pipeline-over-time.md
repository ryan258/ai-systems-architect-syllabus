# Lesson 11.05: Maintaining a Pipeline Over Time

**Parent syllabus:** [Syllabus 11: Data Engineering and Pipelines](../../syllabus-11-data-engineering.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Reindexing policy and pipeline health runbook

---

## Outcome

By the end of this lesson, you can define how a data pipeline stays healthy after launch, including update detection, deletion handling, reindex triggers, and the conditions that require re-training or no action at all.

You will produce:

- a reindexing policy
- a pipeline health runbook
- update and deletion rules

---

## Why This Matters

Launching a pipeline is the beginning of the maintenance problem, not the end.

Common failures:

- New or updated documents never reach the retrieval store.
- Deleted or revoked files remain searchable for weeks.
- Reindexing happens randomly because no trigger rules exist.
- The team fine-tunes a model to fix what was really a stale retrieval corpus.
- Pipeline health is guessed from whether users are complaining.
- The maintenance path is too manual to survive a bad week or low-energy day.

RAG systems age through document drift. Fine-tuned systems age differently. If you do not define the maintenance rules, the system will quietly serve old truth.

---

## Best-Practice Principles

1. **Track document lifecycle events explicitly.**  
   Creation, update, approval change, and deletion should all have pipeline consequences.

2. **Separate corpus maintenance from model maintenance.**  
   Retrieval stores often need reindexing. Fine-tuned models need retraining only for different reasons.

3. **Define reindex triggers before they are urgent.**  
   Content change, schema change, embedding change, and metadata-policy change are all different triggers.

4. **Measure pipeline health as a real operating surface.**  
   Success rate, lag, queue depth, quarantine rate, and deletion lag should all be visible.

5. **Treat deletion and revocation as first-class behavior.**  
   Stale or disallowed content should not remain retrievable because deletion paths were ignored.

6. **Keep the maintenance loop small and repeatable.**  
   Low-friction daily or weekly checks outperform heroic cleanup bursts.

7. **Use health signals to drive decisions.**  
   The runbook should say what to do when lag spikes, quarantine rate rises, or retrieval drift appears.

---

## Concepts

### Reindex

Regenerating chunks, embeddings, or retrieval records so the store reflects current source material.

### Deletion Lag

The time between a source document being removed or revoked and that change being reflected in retrieval.

### Pipeline Lag

The delay between source-system change and searchable availability.

### Drift

A meaningful change in pipeline behavior over time.

Examples:

- more quarantined files
- rising ingestion lag
- lower retrieval relevance
- more stale documents appearing in results

### Maintenance Trigger

A condition that requires action.

Examples:

- embedding model change
- chunking strategy change
- metadata schema change
- document approval-state change

---

## Reindexing Policy Template

Use this structure:

```markdown
# Reindexing Policy: [Pipeline Name]

## Normal Update Triggers

- New document:
- Updated document:
- Deleted document:
- Approval-state change:

## Full Reindex Triggers

- Embedding model change:
- Chunking strategy change:
- Retrieval schema change:

## Deletion Rules

- How fast should revoked content disappear?
- What proof of deletion is recorded?

## Pipeline Health Metrics

- ingestion success rate
- quarantine rate
- searchable lag
- deletion lag
- retrieval drift signal

## Operator Actions

- If lag rises:
- If quarantine rate spikes:
- If stale content appears:
```

Pipeline health runbook structure:

```markdown
# Pipeline Health Runbook

## Daily Checks

- ...

## Weekly Checks

- ...

## Alert Conditions

- ...

## First Response Actions

- ...
```

---

## Walkthrough

Use this example:

> A support knowledge pipeline indexes approved docs daily and should remove revoked content quickly when policy changes.

### Step 1 - Define the normal update path

For each source change, decide:

- do we ingest only the changed document
- do we delete old chunks first
- do we keep version history
- do we need a human approval step before indexing

This is ordinary pipeline maintenance, not an exceptional event.

### Step 2 - Define what deletion really means

If a document is revoked or deleted at the source:

- the retrieval index should remove or suppress it
- caches or derived artifacts may also need invalidation
- the runbook should record expected deletion lag

If deletion is not explicit, revoked content will stay searchable longer than anyone intended.

### Step 3 - Separate reindex from retraining

Good rule of thumb:

- new or changed factual documents usually call for reindexing
- style, classification, or repeated instruction-following problems may justify fine-tuning later

Inference grounded in the retrieval and fine-tuning docs:

Changing knowledge belongs primarily in retrieval. Re-training a model for every factual update is usually the wrong maintenance path.

### Step 4 - Define pipeline health metrics

Useful health metrics:

- ingest success rate
- quarantine rate
- median and p95 source-to-searchable lag
- deletion lag
- retrieval regression or stale-content rate from sampled checks

This is how you know whether the pipeline is still working instead of assuming it is.

### Step 5 - Write the low-friction maintenance loop

Daily:

- review failed or quarantined items
- check ingest lag
- confirm no dangerous deletion backlog

Weekly:

- sample retrieval results
- review top failure classes
- check source-of-truth drift
- review cost and index growth

This is maintainability work, not paperwork.

### Step 6 - Add action rules

Examples:

- if quarantine rate doubles, inspect new source formats or parser changes
- if deletion lag exceeds target, pause rollout to sensitive use cases
- if retrieval samples surface stale content, investigate update propagation before changing the model

Metrics without actions are only passive dashboards.

---

## Practice

Choose one retrieval or ingestion pipeline design from this module.

Create:

1. a reindexing policy
2. update and deletion handling rules
3. a pipeline health runbook
4. at least five health metrics
5. at least three maintenance triggers and operator actions

Minimum requirements:

- one deletion-lag target
- one full-reindex trigger
- one explicit rule about when not to retrain a model

---

## Review Checklist

Your maintenance artifact is acceptable when:

- Update, deletion, and approval-state changes have defined outcomes.
- Reindex triggers are explicit.
- Reindex and retraining are not confused.
- Pipeline health metrics are concrete.
- Daily and weekly maintenance loops are small and repeatable.
- Operator actions are tied to alerts or thresholds.
- Deletion and revocation are treated as first-class concerns.

---

## Common Failure Modes

- **Launch-only mindset:** No maintenance behavior is defined after initial ingest.
- **No deletion discipline:** Revoked content remains searchable.
- **Retrain by reflex:** The team changes the model to fix corpus drift.
- **Health by vibes:** No lag, quarantine, or drift metrics exist.
- **Manual overload:** The maintenance path is too heavy for real life.
- **No action rules:** Metrics are collected but do not change behavior.

---

## Portfolio Evidence

Save:

- the reindexing policy
- the pipeline health runbook
- one short note explaining when you would reindex, retrain, or do nothing

This shows that you can operate a retrieval pipeline as a living system instead of a one-time build.

---

## References

- OpenTelemetry Concepts: https://opentelemetry.io/docs/concepts/
- OpenAI Retrieval guide: https://platform.openai.com/docs/guides/retrieval
- OpenAI Fine-Tuning guide: https://platform.openai.com/docs/guides/fine-tuning
- Pinecone documentation: https://docs.pinecone.io/
- Milvus documentation: https://milvus.io/docs
- pgvector README: https://github.com/pgvector/pgvector
