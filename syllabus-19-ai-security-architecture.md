# Syllabus 19: AI Security Architecture
**Status:** Enterprise Gap - Designing for Adversarial Inputs
**Format:** One chat session per unit, self-paced

---

## What This Is

AI security is not just prompt injection. A real AI system has users, identities, permissions, secrets, tools, data stores, dependencies, logs, and human approval paths. This syllabus teaches you how to design AI systems as if every input, integration, and tool call could be abused.

---

## Unit 1 - Threat Modeling an AI System

**Goal:** Identify how an AI system can be attacked before it goes live.

**Topics:**
- What threat modeling means in plain language
- Assets, actors, trust boundaries, and attack paths
- Threats unique to AI workflows: prompt injection, tool misuse, data exfiltration, and overreliance
- Drawing a data-flow diagram for security review
- Turning threats into concrete mitigations

**Session starter:**
> "Teach me how to threat model an AI system. Walk me through assets, actors, trust boundaries, data flows, attack paths, and mitigations using a RAG workflow with tool access as the example."

---

## Unit 2 - Identity, Authentication, and Authorization

**Goal:** Make sure the right users and systems can do only what they are allowed to do.

**Topics:**
- Authentication vs. authorization in plain language
- API keys, OAuth, OIDC, JWTs, and when each is appropriate
- Role-based access control for clients, admins, and operators
- Scoping permissions for tools and data sources
- Designing least-privilege access for AI agents

**Session starter:**
> "Help me design authentication and authorization for an AI workflow API. Explain API keys, OAuth, OIDC, JWTs, RBAC, and least privilege in plain language, then show me how to choose the right approach."

---

## Unit 3 - OWASP Risks for AI Applications

**Goal:** Learn the major security risks that appear repeatedly in LLM and web applications.

**Topics:**
- Prompt injection and indirect prompt injection
- Insecure output handling
- Sensitive information disclosure
- Excessive agency and unsafe tool use
- Supply-chain, dependency, and model risks
- How the general web security risks still apply to AI systems

**Session starter:**
> "Walk me through the major OWASP risks for LLM applications and normal web applications. Translate each risk into concrete design rules for an AI workflow with API access, tools, and client data."

---

## Unit 4 - Secrets, Sandboxes, and Supply Chain

**Goal:** Prevent keys, tools, dependencies, and runtime environments from becoming the weak point.

**Topics:**
- Secrets management for API keys, database credentials, and tokens
- Why secrets do not belong in code, prompts, logs, or client documents
- Sandboxing tool execution and file access
- Dependency scanning and lockfiles
- Separating dev, staging, and production credentials

**Session starter:**
> "Help me design secrets management, sandboxing, and supply-chain safety for an AI system. I want to avoid leaked API keys, unsafe tool access, dependency risk, and accidental production credential use."

---

## Unit 5 - Audit Logs and Security Review Checklists

**Goal:** Keep enough evidence to understand what happened without storing sensitive data carelessly.

**Topics:**
- What to include in audit logs
- Tracking who requested what, which tools ran, and which data sources were accessed
- Redaction and retention rules
- Security review before launch
- Periodic access review after launch

**Session starter:**
> "Help me build an audit logging and security review checklist for AI systems. I need to know who did what, which tools ran, and which data was accessed, but I do not want to store sensitive data unnecessarily."

---

## Practice Project

Pick one AI workflow that reads client data or calls tools. Draw its trust boundaries and data-flow diagram. Write five realistic threats, one mitigation for each, a least-privilege permission plan, and a launch security checklist.
