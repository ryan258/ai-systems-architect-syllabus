# Lesson 08.01: Auditing 326 Repos Without Losing Your Mind

**Parent syllabus:** [Syllabus 08: Version Control - Organizing an Advanced Repo Library](../../syllabus-08-version-control.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Repository audit rubric and triage ledger

---

## Outcome

By the end of this lesson, you can audit a large GitHub library quickly enough to make real decisions about what stays active, what becomes reference material, what gets archived, and what should be deleted after backup.

You will produce:

- a repository audit rubric
- a triage ledger
- a first audit wave decision list

---

## Why This Matters

A large repo library creates a discoverability problem, a maintenance problem, and sometimes a security problem.

Common failures:

- Good work is buried under abandoned spikes and half-documented experiments.
- A client lands on the wrong repo first and leaves with the wrong impression of your work.
- A repo should be archived, but it still looks active because nobody updated the README or description.
- A repo should be deleted, but nobody backed it up or checked whether it still contains something worth preserving.
- Public repos still expose client names, stale screenshots, or internal assumptions because nobody reviewed them as a library.
- The owner cannot answer a basic question like "What are your best current repos?" without manually searching.

This module is not about Git basics. It is about turning a large repo collection into an intentional library.

---

## Best-Practice Principles

1. **Audit in waves, not all at once.**  
   A large library needs a repeatable process, not a one-time heroic cleanup.

2. **Use explicit triage states.**  
   "Maybe later" is not a useful decision. Repos should end the audit as active, reference, archive, or delete after backup.

3. **Separate historical value from active value.**  
   A repo can still be worth keeping even if it is no longer maintained.

4. **Back up before destructive actions.**  
   Deletion without a preservation decision is avoidable risk.

5. **Check public-facing risk during the audit.**  
   Repo descriptions, screenshots, docs, and commit history may expose more than the code itself.

6. **Record why the decision was made.**  
   A ledger without reasoning becomes stale as soon as you forget the context.

7. **Favor maintainable criteria.**  
   The rubric should be simple enough to reuse on a low-energy day.

---

## Concepts

### Triage Ledger

A working table that records repo-level facts, risks, and final actions.

Typical columns:

- repo name
- purpose
- audience
- current state
- documentation quality
- privacy or security concerns
- final disposition

### Active

A repo that should continue to represent current work.

Usually:

- still maintained
- relevant to current positioning
- understandable enough to send to others

### Reference

A repo worth keeping visible or searchable, but not actively evolving.

Usually:

- still useful as an example
- not a current product
- may need a status note in the README

### Archive

A repo that should remain available for viewing and reference but should no longer look active.

GitHub's archive feature makes the repository read-only for all users and indicates it is no longer actively maintained.

### Delete After Backup

A repo whose value is low enough that it should be removed after preserving anything worth keeping.

This is a governance decision, not just a cleanup impulse.

### Audit Signal

A fact that helps decide the repo's status.

Examples:

- last meaningful update
- whether the README explains the project
- whether the repo matches current positioning
- whether it includes risky public material
- whether it still attracts useful interest

---

## Repository Audit Rubric Template

Use this structure:

```markdown
# Repository Audit Rubric

## Triage States

- Active
- Reference
- Archive
- Delete after backup

## Audit Questions

1. Does this repo still represent work I want people to find?
2. Is the README good enough for a stranger?
3. Does the repo expose any client, privacy, or security risk?
4. Is the repo still maintained, or is it historical?
5. Should the repo remain editable, read-only, or removed?

## Triage Ledger

| Repo | Main purpose | Audience | README status | Risk notes | Triage state | Next action | Reason |
| --- | --- | --- | --- | --- | --- | --- | --- |
|  |  |  |  |  |  |  |  |
```

Add optional fields if useful:

- pushed recently
- stars or external interest
- topic tags
- profile-candidate flag
- backup required

---

## Walkthrough

Use this example:

> An AI systems architect has a public GitHub account with 326 repos covering client experiments, internal utilities, prototypes, tutorials, and abandoned spikes.

### Step 1 - Decide what the audit is for

Weak goal:

> Clean things up.

Better goals:

- make the best current work easy to find
- reduce maintenance burden
- remove misleading or risky public signals
- preserve historical value where appropriate

The review question is:

> After this audit, what should a client, collaborator, or future you understand faster?

### Step 2 - Collect facts quickly

GitHub CLI provides a useful starting point here. The `gh repo list` command lists repositories owned by a user or organization and supports JSON output, archived filtering, and jq-based filtering.

Example audit pull:

```bash
gh repo list YOUR_USERNAME --limit 400 --json name,description,isArchived,updatedAt,pushedAt,stargazerCount,repositoryTopics,url
```

This is not the whole audit. It is the intake.

The ledger still needs human judgment on:

- current relevance
- documentation quality
- client-facing usefulness
- privacy or security concerns

### Step 3 - Define triage criteria

Example working rules:

| State | Use when | Do not use when |
| --- | --- | --- |
| Active | Repo matches current positioning and is worth sending | README is weak or repo is misleading |
| Reference | Repo still teaches or proves something useful | Repo looks active when it is not |
| Archive | Historical value exists, but maintenance has ended | Repo still needs active edits |
| Delete after backup | Low value, duplicate, risky, or obsolete | Repo still contains reusable proof or active links |

### Step 4 - Add risk review, not just relevance review

During the audit, check for:

- client names in repo names or screenshots
- stale credentials or unsafe examples
- public docs that reveal internal assumptions
- abandoned demos that no longer meet your standards

Archiving does not clean the repo. It only makes it read-only. If the risk is in the content itself, archiving may not be enough.

### Step 5 - Decide the first audit wave

Do not start with all 326 repos.

Start with:

- the most visible repos
- the repos likely to be pinned
- the repos closest to client discovery
- the repos that create the highest public confusion

Example first wave:

- top 20 by current strategic value
- top 20 by public visibility or stars
- any repo with obvious privacy, security, or branding risk

### Step 6 - Fill the ledger

Example:

```markdown
| Repo | Main purpose | Audience | README status | Risk notes | Triage state | Next action | Reason |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ai-workflow-api | Production-facing workflow API example | Clients and collaborators | Strong | None found | Active | Refresh topic tags | Strong current proof |
| local-agent-spike-2023 | Old experiment | Personal history | Weak | Misleading unfinished README | Archive | Update README status, then archive | Historical but not current |
| customer-demo-raw | One-off demo | None | Weak | Contains client-identifying screenshots | Delete after backup | Back up, sanitize if needed, then remove | Public risk outweighs value |
```

### Step 7 - Back up before destructive actions

GitHub's backup guidance includes mirror-cloning a repository with Git when you need a preserved copy.

Operational rule:

- back up before delete
- record where the backup lives
- note whether the backup is for history, migration, or possible restore

The artifact is accepted when the decision is reproducible, not when it feels tidy.

---

## Practice

Create a repository audit rubric and triage ledger for your own library.

Minimum scope:

1. define the four triage states
2. write five audit questions
3. audit at least 25 repos in the first wave
4. identify at least five repos that need action soon
5. record whether each action is active, reference, archive, or delete after backup

For each repo in the first wave, capture:

- main purpose
- audience
- README status
- risk notes
- next action
- reason

End with a short decision note:

- what you will fix this week
- what you will archive
- what you will delete only after backup

---

## Review Checklist

Your repo audit is acceptable when:

- The rubric uses explicit triage states.
- The ledger records reasons, not just statuses.
- The first audit wave is small enough to finish.
- At least one privacy or security review field exists.
- Delete decisions include a backup condition.
- Archive decisions include a note to update status signals first.
- The audit makes it easier to identify current flagship repos.
- The ledger can be reused for the next wave.

---

## Common Failure Modes

- **Infinite audit:** The scope is so large that no decisions are made.
- **No reasons:** A future pass cannot tell why a repo was archived or kept active.
- **Archive as avoidance:** Repos are archived without fixing misleading README or description text first.
- **Delete without backup:** Low-value repos are removed before preservation decisions are made.
- **No risk review:** The audit treats discoverability as the only issue.
- **One-time cleanup mindset:** The ledger is not structured for reuse.

---

## Portfolio Evidence

Save:

- The repository audit rubric
- The first-wave triage ledger
- A short note on the highest-risk repo you discovered and what you did about it

This shows that you can manage a code library as a portfolio, not just a pile of repos.

---

## References

- GitHub CLI, `gh repo list`: https://cli.github.com/manual/gh_repo_list
- GitHub Docs, Archiving repositories: https://docs.github.com/repositories/archiving-a-github-repository/archiving-repositories
- GitHub Docs, Backing up a repository: https://docs.github.com/repositories/archiving-a-github-repository/backing-up-a-repository
