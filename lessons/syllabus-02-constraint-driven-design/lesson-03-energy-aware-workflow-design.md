# Lesson 02.03: Energy-Aware Workflow Design

**Parent syllabus:** [Syllabus 02: Constraint-Driven Design](../../syllabus-02-constraint-driven-design.md)  
**Estimated time:** 2 hours  
**Artifact:** Energy mode map

---

## Outcome

By the end of this lesson, you can design workflows that scale down gracefully when energy is low and scale up when energy is available.

You will produce an energy mode map with high-energy, normal, low-energy, and emergency modes.

---

## Why This Matters

Many systems assume the operator has stable energy, attention, and time. Real work does not.

Common failures:

- The workflow has only one mode, and it is too demanding.
- Low-energy days turn into skipped work.
- Emergency mode is invented during the emergency.
- The system requires a complex setup before producing value.
- Users cannot tell what can safely wait.
- The workflow keeps asking for effort after the useful part is already done.

Energy-aware design makes workflows resilient under changing human capacity.

---

## Best-Practice Principles

1. **Build the low-energy mode first.**  
   If the simplest version works, higher-energy versions can add depth.

2. **Separate essential from optional.**  
   The workflow should make clear what must happen and what can wait.

3. **Use mode-specific definitions of done.**  
   Low-energy done is not the same as high-energy done.

4. **Make emergency mode safe.**  
   Emergency mode should reduce scope, avoid irreversible actions, and preserve context for later.

5. **Use defaults aggressively.**  
   Low-energy mode should ask fewer questions and use safer defaults.

6. **Design escalation paths.**  
   If the system cannot safely continue, it should save state and tell the user who or what needs review.

7. **Avoid shame in the interface.**  
   Mode names and instructions should support action, not imply failure.

---

## Concepts

### Energy Mode

A version of the workflow designed for a specific capacity level.

### Low-Energy Default

The simplest safe version of the workflow, used as the starting design.

### Emergency Mode

The minimum safe workflow when time, energy, or risk pressure is high.

### Optional Depth

Extra review, polish, analysis, or optimization that can happen when energy allows but is not required for basic progress.

### Save State

The information needed to resume later without reconstructing context.

---

## Walkthrough

Use this workflow:

> Turn a client call into a follow-up plan.

### Mode map

| Mode | Use when | Goal | Definition of done |
| --- | --- | --- | --- |
| Emergency | Very low energy or urgent deadline | Capture state and avoid dropping the ball | Send or save one safe next-action note |
| Low-energy | Tired but functional | Extract facts and one next action | Facts, open questions, next step saved |
| Normal | Usual working capacity | Draft follow-up plan | Draft reviewed and ready to send |
| High-energy | Good focus and time | Improve system and artifacts | Draft, scope risks, case-study notes, reusable prompt updates |

### Emergency mode

```text
Input:
- Raw notes or transcript

Steps:
1. Ask AI to extract the client name, problem, deadline, and one next action.
2. If anything important is missing, ask one clarifying question.
3. Save the resume note.

Do not:
- Draft a full proposal.
- Quote price.
- Make promises.
- Send final client-facing language without review.
```

### Low-energy mode

```text
Steps:
1. Extract stated facts.
2. Identify missing information.
3. Recommend one next action.
4. Save a short follow-up draft as "needs review."
```

### Normal mode

```text
Steps:
1. Extract facts.
2. Identify scope risks.
3. Draft follow-up.
4. Review assumptions.
5. Send or schedule follow-up.
```

### High-energy mode

```text
Steps:
1. Complete normal mode.
2. Update reusable prompts if needed.
3. Add case-study notes.
4. Improve the workflow checklist.
```

---

## Practice

Pick one workflow that matters to your work.

Create an energy mode map:

1. **Workflow name**
2. **Emergency mode**
3. **Low-energy mode**
4. **Normal mode**
5. **High-energy mode**
6. **Definition of done for each mode**
7. **What each mode must not do**
8. **What gets saved for later**
9. **When to escalate to human review**

Then rewrite the workflow so low-energy mode is the default path.

---

## Review Checklist

Your mode map is acceptable when:

- Low-energy mode works without high-energy setup.
- Emergency mode is safe and small.
- Each mode has a definition of done.
- Each mode says what not to do.
- Optional depth is separated from essential work.
- Resume state is explicit.
- Human review is required before risky action.
- The workflow can still produce value when energy is low.

---

## Common Failure Modes

- **High-energy default:** The system starts with the most demanding version.
- **Emergency chaos:** The emergency path is not written down.
- **No done line:** Low-energy work expands until it becomes too much.
- **Unsafe shortcuts:** Emergency mode skips review on risky actions.
- **Mode shame:** Labels make the user feel like they failed.
- **No saved state:** Lower modes capture output but not context for later.

---

## Portfolio Evidence

Save:

- Energy mode map
- Low-energy default workflow
- Emergency mode checklist
- One sample resume note

This shows that your systems are designed for real operating conditions, not ideal ones.

---

## References

- W3C Accessibility Fundamentals: https://www.w3.org/WAI/fundamentals/
- Google SRE Book, Service Level Objectives: https://sre.google/sre-book/service-level-objectives/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
