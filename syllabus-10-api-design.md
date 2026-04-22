# Syllabus 10: API Design with FastAPI
**Status:** Enterprise Gap - The "Bridge" Between Your Workflows and the World
**Format:** One chat session per unit, self-paced

---

## What This Is

You already know how to call APIs. This syllabus teaches you how to build them - so a client's website, CRM, or Slack workspace can trigger your AI workflow directly, without anyone needing to run a script manually.

---

## Unit 1 - What an API Is When You Are the Builder

**Goal:** Shift from API consumer to API designer.

**Topics:**
- The difference between calling an API and building one
- What REST means - a standard way for software to talk to software over the web
- Why wrapping your AI workflow in an API makes it a product, not a script
- Real examples: a client's CRM calls your API, your workflow runs, the result comes back

**Session starter:**
> "Explain what it means to build an API rather than just call one. What is REST? Give me a concrete example of how a client's existing software could call my AI workflow through an API I build. Why does this make my work more valuable?"

---

## Unit 2 - FastAPI Fundamentals

**Goal:** Build a working API endpoint in Python using FastAPI.

**Topics:**
- What FastAPI is and why it is the right choice for AI workflow APIs
- Creating your first endpoint - a URL that accepts a request and returns a response
- Request and response models using Pydantic
- Running and testing locally before deploying

**Session starter:**
> "Teach me FastAPI fundamentals. How do I create an endpoint that accepts a text input, passes it to a Claude API call, and returns the result? Walk me through the full working example including request and response models with Pydantic."

---

## Unit 3 - Wrapping an Agentic Workflow in an API

**Goal:** Take a multi-step AI workflow and expose it through a clean API endpoint.

**Topics:**
- How to handle long-running workflows - async endpoints and background tasks
- Returning a job ID so the client can check back for results
- Webhooks vs. polling - how the client gets notified when the job is done
- When a chat-like interface needs token streaming instead of waiting for one complete response
- Streaming Claude output through FastAPI using Server-Sent Events (SSE)
- When WebSockets are better than SSE for bidirectional chat or live collaboration
- Handling disconnects, cancellation, partial output, and stream errors safely
- Error handling that gives the client useful information without exposing internals

**Session starter:**
> "Help me wrap a multi-step agentic AI workflow in a FastAPI endpoint. The workflow takes more than a few seconds to run. How do I choose between polling, webhooks, SSE streaming, and WebSockets? Show me how to stream Claude tokens through FastAPI for a chat-like interface and how to handle disconnects safely."

---

## Unit 4 - Security and Authentication

**Goal:** Make sure only authorized clients can call your API.

**Topics:**
- API keys - how to issue them and validate them on every request
- Rate limiting - preventing a single client from overwhelming your workflow
- HTTPS and why unencrypted APIs are not acceptable for client work
- What to log for every API call without logging sensitive data

**Session starter:**
> "Help me add authentication and security to a FastAPI AI workflow API. I need API key validation, rate limiting, and HTTPS. What do I log for every request without storing sensitive client data? Walk me through the implementation."

---

## Unit 5 - Documentation That Clients Can Actually Use

**Goal:** Produce API documentation automatically and make it readable for non-technical clients.

**Topics:**
- FastAPI's automatic docs (Swagger UI and ReDoc) - what they produce and how to customize them
- Writing endpoint descriptions that a non-developer can understand
- How to write a plain-English quickstart guide alongside the technical docs
- Versioning your API so client integrations do not break when you make changes

**Session starter:**
> "Help me write API documentation for a FastAPI AI workflow that will be read by both developers and non-technical clients. What does FastAPI generate automatically? How do I customize it? How do I add a plain-English quickstart? How do I handle versioning?"

---

## Unit 6 - Workflow API Review Package

**Goal:** Turn the boundary, endpoint, runtime, security, and documentation decisions into one reviewable API package that supports prototype, limited rollout, redesign, or hold.

**Topics:**
- Combining the boundary brief, endpoint contract, transport choice, auth policy, and client docs into one reviewable packet
- Checking that async behavior, status codes, auth rules, and quickstart guidance do not contradict each other
- Writing rollout blockers and a final readiness decision
- Naming the future changes that force a re-review

**Session starter:**
> "Help me assemble a workflow API review package for one of my AI services. I want the API boundary, FastAPI endpoint contract, async or streaming decision, auth policy, docs pack, and a final rollout recommendation in one coherent packet."

---

## Practice Project

Wrap one of your existing Python AI workflows in a FastAPI service and turn it into a reviewable workflow API package that includes:

- an API boundary brief and endpoint inventory
- at least one FastAPI endpoint with explicit request and response models
- API key or token-based authentication and rate-limit rules
- one long-running or streaming decision using polling, webhooks, SSE, or WebSockets
- a one-page plain-English quickstart guide for a non-technical client
- a final rollout decision with blockers, assumptions, and next steps

Deploy it using what you learned in Syllabus 13.
