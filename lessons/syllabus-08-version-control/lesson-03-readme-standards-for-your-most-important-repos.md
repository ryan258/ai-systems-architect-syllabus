# Lesson 08.03: README Standards for Your Most Important Repos

**Parent syllabus:** [Syllabus 08: Version Control - Organizing an Advanced Repo Library](../../syllabus-08-version-control.md)  
**Estimated time:** 2-3 hours  
**Artifact:** README standard and repo metadata checklist

---

## Outcome

By the end of this lesson, you can apply one consistent README standard across important repos so both technical collaborators and non-technical clients can understand what a repo is, why it matters, and where to start.

You will produce:

- a README standard
- a repo metadata checklist
- a first-three-sentences template

---

## Why This Matters

In a large repo library, many READMEs quietly fail at the first paragraph.

Common failures:

- The README explains implementation details before it explains what the project is.
- A non-technical visitor cannot tell why the repo is useful.
- The first screen contains badges, installation steps, and jargon but no positioning.
- The repo description, About sidebar, and README opening disagree.
- The repo lacks status, owner, or risk notes, so no one knows whether it is current.
- AI workflow repos skip data, evaluation, or security boundaries even when those boundaries are part of the system.

The README is not only documentation. It is repo-level interface design.

---

## Best-Practice Principles

1. **Use the opening for orientation.**  
   The first three sentences should say what the repo is, why it matters, and who it is for.

2. **Align repo metadata with the README.**  
   Title, description, status, topics, and opening paragraph should tell the same story.

3. **Differentiate client-facing from developer-facing needs.**  
   A strong README can serve both audiences, but only if it separates overview from implementation detail.

4. **Include system boundaries when they matter.**  
   For AI repos, note data boundaries, review gates, evaluation posture, or operational constraints when those define safe use.

5. **State project status clearly.**  
   Active, reference, archived, or experimental should not be guesswork.

6. **Show where to start.**  
   Visitors should know whether to read architecture docs, run a demo, inspect examples, or skip the repo entirely.

7. **Keep the standard repeatable.**  
   If the template is too heavy, it will not be used across the library.

---

## Concepts

### First-Three-Sentences Rule

A simple README opening rule:

1. what the project is
2. why it exists or why it is useful
3. who should use it or how to start

### Repo Metadata

The public signals around the repository besides the README body.

Examples:

- description
- topics
- homepage
- status note
- citation file
- owner or maintainer info

### Client-Facing README

A README optimized for someone deciding whether this repo is relevant and credible.

### Developer-Facing README

A README optimized for setup, contribution, architecture, and operation.

### Repo Metadata Checklist

A reusable checklist that makes sure the repo landing page is coherent.

---

## README Standard Template

Use this structure:

```markdown
# [Repository Name]

Sentence 1: What this project is.
Sentence 2: Why it exists or why it is useful.
Sentence 3: Who it is for or where to start.

## Status

Active, reference, archived, or experimental.

## What This Repo Includes

- ...

## Why It Matters

What problem does it solve?

## Architecture or Structure

High-level overview of the important parts.

## Quick Start or How to Explore

How a visitor should begin.

## Safety and Boundaries

Data, evaluation, review, or security notes if relevant.

## Evidence

Screenshots, architecture notes, demo links, or release notes.

## Maintainer

Who owns the repo?
```

Companion checklist:

```markdown
# Repo Metadata Checklist

- Description matches README opening
- Status is explicit
- Topics are present if the repo should be discoverable
- Homepage or docs link exists if relevant
- README opening follows the first-three-sentences rule
- Maintainer or owner is named
- Sensitive screenshots or client references are removed
```

---

## Walkthrough

Use this example:

> A repo contains an AI workflow API, test harness, and architecture notes for client-safe deployment.

Weak opening:

> FastAPI service using Pydantic, Celery, Redis, structured outputs, evals, and observability.

This tells a developer some implementation facts, but it does not orient a client or collaborator.

Better opening:

> This repo is a production-oriented AI workflow API for handling structured task execution with human review points. It exists to show how an AI system can be built with explicit evaluation, observability, and safety boundaries instead of prompt-only logic. Start with the architecture section if you want the system overview, or the quick start section if you want to run it locally.

### Step 1 - Fix the repo description and README together

Do not update one without the other.

Example repo description:

> Production-oriented AI workflow API with evaluation, observability, and human review gates.

Now the repo description and README opening reinforce each other.

### Step 2 - Add status and boundaries

For an important repo, include:

- status
- intended audience
- safety or review notes
- maintainer

For AI repos specifically, "Safety and Boundaries" may include:

- data sensitivity assumptions
- whether outputs need human review
- whether the repo is a demo, reference, or production path

### Step 3 - Decide what the visitor should do next

GitHub README guidance notes that README files often cover:

- what the project does
- why it is useful
- how users can get started
- where users can get help
- who maintains it

That means the README should not stop at architecture. It should also route the reader.

### Step 4 - Build the checklist

Example checklist use:

```markdown
# Repo Metadata Checklist

- Description matches README opening: yes
- Status is explicit: yes
- Topics are present if the repo should be discoverable: no, add them
- Homepage or docs link exists if relevant: yes
- README opening follows the first-three-sentences rule: yes
- Maintainer or owner is named: yes
- Sensitive screenshots or client references are removed: yes
```

The artifact is accepted when another person could review the repo landing page without guessing what the project is.

---

## Practice

Create a README standard and repo metadata checklist for your library.

Then apply it to at least three important repos.

Your standard must define:

1. the first-three-sentences rule
2. required sections
3. status vocabulary
4. repo metadata fields to review
5. AI-specific boundary notes when relevant

For each of the three repos, record:

- old opening
- new opening
- missing metadata fixed
- remaining gaps

---

## Review Checklist

Your README standard is acceptable when:

- The first three sentences are defined clearly.
- Required sections are short enough to repeat across many repos.
- Status is explicit.
- Repo description and README opening are aligned.
- The checklist includes privacy or security review of public materials.
- The standard works for both client-facing and developer-facing repos.
- At least three repos have been tested against it.

---

## Common Failure Modes

- **Tool-first opening:** The README starts with stack details instead of project meaning.
- **Metadata drift:** Description, README, and status say different things.
- **No status:** Visitors cannot tell whether the repo is active or historical.
- **No boundaries:** AI repos omit evaluation, data, or review context.
- **Template bloat:** The standard is too heavy to apply consistently.
- **No start path:** Visitors still do not know where to begin.

---

## Portfolio Evidence

Save:

- The README standard
- The repo metadata checklist
- Before-and-after openings for three repos

This shows that you can make technical work legible without dumbing it down.

---

## References

- GitHub Docs, About the repository README file: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes
- GitHub Docs, About repositories: https://docs.github.com/en/repositories/creating-and-managing-repositories/about-repositories
- GitHub Docs, About CITATION files: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-citation-files
