---
id: setup
title: Setup Guide
sidebar_label: Setup Guide
---

# VerdeFlow — Setup Guide

This guide explains how to set up a local VerdeFlow environment including the backend API, UI, profiling agent, and sample API.

---

## 1. Prerequisites

### 1.1 Tools

Install the following on your development machine:

- **Docker & Docker Compose**  
- **Node.js** (LTS version recommended)  
- **pnpm** (preferred package manager for the monorepo)  
- **Git**  
- **PostgreSQL client** (optional, useful for debugging the DB)  

### 1.2 Hardware

For realistic profiling, you typically use a **Raspberry Pi** (e.g., Raspberry Pi 4).  
However, any Linux machine can act as a profiling agent as long as it has network access to the backend API and the target API.

---

## 2. Clone Repositories

Clone the required repositories. Adjust URLs to your actual Git server.

```bash
git clone <repo-url>/verde-flow-api.git
git clone <repo-url>/verde-flow-ui.git
git clone <repo-url>/verde-flow-pi-agent.git
git clone <repo-url>/sample-api.git
```

You should now have four sibling folders:

- `verde-flow-api/`  
- `verde-flow-ui/`  
- `verde-flow-pi-agent/`  
- `sample-api/`  

---

## 3. Backend (API) Setup

### 3.1 Install Dependencies

```bash
cd verde-flow-api
pnpm install
```

### 3.2 Environment Configuration

Copy the example environment file and adjust values:

```bash
cp .env.example .env
```

Typical settings include:

- `POSTGRES_URL` – PostgreSQL connection string  
- `TSDB_URL` – Time series database endpoint (e.g., VictoriaMetrics)  
- `S3_ENDPOINT` and `S3_BUCKET` – Object storage for artifacts  
- `TIMEZONE_DEFAULT` – Default timezone (e.g., `Europe/Helsinki`)  

### 3.3 Database Migrations & Seed Data

Run migrations and seed the database:

```bash
pnpm migrate
pnpm seed
```

### 3.4 Start Backend Services

To start all backend apps in development mode:

```bash
pnpm dev
```

This typically launches:

- Gateway API  
- Ingest service  
- Estimator service  
- Scheduler service  

You can also run individual apps via `pnpm --filter`, depending on how the repo is configured.

---

## 4. UI Setup

### 4.1 Install Dependencies

```bash
cd ../verde-flow-ui
pnpm install
```

### 4.2 Configure Environment

Copy and edit the `.env` file if necessary (e.g., API base URL):

```bash
cp .env.example .env
# Set VITE_API_BASE_URL or equivalent to point at the gateway API
```

### 4.3 Start the UI

```bash
pnpm dev
```

Open your browser at the URL shown in the terminal, e.g.:

- `http://localhost:5173`

You should see the VerdeFlow web interface.

---

## 5. Profiling Agent Setup (Raspberry Pi or Linux Host)

### 5.1 Prepare the Host

On the agent device:

```bash
sudo apt update
sudo apt install -y git docker.io docker-compose
sudo usermod -aG docker $USER
```

Log out and back in to ensure your user is added to the `docker` group.

### 5.2 Clone and Configure the Agent

```bash
git clone <repo-url>/verde-flow-pi-agent.git
cd verde-flow-pi-agent
pnpm install
cp .env.example .env
```

Update `.env` with:

- `VERDEFLOW_API_URL` – URL of the backend gateway, e.g. `http://<host>:<port>`  
- `TARGET_API_BASE_URL` – Base URL of the API to be profiled (e.g., sample API)  

### 5.3 Start the Agent

```bash
pnpm start
```

The agent should:

- Register itself with the backend (if applicable)  
- Be ready to accept profiling jobs or manual commands  

---

## 6. Sample API Setup

The Sample API provides a hands-on playground to understand VerdeFlow.

```bash
cd ../sample-api
pnpm install
pnpm dev
```

Make note of the port it runs on, e.g. `http://localhost:3000`, and ensure the profiling agent can reach it (you may need to use `host.docker.internal` or the host IP address when running Docker).

You are now ready to connect everything through the VerdeFlow UI and run your first profiling session.
