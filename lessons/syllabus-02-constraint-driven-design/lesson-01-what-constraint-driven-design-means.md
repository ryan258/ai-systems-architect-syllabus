# Lesson 02.01: What Constraint-Driven Design Actually Means

**Parent syllabus:** [Syllabus 02: Constraint-Driven Design](../../syllabus-02-constraint-driven-design.md)  
**Estimated time:** 90-120 minutes  
**Artifact:** One-sentence definition and proof examples

---

## Outcome

By the end of this lesson, you can explain constraint-driven design in one clear sentence, support it with real examples, and distinguish it from generic accessibility, productivity, or UX language.

You will produce a definition, three proof examples, and a short explanation suitable for a client or portfolio page.

---

## Why This Matters

If you cannot explain your design philosophy clearly, people will misunderstand it.

Common failures:

- It sounds like a personal coping strategy instead of a professional design method.
- It gets reduced to "accessibility" even when it is broader than that.
- It sounds negative because the word "constraint" is heard as limitation only.
- Clients do not understand why constraints should shape architecture.
- The idea stays abstract because there are no concrete examples.

Constraint-driven design becomes powerful when it is specific, useful, and easy to repeat.

---

## Best-Practice Principles

1. **Treat constraints as requirements.**  
   A constraint is not an obstacle to work around after design. It is part of the design brief.

2. **Use plain language.**  
   If the definition needs a long explanation, it is not ready.

3. **Make it operational.**  
   The definition should imply what you do differently: fewer decisions, better defaults, lower context switching, safer handoffs, clearer stopping points.

4. **Show proof through examples.**  
   A philosophy becomes credible when it explains decisions you have already made.

5. **Frame constraints as quality drivers.**  
   Constraints force clarity, resilience, and maintainability.

6. **Avoid oversharing by default.**  
   You can mention lived experience when useful, but client-facing explanations should focus on design value.

7. **Connect to business outcomes.**  
   Constraint-driven systems are easier to adopt, easier to operate, and less likely to fail under pressure.

---

## Concepts

### Constraint

A real limitation the system must respect.

Examples:

- Low attention
- Limited time
- High interruption
- Regulatory limits
- Sensitive data
- Limited budget
- Existing tools
- Small team
- Bad network
- Manual approval requirements

### Design Requirement

A condition the system must satisfy.

Example:

> The workflow must be usable when the operator has only enough energy to make one decision.

### Constraint-Driven Design

A design approach that starts with real limits and uses them to shape simpler, safer, more usable systems.

### Proof Example

A concrete case where a constraint changed the design.

Example:

> Because users were overwhelmed by long AI outputs, the workflow returns one next action first and stores the detailed explanation separately.

---

## Walkthrough

Start with a weak definition:

> Constraint-driven design means designing around limitations.

Problems:

- It is true but too generic.
- It does not say what changes in the system.
- It sounds like compromise.

Better definition:

> Constraint-driven design means treating real limits like energy, attention, tools, time, risk, and budget as design requirements from the start, so the system is easier to use when conditions are not ideal.

This is stronger because it:

- Names real constraints.
- Frames them as requirements.
- Explains the outcome.
- Applies beyond personal productivity.

### Proof example 1 - Low-energy workflow

Constraint:

> The user may not have enough focus to choose between many options.

Design decision:

> The workflow starts with one default path and only asks for choices when the answer changes the outcome.

Business value:

> The workflow gets used consistently instead of being abandoned on difficult days.

### Proof example 2 - Client adoption

Constraint:

> The client already works in Slack and will not reliably open a new dashboard.

Design decision:

> The AI workflow sends review prompts into the existing Slack channel instead of requiring a new portal.

Business value:

> Adoption improves because the system fits existing behavior.

### Proof example 3 - Risk control

Constraint:

> The AI may draft content that affects customers.

Design decision:

> The system drafts but does not send. Human approval is required before external action.

Business value:

> Speed improves without giving up accountability.

---

## Practice

Create your first constraint-driven design statement.

Write:

1. **One-sentence definition**
2. **Three constraints you design around**
3. **Three proof examples from your work**
4. **One client-facing paragraph**
5. **One version for a portfolio page**

Use this proof-example format:

```text
Constraint:

Design decision:

Why it worked:

Business or user value:
```

---

## Review Checklist

Your artifact is acceptable when:

- The definition is one sentence.
- The definition does not require personal health disclosure to make sense.
- At least three constraints are named.
- Each proof example links a constraint to a design decision.
- The client-facing paragraph explains business value.
- The portfolio version sounds professional, not defensive.
- A non-technical person could repeat the core idea.

---

## Common Failure Modes

- **Too abstract:** The idea sounds smart but does not affect design.
- **Too personal:** The explanation depends entirely on your personal story.
- **Too negative:** Constraints sound like limitations instead of design inputs.
- **Too broad:** Every good design principle gets absorbed into one vague concept.
- **No proof:** There are no examples showing the method in action.
- **No business value:** Clients cannot tell why they should care.

---

## Portfolio Evidence

Save:

- One-sentence definition
- Three proof examples
- Client-facing paragraph
- Portfolio paragraph

This becomes the language you reuse in proposals, case studies, and positioning.

---

## References

- W3C Accessibility Fundamentals: https://www.w3.org/WAI/fundamentals/
- PlainLanguage.gov guidelines: https://www.plainlanguage.gov/guidelines/
- NIST AI Risk Management Framework: https://www.nist.gov/itl/ai-risk-management-framework
