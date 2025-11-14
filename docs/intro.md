---
id: intro
title: Introduction
sidebar_label: Introduction
---

# VerdeFlow — Introduction

## 1. What is VerdeFlow?

VerdeFlow is a **commit-aware, workload-driven energy profiling platform for APIs**.

Its goal is to give developers, DevOps engineers, and engineering managers **actionable insights** into:

- How API endpoints behave under load  
- How resource usage (CPU, memory, network) changes across commits  
- How performance optimizations influence energy consumption  
- Which parts of the API are “guzzlers” and where improvements matter most  

VerdeFlow integrates directly into the development process, letting teams:

- Profile API endpoints on real hardware (e.g., Raspberry Pi agents)  
- Use OpenAPI specs to define or auto-generate workloads  
- Run **differential measurements** across commits or scenarios  
- Attribute energy consumption per endpoint, scenario, or commit  
- Compare optimizations like caching, compression, batching, and refactoring  

---

## 2. How VerdeFlow Works (Conceptual Overview)

At a high level:

1. A developer pushes code or manually triggers a profiling run.  
2. VerdeFlow builds and/or deploys the target API (or uses an existing instance).  
3. A **Profiling Agent** (usually a Raspberry Pi or Linux host) runs predefined workloads against that API.  
4. The Agent:
   - Executes HTTP calls against the target API  
   - Collects timing, CPU, memory, and network metrics  
   - Streams metrics to the VerdeFlow backend ingest service  
5. The backend **Estimator**:
   - Integrates power to energy  
   - Attributes energy usage per endpoint/scenario/commit  
   - Calculates optional carbon metrics  
6. The **UI** visualizes:
   - Trends per commit  
   - Scenario comparisons (baseline vs cached vs compressed vs refactored)  
   - Bottleneck endpoints and potential “energy guzzlers”  

---

## 3. System Architecture (Developer-Friendly Summary)

VerdeFlow is implemented as a modular TypeScript monorepo for the backend, plus separate repositories for the UI, agent, and sample API.

### 3.1 Core Components

- **VerdeFlow API (Control Plane)**  
  Orchestrates projects, devices, reservations, runs, metrics ingest, and energy estimation. It exposes public APIs for the UI, CLI, and CI integrations, and private RPC endpoints for profiling agents.

- **VerdeFlow UI**  
  A web dashboard used to manage projects, define workloads, trigger runs, and inspect results.

- **VerdeFlow Agents (Runners)**  
  Device-side processes (often on Raspberry Pis) that execute workloads, collect metrics, and send them back to the backend.

- **Sample API**  
  A demonstration API with multiple implementation variants (baseline, cached, compressed, refactored, heavy load), designed to showcase VerdeFlow’s capabilities.

- **Storage (PostgreSQL + Time Series DB + Object Storage)**  
  Stores configuration, runs, reservations, raw metrics, and aggregated results.

### 3.2 Backend Monorepo Structure (Summary)

The `verde-flow-api` repository is a pnpm monorepo with apps and shared packages, for example:

- `apps/gateway` – Public HTTP/GraphQL API surface  
- `apps/ingest` – Metrics and workload timeline ingest  
- `apps/estimator` – Energy and carbon computation  
- `apps/scheduler` – Reservations, queueing, and lifecycle orchestration  
- `packages/*` – Domain, storage, workloads, metrics schema, policies, telemetry, configuration, and more  

This separation provides:

- Clear boundaries between responsibilities  
- Reusable and testable shared libraries  
- Scalable service orchestration and CI/CD
