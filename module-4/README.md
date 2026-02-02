# Module 4: Azure Container Deployment Options

## Overview

Module 4 explores the various Azure services for deploying and running containerized .NET applications. You'll learn when to use each service, how to deploy containers, configure autoscaling, implement CI/CD pipelines, and monitor your applications in production.

## Prerequisites

Before starting this module, ensure you have completed:

- ✅ Module 1: Container Fundamentals
- ✅ Module 2: .NET Containerization
- ✅ Module 3: Azure Container Registry
- ✅ At least one .NET container image in ACR
- ✅ Azure subscription with appropriate permissions
- ✅ Azure CLI installed and configured
- ✅ kubectl installed (for AKS labs)
- ✅ Helm installed (for AKS labs)

## Learning Objectives

By the end of this module, you will be able to:

- ✅ Compare Azure container services and choose the right one for your workload
- ✅ Deploy .NET containers to Azure Container Instances (ACI)
- ✅ Deploy microservices to Azure Container Apps (ACA)
- ✅ Deploy web applications to Azure App Service with containers
- ✅ Deploy enterprise applications to Azure Kubernetes Service (AKS)
- ✅ Configure autoscaling for container workloads
- ✅ Implement CI/CD pipelines with GitHub Actions and Azure DevOps
- ✅ Monitor and troubleshoot containerized applications

## Module Structure

### Files in This Module

| File | Purpose |
|------|---------|
| `presentation.html` | Interactive slide presentation (20 slides) |
| `README.md` | This file - module overview and guide |
| `4.1-container-services-overview.md` | Lab: Compare Azure container services |
| `4.2-azure-container-instances.md` | Lab: Deploy to ACI |
| `4.3-azure-container-apps.md` | Lab: Deploy to Container Apps |
| `4.4-app-service-containers.md` | Lab: Deploy to App Service |
| `4.5-azure-kubernetes-service.md` | Lab: Deploy to AKS |
| `4.6-cicd-pipelines.md` | Lab: CI/CD with GitHub Actions and Azure DevOps |

### Topics Covered

#### 4.1 Azure Container Services Overview
- Comparison of ACI, ACA, App Service, and AKS
- Decision matrix for service selection
- Use cases and scenarios
- Pricing models and cost optimization
- SLA and availability considerations

#### 4.2 Azure Container Instances (ACI)
- ACI architecture and features
- Deploying single containers
- Multi-container groups (sidecar pattern)
- Container lifecycle management
- Networking, DNS, and public IPs
- Environment variables and secrets
- Volume mounting with Azure Files

#### 4.3 Azure Container Apps (ACA)
- Container Apps architecture (KEDA, Dapr, Envoy)
- Creating Container Apps environments
- Deploying .NET microservices
- HTTP-based and queue-based autoscaling
- Revision management and versioning
- Traffic splitting for blue-green deployments
- Dapr integration for service-to-service calls

#### 4.4 Azure App Service with Containers
- App Service with custom containers
- Creating Linux App Service Plans
- ACR integration and continuous deployment
- Configuration and app settings
- Deployment slots for staging
- Manual and auto-scaling
- Custom domains and SSL certificates

#### 4.5 Azure Kubernetes Service (AKS)
- Kubernetes fundamentals (pods, services, deployments)
- AKS cluster provisioning with ACR integration
- Deploying applications with Kubernetes manifests
- Services and Ingress controllers
- Horizontal Pod Autoscaler (HPA)
- Cluster autoscaler
- Helm package management
- Advanced networking and security

#### 4.6 CI/CD for Container Deployments
- CI/CD pipeline overview
- GitHub Actions workflows for ACR and AKS
- Azure DevOps pipelines
- ACR webhooks for continuous deployment
- GitOps with Flux and Argo CD
- Secrets management with Azure Key Vault

## Getting Started

### Step 1: Review the Presentation

Open [presentation.html](presentation.html) in your web browser to understand Azure container deployment options.

```bash
# Navigate to module-4 directory
cd /Users/john/Documents/progress/module-4

# Open presentation in browser
open presentation.html
```

### Step 2: Complete the Labs in Order

Work through each lab sequentially:

1. **Lab 4.1**: Understand and compare Azure container services
2. **Lab 4.2**: Deploy your first container to ACI
3. **Lab 4.3**: Deploy microservices to Container Apps
4. **Lab 4.4**: Deploy web app to App Service
5. **Lab 4.5**: Deploy to AKS with Kubernetes
6. **Lab 4.6**: Implement CI/CD pipelines

### Step 3: Hands-On Projects

Apply your knowledge with these practical projects:

- Deploy a multi-tier .NET application to AKS
- Implement blue-green deployment with Container Apps
- Set up CI/CD pipeline with GitHub Actions
- Configure autoscaling for production workloads
- Implement monitoring with Application Insights

## Service Comparison

### When to Use Which Service?

| Scenario | Recommended Service | Why? |
|----------|-------------------|------|
| Quick test or POC | **ACI** | Fastest setup, pay-per-second |
| Microservices architecture | **Container Apps** | Auto-scaling, Dapr, serverless |
| Traditional web app | **App Service** | Familiar platform, deployment slots |
| Enterprise workloads | **AKS** | Full Kubernetes, complete control |
| Batch jobs | **ACI** or **Container Apps Jobs** | Short-lived, cost-effective |
| Event-driven apps | **Container Apps** | KEDA-based scaling |
| Multi-cloud strategy | **AKS** | Portable Kubernetes |

## Required Tools

| Tool | Purpose | Installation |
|------|---------|--------------|
| Azure CLI | Azure resource management | Module 3: Lab 3.1 |
| kubectl | Kubernetes CLI | `az aks install-cli` |
| Helm | Kubernetes package manager | [helm.sh](https://helm.sh/docs/intro/install/) |
| Docker Desktop | Local container development | Module 2 |
| Git | Source control | Pre-installed on macOS |

## Azure Resources Required

This module will create the following Azure resources:

### Common Resources
- **Resource Group**: `rg-containers`
- **Azure Container Registry**: `myuniqueregistry` (from Module 3)

### Per Service
- **ACI**: Container instances (billed per-second)
- **Container Apps**: Environment + Container Apps (consumption-based)
- **App Service**: Linux App Service Plan (P1V2 or higher)
- **AKS**: Kubernetes cluster with 3+ nodes (Standard_D2s_v3)
- **Log Analytics**: Workspace for monitoring (optional)

### Estimated Costs

**Development/Testing:**
- ACI: ~$0.05/hour per container
- Container Apps: ~$0.15/hour (with scale to zero)
- App Service: ~$0.10/hour (P1V2)
- AKS: ~$0.30/hour (3 nodes)

**Production:**
- Plan for higher costs with more replicas and larger VMs
- Use Azure Pricing Calculator for accurate estimates

## Key Commands Reference

### Azure Container Instances
```bash
# Create container
az container create --resource-group rg-containers --name myapi \
  --image myregistry.azurecr.io/myapi:1.0 --ports 8080

# View logs
az container logs --resource-group rg-containers --name myapi

# Delete container
az container delete --resource-group rg-containers --name myapi
```

### Azure Container Apps
```bash
# Create environment
az containerapp env create --name myenv --resource-group rg-containers

# Create app
az containerapp create --name myapi --resource-group rg-containers \
  --environment myenv --image myregistry.azurecr.io/myapi:1.0 \
  --target-port 8080 --ingress external

# View logs
az containerapp logs show --name myapi --resource-group rg-containers
```

### Azure App Service
```bash
# Create App Service Plan
az appservice plan create --name myplan --resource-group rg-containers \
  --is-linux --sku P1V2

# Create web app
az webapp create --resource-group rg-containers --plan myplan \
  --name mywebapi --deployment-container-image-name myregistry.azurecr.io/myapi:1.0

# View logs
az webapp log tail --name mywebapi --resource-group rg-containers
```

### Azure Kubernetes Service
```bash
# Create AKS cluster
az aks create --resource-group rg-containers --name myakscluster \
  --node-count 3 --attach-acr myregistry --generate-ssh-keys

# Get credentials
az aks get-credentials --resource-group rg-containers --name myakscluster

# Deploy application
kubectl apply -f deployment.yaml

# View pods
kubectl get pods
```

## Lab Completion Checklist

Track your progress through Module 4:

### Service Overview
- [ ] Reviewed comparison of all Azure container services
- [ ] Completed decision matrix exercise
- [ ] Explored services in Azure Portal

### Azure Container Instances
- [ ] Deployed single container to ACI
- [ ] Created multi-container group
- [ ] Configured environment variables
- [ ] Mounted Azure Files volume

### Azure Container Apps
- [ ] Created Container Apps environment
- [ ] Deployed .NET container app
- [ ] Configured HTTP autoscaling
- [ ] Implemented traffic splitting
- [ ] Created multiple revisions

### Azure App Service
- [ ] Created Linux App Service Plan
- [ ] Deployed container to App Service
- [ ] Configured ACR integration
- [ ] Enabled continuous deployment
- [ ] Created deployment slot

### Azure Kubernetes Service
- [ ] Provisioned AKS cluster
- [ ] Deployed application with manifests
- [ ] Created service and ingress
- [ ] Configured HPA
- [ ] Packaged app with Helm

### CI/CD
- [ ] Created GitHub Actions workflow
- [ ] Configured Azure DevOps pipeline
- [ ] Set up ACR webhooks
- [ ] Implemented automated deployment

## Troubleshooting

### Common Issues

**Issue**: `InsufficientQuotaError` when creating resources
- **Solution**: Request quota increase in Azure Portal or use smaller VM sizes.

**Issue**: AKS cluster creation fails
- **Solution**: Ensure you have enough vCPU quota and the service principal has proper permissions.

**Issue**: Container fails to start in ACI/ACA
- **Solution**: Check logs with `az container logs` or `az containerapp logs show`. Verify ACR credentials.

**Issue**: Cannot pull image from ACR
- **Solution**: Ensure ACR is attached (`--attach-acr`) or configure credentials properly.

**Issue**: `kubectl` commands not working
- **Solution**: Get AKS credentials with `az aks get-credentials`.

### Getting Help

1. Check the troubleshooting section in each lab
2. Review Azure CLI command help: `az <service> --help`
3. Check service documentation (links in presentation)
4. Review logs with service-specific commands
5. Contact training coordinator: training@progress.com

## Best Practices

### Service Selection
- Start with simpler services (ACI, Container Apps) before AKS
- Use Container Apps for most microservices scenarios
- Reserve AKS for complex enterprise requirements
- Consider serverless options (Container Apps, ACI) for cost optimization

### Security
- ✅ Use managed identities instead of credentials
- ✅ Store secrets in Azure Key Vault
- ✅ Implement network security (private endpoints, NSGs)
- ✅ Enable Azure AD authentication
- ✅ Scan images before deployment
- ❌ Never hardcode credentials in code or manifests

### Scaling
- Configure autoscaling for production workloads
- Set appropriate min/max replica counts
- Use different scaling rules (CPU, HTTP, queue)
- Test scaling behavior under load
- Monitor scaling metrics

### Cost Optimization
- Use scale-to-zero for dev/test (Container Apps)
- Right-size VM/container resources
- Use reserved instances for production (App Service, AKS)
- Implement autoscaling to avoid over-provisioning
- Clean up unused resources

### CI/CD
- Automate deployments with GitHub Actions or Azure DevOps
- Use different environments (dev, staging, production)
- Implement blue-green or canary deployments
- Tag images with commit SHA for traceability
- Use ACR webhooks for automatic updates

## Additional Resources

### Microsoft Documentation
- [Azure Container Services](https://azure.microsoft.com/en-us/products/category/containers/)
- [ACI Documentation](https://learn.microsoft.com/en-us/azure/container-instances/)
- [Container Apps Documentation](https://learn.microsoft.com/en-us/azure/container-apps/)
- [App Service Documentation](https://learn.microsoft.com/en-us/azure/app-service/)
- [AKS Documentation](https://learn.microsoft.com/en-us/azure/aks/)

### Kubernetes
- [Kubernetes Official Docs](https://kubernetes.io/docs/home/)
- [Helm Documentation](https://helm.sh/docs/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

### CI/CD
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Azure DevOps Pipelines](https://learn.microsoft.com/en-us/azure/devops/pipelines/)
- [GitOps with Flux](https://fluxcd.io/docs/)

### Training
- [Microsoft Learn: Deploy containers to Azure](https://learn.microsoft.com/en-us/training/paths/deploy-applications-with-azure-devops/)
- [Microsoft Learn: AKS Workshop](https://learn.microsoft.com/en-us/training/modules/aks-workshop/)

## Estimated Time

- **Presentation Review**: 45-60 minutes
- **Lab 4.1**: 30 minutes
- **Lab 4.2**: 1 hour
- **Lab 4.3**: 1.5-2 hours
- **Lab 4.4**: 1-1.5 hours
- **Lab 4.5**: 2-3 hours
- **Lab 4.6**: 1.5-2 hours
- **Total**: 8-12 hours

## Course Completion

Congratulations on completing Module 4 and the entire **Container-Based Development with .NET** training course!

### What You've Learned

Across all 4 modules, you've mastered:

1. **Module 1**: Container fundamentals and architecture
2. **Module 2**: Building and optimizing .NET containers
3. **Module 3**: Managing images with Azure Container Registry
4. **Module 4**: Deploying containers to Azure services

### Next Steps

- Apply these skills to your production projects at Progress Software
- Explore advanced Kubernetes features (Helm, Operators, Service Mesh)
- Learn about monitoring with Azure Monitor and Application Insights
- Investigate GitOps workflows with Flux or Argo CD
- Obtain Azure certifications (AZ-104, AZ-204, AZ-305)

### Feedback

Please provide feedback on this training:
- Email: training@progress.com
- Share your success stories and challenges
- Suggest improvements for future iterations

---

**Module Version**: 1.0  
**Last Updated**: January 2026  
**Target Audience**: Progress Software Development Teams

Thank you for participating in this training! 🎉
