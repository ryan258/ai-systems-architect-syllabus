# Lesson 13.02: Container Packaging and Docker Basics

**Parent syllabus:** [Syllabus 13: Cloud Deployment](../../syllabus-13-cloud-deployment.md)  
**Estimated time:** 2-3 hours  
**Artifact:** Container packaging specification and Dockerfile

---

## Outcome

By the end of this lesson, you can package a Python AI workflow into a container image with a clear runtime contract so the same artifact can be built, tested, and deployed outside your laptop.

You will produce:

- a container packaging specification
- a Dockerfile
- local build and run rules

---

## Why This Matters

Cloud deployment usually fails before the first request when packaging is vague.

Common failures:

- The image works only because untracked local files were present during build.
- Secrets are copied into the image or committed into the build context.
- The container starts locally but not on the platform because the entrypoint, port, or working directory is wrong.
- The image is huge because build tools, caches, and development artifacts were left inside it.
- Temporary assumptions about filesystem paths become production dependencies.
- There is no documented local run command, so each developer invents a different test path.

Containers are not magic. They are a reproducible runtime contract. If the contract is weak, deployment drift just moves from the laptop to the registry.

---

## Best-Practice Principles

1. **Package the runtime intentionally.**  
   The image should contain what the service needs to run, not the whole workstation.

2. **Keep build inputs explicit.**  
   If the image depends on a file, library, or generated artifact, name it and copy it deliberately.

3. **Do not bake secrets into images.**  
   Secrets should arrive at runtime from the platform, not from the Docker build context.

4. **Treat the image as a deployment artifact, not a debugging scrapbook.**  
   A smaller, clearer image is easier to scan, patch, and trust.

5. **Define the startup command and port contract.**  
   If the service listens on HTTP, the container should say how it starts and where it listens.

6. **Make local container testing part of the workflow.**  
   Build and run the image before cloud deployment.

7. **Keep filesystem assumptions minimal.**  
   Images should not rely on mutable local paths for durable correctness.

---

## Concepts

### Container Image

A packaged filesystem and startup command used to run the service consistently across environments.

### Dockerfile

A declarative build recipe for the image.

### Build Context

The files available to the container build.

Bad build context:

- secrets
- notebooks
- test artifacts
- unrelated datasets

### Runtime Contract

The set of expectations the container has when it starts.

Examples:

- working directory
- startup command
- listening port
- required environment variables

### `.dockerignore`

A file that keeps unnecessary or sensitive files out of the build context.

### Mutable Local Filesystem

Writable container disk that may disappear on restart or scale-out and should not be treated as durable state.

---

## Container Packaging Specification Template

Use this structure:

```markdown
# Container Packaging Specification: [Workflow Name]

## Service Entry Point

- Main command:
- Working directory:
- Expected port:

## Required Files in the Image

- Application code:
- Dependency manifest:
- Static assets, if any:

## Excluded from Build Context

- Secrets:
- Local virtual environments:
- Test fixtures not needed in runtime:
- Large data files not needed in runtime:

## Runtime Environment Variables

- ...

## Local Build and Run Rules

- Build command:
- Run command:
- Smoke test:

## Security and Privacy Notes

- Secret source:
- Logging rule:
- Temporary file rule:
```

Example Dockerfile pattern:

```dockerfile
FROM python:3.12-slim

WORKDIR /app

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PORT=8080

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app ./app

CMD ["python", "-m", "uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
```

Example `.dockerignore` starter:

```text
.git
.venv
__pycache__
.pytest_cache
.env
*.log
tests/
```

Review questions:

- What files actually enter the image?
- What runtime variables are required?
- Which files must never enter the build context?

---

## Walkthrough

Use this example:

> A support drafting FastAPI service receives ticket webhooks, calls an LLM, and writes review-ready output to an external queue.

### Step 1 - Define what the image must contain

This service needs:

- application code
- dependency manifest
- startup command

It does not need:

- local credentials
- notebooks
- `.venv`
- test snapshots
- exported ticket data

If the image includes those anyway, the packaging boundary is already weak.

### Step 2 - Write the runtime contract

For this service, the contract is simple:

- start one web process
- listen on the service port
- read secrets from environment variables or the platform secret source
- send logs to standard output

That contract should be readable before anyone runs `docker build`.

### Step 3 - Keep the image small and boring

Weak packaging pattern:

> Copy the whole repository, install everything, and debug it later.

Better pattern:

- copy the dependency manifest first
- install only runtime dependencies
- copy only the service code
- exclude local development artifacts

Simple packaging improves speed, security review, and patching.

### Step 4 - Protect secrets and private data

Do not:

- copy `.env` into the image
- use a build argument for long-lived secrets
- store private input data in the image for convenience

Better:

- inject secrets at runtime
- keep sample data redacted
- log request IDs and statuses, not raw sensitive bodies

### Step 5 - Test the container locally

Before any cloud deploy:

- build the image
- run it locally with non-production settings
- hit the health or root endpoint
- confirm that logs, startup, and required environment variables behave as expected

Local container testing is the last easy place to catch packaging mistakes.

### Step 6 - Keep durable state outside the container

If the support drafting service needs durable queue state, review state, or uploaded file storage, those should live outside the container.

The image should assume:

- process restarts happen
- scale-out may create multiple instances
- local filesystem contents may be lost

Containers are runtime envelopes, not durable application databases.

---

## Practice

Choose one Python AI workflow or API you want to deploy.

Create:

1. a container packaging specification
2. a Dockerfile
3. a `.dockerignore` starter
4. a local build and run path

Minimum requirements:

- one explicit startup command
- one port contract
- one list of excluded build-context files
- one rule for runtime secret injection

---

## Review Checklist

Your container packaging artifact is acceptable when:

- The image contents are deliberate.
- The startup command and port are defined.
- Secrets are not baked into the image.
- Unnecessary local files are excluded from the build context.
- Logs are designed for platform capture.
- Local container testing is defined.
- Durable state is not assumed to live in the container filesystem.
- A future maintainer could rebuild the image without hidden laptop dependencies.

---

## Common Failure Modes

- **Copy-everything build:** The image includes secrets, notebooks, caches, or datasets it does not need.
- **Undocumented startup:** The service starts only because one developer knows the right command.
- **Secret baking:** Credentials are added at build time.
- **Port mismatch:** Local and platform expectations do not align.
- **Image bloat:** Packaging includes development tools and artifacts unnecessarily.
- **State confusion:** The service writes important data to local container disk and assumes it will persist.

---

## Portfolio Evidence

Save:

- the container packaging specification
- the Dockerfile
- one short note explaining the secret-injection and build-context rules

This shows that you can package an AI workflow into a reproducible deployment artifact instead of a laptop-specific environment.

---

## References

- Docker documentation: https://docs.docker.com/
- FastAPI Deployment: https://fastapi.tiangolo.com/deployment/
- Google Cloud Run documentation: https://cloud.google.com/run/docs
- AWS Lambda container image support: https://docs.aws.amazon.com/lambda/latest/dg/images-create.html
