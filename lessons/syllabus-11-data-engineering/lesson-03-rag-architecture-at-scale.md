# Lesson 11.03: RAG Architecture at Scale

**Parent syllabus:** [Syllabus 11: Data Engineering and Pipelines](../../syllabus-11-data-engineering.md)  
**Estimated time:** 2-3 hours  
**Artifact:** RAG architecture brief and chunking strategy worksheet

---

## Outcome

By the end of this lesson, you can design a RAG system that handles large document sets intentionally, including chunking, embedding choice, retrieval quality, and the boundary between retrieval and fine-tuning.

You will produce:

- a RAG architecture brief
- a chunking strategy worksheet
- a RAG versus fine-tuning decision note

---

## Why This Matters

At scale, retrieval quality becomes product quality.

Common failures:

- Chunking decisions are arbitrary, so important context is split badly or duplicated excessively.
- Retrieval returns semantically similar but operationally wrong material because metadata, recency, or document boundaries were ignored.
- A client says "train on our data," and the team chooses fine-tuning for fast-changing factual knowledge.
- Embedding choice is made once and never revisited even after the corpus changes.
- The system retrieves too much low-quality context and quietly degrades answer quality and cost.
- Nobody evaluates retrieval separately from final model output.

RAG systems fail less from the model than from the retrieval contract around the model.

---

## Best-Practice Principles

1. **Design retrieval around the question the system must answer.**  
   Good chunking and filtering depend on the real query behavior, not generic defaults.

2. **Keep factual knowledge and behavioral tuning separate.**  
   Retrieval is usually for changing knowledge. Fine-tuning is often for behavior, format, or repeated instruction-following improvement.

3. **Chunk for retrieval, not for convenience.**  
   Chunk size, overlap, and boundaries should help relevant material surface without drowning the model in noise.

4. **Evaluate retrieval quality directly.**  
   If the wrong chunks are returned, answer quality will be unstable even with a strong model.

5. **Use metadata aggressively.**  
   Document type, version, recency, tenant, and visibility often matter as much as vector similarity.

6. **Treat embedding choice as a system variable.**  
   Relevance, latency, and cost all change with embedding models.

7. **Keep retrieval architecture maintainable.**  
   A system that nobody can reindex or explain will rot after the first corpus change.

---

## Concepts

### Retrieval-Augmented Generation (RAG)

A pattern where the system fetches relevant context first and then uses that context to answer or generate.

### Chunk

A unit of text stored and retrieved for semantic search.

### Chunking Strategy

The rule for how documents are split into retrieval units.

Examples:

- fixed-size chunks
- paragraph or heading boundaries
- semantic or section-aware chunks

### Embedding Model

The model used to convert text into vectors for similarity search.

### Retrieval Precision

How often the returned chunks are actually relevant to the query.

### Retrieval Recall

How often the important relevant chunks are included in the returned set.

---

## RAG Architecture Brief Template

Use this structure:

```markdown
# RAG Architecture Brief: [Workflow Name]

## Workflow Goal

What question or task should retrieval support?

## Corpus Shape

- Number of documents:
- Document types:
- Update frequency:
- Access boundaries:

## Retrieval Plan

- Chunking strategy:
- Embedding model:
- Metadata filters:
- Top-k:
- Reranking, if any:

## Failure Modes That Matter

- ...

## Evaluation Questions

- Are the right chunks returned?
- Are stale chunks suppressed?
- Are cross-tenant results impossible?

## RAG vs Fine-Tuning Decision

- Retrieval is for:
- Fine-tuning is for:
- Not doing:
```

Chunking strategy worksheet:

```markdown
# Chunking Strategy Worksheet

## Candidate Strategy

- Fixed-size
- Section-aware
- Semantic

## Why It Fits

- ...

## Risks

- Too small:
- Too large:
- Too much overlap:
- Too little overlap:

## Test Queries

- ...
```

---

## Walkthrough

Use this example:

> A support assistant must answer product and policy questions from thousands of approved documents that change weekly.

### Step 1 - Separate retrieval knowledge from behavioral tuning

For this workflow:

- current product facts and policy content belong in retrieval
- answer tone and stable output format may justify fine-tuning later

Inference grounded in the OpenAI retrieval and fine-tuning docs:

Retrieval is the right fit for changing source material. Fine-tuning is more appropriate when the goal is improving style, structure, classification behavior, or repeated instruction-following performance.

If the corpus changes weekly, baking factual knowledge into model weights is usually the wrong first move.

### Step 2 - Design chunking around the documents and queries

Bad chunking rule:

> Split everything every 500 characters because that is what the sample notebook did.

Better chunking questions:

- do users ask about specific sections or broad policies?
- do headings carry meaning?
- do tables or lists need special handling?
- does one answer usually need one chunk or several related chunks?

For policy and product docs, heading- or section-aware chunking often beats blind fixed-size splitting because provenance stays clearer.

### Step 3 - Use metadata to reduce wrong matches

Useful filters may include:

- tenant
- document status
- effective date
- product area
- document type

This protects:

- privacy
- retrieval precision
- answer trustworthiness

### Step 4 - Pick an embedding path intentionally

Your embedding choice should consider:

- relevance quality on your corpus
- latency budget
- indexing cost
- reindexing cost if the model changes

Do not assume the first embedding model used in a tutorial is still the right one once the corpus grows or query patterns change.

### Step 5 - Evaluate retrieval before blaming generation

Questions to test:

- Did the correct document family appear?
- Was the most relevant chunk in the top results?
- Did stale or draft content appear when it should not?
- Did the system retrieve the wrong tenant's content?

If retrieval fails, changing the answer model often treats symptoms, not the cause.

### Step 6 - Keep the client conversation honest

When a client says:

> Train an AI on our data.

The review question is:

> Do you need the system to know changing facts, or do you need it to behave in a particular way every time?

That is the boundary between retrieval and fine-tuning.

---

## Practice

Choose one workflow that would benefit from document retrieval.

Create:

1. a RAG architecture brief
2. a chunking strategy worksheet comparing at least two approaches
3. an embedding-selection note
4. a RAG versus fine-tuning decision memo
5. at least five retrieval-evaluation questions

Minimum requirements:

- one rule about metadata filtering
- one rule about recency or document version
- one statement about what should not be fine-tuned

---

## Review Checklist

Your RAG artifact is acceptable when:

- The corpus shape is described.
- Chunking strategy is justified by query behavior.
- Metadata filters are explicit.
- Retrieval quality is treated as a measurable system property.
- The RAG versus fine-tuning boundary is clear.
- Embedding choice is considered operationally.
- The design can be explained without jargon inflation.

---

## Common Failure Modes

- **Chunking by habit:** Split sizes are copied without regard to document structure.
- **RAG for behavior problems:** Fine-tuning needs are misdiagnosed as retrieval needs, or vice versa.
- **Filter neglect:** Tenant, recency, or approval state are ignored.
- **Generation blame:** Wrong answers are blamed on the model when retrieval was weak.
- **Embedding freeze:** The embedding path never gets revisited after the corpus changes.
- **No retrieval evals:** The team measures only final answer quality.

---

## Portfolio Evidence

Save:

- the RAG architecture brief
- the chunking strategy worksheet
- the RAG versus fine-tuning decision memo

This shows that you can design retrieval around system behavior instead of around slogans about "training on data."

---

## References

- OpenAI Retrieval guide: https://platform.openai.com/docs/guides/retrieval
- OpenAI Fine-Tuning guide: https://platform.openai.com/docs/guides/fine-tuning
- OpenAI Supervised Fine-Tuning guide: https://platform.openai.com/docs/guides/supervised-fine-tuning
- Pinecone documentation: https://docs.pinecone.io/
- Milvus documentation: https://milvus.io/docs
