# Module 5: Monitoring, Troubleshooting, and Best Practices

## Overview

Welcome to the final module of the Container-Based Development training! This module focuses on building production-ready containerized .NET applications on Azure with comprehensive monitoring, effective troubleshooting techniques, and industry best practices.

By the end of this module, you'll be able to:
- Monitor containerized applications using Azure Monitor and Application Insights
- Query logs with KQL (Kusto Query Language)
- Implement distributed tracing with OpenTelemetry
- Troubleshoot common container issues in Azure
- Optimize application performance and costs
- Secure applications with Azure Key Vault
- Implement high availability and disaster recovery strategies

## Prerequisites

Before starting this module, ensure you have:

- ✅ Completed Modules 1-4 of this training
- ✅ Active Azure subscription with appropriate permissions
- ✅ Azure CLI installed and configured
- ✅ Docker Desktop running
- ✅ kubectl installed (for AKS troubleshooting)
- ✅ A deployed application from Module 4 (ACI, ACA, or AKS)

## Module Structure

This module consists of 15 slides and 3 comprehensive hands-on labs:

### **Presentation: [presentation.html](./presentation.html)**
Navigate through 15 slides covering:
- The three pillars of observability (metrics, logs, traces)
- Azure Monitor and Container Insights
- Application Insights for .NET applications
- Log Analytics and KQL queries
- Alerts and dashboards
- Distributed tracing with OpenTelemetry
- Common troubleshooting scenarios
- Local and remote debugging techniques
- Performance optimization strategies
- Production best practices (HA/DR)
- Secrets management with Azure Key Vault
- Cost optimization strategies

### **Lab 5.1: Monitoring and Observability** (2-3 hours)
**File:** [5.1-monitoring-observability.md](./5.1-monitoring-observability.md)

Set up comprehensive monitoring for your containerized applications:
- Enable Azure Monitor and Container Insights for AKS
- Integrate Application Insights with .NET applications
- Write KQL queries to analyze logs
- Configure alerts and dashboards
- (Optional) Implement Prometheus and Grafana
- Implement distributed tracing with OpenTelemetry

**Key Technologies:** Azure Monitor, Application Insights, Log Analytics, KQL, OpenTelemetry

### **Lab 5.2: Troubleshooting Techniques** (3-4 hours)
**File:** [5.2-troubleshooting-techniques.md](./5.2-troubleshooting-techniques.md)

Learn practical troubleshooting skills:
- Debug containers locally with Docker commands
- Troubleshoot remote containers in ACI and AKS
- Diagnose and fix image pull errors
- Resolve networking and connectivity issues
- Address resource constraints (CPU, memory)
- Optimize application performance
- Use kubectl for Kubernetes troubleshooting

**Key Technologies:** Docker CLI, Azure CLI, kubectl, debugging tools

### **Lab 5.3: Production Best Practices** (3-4 hours)
**File:** [5.3-production-best-practices.md](./5.3-production-best-practices.md)

Implement production-ready patterns:
- Configure Azure Key Vault for secrets management
- Set up geo-replication for ACR
- Implement backup and disaster recovery procedures
- Optimize costs with autoscaling and spot instances
- Harden security with network policies and RBAC
- Deploy multi-region architecture with Traffic Manager

**Key Technologies:** Azure Key Vault, ACR geo-replication, Azure Traffic Manager, cost optimization tools

## Estimated Time

**Total Module Time:** 8-10 hours

- Presentation review: 1 hour
- Lab 5.1: 2-3 hours
- Lab 5.2: 3-4 hours
- Lab 5.3: 3-4 hours

You can complete the labs over multiple sessions. Each lab includes checkpoints where you can safely stop and resume.

## Getting Started

1. **Review the Presentation**
   ```bash
   # Open in your browser
   open presentation.html
   # or
   start presentation.html
   ```

2. **Set Up Your Environment**
   Ensure you have the required tools from Module 2:
   ```bash
   # Verify installations
   az --version
   docker --version
   kubectl version --client
   
   # Login to Azure
   az login
   
   # Set your subscription
   az account set --subscription "<your-subscription-id>"
   ```

3. **Start with Lab 5.1**
   Begin with monitoring setup to establish observability before troubleshooting.

## Lab Completion Checklist

Track your progress through Module 5:

### Lab 5.1: Monitoring and Observability
- [ ] Enable Container Insights on AKS cluster
- [ ] Integrate Application Insights with .NET app
- [ ] Write and execute KQL queries
- [ ] Create custom dashboards
- [ ] Configure alerts for critical metrics
- [ ] (Optional) Set up Prometheus and Grafana
- [ ] Implement OpenTelemetry distributed tracing

### Lab 5.2: Troubleshooting Techniques
- [ ] Debug containers locally with Docker
- [ ] View logs from ACI and AKS containers
- [ ] Resolve image pull errors
- [ ] Fix networking issues
- [ ] Address OOMKilled errors
- [ ] Optimize application performance
- [ ] Use kubectl for Kubernetes troubleshooting

### Lab 5.3: Production Best Practices
- [ ] Create and configure Azure Key Vault
- [ ] Integrate Key Vault with Container Apps/AKS
- [ ] Enable ACR geo-replication
- [ ] Set up backup procedures
- [ ] Implement cost optimization strategies
- [ ] Harden security with network policies
- [ ] Deploy multi-region architecture

## Key Concepts

### Observability
The ability to understand the internal state of your system by examining its outputs:
- **Metrics:** Numerical values that change over time (CPU, memory, request rate)
- **Logs:** Timestamped records of discrete events
- **Traces:** Records showing the path of a request through multiple services

### Monitoring vs. Observability
- **Monitoring:** Watching known metrics and setting alerts
- **Observability:** Understanding system behavior by asking questions of your data

### Azure Monitor Stack
```
Application Insights → Application-level telemetry
         ↓
Container Insights → Container-level metrics
         ↓
Log Analytics → Query and analyze all data
         ↓
Azure Monitor → Alerts, dashboards, actions
```

## Essential Tools and Commands

### Azure Monitor
```bash
# Enable Container Insights
az aks enable-addons \
  --resource-group rg-containers \
  --name myakscluster \
  --addons monitoring

# Create metric alert
az monitor metrics alert create \
  --name high-cpu-alert \
  --resource-group rg-containers \
  --scopes <resource-id> \
  --condition "avg Percentage CPU > 80"
```

### Troubleshooting Commands
```bash
# Docker debugging
docker logs <container-id>
docker exec -it <container-id> /bin/bash
docker inspect <container-id>

# ACI debugging
az container logs --resource-group rg --name myapi
az container exec --resource-group rg --name myapi --exec-command "/bin/bash"

# AKS debugging
kubectl logs <pod-name>
kubectl exec -it <pod-name> -- /bin/bash
kubectl describe pod <pod-name>
kubectl top pod <pod-name>
```

### Key Vault
```bash
# Create Key Vault
az keyvault create \
  --name mykeyvault \
  --resource-group rg-containers

# Store secret
az keyvault secret set \
  --vault-name mykeyvault \
  --name ConnectionString \
  --value "Server=..."

# Retrieve secret
az keyvault secret show \
  --vault-name mykeyvault \
  --name ConnectionString
```

## Common KQL Queries

### View Container Logs
```kusto
ContainerLog
| where TimeGenerated > ago(1h)
| where Name contains "mywebapi"
| project TimeGenerated, LogEntry
| order by TimeGenerated desc
```

### CPU Usage Over Time
```kusto
Perf
| where ObjectName == "K8SContainer"
| where CounterName == "cpuUsageNanoCores"
| summarize AvgCPU = avg(CounterValue) by bin(TimeGenerated, 5m), InstanceName
| render timechart
```

### Failed Requests
```kusto
requests
| where success == false
| summarize Count = count() by name, resultCode
| order by Count desc
```

### Exception Analysis
```kusto
exceptions
| where timestamp > ago(24h)
| summarize Count = count() by type, outerMessage
| order by Count desc
| take 10
```

## Troubleshooting Guide

### Common Issues and Solutions

| Issue | Symptoms | First Steps |
|-------|----------|-------------|
| **Image Pull Error** | Pod stuck in `ImagePullBackOff` | Verify ACR credentials, check image exists |
| **CrashLoopBackOff** | Container repeatedly crashes | Check logs with `kubectl logs`, verify config |
| **OOMKilled** | Container killed due to memory | Increase memory limit, check for memory leaks |
| **Network Issues** | Can't connect to services | Test DNS, verify network policies, check NSG rules |
| **Slow Performance** | High latency, timeouts | Check resource usage, profile application, review queries |
| **Authentication Failures** | 401/403 errors | Verify managed identity, check Key Vault access policies |

### Debugging Workflow
```
1. Identify the problem (alerts, user reports)
   ↓
2. Check logs (kubectl logs, Application Insights)
   ↓
3. Verify configuration (kubectl describe, az show)
   ↓
4. Test connectivity (kubectl exec, curl, nslookup)
   ↓
5. Review metrics (Azure Monitor, Container Insights)
   ↓
6. Apply fix
   ↓
7. Verify resolution
   ↓
8. Document issue and resolution
```

## Best Practices Summary

### Monitoring Best Practices
- ✅ Implement all three pillars: metrics, logs, and traces
- ✅ Use structured logging with appropriate log levels
- ✅ Set up alerts for critical metrics
- ✅ Create dashboards for different audiences (ops, devs, business)
- ✅ Use correlation IDs for distributed tracing
- ✅ Monitor business metrics, not just technical metrics

### Security Best Practices
- ✅ Store secrets in Azure Key Vault, never in code or environment variables
- ✅ Use managed identities for authentication
- ✅ Scan container images for vulnerabilities
- ✅ Implement network policies to restrict traffic
- ✅ Use RBAC for access control
- ✅ Enable Azure Defender for containers

### Reliability Best Practices
- ✅ Implement health checks (liveness and readiness probes)
- ✅ Set appropriate resource limits
- ✅ Use autoscaling for variable workloads
- ✅ Deploy across multiple regions for high availability
- ✅ Test disaster recovery procedures regularly
- ✅ Implement circuit breakers and retry policies

### Performance Best Practices
- ✅ Use multi-stage builds to reduce image size
- ✅ Implement ReadyToRun (R2R) compilation for faster startup
- ✅ Use connection pooling for databases
- ✅ Cache frequently accessed data
- ✅ Optimize queries and avoid N+1 problems
- ✅ Use async/await for I/O operations

### Cost Optimization Best Practices
- ✅ Right-size resources based on actual usage
- ✅ Use scale-to-zero for idle workloads (Container Apps)
- ✅ Implement autoscaling to match demand
- ✅ Use spot instances for non-critical workloads
- ✅ Clean up unused resources and old images
- ✅ Set up budget alerts to track spending

## Production Readiness Checklist

Before deploying to production, ensure:

### Infrastructure
- [ ] Multi-region deployment configured
- [ ] Load balancing and traffic management in place
- [ ] Autoscaling policies defined
- [ ] Network security groups and policies configured
- [ ] Backup and disaster recovery procedures documented

### Monitoring
- [ ] Application Insights integrated
- [ ] Container Insights enabled
- [ ] Custom dashboards created
- [ ] Critical alerts configured
- [ ] On-call rotation established

### Security
- [ ] Secrets stored in Key Vault
- [ ] Managed identities configured
- [ ] Image vulnerability scanning enabled
- [ ] Network policies implemented
- [ ] RBAC properly configured
- [ ] Azure Defender enabled

### Performance
- [ ] Load testing completed
- [ ] Resource limits optimized
- [ ] Application profiled for bottlenecks
- [ ] Database queries optimized
- [ ] Caching strategy implemented

### Operations
- [ ] Runbooks documented for common issues
- [ ] Disaster recovery plan tested
- [ ] Change management process established
- [ ] Team trained on troubleshooting procedures
- [ ] Post-incident review process defined

## Additional Resources

### Microsoft Documentation
- [Azure Monitor Overview](https://learn.microsoft.com/en-us/azure/azure-monitor/)
- [Container Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-overview)
- [Application Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview)
- [KQL Reference](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/)
- [Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/)
- [Azure Well-Architected Framework](https://learn.microsoft.com/en-us/azure/architecture/framework/)

### Kubernetes Documentation
- [Kubernetes Troubleshooting](https://kubernetes.io/docs/tasks/debug/)
- [Debug Running Pods](https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

### OpenTelemetry
- [OpenTelemetry Overview](https://opentelemetry.io/)
- [.NET Distributed Tracing](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/distributed-tracing)
- [OpenTelemetry for .NET](https://opentelemetry.io/docs/instrumentation/net/)

### Tools
- [Azure Pricing Calculator](https://azure.microsoft.com/en-us/pricing/calculator/)
- [Azure Cost Management](https://learn.microsoft.com/en-us/azure/cost-management-billing/)
- [Prometheus Documentation](https://prometheus.io/docs/)
- [Grafana Documentation](https://grafana.com/docs/)

## Support and Feedback

If you encounter issues or have questions:

1. **Check the troubleshooting guide** in each lab
2. **Review the common issues** section above
3. **Search Azure documentation** for specific error messages
4. **Consult with your team** or instructor
5. **Open an issue** in the training repository

## What's Next?

Congratulations on completing Module 5 and the entire Container-Based Development training course! 🎉

You've learned:
- ✅ Container fundamentals (Module 1)
- ✅ .NET containerization (Module 2)
- ✅ Azure Container Registry (Module 3)
- ✅ Azure deployment options (Module 4)
- ✅ Monitoring and production best practices (Module 5)

### Recommended Next Steps:

1. **Apply your knowledge** to real Progress Software projects
2. **Earn certifications:**
   - [Microsoft Certified: Azure Developer Associate](https://learn.microsoft.com/en-us/certifications/azure-developer/)
   - [Microsoft Certified: DevOps Engineer Expert](https://learn.microsoft.com/en-us/certifications/devops-engineer/)

3. **Explore advanced topics:**
   - Service mesh (Istio, Linkerd)
   - GitOps with Flux or ArgoCD
   - Advanced Kubernetes patterns
   - Serverless containers with Azure Container Apps

4. **Join communities:**
   - [Azure Community](https://techcommunity.microsoft.com/t5/azure/ct-p/Azure)
   - [Kubernetes Community](https://kubernetes.io/community/)
   - [.NET Community](https://dotnet.microsoft.com/platform/community)

5. **Keep learning:**
   - [Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)
   - [Cloud Design Patterns](https://learn.microsoft.com/en-us/azure/architecture/patterns/)
   - [Microservices Architecture](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/microservices)

## Course Completion Certificate

You have successfully completed the **Container-Based Development with .NET on Azure** training course!

**Skills Acquired:**
- Container fundamentals and Docker
- .NET application containerization
- Azure Container Registry management
- Azure container deployment (ACI, ACA, AKS)
- CI/CD pipelines for containers
- Production monitoring and troubleshooting
- Azure security and cost optimization

**Time Invested:** 35-45 hours of hands-on training

**Next Step:** Apply these skills to migrate Progress Software applications to containerized deployments on Azure!

---

**Ready to get started?** Open [presentation.html](./presentation.html) to begin Module 5, then proceed to [Lab 5.1](./5.1-monitoring-observability.md).

**Questions?** Contact your training coordinator or open an issue in the training repository.

Good luck with your production deployments! 🚀
