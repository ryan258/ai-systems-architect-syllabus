# Syllabus 15: Infrastructure as Code and CI/CD
**Status:** Enterprise Gap - Repeatable Deployment Without Dashboard Clicking
**Format:** One chat session per unit, self-paced

---

## What This Is

Cloud deployment is the start. Repeatable deployment is the professional version. This syllabus teaches you how to define infrastructure in files, test changes before shipping them, deploy through a pipeline, and roll back when a release goes wrong.

---

## Unit 1 - Environments and Release Flow

**Goal:** Understand how code moves from laptop to production without chaos.

**Topics:**
- Local, development, staging, and production environments
- Why staging exists and when it is worth the overhead
- Environment variables and configuration per environment
- Promotion flow: build once, deploy the same artifact
- Keeping client demos separate from production systems

**Session starter:**
> "Explain environments and release flow for a solo AI systems architect. What should local, dev, staging, and production mean? When is staging worth it? How do I avoid mixing demo and production systems?"

---

## Unit 2 - Terraform and Infrastructure as Code

**Goal:** Define cloud resources in version-controlled files instead of clicking through dashboards.

**Topics:**
- What infrastructure as code means
- Terraform basics: providers, resources, variables, state, and plans
- Reviewing infrastructure changes before applying them
- Avoiding drift between what is deployed and what is documented
- Choosing the minimum useful Terraform setup for small client projects

**Session starter:**
> "Teach me Terraform for small AI workflow deployments. Explain providers, resources, variables, state, plans, and drift. Show me the minimum useful setup for deploying a containerized FastAPI service."

---

## Unit 3 - CI for AI Workflow Repos

**Goal:** Automatically test every change before it can break a client workflow.

**Topics:**
- GitHub Actions basics for Python projects
- Running unit tests, lint checks, and type checks
- Mocking model calls so CI stays fast and cheap
- Secret handling in CI
- Required checks before merging

**Session starter:**
> "Help me set up GitHub Actions CI for an AI workflow repo. I want tests, linting, type checks, mocked model calls, and safe secret handling. Keep it simple enough to reuse across projects."

---

## Unit 4 - CD, Deployment Gates, and Rollbacks

**Goal:** Deploy changes predictably and recover quickly when something goes wrong.

**Topics:**
- Continuous delivery vs. continuous deployment
- Manual approval gates for production
- Blue-green, canary, and simple versioned rollbacks
- Database and vector index changes during deploys
- Writing a deployment checklist that is short enough to use

**Session starter:**
> "Teach me continuous delivery for AI systems. I want a practical pipeline with staging, manual approval for production, rollback instructions, and special care for database or vector index changes."

---

## Unit 5 - Config, Secrets, and Drift Control

**Goal:** Keep deployment configuration understandable and recoverable over time.

**Topics:**
- Configuration files vs. environment variables vs. secret stores
- Rotating secrets without breaking production
- Detecting infrastructure drift
- Backing up Terraform state
- Documenting how to recreate the system from scratch

**Session starter:**
> "Help me design config, secrets, and drift control for small AI systems. I need to know where settings live, how secrets rotate, how to detect drift, and how to recreate the system if something breaks."

---

## Practice Project

Take one FastAPI or Python AI workflow. Add a GitHub Actions CI workflow with mocked model-call tests. Write a Terraform sketch for its deployment resources. Create a release checklist with staging verification, production approval, and rollback steps.
