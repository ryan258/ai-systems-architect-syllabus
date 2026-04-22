# Syllabus 11: Data Engineering and Pipelines
**Status:** Enterprise Gap - The "Fuel" That Powers Your AI Systems
**Format:** One chat session per unit, self-paced

---

## What This Is

You already know how to pass context to an AI model. This syllabus teaches you how to design the systems that fetch, clean, and prepare that context before the AI ever sees it - at a scale that handles thousands of client documents, not just a few files.

---

## Unit 1 - Why Data Pipelines Matter for AI Architects

**Goal:** Understand what separates a toy RAG demo from a production-grade knowledge system.

**Topics:**
- What a data pipeline is - the process that moves raw data into a form the AI can use
- Why garbage in, garbage out is the most common reason AI workflows fail in production
- The three stages every AI data pipeline needs: ingest, clean, store
- How this connects to your existing context management thinking

**Session starter:**
> "Explain what a data pipeline is in the context of AI systems. Why do production AI workflows fail because of data problems rather than model problems? What are the three stages every AI data pipeline needs? Use concrete examples."

---

## Unit 2 - Vector Databases

**Goal:** Understand what vector databases are, why they exist, and when to use one.

**Topics:**
- What a vector is - a number-based representation of meaning (not just keywords)
- Why traditional search fails for AI workflows and semantic search fixes it
- Pinecone, Milvus, and pgvector - what each is good for
- When a vector database is overkill vs. when it is necessary
- How embeddings connect documents to search queries by meaning

**Session starter:**
> "Explain vector databases in plain language. What is a vector? Why does semantic search work better than keyword search for AI systems? Compare Pinecone, Milvus, and pgvector - when would I choose each one?"

---

## Unit 3 - RAG Architecture at Scale

**Goal:** Design a Retrieval-Augmented Generation system that handles thousands of documents reliably.

**Topics:**
- What RAG means - fetching relevant content before asking the model to answer
- The boundary between RAG and fine-tuning: injecting knowledge into the prompt vs. changing model weights and behavior
- When a client says "train an AI on our data," how to decide whether they actually need RAG, fine-tuning, or both
- Chunking strategies: fixed size, sentence boundary, semantic chunking
- Choosing the right embedding model for your use case
- How retrieval quality directly determines answer quality
- Common RAG failure modes and how to design around them
- When fine-tuning is better for tone, format, classification, or repeated instruction-following failures
- Why fine-tuning is usually the wrong tool for frequently changing factual knowledge

**Session starter:**
> "Help me design a production-grade RAG architecture. Explain the boundary between RAG and fine-tuning: when should I inject knowledge through retrieval, and when should I adjust a model's behavior or tone through fine-tuning? Include chunking, embedding choice, retrieval failure modes, and how to answer a client who asks to 'train an AI on our data.'"

---

## Unit 4 - Building a Simple Ingestion Pipeline

**Goal:** Write a Python pipeline that takes raw client documents and prepares them for AI retrieval.

**Topics:**
- Reading documents from multiple formats (PDF, DOCX, plain text)
- Cleaning and normalizing text before embedding
- Chunking documents and storing them in a vector database
- Handling pipeline failures gracefully so bad documents do not break the whole run

**Session starter:**
> "Help me build a Python ingestion pipeline that reads client documents in multiple formats, cleans the text, chunks it, and stores the chunks in a vector database. Include error handling so one bad document does not stop the whole pipeline."

---

## Unit 5 - Maintaining a Pipeline Over Time

**Goal:** Keep a data pipeline healthy after launch without constant manual intervention.

**Topics:**
- How to detect when new documents need to be re-indexed
- Handling document updates and deletions cleanly
- Monitoring pipeline health - how do you know it is still working?
- How changing documents affects RAG differently from fine-tuned models
- When to re-index, when to re-train, and when to leave the model alone
- Designing for low-maintenance operation on bad energy days

**Session starter:**
> "Help me design a data pipeline that maintains itself over time. How do I detect when documents need re-indexing? How do I handle updates and deletions? How is this different from maintaining a fine-tuned model? What does pipeline health monitoring look like? Design this for minimal ongoing maintenance."

---

## Unit 6 - Data Pipeline Review Package

**Goal:** Turn the boundary, storage, retrieval, ingestion, and maintenance decisions into one reviewer-ready pipeline package that supports prototype, limited rollout, redesign, or hold.

**Topics:**
- Combining the source-of-truth map, vector-store decision, RAG design, ingestion runbook, and maintenance policy into one reviewable packet
- Checking that metadata boundaries, deletion behavior, and chunk provenance do not contradict each other
- Writing rollout blockers and a final readiness decision
- Naming the future changes that force a re-review

**Session starter:**
> "Help me assemble a data pipeline review package for one of my AI workflows. I want the source-of-truth map, vector-store decision, RAG design, ingestion runbook, maintenance policy, and a final rollout recommendation in one coherent packet."

---

## Practice Project

Take a real set of documents - your own project notes, scripts, or guides - and turn them into a reviewable retrieval pipeline package that includes:

- a source-of-truth map and pipeline boundary brief
- a vector-store decision memo
- a chunking and RAG architecture plan
- an ingestion manifest specification and quarantine rules
- a maintenance and reindexing policy
- a plain-English retrieval test using a real question and relevance check
- a short RAG vs. fine-tuning decision memo
- a final rollout recommendation with blockers and next steps
