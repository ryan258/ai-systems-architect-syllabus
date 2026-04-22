# Syllabus 15: Infrastructure as Code and CI/CD
**Status:** Production Spine - Repeatable Delivery and Change Control
**Format:** One chat session per unit, self-paced

---

## What This Is

Once a service is deployed and instrumented, the next question is whether changes can move safely and repeatably from laptop to production. This syllabus is the delivery-control layer in the course path.

It turns deployment from a sequence of remembered steps into reviewed files, enforced checks, rollout gates, and recovery rules. It assumes one deployable service and prepares the ground for the distributed runtime patterns in Syllabus 16.

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

## Unit 6 - Infrastructure as Code and CI/CD Review Package

**Goal:** Combine the delivery artifacts into one reviewer-ready package.

**Topics:**
- Assembling environment design, Terraform posture, CI checks, deployment gates, rollback rules, and config controls into one packet
- Checking that promotion rules, infrastructure files, and release expectations do not contradict each other
- Writing delivery blockers and a final readiness decision
- Naming the changes that should force a re-review of the delivery path

**Session starter:**
> "Help me assemble an infrastructure as code and CI/CD review package for one AI workflow. I want the environment plan, Terraform sketch, CI policy, delivery and rollback rules, config controls, and a final release recommendation in one coherent packet."

---

## Practice Project

Take one deployed workflow that already has reliability signals from Syllabus 14 and turn it into a repeatable delivery package that includes:

- an environment matrix and promotion-flow policy
- a Terraform stack sketch and state policy
- a CI workflow contract with mocked model-call tests
- a deployment-gates and rollback brief
- a config, secrets, and drift-control plan
- a final delivery recommendation with blockers and next steps
