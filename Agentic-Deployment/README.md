# Agentic Deployment: Microsoft Foundry Agents on Azure App Service

## Overview
This guide describes approaches and steps to deploy **Microsoft Foundry** agents to **Azure App Service**. The focus is on safe rollouts, reliability, and repeatable deployment workflows.

> Note: Azure AI Foundry is now **Microsoft Foundry**.

## Architecture Options
### 1) Single App Service + Deployment Slots
**Best for**: simplicity, fast swaps, lower cost.
- Use **production** and **staging** slots.
- Deploy to staging, validate, then **swap**.
- Keep slot-specific settings for secrets and environment labels.

### 2) Dual App Services (Blue/Green)
**Best for**: strict isolation and high-risk changes.
- Separate **blue** and **green** App Services.
- Route traffic via **Front Door**, **Traffic Manager**, or **Application Gateway**.
- Use weighted routing for controlled cutover.

### 3) Regional + Global Router
**Best for**: multi-region resiliency.
- Each region uses slots or blue/green.
- Global router shifts traffic by region.

## Steps: Deploy from Foundry and Target App Service
### A) Prepare Foundry resources
1. Create a **Foundry project** and **model deployment** (choose the model ID and deployment name).
2. Record the **endpoint URL**, **deployment name**, and **auth method**.
3. Decide on agent tooling: **Microsoft Agent Framework** is recommended for agent apps.

### B) Build the agent service
1. Implement a web API wrapper around your agent (HTTP endpoint for chat or tool execution).
2. Externalize configuration for:
   - Foundry endpoint
   - Deployment name
   - API key or Managed Identity
   - Feature flags and labels
3. Add **health** and **readiness** endpoints (for slot swaps and probes).

### C) Configure App Service
1. Create an **App Service Plan** and **Web App** (Linux or Windows).
2. Set **App Settings** for Foundry endpoint and deployment name.
3. Use **Key Vault references** for secrets or configure **Managed Identity**.
4. Enable **Application Insights** and health checks.

### D) Deploy and validate
1. Deploy to **staging** (slot or green app) using your CI/CD pipeline.
2. Warm up and run smoke tests.
3. Validate agent behavior, latency, and dependency availability.
4. Swap traffic or adjust routing weights.

## Deployment Techniques
- **Slots**: faster rollback via swap.
- **Weighted routing**: gradual exposure.
- **Feature flags**: controlled activation independent of deployment.
- **Kill switch**: immediate disable if regressions appear.

## CI/CD Guidance (High-Level)
- Build → deploy to staging → test → swap.
- Treat deployment steps as **idempotent** to recover from timeouts.
- Capture release metadata in tags/annotations.

## Observability
Track:
- agent request latency
- dependency failures (LLM endpoint, vector store)
- error rates and timeouts
- token usage anomalies

## Security
- Use **Managed Identity** where possible.
- Keep secrets in **Key Vault**.
- Apply least-privilege access to Foundry and storage resources.

## Notes on SDKs
- **Microsoft Agent Framework** is recommended for agent apps.
- If you include install steps, remember preview flags:
  - Python: `--pre` is required.
  - .NET: `--prerelease` is required.
