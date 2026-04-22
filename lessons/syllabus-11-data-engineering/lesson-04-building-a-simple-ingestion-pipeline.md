# Lesson 11.04: Building a Simple Ingestion Pipeline

**Parent syllabus:** [Syllabus 11: Data Engineering and Pipelines](../../syllabus-11-data-engineering.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Ingestion pipeline runbook and ingest manifest specification

---

## Outcome

By the end of this lesson, you can define a Python ingestion pipeline that reads multiple document formats, normalizes text, creates chunks, and records enough metadata that bad files do not quietly poison the whole run.

You will produce:

- an ingestion pipeline runbook
- an ingest manifest specification
- failure handling and quarantine rules

---

## Why This Matters

Retrieval quality starts with ingestion quality.

Common failures:

- One malformed PDF crashes the whole batch.
- Extracted text is empty or broken, but the document still gets indexed.
- The pipeline cannot tell which chunks came from which source file.
- Metadata required for access control or deletion is missing.
- Temporary files and logs accumulate sensitive text carelessly.
- The team can re-run ingestion, but cannot explain what changed between runs.

The ingestion pipeline is where raw document chaos becomes system behavior.

---

## Best-Practice Principles

1. **Create a manifest for every run.**  
   A pipeline run should record what was discovered, what was accepted, what failed, and why.

2. **Fail per document when possible, not per batch.**  
   One bad file should usually be quarantined, not treated as a reason to lose the whole ingest wave.

3. **Preserve provenance.**  
   Every chunk should be traceable to a source file, source ID, and document version or checksum.

4. **Normalize before embedding.**  
   Encoding cleanup, whitespace cleanup, and metadata attachment belong before chunking and indexing.

5. **Treat extraction as an untrusted stage.**  
   OCR failures, empty text, and parser weirdness are normal.

6. **Log outcomes, not raw private content.**  
   Operators need counts, IDs, statuses, and error classes more than full document text in logs.

7. **Make reruns safe.**  
   Re-ingesting the same file should not create silent duplicate chunks or ambiguous document state.

---

## Concepts

### Ingest Manifest

A structured record of document-level ingestion outcomes for one run.

Typical fields:

- run ID
- source file
- document ID
- checksum
- extraction status
- chunk count
- store status
- error class

### Quarantine

A holding state for documents that failed ingestion or require human review before indexing.

### Normalization

Cleaning text and metadata into a consistent form before chunking.

### Provenance

The ability to trace a chunk or embedding back to its source document and ingest run.

### Idempotent Ingest

An ingestion path where reprocessing the same document does not create unintended duplicates.

---

## Ingest Manifest Specification Template

Use this structure:

```markdown
# Ingest Manifest Specification: [Pipeline Name]

## Run Metadata

- Run ID:
- Started at:
- Operator or trigger:
- Source set:

## Per-Document Fields

| Field | Meaning |
| --- | --- |
| document_id | stable source identifier |
| source_path | original location |
| checksum | content fingerprint |
| format | pdf, docx, txt, etc. |
| extraction_status | success, empty, failed, quarantined |
| normalized_text_length | text length after cleanup |
| chunk_count | number of chunks created |
| store_status | stored, skipped, failed |
| error_class | parser_error, empty_text, missing_metadata, etc. |

## Quarantine Rules

- ...

## Logging Rules

- ...
```

Example ingestion skeleton:

```python
from pathlib import Path


def process_document(path: Path) -> dict:
    try:
        text = extract_text(path)
        normalized = normalize_text(text)
        if not normalized.strip():
            return {"status": "quarantined", "error_class": "empty_text"}

        chunks = chunk_text(normalized)
        store_chunks(chunks)
        return {"status": "stored", "chunk_count": len(chunks)}
    except Exception as exc:
        return {"status": "failed", "error_class": type(exc).__name__}
```

The code pattern matters less than the outcome: each document gets an explicit status.

---

## Walkthrough

Use this example:

> A pipeline ingests PDF guides, DOCX policy notes, and plain-text release notes into a retrieval store.

### Step 1 - Discover files and attach stable IDs

Before extraction, record:

- source path
- document ID
- file format
- checksum or content hash
- tenant or visibility metadata

Without stable IDs, later deletion, updates, and deduplication become messy.

### Step 2 - Extract text carefully

For PDFs:

- extraction can fail because the document is image-only, malformed, or huge
- some PDFs require OCR beyond normal text extraction

The `pypdf` docs explicitly note that text extraction can require significant memory and that scanned pages may yield little usable text.

For DOCX:

- the structure is often cleaner, but you still need to decide how to treat headings, tables, and comments

The extraction stage should not assume "file readable" means "text usable."

### Step 3 - Normalize and enrich

Normalization may include:

- whitespace cleanup
- line-break cleanup
- heading preservation
- metadata attachment
- document-type tagging

This is also the stage to reject files missing required metadata such as tenant ID or approval state.

### Step 4 - Chunk and store with provenance

Each chunk should retain:

- source document ID
- chunk index
- document title or section
- checksum or version
- visibility metadata

Retrieval without provenance makes debugging and deletion hard later.

### Step 5 - Quarantine bad documents

Examples to quarantine:

- extraction returned empty text
- parser raised an error
- file missing required metadata
- extracted text is suspiciously small for the file type

Bad ingestion systems hide these cases. Good ones make them visible.

### Step 6 - Record the run

At the end of the batch, the runbook should answer:

- how many files were discovered
- how many were stored
- how many were quarantined
- how many failed
- what the top error classes were

This is the minimum observability needed to trust the pipeline.

---

## Practice

Design an ingestion pipeline for a mixed document set you already have or want to use.

Create:

1. an ingestion pipeline runbook
2. an ingest manifest specification
3. at least five per-document status fields
4. quarantine rules for at least three failure classes
5. logging rules that avoid raw-content leakage

Minimum requirements:

- support for at least three document formats
- one deduplication or checksum rule
- one provenance rule for chunks
- one rerun-safety rule

---

## Review Checklist

Your ingestion artifact is acceptable when:

- Every document gets an explicit status.
- Manifest fields support provenance and reruns.
- Bad documents are quarantined instead of silently indexed.
- Chunk provenance is preserved.
- Logging avoids raw sensitive content.
- Mixed formats are handled deliberately.
- The run summary would help an operator debug the batch quickly.

---

## Common Failure Modes

- **Batch crash mentality:** One bad file stops the whole run.
- **No manifest:** The run cannot be reconstructed document by document.
- **No provenance:** Chunks cannot be traced back to source.
- **Silent empty text:** Bad extraction looks like a successful ingest.
- **Unsafe logs:** Raw document content leaks into logs or temp files.
- **Duplicate drift:** Re-runs create duplicate chunks because identity rules are weak.

---

## Portfolio Evidence

Save:

- the ingestion pipeline runbook
- the ingest manifest specification
- one redacted example quarantine case with the error class and decision

This shows that you can turn messy source files into a controlled ingest process instead of a hopeful batch script.

---

## References

- pypdf documentation: https://pypdf.readthedocs.io/
- Extract Text from a PDF: https://pypdf.readthedocs.io/en/6.5.0/user/extract-text.html
- python-docx documentation: https://python-docx.readthedocs.io/
- OpenAI Retrieval guide: https://platform.openai.com/docs/guides/retrieval
