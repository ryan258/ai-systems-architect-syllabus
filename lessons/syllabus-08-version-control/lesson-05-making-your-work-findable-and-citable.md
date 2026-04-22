# Lesson 08.05: Making Your Work Findable and Citable

**Parent syllabus:** [Syllabus 08: Version Control - Organizing an Advanced Repo Library](../../syllabus-08-version-control.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Repo discoverability and citation plan

---

## Outcome

By the end of this lesson, you can make your strongest repos easier to find through GitHub search and easier to cite or reference correctly when someone wants to use, mention, or review the work.

You will produce:

- a topic-tagging strategy
- improved description rules
- a citation plan
- a GitHub-only versus portfolio-page decision note

---

## Why This Matters

Strong work still gets missed when the repo metadata is weak.

Common failures:

- The repo description is too vague to help search or browsing.
- Topic tags are missing, inconsistent, or too generic.
- Topics on a private repo are treated like private metadata even though topic names are public.
- A strong research or architecture repo has no citation guidance.
- The repo library has many good pieces, but there is no curated page connecting them.
- A separate portfolio page is created too early, adding maintenance cost without solving the underlying metadata problems.

Findability is architecture for your public technical surface.

---

## Best-Practice Principles

1. **Write for search and scanning, not cleverness.**  
   Repo descriptions should be explicit about project type and purpose.

2. **Use a controlled topic vocabulary.**  
   Inconsistent tags make the library harder to navigate.

3. **Remember topic names are public.**  
   Do not use sensitive, client-specific, or confidential topic labels.

4. **Add citation support where the work merits reuse or reference.**  
   Architecture frameworks, libraries, datasets, and research-adjacent repos benefit from clear citation metadata.

5. **Fix GitHub metadata before adding another surface.**  
   A portfolio page helps most when it curates already legible repos.

6. **Choose a small number of discoverability patterns and repeat them.**  
   Consistency helps both humans and future maintenance.

7. **Review maintainability cost.**  
   Every extra homepage, landing page, or citation path adds work to keep accurate.

---

## Concepts

### Repo Description

The short public summary shown in GitHub lists, sidebars, and search contexts.

### Topic Strategy

A controlled set of tags used across the library.

Examples:

- `ai-workflow`
- `evaluation`
- `observability`
- `fastapi`
- `agent-orchestration`

### Citation Metadata

Information that tells others how to reference the repo correctly.

GitHub supports `CITATION.cff` in the repository root and adds a "Cite this repository" link when it is present on the default branch.

### GitHub-Only Strategy

Relying on GitHub profile, repo metadata, pinned repos, and README navigation without an additional portfolio site.

### Portfolio-Page Strategy

A separate landing page that curates and groups repos beyond what the GitHub profile can do easily.

---

## Discoverability and Citation Plan Template

Use this structure:

```markdown
# Repo Discoverability and Citation Plan

## Topic Vocabulary

- Primary topics:
- Secondary topics:
- Topics never to use:

## Description Rules

- Rule 1:
- Rule 2:
- Rule 3:

## Repo Plan

| Repo | Description | Topics | Citation needed? | Homepage needed? | Notes |
| --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |

## Citation Policy

- Which repos get `CITATION.cff`:
- Which repos only need README citation guidance:
- Which repos do not need citation support:

## GitHub-Only vs Portfolio Decision

- Current choice:
- Why:
- Revisit when:
```

---

## Walkthrough

Use this example:

> An AI systems architect has several production-style repos, several teaching repos, and a few framework or library repos that others may cite or reuse.

### Step 1 - Define a controlled topic vocabulary

Weak topic pattern:

- every repo gets different tags
- some tags describe stack
- some describe outcomes
- some are jokes or internal language

Better topic pattern:

- a small core vocabulary used consistently
- optional secondary tags for stack or domain

Example:

- primary: `ai-workflow`, `ai-systems`, `evaluation`, `observability`, `fastapi`
- secondary: `python`, `retrieval`, `client-safety`, `multi-agent`
- never use: client names, confidential internal labels, topics that mislead about maturity

GitHub docs also note that topic names are public, even if you create the topic from within a private repository. That is a privacy review issue, not just a metadata note.

### Step 2 - Write descriptions that scan well

Weak description:

> experiments and tools

Better description:

> AI workflow evaluation harness for regression testing, trace review, and release gates.

Rule of thumb:

- name the project type
- name the job it does
- keep it short enough to scan

### Step 3 - Decide which repos need citation support

Some repos deserve structured citation support:

- reusable libraries
- frameworks
- research-adjacent repos
- public methodology repos

GitHub docs explain that adding `CITATION.cff` to the root of the default branch creates a "Cite this repository" link and gives people a structured way to reference the work.

### Step 4 - Decide whether GitHub alone is enough

Use GitHub-only when:

- the pin set is coherent
- repo descriptions are strong
- README openings are consistent
- the story is not too fragmented

Use a separate portfolio page when:

- your best work spans many repos and needs grouping
- you want narrative case-study context across repos
- GitHub profile constraints make the story unclear

Do not add a portfolio page just because discoverability feels weak if the repo metadata is still messy.

### Step 5 - Fill the plan

Example:

```markdown
| Repo | Description | Topics | Citation needed? | Homepage needed? | Notes |
| --- | --- | --- | --- | --- | --- |
| ai-eval-harness | AI workflow evaluation harness for regression testing and release gates | ai-workflow, evaluation, observability, python | Yes | No | Add CITATION.cff |
| workflow-api-demo | Production-style AI workflow API with human review and safety boundaries | ai-workflow, fastapi, client-safety, python | No | No | Improve description |
| curriculum | AI systems architect curriculum with artifact-driven lessons and templates | ai-systems, education, architecture | Yes | Optional | Consider portfolio landing page link |
```

---

## Practice

Create a repo discoverability and citation plan for at least 20 repos.

Include:

1. a controlled topic vocabulary
2. description rules
3. a 20-repo metadata plan
4. a citation policy
5. a GitHub-only versus portfolio-page decision

For each repo in the plan, define:

- description
- topics
- whether citation support is needed
- whether a homepage or portfolio link is needed

At least three repos should be marked for citation review.

---

## Review Checklist

Your discoverability and citation plan is acceptable when:

- Topic vocabulary is controlled and reusable.
- Topic names avoid sensitive or client-specific labels.
- Description rules are explicit.
- At least 20 repos are covered in the plan.
- Citation support is applied selectively, not blindly.
- The GitHub-only versus portfolio decision includes a reason and a revisit trigger.
- The plan reduces maintenance sprawl instead of adding it.

---

## Common Failure Modes

- **Tag chaos:** Topics are inconsistent, vague, or duplicated.
- **Public metadata blind spot:** Sensitive topic names are treated as private.
- **Cute descriptions:** Repo summaries are clever but not useful in search.
- **Citation everywhere:** `CITATION.cff` is added without deciding whether it serves the repo.
- **Portfolio too early:** A second surface is added before GitHub metadata is fixed.
- **No revisit trigger:** The portfolio decision never gets reviewed as the library changes.

---

## Portfolio Evidence

Save:

- The discoverability and citation plan
- The controlled topic vocabulary
- A short note explaining whether GitHub alone is currently enough

This shows that you can make a repo library easier to search, reference, and trust.

---

## References

- GitHub Docs, Classifying your repository with topics: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics
- GitHub Docs, About CITATION files: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-citation-files
- GitHub Docs, About the repository README file: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes
