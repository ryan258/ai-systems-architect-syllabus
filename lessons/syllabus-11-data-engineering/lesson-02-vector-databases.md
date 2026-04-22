# Lesson 11.02: Vector Databases

**Parent syllabus:** [Syllabus 11: Data Engineering and Pipelines](../../syllabus-11-data-engineering.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Vector storage decision memo and retrieval store scorecard

---

## Outcome

By the end of this lesson, you can decide whether a vector database is actually necessary for your workflow and compare Pinecone, Milvus, and pgvector in terms that matter operationally.

You will produce:

- a vector storage decision memo
- a retrieval store scorecard
- a fit or overkill recommendation

---

## Why This Matters

Vector databases are useful, but they are often added before the retrieval problem is understood.

Common failures:

- A vector database is chosen because "RAG needs one" without checking scale or operational burden.
- Retrieval works in a demo but becomes expensive or noisy because metadata filtering was never designed.
- The team stores vectors and metadata separately and loses traceability.
- Exact or keyword search would have been enough, but the system is now harder to maintain.
- A managed vector store is selected without considering tenant isolation, cost drift, or export paths.
- A self-managed store is selected without acknowledging operational overhead.

Semantic retrieval is valuable. That does not mean every workflow deserves a dedicated vector database on day one.

---

## Best-Practice Principles

1. **Choose storage based on retrieval requirements, not market category.**  
   Query pattern, scale, metadata needs, and operating model matter more than vendor familiarity.

2. **Treat metadata as part of retrieval quality.**  
   Good semantic search often depends on filters, namespaces, or tenant boundaries as much as embeddings.

3. **Start with the simplest store that fits.**  
   If Postgres plus `pgvector` is enough, introducing a separate system may add more cost than value.

4. **Do not separate vectors from the records needed to interpret them.**  
   Retrieval without provenance and metadata is weak.

5. **Plan for reindexing and migration before launch.**  
   Embedding model changes and schema changes are normal.

6. **Make the operating model explicit.**  
   Managed versus self-managed is a reliability and staffing decision, not just a feature decision.

7. **Check cost and observability early.**  
   Query volume, storage growth, and retrieval latency all need visibility before scale.

---

## Concepts

### Vector

A numerical representation of content used to compare semantic similarity.

### Semantic Search

Search based on meaning similarity rather than exact keyword overlap alone.

### Metadata Filter

A constraint applied alongside semantic search.

Examples:

- tenant ID
- document type
- visibility level
- effective date

### Namespace or Partition

A logical separation of data inside a retrieval store.

Useful for:

- multitenancy
- environment separation
- access boundaries

### Managed Store

A service where infrastructure, scaling, and some operations are handled for you.

### Self-Managed Store

A system you run and observe directly, usually with more control and more operational burden.

---

## Retrieval Store Scorecard Template

Use this structure:

```markdown
# Retrieval Store Scorecard: [Workflow Name]

## Requirements

- Query volume:
- Data size:
- Metadata filtering:
- Multitenancy:
- Operator skill:
- Budget sensitivity:

## Options

| Option | Best fit | Operational burden | Cost posture | Notes |
| --- | --- | --- | --- | --- |
| Pinecone |  |  |  |  |
| Milvus |  |  |  |  |
| pgvector |  |  |  |  |

## Decision

- Chosen option:
- Why:
- Why not the others:

## Revisit Triggers

- ...
```

Review questions:

- Do we need approximate nearest-neighbor behavior at this scale?
- Do we already operate Postgres successfully?
- Is multitenancy a hard requirement?
- How costly will reindexing be?

---

## Walkthrough

Use this example:

> A client wants semantic retrieval over thousands of support and policy documents with tenant isolation and frequent metadata filtering.

### Step 1 - Define the real retrieval requirements

Before comparing stores, answer:

- how many documents or chunks
- how often queries run
- whether tenant isolation is required
- whether metadata filtering is critical
- who will operate the store

Without those answers, "compare vector databases" is mostly noise.

### Step 2 - Compare the options in operational terms

Pinecone:

- managed service
- designed around production semantic search workflows
- supports namespaces and metadata filtering
- reduces infrastructure work, but adds service spend and vendor dependency

Milvus:

- strong dedicated vector database option
- broader self-managed or managed choices depending on deployment path
- better fit when teams want more direct control and can absorb operational complexity

pgvector:

- keeps vectors with the rest of the data in Postgres
- benefits from Postgres features such as ACID behavior, joins, and point-in-time recovery
- often a strong fit when the team already runs Postgres and scale is still moderate

### Step 3 - Identify overkill cases

A dedicated vector database may be overkill when:

- the corpus is small
- metadata filtering matters more than semantic nuance
- operator capacity is limited
- keyword or structured search already solves most cases

The right question is:

> What is the smallest store that satisfies retrieval quality and operating needs?

### Step 4 - Add metadata and tenant design

If the workflow serves multiple clients, retrieval should usually filter by tenant or namespace first.

This is both:

- a privacy boundary
- a retrieval quality boundary

Vector search without proper filtering often fails in both ways.

### Step 5 - Plan for evolution

Revisit the store choice if:

- query volume rises sharply
- latency targets tighten
- the embedding model changes
- tenant count grows
- operators cannot keep up with the chosen system

The first store choice does not have to be permanent. It does need to be deliberate.

---

## Practice

Choose one workflow that needs semantic retrieval or might appear to need it.

Create:

1. a vector storage decision memo
2. a retrieval store scorecard comparing Pinecone, Milvus, and pgvector
3. a fit or overkill recommendation
4. at least three revisit triggers

Minimum requirements:

- one requirement about metadata filtering
- one requirement about multitenancy or access control
- one requirement about operator capacity or cost

---

## Review Checklist

Your storage artifact is acceptable when:

- Retrieval requirements are stated first.
- Pinecone, Milvus, and pgvector are compared in operational terms.
- Metadata and tenant boundaries are considered.
- Managed versus self-managed trade-offs are visible.
- Overkill cases are acknowledged.
- Revisit triggers are named.
- The final choice is tied to the workflow, not to hype.

---

## Common Failure Modes

- **Vector DB by default:** Storage is chosen before the retrieval problem is defined.
- **Metadata blindness:** Filters and access boundaries are treated as secondary.
- **Operator mismatch:** A system is selected that nobody is prepared to run.
- **Store split:** Vectors are detached from the records needed to interpret them.
- **No exit path:** Reindexing or migration assumptions are absent.
- **Cost amnesia:** Storage and query growth are not part of the decision.

---

## Portfolio Evidence

Save:

- the vector storage decision memo
- the scorecard
- one paragraph on why the rejected options were rejected

This shows that you can choose retrieval infrastructure as an engineering decision, not a category purchase.

---

## References

- Pinecone documentation: https://docs.pinecone.io/
- Milvus documentation: https://milvus.io/docs
- pgvector README: https://github.com/pgvector/pgvector
- OpenAI Retrieval guide: https://platform.openai.com/docs/guides/retrieval
