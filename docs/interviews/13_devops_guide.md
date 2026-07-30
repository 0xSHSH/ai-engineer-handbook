# 1️⃣3️⃣ DevOps Interview Guide

> Part of the [Interview Handbook](README.md).

## 📑 Contents
- [Docker](#docker)
- [Kubernetes](#kubernetes)
- [CI/CD](#cicd)
- [GitHub Actions](#github-actions)
- [Terraform (IaC)](#terraform-iac)
- [Monitoring & Logging](#monitoring--logging)
- [Deployment Strategies](#deployment-strategies)
- [Production Practices](#production-practices)
- [Interview Questions](#interview-questions)

---

## Docker
```dockerfile
# Multi-stage build — keeps final image small by discarding build-only dependencies
FROM python:3.12-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

FROM python:3.12-slim
WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY . .
ENV PATH=/root/.local/bin:$PATH
CMD ["python", "app.py"]
```
Key points interviewers check: layer caching (order instructions least-to-most frequently changing so cache hits are maximized), `.dockerignore` to keep build context small, running as a non-root user, and multi-stage builds to strip build tools/secrets from the final image.

## Kubernetes
- **Pod lifecycle**: Pending → Running → Succeeded/Failed; a `Deployment` maintains a desired replica count and handles rolling updates.
- **Readiness vs liveness probes**: readiness controls whether a pod receives traffic (temporarily removed from load balancing if failing); liveness controls whether the pod gets restarted (container is stuck/deadlocked).
- **Resource requests/limits**: requests guarantee scheduling capacity, limits cap usage — set both to avoid a single pod starving the node (no limits) or being evicted unnecessarily (limits set too low).
- **ConfigMaps/Secrets**: externalize configuration and credentials from the image so the same image is promoted unchanged across environments.

## CI/CD
- **Continuous Integration**: every push triggers automated build + test, catching integration issues early rather than at a big merge.
- **Continuous Delivery**: every change that passes CI is automatically prepared for release (but a human approves the deploy).
- **Continuous Deployment**: passes CI → automatically deployed to production with no manual gate — requires strong test coverage and fast rollback capability.
- A good pipeline: lint → unit tests → build artifact/image → integration tests → deploy to staging → smoke tests → deploy to production (often gated by a canary or manual approval for critical services).

## GitHub Actions
```yaml
name: CI
on:
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install -r requirements.txt
      - run: pytest --cov=src tests/
      - run: ruff check .

  build-and-push:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: docker/build-push-action@v6
        with:
          push: true
          tags: myorg/myapp:${{ github.sha }}
```
Key ideas: `needs:` for job dependencies, `if:` conditions to gate deploy jobs to specific branches, caching dependencies (`actions/cache`) to speed up repeated runs, and using commit SHA (not `latest`) as the image tag for traceable, reproducible deploys.

## Terraform (IaC)
```hcl
resource "aws_s3_bucket" "data" {
  bucket = "my-app-data-bucket"
}

resource "aws_instance" "app_server" {
  ami           = "ami-0abcdef1234567890"
  instance_type = "t3.medium"
  tags = { Name = "app-server" }
}
```
- **Declarative, not imperative**: you describe desired end state; Terraform computes the diff and applies only the changes needed.
- **State file**: tracks what Terraform believes exists — must be stored remotely (S3 + DynamoDB lock, or Terraform Cloud) for team use, never just locally, to avoid state drift/corruption from concurrent applies.
- **Plan before apply**: `terraform plan` shows the diff before anything changes — always review it, especially for destructive changes (resource replacement/deletion).
- Modules let you package reusable infrastructure components (e.g., a "standard VPC" module) instead of copy-pasting HCL across environments.

## Monitoring & Logging
- **Structured logging** (JSON logs with consistent fields: timestamp, service, trace_id, level) is queryable at scale; unstructured `print()`-style logs are not.
- **Metrics** (Prometheus-style time series) power dashboards and alerting; **traces** (OpenTelemetry) show a request's path across services — all three (logs, metrics, traces) are needed for real production debugging.
- **Alert fatigue** is a real failure mode: alert on symptoms that require action, route by severity, and regularly prune alerts nobody acts on.

## Deployment Strategies
| Strategy | How | Trade-off |
|---|---|---|
| Rolling update | Replace instances gradually | Default in K8s, brief mixed-version window |
| Blue-green | Deploy new version alongside old, switch traffic at once | Fast rollback (switch back), needs 2x capacity briefly |
| Canary | Route a small % of traffic to the new version, expand gradually | Limits blast radius of a bad deploy, needs good metrics to auto-abort |
| Feature flags | Deploy code dark, enable behavior via a runtime flag | Decouples deploy from release, enables gradual/targeted rollout |

## Production Practices
- **Infrastructure as Code** for everything — no manual console changes that aren't reflected in version control ("ClickOps" causes drift and un-reproducible environments).
- **Immutable infrastructure**: don't patch running servers; build a new image/artifact and replace instances.
- **Runbooks** for known failure scenarios, and blameless postmortems after incidents to fix systemic causes, not assign blame.
- **Secrets management**: never commit secrets to git — use a secrets manager (Vault, AWS Secrets Manager) with rotation, not `.env` files in the repo.

---

## Interview Questions
1. Walk through your ideal CI/CD pipeline for a Python microservice, start to finish.
2. What's the difference between blue-green and canary deployments, and when would you choose each?
3. How do you manage secrets across environments without committing them to source control?
4. Explain readiness vs liveness probes and what happens if you misconfigure them.
5. How would you debug a service that's healthy in staging but crash-looping in production?
6. What does "immutable infrastructure" mean, and why does it reduce operational risk?

---
*Part of the [AI Engineer Handbook](../../README.md) · [Interview Handbook](README.md) · Next: [Company Preparation](14_company_preparation.md).*
