# Syllabus 14: Cloud Infrastructure and Deployment
**Status:** Enterprise Gap — The "Where" Your Workflows Live
**Format:** One chat session per unit, self-paced

---

## What This Is

Your AI workflows currently run on your laptop. This syllabus teaches you how to deploy them to the cloud so they run 24/7, trigger automatically, and do not depend on your machine being on or your energy being available.

---

## Unit 1 — Why Cloud Deployment Matters for Architects

**Goal:** Understand what changes when a workflow moves from your laptop to production.

**Topics:**
- Why "it works on my machine" is not a deliverable
- What 24/7 availability actually requires
- The difference between a script and a deployed service
- How cloud deployment connects to your constraint-driven philosophy — systems that run without you

**Session starter:**
> "Explain what changes when an AI workflow moves from a local Python script to a cloud deployment. What does 24/7 availability actually require? Why is this the line between a prototype and a production system?"

---

## Unit 2 — Containers and Docker Basics

**Goal:** Understand what Docker is and how to put a Python AI workflow inside one.

**Topics:**
- What a container is — a portable box that holds your code and everything it needs to run
- Writing a basic Dockerfile for a Python AI workflow
- How containers eliminate "works on my machine" problems
- Building and running your first container locally before deploying it

**Session starter:**
> "Teach me Docker from the perspective of an AI workflow architect. What is a container? How do I write a Dockerfile for a Python script that calls the Claude API? Walk me through building and running it locally first."

---

## Unit 3 — Serverless Deployment

**Goal:** Deploy a containerized AI workflow to the cloud so it triggers automatically without a running server.

**Topics:**
- What serverless means — code that runs only when triggered, costs nothing when idle
- Google Cloud Run vs. AWS Lambda — when to use each
- How webhooks trigger your workflow from external tools
- Environment variables and secrets management in the cloud

**Session starter:**
> "Help me deploy a Python AI workflow to Google Cloud Run or AWS Lambda. I want it to trigger via webhook. Explain serverless in plain terms first, then walk me through the deployment steps. Include how to handle API keys safely in the cloud."

---

## Unit 4 — Making Deployments Low-Energy Friendly

**Goal:** Design cloud deployments that require minimal maintenance on bad days.

**Topics:**
- Infrastructure as code — defining your deployment in a file so you never click through dashboards
- Auto-restart and health checks so the system recovers without you
- Alerts that tell you something broke without requiring you to watch dashboards
- Keeping deployments simple enough to understand when foggy

**Session starter:**
> "Help me design a cloud deployment for an AI workflow that requires minimal ongoing maintenance. I want auto-restart, health checks, and alerts — but simple enough that I can understand what is happening on a bad MS day. What is the minimum viable cloud setup?"

---

## Unit 5 — Your First End-to-End Cloud Deployment

**Goal:** Take one existing workflow from local script to live cloud deployment.

**Topics:**
- Choosing the right workflow to deploy first
- The full path: Dockerfile → container build → cloud deploy → webhook test
- Verifying it works without touching your laptop
- Writing a one-page runbook (a plain-English document describing how to restart or fix it)

**Session starter:**
> "Walk me through deploying one of my AI workflows end-to-end to the cloud. Start with Docker, move to Cloud Run or Lambda, set up a webhook trigger, and help me write a simple runbook so I can fix problems on a bad day without thinking too hard."

---

## Practice Project

Pick one Python AI workflow you have already built. Write its Dockerfile. Deploy it to a free tier of Google Cloud Run or AWS Lambda. Trigger it with a test webhook. Write a one-page runbook.
