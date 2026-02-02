# Module 3: Azure Container Registry (ACR)

## Overview

Module 3 focuses on Azure Container Registry, Microsoft's private container registry service. You'll learn how to provision ACR, manage container images, work with OCI artifacts using ORAS CLI, and implement security best practices.

## Prerequisites

Before starting this module, ensure you have:

- Completed Module 2: .NET Containerization
- Docker Desktop installed and running
- Azure subscription with appropriate permissions
- Azure CLI installed
- ORAS CLI installed
- At least one containerized .NET application from Module 2

## Learning Objectives

By the end of this module, you will be able to:

- ✅ Install and configure Azure CLI
- ✅ Provision Azure Container Registry with different service tiers
- ✅ Push and pull container images to/from ACR
- ✅ Manage image lifecycle with retention policies
- ✅ Use ORAS CLI to store and manage OCI artifacts
- ✅ Implement ACR security with service principals and managed identities
- ✅ Configure network security with private endpoints and firewall rules
- ✅ Scan images for vulnerabilities with Microsoft Defender for Containers

## Module Structure

### Files in This Module

| File | Purpose |
|------|---------|
| `presentation.html` | Interactive slide presentation (16 slides) |
| `README.md` | This file - module overview and guide |
| `3.1-azure-cli-setup.md` | Lab: Azure CLI installation and configuration |
| `3.2-provisioning-acr.md` | Lab: Creating and configuring ACR |
| `3.3-working-with-images.md` | Lab: Push, pull, and manage container images |
| `3.4-oras-artifacts.md` | Lab: Working with OCI artifacts using ORAS |
| `3.5-acr-security.md` | Lab: Security, access control, and scanning |

### Topics Covered

#### 3.1 Environment Setup for Azure
- Azure CLI installation (Windows, macOS, Linux)
- Authentication with `az login`
- Subscription management
- ORAS CLI configuration for ACR
- Verification and troubleshooting

#### 3.2 Azure Container Registry Fundamentals
- ACR overview and features
- Service tiers: Basic, Standard, Premium
- Provisioning ACR with Azure CLI
- Portal-based ACR creation
- Understanding geo-replication
- ACR Tasks introduction

#### 3.3 Working with Container Images in ACR
- ACR authentication methods
- Tagging images for ACR
- Pushing images to ACR
- Pulling images from ACR
- Listing repositories and tags
- Image lifecycle management
- Retention policies and auto-purge
- Content trust (Premium tier)

#### 3.4 ORAS CLI for OCI Artifacts
- Understanding OCI artifacts
- ORAS vs Docker CLI
- Pushing configuration files as artifacts
- Pushing multiple files as artifacts
- Storing Helm charts in ACR
- Attaching SBOMs and signatures to images
- Discovering and pulling artifacts
- Artifact lifecycle management

#### 3.5 ACR Security and Access Control
- Authentication methods overview
- Service principal creation and management
- RBAC role assignments (acrpull, acrpush)
- Managed identity integration
- Repository-scoped tokens (Premium)
- Network security configuration
- Disabling public access
- Private endpoints and VNet integration
- Firewall rules
- Microsoft Defender for Containers
- Vulnerability scanning with ACR Tasks

## Getting Started

### Step 1: Review the Presentation

Open [presentation.html](presentation.html) in your web browser to get an overview of Module 3 concepts.

```bash
# Navigate to module-3 directory
cd /Users/john/Documents/progress/module-3

# Open presentation in browser
open presentation.html
```

### Step 2: Complete the Labs in Order

Work through each lab sequentially:

1. **Lab 3.1**: Set up Azure CLI and authenticate
2. **Lab 3.2**: Provision your first ACR
3. **Lab 3.3**: Push and manage container images
4. **Lab 3.4**: Work with OCI artifacts using ORAS
5. **Lab 3.5**: Implement security and scanning

### Step 3: Hands-On Projects

Apply your knowledge with these practical projects:

- Migrate existing container images to ACR
- Set up automated builds with ACR Tasks
- Implement image retention policies
- Create an artifact repository with ORAS
- Configure private endpoint for secure access
- Set up geo-replication (Premium tier)

## Required Tools

| Tool | Purpose | Installation Guide |
|------|---------|-------------------|
| Azure CLI | Azure resource management | `3.1-azure-cli-setup.md` |
| ORAS CLI | OCI artifact management | Module 2: `your-computer-setup.md` |
| Docker Desktop | Container runtime | Module 2: `your-computer-setup.md` |
| Azure Subscription | Cloud resources | [Azure Free Account](https://azure.microsoft.com/free/) |

## Azure Resources Required

This module will create the following Azure resources:

- **Resource Group**: `rg-containers`
- **Azure Container Registry**: `myuniqueregistry` (name must be globally unique)
- **Service Principals**: For authentication
- **Private Endpoints**: (Optional, Premium tier)
- **Log Analytics Workspace**: (Optional, for monitoring)

**Estimated Cost**: 
- Basic tier: ~$0.167/day (~$5/month)
- Standard tier: ~$0.667/day (~$20/month)
- Premium tier: ~$1.667/day (~$50/month)
- Plus storage and network egress charges

## Key Commands Reference

### Azure CLI
```bash
# Login
az login

# Create ACR
az acr create --resource-group rg-containers --name myregistry --sku Premium

# Login to ACR
az acr login --name myregistry

# List repositories
az acr repository list --name myregistry

# Show tags
az acr repository show-tags --name myregistry --repository myapp
```

### ORAS CLI
```bash
# Push artifact
oras push myregistry.azurecr.io/configs/myapp:v1 \
  --artifact-type application/json \
  config.json:application/json

# Pull artifact
oras pull myregistry.azurecr.io/configs/myapp:v1

# Discover attached artifacts
oras discover myregistry.azurecr.io/myapp:1.0
```

### Docker with ACR
```bash
# Tag for ACR
docker tag myapp:1.0 myregistry.azurecr.io/myapp:1.0

# Push to ACR
docker push myregistry.azurecr.io/myapp:1.0

# Pull from ACR
docker pull myregistry.azurecr.io/myapp:1.0
```

## Lab Completion Checklist

Track your progress through Module 3:

### Environment Setup
- [ ] Azure CLI installed and verified
- [ ] Successfully logged in with `az login`
- [ ] Active subscription selected
- [ ] ORAS CLI configured for ACR

### ACR Provisioning
- [ ] Created resource group
- [ ] Provisioned ACR (choose tier)
- [ ] Logged into ACR
- [ ] Retrieved ACR credentials

### Image Management
- [ ] Tagged local image for ACR
- [ ] Pushed image to ACR
- [ ] Listed repositories in ACR
- [ ] Pulled image from ACR
- [ ] Deleted old image tags
- [ ] Configured retention policy

### OCI Artifacts
- [ ] Pushed configuration file as artifact
- [ ] Pushed multi-file artifact
- [ ] Stored Helm chart in ACR
- [ ] Attached SBOM to image
- [ ] Discovered artifact referrers

### Security
- [ ] Created service principal
- [ ] Assigned RBAC roles
- [ ] Configured firewall rules
- [ ] Enabled Microsoft Defender (optional)
- [ ] Scanned images for vulnerabilities

## Troubleshooting

### Common Issues

**Issue**: `az: command not found`
- **Solution**: Azure CLI not installed or not in PATH. Follow Lab 3.1 installation steps.

**Issue**: `unauthorized: authentication required`
- **Solution**: Run `az acr login --name myregistry` before pushing/pulling images.

**Issue**: `The registry name is already taken`
- **Solution**: ACR names must be globally unique. Try a different name with your company/project prefix.

**Issue**: `insufficient privileges`
- **Solution**: Ensure your Azure account has Contributor or Owner role on the subscription.

**Issue**: `This operation is not allowed for Basic/Standard SKU`
- **Solution**: Some features (geo-replication, private endpoints, content trust) require Premium tier.

## Best Practices

### Naming Conventions
- Use lowercase alphanumeric characters only
- Include organization prefix: `progressregistry`, `progressacr`
- Keep names short but descriptive
- Use consistent naming across environments

### Security
- ✅ Use managed identities when possible
- ✅ Disable admin account in production
- ✅ Use repository-scoped tokens for fine-grained access
- ✅ Enable content trust for production (Premium)
- ✅ Implement private endpoints for sensitive workloads
- ✅ Scan all images before deployment
- ❌ Never commit ACR credentials to source control
- ❌ Avoid using admin account in production

### Image Management
- Use semantic versioning for tags: `1.0.0`, `1.0.1`
- Always tag with specific versions, not just `latest`
- Implement retention policies to control costs
- Use geo-replication for global deployments (Premium)
- Automate builds with ACR Tasks

### Cost Optimization
- Start with Basic tier for development
- Use Standard tier for production (without geo-replication)
- Use Premium tier only when needed (geo-replication, private endpoints)
- Implement retention policies to reduce storage costs
- Monitor and clean up unused images regularly

## Additional Resources

### Microsoft Documentation
- [Azure Container Registry Documentation](https://learn.microsoft.com/en-us/azure/container-registry/)
- [ACR Best Practices](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-best-practices)
- [Azure CLI Reference for ACR](https://learn.microsoft.com/en-us/cli/azure/acr)
- [ACR Service Tiers](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-skus)

### ORAS and OCI Artifacts
- [ORAS Project](https://oras.land/)
- [OCI Artifacts in ACR](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-oci-artifacts)
- [Open Container Initiative](https://opencontainers.org/)

### Security
- [ACR Authentication](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-authentication)
- [Microsoft Defender for Containers](https://learn.microsoft.com/en-us/azure/defender-for-cloud/defender-for-containers-introduction)
- [ACR Private Link](https://learn.microsoft.com/en-us/azure/container-registry/container-registry-private-link)

### Training
- [Microsoft Learn: Introduction to Docker Containers](https://learn.microsoft.com/en-us/training/modules/intro-to-docker-containers/)
- [Microsoft Learn: Build and store images with ACR](https://learn.microsoft.com/en-us/training/modules/build-and-store-container-images/)

## Getting Help

If you encounter issues:

1. Check the troubleshooting section in each lab
2. Review Azure CLI command help: `az acr --help`
3. Check ORAS documentation: [https://oras.land/](https://oras.land/)
4. Review Azure documentation links above
5. Contact your training coordinator
6. Email: training@progress.com

## Next Steps

After completing Module 3, you'll be ready for:

**Module 4: Azure Container Deployment Options**
- Azure Container Instances (ACI)
- Azure Container Apps (ACA)
- Azure App Service for Containers
- Azure Kubernetes Service (AKS)
- CI/CD for container deployments

## Estimated Time

- **Presentation Review**: 30-45 minutes
- **Lab 3.1**: 30 minutes
- **Lab 3.2**: 45 minutes
- **Lab 3.3**: 1 hour
- **Lab 3.4**: 1-1.5 hours
- **Lab 3.5**: 1-1.5 hours
- **Total**: 5-7 hours

---

**Module Version**: 1.0  
**Last Updated**: January 2026  
**Target Audience**: Progress Software Development Teams
