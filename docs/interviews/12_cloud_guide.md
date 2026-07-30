# 1️⃣2️⃣ Cloud Interview Guide

> Part of the [Interview Handbook](README.md). AWS-first with Azure/GCP equivalents noted — interviewers rarely care which provider you know, they care that you understand the underlying concept.

## 📑 Contents
- [Provider Service Map](#provider-service-map)
- [Storage](#storage)
- [IAM](#iam)
- [Networking](#networking)
- [Compute](#compute)
- [Containers](#containers)
- [Monitoring](#monitoring)
- [Interview Questions](#interview-questions)

---

## Provider Service Map
| Concept | AWS | Azure | GCP |
|---|---|---|---|
| Object storage | S3 | Blob Storage | Cloud Storage |
| VM compute | EC2 | Virtual Machines | Compute Engine |
| Serverless functions | Lambda | Functions | Cloud Functions |
| Managed Kubernetes | EKS | AKS | GKE |
| Relational DB | RDS | Azure SQL / PostgreSQL | Cloud SQL |
| NoSQL DB | DynamoDB | Cosmos DB | Firestore / Bigtable |
| IAM | IAM | Entra ID (Azure AD) | Cloud IAM |
| CDN | CloudFront | Azure CDN | Cloud CDN |
| Queue | SQS | Service Bus | Pub/Sub |

## Storage
- **Object storage** (S3-style): flat namespace of key → blob, virtually unlimited scale, eventual or strong consistency depending on provider/config, used for files/backups/static assets/data lake.
- **Block storage** (EBS-style): attached to a single VM like a virtual disk, needed for databases and filesystems requiring low-latency random I/O.
- **File storage** (EFS-style): shared network filesystem mountable by multiple instances concurrently.
- **Storage classes/tiers**: hot (frequent access, higher cost) vs cold/archive (infrequent access, much cheaper, higher retrieval latency) — lifecycle policies auto-transition objects by age.

## IAM
- **Principle of least privilege**: grant only the permissions a principal (user/service/role) needs, nothing more — the single most-tested IAM concept in interviews.
- **Roles vs users**: a role is an assumable identity with a permission set, not tied to a specific person — services and cross-account access should use roles, not long-lived user credentials.
- **Policies**: JSON documents attaching permissions to identities or resources; know the difference between identity-based (attached to a user/role) and resource-based (attached to, e.g., an S3 bucket) policies.
- Avoid hardcoded credentials in code — use instance/task roles so the cloud provider injects short-lived credentials automatically.

## Networking
- **VPC**: an isolated virtual network you control the IP range, subnets, and routing for.
- **Public vs private subnet**: public subnets route to an internet gateway; private subnets route outbound only through a NAT gateway (no direct inbound internet access) — put databases and internal services in private subnets.
- **Security Groups vs NACLs**: security groups are stateful, instance-level (return traffic auto-allowed); NACLs are stateless, subnet-level (must explicitly allow both directions).
- **Load balancers**: L4 (network, fast, protocol-agnostic) vs L7 (application, can route on path/host/headers, supports SSL termination).

## Compute
- **VMs**: full control over the OS, slowest to provision, most operational overhead.
- **Containers**: package app + dependencies, faster startup than VMs, consistent across environments — the default choice for most modern services.
- **Serverless functions**: no server management, scale to zero, pay per invocation — best for event-driven, bursty, or infrequent workloads; watch for cold-start latency and execution time limits.
- **Autoscaling**: scale out/in based on CPU, memory, queue depth, or custom metrics — set both a minimum (availability floor) and maximum (cost ceiling).

## Containers
- **Docker**: packages an app with its dependencies into an image built from layered, cacheable instructions (a `Dockerfile`); containers share the host kernel (unlike VMs), making them lighter and faster to start.
- **Kubernetes**: orchestrates containers across a cluster — schedules Pods onto Nodes, handles self-healing (restarts failed containers), scaling (Horizontal Pod Autoscaler), service discovery, and rolling deployments.
- Core K8s objects to know: **Pod** (smallest deployable unit, one or more containers), **Deployment** (manages replica sets and rollout strategy), **Service** (stable network endpoint for a set of pods), **ConfigMap/Secret** (externalized config/credentials).

## Monitoring
- **Three pillars of observability**: metrics (numeric time series — CPU, latency, error rate), logs (discrete timestamped events), traces (request path across distributed services).
- **SLI/SLO/SLA**: SLI = measured indicator (e.g., p99 latency), SLO = internal target (e.g., p99 < 300ms), SLA = external contractual commitment (often looser than the SLO, with consequences for breach).
- **Alerting on symptoms, not causes**: alert on user-facing signals (error rate, latency) rather than every internal metric, to avoid alert fatigue.
- Distributed tracing (OpenTelemetry) is essential once you have more than a couple of services — without it, debugging a slow request across 5 microservices is close to impossible.

---

## Interview Questions
1. Design the network architecture for a 3-tier web app (public web tier, private app tier, private DB tier).
2. When would you choose serverless functions over a containerized service?
3. Explain least-privilege IAM with a concrete example for a data pipeline that reads from S3 and writes to a database.
4. What's the difference between horizontal and vertical scaling, and when is each appropriate?
5. How would you design for high availability across multiple availability zones/regions?
6. Walk through what happens end-to-end when you `kubectl apply` a new Deployment.

---
*Part of the [AI Engineer Handbook](../../README.md) · [Interview Handbook](README.md) · Next: [DevOps Guide](13_devops_guide.md).*
