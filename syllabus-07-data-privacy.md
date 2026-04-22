# Syllabus 07: Data Privacy and Client Safety
**Status:** Hidden Gap - Protects You and Your Clients
**Format:** One chat session per unit, self-paced

---

## What This Is

When you build AI workflows for clients, you often touch their data. This syllabus teaches you what questions to ask, what to avoid, and how to protect yourself legally and ethically.

This is not a law degree. It is the minimum you need to work safely.

---

## Unit 1 - What Data Privacy Actually Means

**Goal:** Understand what client data is and why handling it wrong creates real risk.

**Topics:**
- Personal data means anything that can identify a person
- Why putting client data into AI tools without permission is a problem
- The difference between data you own and data you're trusted with

**Session starter:**
> "Explain data privacy in plain language for a solo AI consultant. What is personal data? Why does it matter how I handle it? What can go wrong if I'm careless?"

---

## Unit 2 - What to Ask Before Touching Client Data

**Goal:** Build a short checklist of questions you ask every client before starting.

**Topics:**
- Do they have sensitive customer information?
- What tools are they comfortable with?
- Have they agreed to let their data be used in AI tools?
- Who owns the output?
- Whether SOC 2, HIPAA, the EU AI Act, or another compliance framework may apply
- Who at the client can answer security questionnaire questions accurately

**Session starter:**
> "Help me build a data privacy checklist for new clients. These are the questions I ask before I start any work that involves their files or information. Include practical questions that reveal whether SOC 2, HIPAA, the EU AI Act, or a corporate security questionnaire may apply."

---

## Unit 3 - What NOT to Put Into AI Tools

**Goal:** Know what types of information should never go into Claude, ChatGPT, or other AI tools.

**Topics:**
- Social security numbers, passwords, financial account details
- Private medical information
- Children's data
- Confidential business information without permission
- Protected health information (PHI) and when HIPAA may apply because the client is a covered entity or business associate
- Regulated, high-risk, or jurisdiction-specific data that needs professional review before automation

**Session starter:**
> "Give me a plain-language list of what should never be put into an AI tool when working with client data. Include private medical information, PHI, children's data, confidential business data, and regulated data. Why is each category risky, and what do I do if a client sends me something sensitive by mistake?"

---

## Unit 4 - Simple Protective Language for Contracts

**Goal:** Know what to include in a client agreement to protect yourself.

**Topics:**
- A data handling clause (what you will and won't do with their data)
- Who owns the AI-generated output
- What happens if there is a data problem
- Plain-English notes about security controls, vendor data use, retention, and deletion
- How to answer corporate IT questions without pretending to be a SOC 2, HIPAA, or EU AI Act auditor

**Session starter:**
> "Help me write simple data handling language I can include in a client agreement. I am not a lawyer, so make it plain English. Include security controls, vendor data use, retention, deletion, and how to answer corporate IT questions without overstating my compliance status."

---

## Unit 5 - What to Do If Something Goes Wrong

**Goal:** Know your basic steps if a data mistake happens.

**Topics:**
- Tell the client immediately - do not wait
- Document what happened
- What not to say that could create legal problems
- When to get professional help
- When an incident may trigger contractual, HIPAA, SOC 2-related customer commitments, or EU AI Act escalation obligations

**Session starter:**
> "Walk me through what I should do if I accidentally expose client data or make a data handling mistake. What are the steps? What do I say to the client? When might HIPAA, SOC 2-related customer commitments, the EU AI Act, or a contract require escalation? When do I need a lawyer or compliance professional?"

---

## Unit 6 - Prompt Injection and Active AI Security

**Goal:** Build AI systems that treat every user input as a potential attack.

**Topics:**
- What prompt injection means - when hidden instructions inside user input hijack the AI
- How to separate system instructions from user input safely
- Input filtering before content reaches the model
- Output filtering to catch cases where manipulation succeeded
- Designing with the assumption that all inputs could be adversarial

**Session starter:**
> "Teach me how to defend against prompt injection in AI workflows I build for clients. How do I separate system instructions from user input? What filters should I build on input and output? I want to treat every input as potentially adversarial."

---

## Unit 7 - Human Handoff Design

**Goal:** Build AI workflows that pause and ask for human approval before taking major actions.

**Topics:**
- Identifying decision points where the AI must never proceed alone
- Building a pause-and-approve mechanism
- Designing the review interface for low-energy days - minimal reading, clear choices
- Requiring approval before any irreversible action
- Logging handoff reasons to improve the system over time

**Session starter:**
> "Help me design a human handoff system for an AI workflow. The AI should pause before major or irreversible actions and request my approval. The review must work on my worst brain fog days - clear, minimal, and fast. Show me the design pattern."

---

## Unit 8 - Client Data Safety Review Package

**Goal:** Turn the module's privacy, security, contract, incident, and handoff artifacts into one reviewer-ready packet before client data moves.

**Topics:**
- Combining the data map, intake checklist, exclusion matrix, clause pack, incident plan, prompt injection controls, and handoff policy into one package
- Checking that approved paths, exclusions, and reviewer roles do not contradict each other
- Surfacing blockers, missing approvals, and scope limits as real decision inputs
- Writing a final proceed, narrow, wait, or stop recommendation
- Defining the change triggers that force a re-review later

**Session starter:**
> "Help me assemble a client data safety review package for one AI workflow. I want the data map, intake blockers, exclusion rules, contract language, incident plan, prompt injection defenses, human handoff design, and final recommendation in one coherent packet."

---

## Practice Project

Choose one workflow you have built, designed, or want to build next. Create a client data safety review package that includes:

- a client data classification and handling map
- a pre-engagement intake checklist and decision log
- an AI tool exclusion and escalation matrix
- plain-English data handling and questionnaire language
- a data incident response packet
- a prompt injection defense specification
- a human handoff policy
- a final proceed, narrow, wait, or stop decision
