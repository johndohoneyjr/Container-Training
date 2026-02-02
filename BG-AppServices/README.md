# Blue-Green Deployment for Azure App Service

## Overview
Blue-green deployment maintains two production-identical environments:
- **Blue**: current live environment.
- **Green**: staging environment for the next release.

Traffic switches between environments after validation, enabling fast rollback.

## Reference Architecture (App Service)
- **App Service Plan**: shared or separate plans for blue and green apps.
- **App Service (Blue)**: live production app.
- **App Service (Green)**: next-release app.
- **Deployment Slots**: optionally use slots in a single App Service.
- **Traffic Manager / Front Door / App Gateway**: controls routing.
- **Key Vault**: secrets and connection strings.
- **App Configuration**: environment settings.
- **Monitoring**: Application Insights + Log Analytics.
- **CI/CD**: builds, deploys, validates, and swaps.

## Approaches
### 1) Slot-Based Blue-Green (Single App Service)
**Best for**: simplicity, lower cost, quick swaps.
- Use **production** and **staging** slots.
- Deploy to staging, warm up, run health checks, then **swap**.
- Use slot settings for environment-specific values.
- Rollback: swap back immediately.

**Key techniques**
- Enable **health check** endpoint and warm-up.
- Configure **slot settings** for secrets and connections.
- Use **auto swap** only after tests pass.

### 2) Dual App Services (Parallel Apps)
**Best for**: isolation, different plans, or high-risk changes.
- Separate App Services for blue and green.
- Routing via **Front Door**, **Traffic Manager**, or **Application Gateway**.
- Swap traffic by changing routing rules/weights.

**Key techniques**
- Use **weighted routing** for canary-style validation.
- Separate **App Service Plans** for resource isolation.
- Keep identical infrastructure via IaC.

### 3) Hybrid Slot + Global Router
**Best for**: multi-region, HA scenarios.
- Each region uses slots for local blue/green.
- Global router shifts traffic between regions.

## Deployment Flow (Recommended)
1. Build and package.
2. Deploy to green (slot or parallel app).
3. Run smoke tests and synthetic checks.
4. Warm up and validate performance.
5. Switch traffic to green.
6. Monitor errors, latency, and saturation.
7. Keep blue for fast rollback.

## Handling Timeouts & Network Disruptions
- **Idempotent deployments**: deployment steps should be safe to re-run after a timeout.
- **Retry with backoff** for publishing packages and slot swaps.
- **Stage verification**: validate package integrity before swap.
- **Warm-up + health probes**: confirm readiness before routing traffic.
- **Abort & revert**: if deployment is interrupted after swap, immediately route back to blue.
- **Automated recovery**: pipelines should detect stuck or timed-out stages and resume from last safe checkpoint.

## Client-by-Client Rollout Strategies
Use this when you need controlled exposure per customer or tenant.
- **Tenant routing**: map client IDs to blue/green endpoints via router rules.
- **Weighted rollout**: gradually add clients to green by increasing weights.
- **Ring deployment**: internal users → pilot clients → full production.

## Deploying to a Subset of Clients
- **Allowlist routing**: direct selected client IDs to green.
- **Segmented DNS or headers**: use hostnames or headers to route subsets.
- **Per-tenant configuration**: enable green for specific tenant groups.

## Gradual or Selective Feature Exposure
- **Feature flags**: release features independently of deployment.
- **Config labels**: enable features per environment, region, or tenant.
- **Kill switch**: immediate disable for critical regressions.
- **Progressive exposure**: incrementally widen scope after validation.

## Validation & Health
- Health endpoint: `/health` or `/ready`.
- Automated smoke tests post-deploy.
- Synthetic checks and alerting thresholds.

## Data & State Considerations
- Use **backward-compatible schema** changes.
- Prefer **expand-and-contract** migrations.
- Avoid in-place breaking changes during swap.

## Configuration & Secrets
- Store secrets in **Key Vault**.
- Use **slot settings** and **app configuration labels**.

## Rollback Strategy
- Slot swap back or routing revert.
- Keep blue environment intact until stability confirmed.

## CI/CD Recommendations
- Use staged pipelines with approvals.
- Enforce automated tests before swap.
- Capture release metadata in tags/annotations.

## Monitoring Checklist
- Errors, latency, CPU/memory, saturation.
- Dependency failures (DB, cache, APIs).
- Alert on anomalous traffic patterns.

## When to Choose Each Approach
- **Slots**: fastest and cheapest.
- **Dual Apps**: strict isolation or plan differences.
- **Hybrid**: global resiliency with controlled cutover.
