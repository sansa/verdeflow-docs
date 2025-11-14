---
id: sample-api
title: Sample API Guide
sidebar_label: Sample API Guide
---

# VerdeFlow — Sample API Guide

The Sample API is a demonstration service designed to help you quickly learn how VerdeFlow works in practice.

It provides multiple implementation variants so you can see how design decisions affect:

- Response time  
- Resource usage (CPU, memory, network)  
- Energy consumption per request or scenario  

---

## 1. Sample API Scenarios

Typical scenarios (your actual API may expose slightly different routes/names):

- **Baseline**  
  Straightforward implementation, no special optimizations.

- **Cache**  
  Uses in-memory or external caching to reduce repeated work.

- **Compression**  
  Compresses responses (e.g., GZIP) to reduce network usage, often at the cost of additional CPU.

- **Refactored**  
  More efficient algorithms or data structures, resulting in lower CPU and faster responses.

- **Heavy / Stress Endpoints**  
  Deliberately expensive endpoints to highlight bottlenecks and “guzzlers”.

These variants can be implemented as separate routes or as query parameters that switch behavior.

---

## 2. Running the Sample API

From the repository:

```bash
cd sample-api
pnpm install
pnpm dev
```

Check which port the API is running on (for example, `http://localhost:3000`).  
Ensure that the **profiling agent** can reach this URL. With Docker in the mix, you might need to use:

- `http://host.docker.internal:3000` (on some platforms), or  
- The host machine’s IP address.

You’ll use this URL later as the **Target API Base URL** in VerdeFlow.

---

## 3. Creating a Project for the Sample API

In the **VerdeFlow UI**:

1. Go to **Projects → New Project**.  
2. Fill in fields such as:
   - **Name**: `Sample API – Energy Demo`  
   - **Repository URL**: `<URL of sample-api repo>`  
   - **Default Branch**: `main` (or your default)  
   - **Target Base URL**: the URL at which the sample API is reachable from the agent (e.g., `http://host.docker.internal:3000`).  
3. Save the project.

This config allows VerdeFlow to associate profiling runs with the sample API and its commits.

---

## 4. Running a Profiling Session

You can start profiling via the UI or CI. For a first test, use the UI.

### 4.1 Manual Run from the UI

1. Open the **Sample API project** you created.  
2. Click **“Run Profiling”** or the equivalent action.  
3. Choose:
   - A **Scenario** such as `baseline`, `cache`, `compression`, or `refactored`.  
   - A **Workload** (for example, “100 requests over 60 seconds”).  
4. Confirm and start the run.

The backend scheduler assigns the run to an available agent, which then:

- Executes the workload against the sample API  
- Collects metrics  
- Streams them to the ingest service  

### 4.2 CI-Triggered Runs (Optional)

You can also configure CI pipelines or Git webhooks to automatically trigger runs whenever:

- A new commit is pushed  
- A pull request is opened or updated  

This allows you to treat energy metrics similarly to tests or quality checks.

---

## 5. Interpreting Results for the Sample API

Once a run finishes, open the project in the UI and explore:

### 5.1 Metrics Over Time

Common charts include:

- CPU usage vs time  
- Memory usage vs time  
- Network throughput vs time  
- Response times (min/avg/max or histograms)  
- Estimated power and energy consumption  

### 5.2 Scenario Comparisons

Use the comparison views to answer questions such as:

- Did **caching** reduce CPU, latency, and energy usage?  
- Did **compression** reduce network traffic but increase CPU time?  
- Did **refactoring** significantly lower response times and total energy per request?  
- Are there endpoints that dominate energy usage even with optimizations?  

### 5.3 Commit and Branch Trends

For commit-aware configurations, you can:

- Compare energy metrics across commits or branches  
- Spot regressions early  
- Justify refactoring work with quantified improvements  

The Sample API is deliberately small and safe to experiment with. Use it to build intuition before applying VerdeFlow to production services.
