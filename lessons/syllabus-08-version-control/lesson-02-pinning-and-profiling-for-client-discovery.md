# Lesson 08.02: Pinning and Profiling for Client Discovery

**Parent syllabus:** [Syllabus 08: Version Control - Organizing an Advanced Repo Library](../../syllabus-08-version-control.md)  
**Estimated time:** 2 hours  
**Artifact:** GitHub profile positioning brief and pin set

---

## Outcome

By the end of this lesson, you can make a GitHub profile tell a coherent story about your work instead of showing six technically interesting but strategically disconnected repos.

You will produce:

- a profile positioning brief
- a six-item pin set
- a profile README outline

---

## Why This Matters

A large repo library still gets judged through a very small number of first impressions.

Common failures:

- The six pinned repos are individually good but collectively incoherent.
- A visitor sees side projects, old experiments, and utilities but cannot tell what kind of work you want to be hired for.
- The profile README reads like a resume summary instead of a project-navigation surface.
- A pinned repo still has a weak description, so the profile sends the wrong signal anyway.
- The profile exposes more personal or client-specific detail than intended because nobody reviewed it as a public page.
- A strong repo library exists, but the landing page does not guide anyone into it.

Your GitHub profile is not the whole portfolio. It is the top of the funnel.

---

## Best-Practice Principles

1. **Pin a story, not six favorites.**  
   The set should answer what you build, how you think, and what kind of work you do now.

2. **Use the first screen for orientation.**  
   A visitor should quickly understand your focus, not search across dozens of repos.

3. **Choose breadth with intent.**  
   The pins should show different kinds of proof: flagship system, documentation quality, operations discipline, security awareness, or reusable tooling.

4. **Keep profile claims aligned with repo evidence.**  
   If the README says one thing and the pinned repos show another, the profile loses credibility.

5. **Remember the page is public.**  
   GitHub's profile docs note that public profile details are visible to all users. Do not include personal or client-sensitive details casually.

6. **Reorder the pins intentionally.**  
   The top-left slot should usually hold the clearest flagship repo.

7. **Review the profile as a landing page.**  
   The question is not "Do I like these repos?" The question is "Does this profile help the right person know where to click next?"

---

## Concepts

### Profile Positioning Brief

A short internal document that explains:

- the audience
- the story the profile should tell
- which repos support that story
- what the profile README should say

### Pin Set

The limited group of repositories or gists highlighted on the profile.

GitHub allows up to six pinned repositories and gists combined.

### Flagship Repo

The repo that best represents the kind of work you most want to be known for now.

### Profile README

A special public README shown on the profile page when the user has a public repository whose name matches the username and contains a root `README.md`.

### Public Signal

Anything on the profile that shapes how strangers interpret the work.

Examples:

- bio
- profile README
- pinned items
- repo descriptions

---

## Profile Positioning Brief Template

Use this structure:

```markdown
# GitHub Profile Positioning Brief

## Primary Audience

- Potential clients:
- Technical collaborators:
- Recruiters or peers:

## Profile Goal

What should a first-time visitor understand within 30 seconds?

## Core Story

What is the short sentence that the profile should prove?

## Pin Set

| Position | Repo | Why it is pinned | What proof it provides |
| --- | --- | --- | --- |
| 1 |  |  |  |

## Profile README Outline

- Opening line:
- What I build:
- Proof points:
- Where to start:
- How to contact or continue:

## Public Risk Notes

- What should stay off the profile:
- What should be sanitized:
```

---

## Walkthrough

Use this example:

> An AI systems architect wants the profile to attract client work around AI workflow architecture, production operations, and technical leadership.

### Step 1 - Define the audience before choosing the pins

Weak approach:

> Pick the repos with the most stars.

Better approach:

> Pick the repos that best explain the kind of work you want to be contacted about.

Possible audiences:

- clients looking for workflow architecture judgment
- technical collaborators evaluating engineering standards
- peers looking for reusable patterns

These audiences overlap, but not perfectly. The profile should choose a priority.

### Step 2 - Build the proof set

A strong six-pin set might include:

1. flagship AI systems repo
2. production or infrastructure repo
3. evaluation or observability repo
4. documentation-heavy repo
5. tool or automation repo
6. teaching or framework repo that explains your approach clearly

The point is not variety for its own sake. The point is coverage of the story.

### Step 3 - Put the flagship first

GitHub lets you reorder pinned items. Use that.

Position 1 should usually be:

- the clearest current proof
- not the most nostalgic repo
- not the repo that requires the most context to understand

### Step 4 - Write the profile README as a guide

GitHub displays a profile README when:

- the repository name matches your username
- the repository is public
- it has a root `README.md`

Good profile README elements:

- one-line positioning
- what kinds of systems you build
- where to start in the pinned repos
- one or two operating principles

Weak profile README:

> Long biography, many badges, no clear repo path.

Better profile README:

```markdown
# AI Systems Architect

I design and build AI workflow systems with strong operational boundaries: architecture, evaluation, observability, security, and client-safe delivery.

Start here:
- [Flagship repo] for the clearest current system
- [Operations repo] for production discipline
- [Curriculum repo] for how I think and document systems
```

### Step 5 - Review the public risk

GitHub's profile page is public by default for the information you choose to expose there.

Check for:

- client names that should not be public
- personal details that add risk but no value
- pinned repos that expose unfinished or misleading work
- README text that overstates current activity

---

## Practice

Create a GitHub profile positioning brief for your own account.

Include:

1. primary audience
2. 30-second profile goal
3. one-sentence core story
4. six pinned repos in order
5. why each repo is pinned
6. a profile README outline
7. public risk notes

Then apply this test:

- If a potential client saw only your profile page and the pinned repo descriptions, what wrong conclusion might they draw?
- Adjust the pin set or README to remove that confusion.

---

## Review Checklist

Your profile positioning brief is acceptable when:

- The audience is explicit.
- The six pinned items tell a coherent story.
- The first pinned item is intentional.
- The profile README outline tells visitors where to start.
- Each pin has a stated reason.
- Public risk notes exist.
- The profile story matches the repos you actually have.
- A stranger could identify your focus quickly.

---

## Common Failure Modes

- **Favorites, not story:** The pins are individually good but collectively random.
- **Star bias:** Old popular repos outweigh current strategic work.
- **README drift:** The profile README says one thing while the pinned repos prove another.
- **No ordering logic:** The strongest proof is buried.
- **Public oversharing:** Unnecessary personal or client detail appears on the profile.
- **No call path:** A visitor sees the page but does not know where to start.

---

## Portfolio Evidence

Save:

- The profile positioning brief
- The ordered six-pin set
- The profile README outline

This shows that you can position technical work intentionally, not just publish it.

---

## References

- GitHub Docs, About your profile: https://docs.github.com/en/account-and-profile/concepts/personal-profile
- GitHub Docs, Managing your profile README: https://docs.github.com/en/account-and-profile/how-tos/profile-customization/managing-your-profile-readme
- GitHub Docs, Pinning items to your profile: https://docs.github.com/en/account-and-profile/how-tos/profile-customization/pinning-items-to-your-profile
