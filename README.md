# Migration to Container-Based Development with .NET

A comprehensive training program for Progress Software development teams covering the complete journey of migrating .NET applications to container-based development on Azure.

## 📚 Course Overview

This repository contains training materials, presentations, labs, and resources for learning container-based development with .NET. The curriculum guides developers from fundamental container concepts through production deployment on Azure, covering security best practices, modern DevOps techniques, and Azure-specific containerization strategies.

**Target Audience:** Progress Software Development Teams  
**Level:** Intermediate to Advanced

---

## 📖 Table of Contents

### Core Curriculum
- [Course Curriculum](curriculum.md) - Complete training program outline and learning objectives

### Training Modules

#### [Module 1: Container Fundamentals](module-1/)
Introduction to containerization concepts, architecture, and ecosystem
- Container vs. Virtual Machine architecture
- Docker components and runtime environments
- Container security trends and best practices
- Open Container Initiative (OCI) standards
- 📊 Interactive presentation with 50+ curated resources

#### [Module 2: .NET Containerization](module-2/)
Hands-on containerization of .NET applications
- [Environment Setup](module-2/2.1-environment-setup.md) - Docker Desktop and ORAS CLI installation
- [.NET Images](module-2/2.2-dotnet-images.md) - Understanding .NET container base images
- [Creating Dockerfiles](module-2/2.3-creating-dockerfiles.md) - Dockerfile creation for .NET apps
- [Building & Running](module-2/2.4-building-running.md) - Multi-stage builds and local testing
- [Best Practices](module-2/2.5-best-practices.md) - Security and performance optimization
- [Computer Setup Guide](module-2/your-computer-setup.md) - Complete tool installation (Mac & Windows)

#### [Module 3: Azure Container Registry](module-3/)
Managing container images with Azure Container Registry (ACR)
- [Azure CLI Setup](module-3/3.1-azure-cli-setup.md) - Installing and configuring Azure CLI
- [Provisioning ACR](module-3/3.2-provisioning-acr.md) - Creating and configuring container registries
- [Working with Images](module-3/3.3-working-with-images.md) - Pushing, pulling, and managing images
- [ORAS Artifacts](module-3/3.4-oras-artifacts.md) - Storing supply chain artifacts
- [ACR Security](module-3/3.5-acr-security.md) - Access control and security practices

#### [Module 4: Azure Container Deployment](module-4/)
Deploying containers to Azure services
- [Container Services Overview](module-4/4.1-container-services-overview.md) - Comparison of Azure container platforms
- [Azure Container Instances](module-4/4.2-azure-container-instances.md) - Serverless container deployment
- [Azure Container Apps](module-4/4.3-azure-container-apps.md) - Microservices and event-driven applications
- [App Service Containers](module-4/4.4-app-service-containers.md) - Web app container hosting
- [Azure Kubernetes Service](module-4/4.5-azure-kubernetes-service.md) - Enterprise orchestration
- [CI/CD Pipelines](module-4/4.6-cicd-pipelines.md) - Automated deployment workflows

#### [Module 5: Production Operations](module-5/)
Monitoring, troubleshooting, and production best practices
- [Monitoring & Observability](module-5/5.1-monitoring-observability.md) - Azure Monitor and Application Insights
- [Troubleshooting Techniques](module-5/5.2-troubleshooting-techniques.md) - Debugging containerized applications
- [Production Best Practices](module-5/5.3-production-best-practices.md) - Performance, security, and reliability

### Additional Resources

#### [Agentic Deployment](Agentic-Deployment/)
Advanced deployment strategies and automation patterns

#### [BG-AppServices](BG-AppServices/)
Background services and App Service integration guides

---

## 🎯 What You'll Learn

- ✅ Container fundamentals and architecture
- ✅ Containerizing .NET applications with Docker
- ✅ Managing container images in Azure Container Registry
- ✅ Deploying to Azure Container Instances, Container Apps, and AKS
- ✅ Implementing CI/CD pipelines for containers
- ✅ Security best practices and vulnerability management
- ✅ Monitoring and troubleshooting production containers
- ✅ Performance optimization and cost management

---

## 🚀 Getting Started

1. Review the [curriculum.md](curriculum.md) for the complete course outline
2. Start with [Module 1](module-1/) for container fundamentals
3. Follow the [environment setup guide](module-2/2.1-environment-setup.md) to install required tools
4. Progress through modules sequentially for best learning experience
5. Use the interactive presentations in each module for visual learning

---

## 🛠️ Prerequisites

- Basic .NET development experience
- Familiarity with command-line interfaces
- Azure subscription (for cloud modules)
- Development environment (Windows or macOS)

---

## 📝 Notes

Track your progress and notes in [notes.txt](notes.txt)

---

## 📄 License

Progress Software internal training materials

---

## 🤝 Contributing

For questions, feedback, or updates to the training materials, please contact the Progress Software training team.
