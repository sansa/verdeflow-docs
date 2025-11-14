---
id: workflows
title: Developer Workflows
sidebar_label: Developer Workflows
---

# VerdeFlow — Developer Workflows & Best Practices

This guide focuses on how developers use VerdeFlow day-to-day, including typical workflows, best practices, and useful commands.

---

## 1. Typical Developer Workflows

### 1.1 After Implementing a Feature or Optimization

1. Implement your code change in the API.  
2. Push to the repository or open a pull request.  
3. Trigger a profiling run (manually or via CI).  
4. In the VerdeFlow UI, compare:
   - The new commit vs the previous commit  
   - Resource usage (CPU, memory, network)  
   - Response times  
   - Energy consumption per endpoint or scenario  

Use these insights to confirm whether the change is actually beneficial from both performance and energy perspectives.

---

### 1.2 Before Merging a Pull Request

Treat energy profiling as part of your quality gate:

1. Ensure at least one profiling run is associated with the PR’s latest commit.  
2. Review dashboards for:
   - Energy regressions (marked or highlighted where supported)  
   - Significant latency increases  
   - Memory or CPU spikes  
3. If regressions are unacceptable, iterate on the implementation before merging.

---

### 1.3 When Refactoring or Simplifying Logic

Refactoring often aims to make code cleaner or faster. VerdeFlow lets you:

1. Run profiling on the **pre-refactor** commit.  
2. Run profiling again on the **post-refactor** commit.  
3. Compare metrics side-by-side to quantify improvements or regressions.  

You can answer questions like:

- Did refactoring reduce CPU time?  
- Are expensive calls invoked less often?  
- Is energy per request now lower?  

---

### 1.4 Investigating a “Guzzler” Endpoint

When an endpoint appears suspicious (slow, heavy, or frequently used):

1. Design focused workloads hitting only that endpoint (or a small subset).  
2. Run profiling sessions with different scenarios, for example:
   - Current implementation  
   - Version with caching  
   - Version with batching or streaming  
3. Compare:
   - Response time distributions  
   - CPU and memory usage  
   - Energy per request  

Use these insights to prioritize optimizations that offer the best trade-offs.

---

## 2. Best Practices

- **Use deterministic workloads**  
  Ensure your workloads are repeatable and not heavily influenced by external randomness or unstable dependencies.

- **Stabilize the environment**  
  Run profiling on relatively idle machines or dedicated profiling devices to reduce noise from other processes.

- **Use OpenAPI-based scenarios where possible**  
  Generating workloads from an OpenAPI spec helps keep scenarios aligned with the live API surface.

- **Measure before and after**  
  Always take a baseline measurement before making changes; this makes it easier to highlight improvements or regressions.

- **Think in trade-offs**  
  Some optimizations may reduce energy at the cost of higher latency or vice versa. Use VerdeFlow to make informed decisions.

- **Automate in CI**  
  Integrate profiling into CI so energy regressions are detected early, ideally before code is merged into main branches.

---

## 3. Quick Reference Commands

These examples assume standard scripts; adjust to your actual repo scripts as needed.

### 3.1 Backend

```bash
# Install dependencies
cd verde-flow-api
pnpm install

# Run database migrations and seed data
pnpm migrate
pnpm seed

# Start all backend applications
pnpm dev

# Run tests and type checks
pnpm test
pnpm lint
pnpm typecheck
```

### 3.2 UI

```bash
cd ../verde-flow-ui
pnpm install
pnpm dev
```

### 3.3 Profiling Agent

```bash
cd ../verde-flow-pi-agent
pnpm install
pnpm start
```

### 3.4 Sample API

```bash
cd ../sample-api
pnpm install
pnpm dev
```

---

## 4. Extending VerdeFlow

As VerdeFlow evolves, you can extend it with:

- **New collectors** (e.g., different power measurement tools or hardware)  
- **New workload types** (e.g., asynchronous patterns, batch jobs)  
- **Additional dashboards and reports** tailored to your domain  
- **PR annotations and status checks** in your Git hosting platform  

Always update the documentation and onboarding material when new capabilities are added so that developers can discover and use them effectively.
