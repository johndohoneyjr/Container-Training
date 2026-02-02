# Migration to Container-Based Development with .NET
## Training Curriculum

**Target Audience:** Progress Software Development Teams  
**Level:** Intermediate to Advanced

---

## Course Overview

This comprehensive training program guides developers through the complete journey of migrating .NET applications to container-based development. The curriculum covers fundamental container concepts, modern security practices, .NET-specific containerization techniques, Azure Container Registry management, and deployment options in Azure.

---

## Module 1: Container Fundamentals

### Learning Objectives
- Understand container architecture and runtime environments
- Differentiate between containers and virtual machines
- Comprehend container ecosystem components (images, registries, orchestration)
- Recognize container use cases and benefits for .NET applications

### Topics Covered

#### 1.1 Introduction to Containers
- What are containers and how do they work?
- Container vs. Virtual Machine architecture
- Container runtime environments (containerd, CRI-O)
- The evolution from monoliths to microservices
- Container standards (OCI - Open Container Initiative)

#### 1.2 Container Ecosystem
- Docker architecture and components
  - Docker Engine
  - Docker CLI
  - Docker daemon
- Container images and layers
- Container registries (public vs. private)
- Container orchestration overview (Kubernetes, Docker Swarm, Azure Container Apps)

#### 1.3 Current Trends in Container Security
- **Shift-Left Security in Development**
  - Integrating security early in the development lifecycle
  - Security scanning in CI/CD pipelines
  - Developer-centric security tools
  - Policy-as-Code implementation
  
- **Docker Hardened Images**
  - Near-zero CVEs with minimal attack surface
  - SLSA Level 3 provenance and verifiable SBOMs
  - Drop-in adoption with multi-distro compatibility (Alpine, Debian)
  - Transparent supply chain security
  - Apache 2.0 licensing for DHI
  - Enterprise options: FIPS/STIG compliance, 7-day CVE patches
  - Extended Lifecycle Support (ELS): +5 years of hardened updates
  - Non-root containers and principle of least privilege
  
- **Supply Chain Security**
  - Software Bill of Materials (SBOM)
  - Container image signing and verification
  - Vulnerability scanning and remediation
  - Base image selection and maintenance
  - Dependency management

#### 1.4 Container Security Best Practices
- Minimal base images
- Multi-stage builds for reduced attack surface
- Secrets management (avoiding hardcoded credentials)
- Runtime security and monitoring
- Network segmentation and policies
- Immutable infrastructure principles

### Resources
- Docker Official Documentation
- OCI Specifications
- Docker Hardened Images Catalog
- NIST Container Security Guidelines

---

## Module 2: .NET Containerization

### Learning Objectives
- Install and configure Docker for .NET development
- Create optimized Dockerfiles for .NET applications
- Build multi-stage Docker images for .NET
- Implement security best practices in .NET containers
- Debug containerized .NET applications

### Topics Covered

#### 2.1 Environment Setup

##### Lab: Installing Docker Desktop
- **Windows Installation**
  - System requirements (WSL2, Hyper-V)
  - Docker Desktop installation and configuration
  - WSL2 integration setup
  - Docker Desktop settings optimization
  
- **macOS Installation**
  - System requirements (Intel vs. Apple Silicon)
  - Docker Desktop installation
  - Resource allocation configuration
  
- **Linux Installation**
  - Docker Engine installation (Ubuntu, RHEL, CentOS)
  - Post-installation steps (non-root user)
  - Docker Compose installation
  
- **Verification Steps**
  ```bash
  docker --version
  docker run hello-world
  docker info
  ```

##### Lab: Installing ORAS CLI
- **What is ORAS?**
  - OCI Registry As Storage
  - Artifact management beyond container images
  - Use cases: Helm charts, configuration files, ML models
  
- **Installation Across Platforms**
  - Windows: Chocolatey, Scoop, or manual installation
  - macOS: Homebrew installation
  - Linux: Package managers and binary installation
  
- **Verification**
  ```bash
  oras version
  oras help
  ```

#### 2.2 Understanding .NET Container Images
- Microsoft Container Registry (MCR)
- Official .NET runtime images hierarchy
  - `mcr.microsoft.com/dotnet/sdk` - Development image
  - `mcr.microsoft.com/dotnet/aspnet` - ASP.NET Core runtime
  - `mcr.microsoft.com/dotnet/runtime` - .NET runtime
  - `mcr.microsoft.com/dotnet/runtime-deps` - Minimal dependencies
- Alpine vs. Debian variants
- Version tagging strategies (LTS, STS, specific versions)
- Image size optimization considerations
- Docker Hardened Images for .NET (dhia.io/dotnet)

#### 2.3 Creating Dockerfiles for .NET Applications

##### Theory: Dockerfile Anatomy for .NET
- Dockerfile instructions and best practices
- Layer caching optimization
- Build context management
- .dockerignore configuration

##### Lab: Basic .NET Console Application Dockerfile
```dockerfile
# Basic single-stage Dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app
COPY *.csproj .
RUN dotnet restore
COPY . .
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/runtime:8.0
WORKDIR /app
COPY --from=build /app/out .
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

##### Lab: ASP.NET Core Web API Dockerfile (Multi-stage)
```dockerfile
# Multi-stage build for ASP.NET Core
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["MyWebApi/MyWebApi.csproj", "MyWebApi/"]
RUN dotnet restore "MyWebApi/MyWebApi.csproj"
COPY . .
WORKDIR "/src/MyWebApi"
RUN dotnet build "MyWebApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MyWebApi.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
EXPOSE 8080
EXPOSE 8081
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MyWebApi.dll"]
```

##### Lab: Optimized Dockerfile with Security Hardening
```dockerfile
# Hardened multi-stage build
FROM mcr.microsoft.com/dotnet/sdk:8.0-alpine AS build
WORKDIR /src

# Copy and restore dependencies first (layer caching)
COPY ["*.sln", "./"]
COPY ["MyWebApi/*.csproj", "MyWebApi/"]
RUN dotnet restore "MyWebApi/MyWebApi.csproj"

# Copy source and build
COPY . .
WORKDIR "/src/MyWebApi"
RUN dotnet build "MyWebApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MyWebApi.csproj" -c Release -o /app/publish \
    /p:UseAppHost=false \
    --no-restore

# Final stage with hardened base image
FROM dhia.io/dotnet/aspnet:8.0-alpine AS final
# Create non-root user
RUN addgroup -g 1000 appuser && adduser -D -u 1000 -G appuser appuser
WORKDIR /app
EXPOSE 8080

# Copy published app
COPY --from=publish --chown=appuser:appuser /app/publish .

# Switch to non-root user
USER appuser

HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:8080/health || exit 1

ENTRYPOINT ["dotnet", "MyWebApi.dll"]
```

#### 2.4 Building and Running .NET Containers

##### Lab: Build and Run Commands
```bash
# Build the image
docker build -t mywebapi:1.0 .

# Run the container
docker run -d -p 8080:8080 --name myapi mywebapi:1.0

# View logs
docker logs myapi

# Execute commands in running container
docker exec -it myapi /bin/sh

# Stop and remove
docker stop myapi
docker rm myapi
```

##### Lab: Docker Compose for Multi-Container .NET Applications
```yaml
version: '3.8'
services:
  api:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "8080:8080"
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ConnectionStrings__DefaultConnection=Server=db;Database=MyDb;User=sa;Password=YourStrong@Passw0rd
    depends_on:
      - db
  
  db:
    image: mcr.microsoft.com/mssql/server:2022-latest
    environment:
      - ACCEPT_EULA=Y
      - SA_PASSWORD=YourStrong@Passw0rd
    ports:
      - "1433:1433"
    volumes:
      - sqldata:/var/opt/mssql

volumes:
  sqldata:
```

#### 2.5 .NET Container Best Practices

##### Security Best Practices
- Use official Microsoft images or Docker Hardened Images
- Implement multi-stage builds to minimize final image size
- Run containers as non-root users
- Scan images for vulnerabilities using Docker Scout or Trivy
- Use specific version tags (avoid `latest` in production)
- Implement health checks
- Manage secrets with Docker secrets or environment variables from secure stores

##### Performance Best Practices
- Optimize layer caching (COPY dependencies before source code)
- Use `.dockerignore` to exclude unnecessary files
- Enable ReadyToRun (R2R) compilation for faster startup
- Consider trimming and AOT compilation for .NET 8+
- Use Alpine images for smaller footprint
- Implement proper logging and diagnostics

##### Lab: Scanning and Securing .NET Images
```bash
# Scan with Docker Scout
docker scout cves mywebapi:1.0

# Scan with Trivy
trivy image mywebapi:1.0

# Generate SBOM
docker sbom mywebapi:1.0

# Sign image with Docker Content Trust
export DOCKER_CONTENT_TRUST=1
docker push mywebapi:1.0
```

### Hands-On Projects
- Containerize an existing .NET Web API
- Containerize a .NET console application with worker service
- Create a multi-container application with .NET API + SQL Server
- Optimize image size and build time
- Implement health checks and monitoring

---

## Module 3: Azure Container Registry (ACR)

### Learning Objectives
- Provision and configure Azure Container Registry
- Push and pull container images to/from ACR
- Implement ACR security and access controls
- Use ORAS CLI for OCI artifact management
- Automate ACR tasks and image maintenance

### Topics Covered

#### 3.1 Environment Setup for Azure

##### Lab: Installing Azure CLI
- **Windows**: MSI installer, winget, or Chocolatey
- **macOS**: Homebrew installation
- **Linux**: Package manager installation
  
```bash
# Verify installation
az --version

# Login to Azure
az login

# Set subscription
az account set --subscription "Your Subscription Name"
```

##### Lab: Configuring ORAS CLI for ACR Integration
```bash
# Configure ORAS for ACR authentication
az acr login --name myregistry

# Test ORAS connectivity
oras version
```

#### 3.2 Azure Container Registry Fundamentals

##### Theory: ACR Overview
- ACR tiers: Basic, Standard, Premium
- Geo-replication capabilities (Premium)
- ACR Tasks for automated builds
- Webhooks and event notifications
- Integration with Azure services (AKS, Container Apps, App Service)

##### Lab: Provisioning ACR
```bash
# Create resource group
az group create --name rg-containers --location eastus

# Create ACR (Premium tier for geo-replication)
az acr create \
  --resource-group rg-containers \
  --name myuniqueregistry \
  --sku Premium \
  --location eastus

# Enable admin user (not recommended for production)
az acr update --name myuniqueregistry --admin-enabled true

# Get login credentials
az acr credential show --name myuniqueregistry
```

##### Lab: Portal-Based ACR Creation
- Navigate to Azure Portal
- Create ACR resource with appropriate settings
- Configure networking and access controls
- Review monitoring and diagnostics options

#### 3.3 Working with Container Images in ACR

##### Lab: Pushing .NET Images to ACR
```bash
# Login to ACR
az acr login --name myuniqueregistry

# Tag image for ACR
docker tag mywebapi:1.0 myuniqueregistry.azurecr.io/mywebapi:1.0
docker tag mywebapi:1.0 myuniqueregistry.azurecr.io/mywebapi:latest

# Push to ACR
docker push myuniqueregistry.azurecr.io/mywebapi:1.0
docker push myuniqueregistry.azurecr.io/mywebapi:latest

# List images in ACR
az acr repository list --name myuniqueregistry --output table

# Show image tags
az acr repository show-tags --name myuniqueregistry --repository mywebapi --output table
```

##### Lab: Pulling Images from ACR
```bash
# Pull from ACR
docker pull myuniqueregistry.azurecr.io/mywebapi:1.0

# Run container from ACR image
docker run -d -p 8080:8080 myuniqueregistry.azurecr.io/mywebapi:1.0
```

##### Lab: Managing Image Lifecycle
```bash
# Delete image tag
az acr repository delete \
  --name myuniqueregistry \
  --image mywebapi:old-version \
  --yes

# Purge old images (Premium only)
az acr run \
  --registry myuniqueregistry \
  --cmd "acr purge --filter 'mywebapi:.*' --ago 30d --untagged" \
  /dev/null

# Enable content trust
az acr config content-trust update --registry myuniqueregistry --status enabled
```

#### 3.4 ORAS CLI for OCI Artifacts

##### Theory: OCI Artifacts Beyond Images
- What are OCI artifacts?
- Use cases: Helm charts, Terraform modules, configuration files, ML models
- ORAS vs. Docker CLI
- ACR support for OCI artifacts

##### Lab: Pushing Artifacts with ORAS
```bash
# Login ORAS to ACR
oras login myuniqueregistry.azurecr.io --username <username> --password <password>

# Or use Azure identity
az acr login --name myuniqueregistry
export REGISTRY=myuniqueregistry.azurecr.io

# Push a configuration file as artifact
echo "config data" > config.json
oras push $REGISTRY/configs/myapp:v1 \
  --artifact-type application/vnd.mycompany.config.v1+json \
  config.json:application/json

# Push multiple files
oras push $REGISTRY/artifacts/deployment:v1 \
  --artifact-type application/vnd.mycompany.deployment.v1+tar \
  ./deployment.yaml:application/yaml \
  ./values.yaml:application/yaml \
  ./README.md:text/markdown

# List artifacts
oras repo tags $REGISTRY/configs/myapp

# Pull artifacts
oras pull $REGISTRY/configs/myapp:v1

# Discover artifact referrers
oras discover $REGISTRY/mywebapi:1.0
```

##### Lab: Storing Helm Charts in ACR
```bash
# Enable Helm support in ACR
az acr helm repo add --name myuniqueregistry

# Package Helm chart
helm package ./mychart

# Push Helm chart to ACR
az acr helm push -n myuniqueregistry mychart-1.0.0.tgz

# List Helm charts
az acr helm list -n myuniqueregistry

# Install from ACR
helm repo add myrepo https://myuniqueregistry.azurecr.io/helm/v1/repo
helm install myapp myrepo/mychart
```

##### Lab: Attaching Signatures and SBOMs
```bash
# Generate SBOM
docker sbom mywebapi:1.0 --format spdx-json --output sbom.spdx.json

# Attach SBOM to image in ACR
oras attach $REGISTRY/mywebapi:1.0 \
  --artifact-type application/vnd.example.sbom.v1+json \
  sbom.spdx.json:application/json

# Verify attachment
oras discover $REGISTRY/mywebapi:1.0
```

#### 3.5 ACR Security and Access Control

##### Lab: Implementing Authentication and Authorization
```bash
# Create service principal for ACR access
az ad sp create-for-rbac \
  --name acr-service-principal \
  --scopes /subscriptions/{subscription-id}/resourceGroups/rg-containers/providers/Microsoft.ContainerRegistry/registries/myuniqueregistry \
  --role acrpull

# Assign roles
az role assignment create \
  --assignee <service-principal-id> \
  --scope /subscriptions/{subscription-id}/resourceGroups/rg-containers/providers/Microsoft.ContainerRegistry/registries/myuniqueregistry \
  --role acrpush

# Use managed identity
az acr credential renew --name myuniqueregistry --password-name password
```

##### Lab: Network Security
```bash
# Disable public access (Premium only)
az acr update --name myuniqueregistry --public-network-enabled false

# Create private endpoint
az network private-endpoint create \
  --resource-group rg-containers \
  --name myacr-private-endpoint \
  --vnet-name myvnet \
  --subnet mysubnet \
  --private-connection-resource-id $(az acr show --name myuniqueregistry --query id --output tsv) \
  --group-id registry \
  --connection-name myacr-connection

# Configure firewall rules
az acr network-rule add \
  --name myuniqueregistry \
  --ip-address 40.76.54.131
```

##### Lab: Image Scanning and Security
```bash
# Enable Microsoft Defender for Containers
az security pricing create \
  --name Containers \
  --tier Standard

# Scan image with ACR Tasks
az acr task create \
  --registry myuniqueregistry \
  --name security-scan \
  --cmd "docker scan mywebapi:{{.Run.ID}}" \
  --context /dev/null

# View scan results
az acr repository show-tags \
  --name myuniqueregistry \
  --repository mywebapi \
  --detail
```

### Hands-On Projects
- Provision ACR with geo-replication
- Migrate existing container images to ACR
- Set up automated builds with ACR Tasks
- Implement image retention policies
- Create artifact repository with ORAS
- Configure private endpoint for secure access

---

## Module 4: Azure Container Deployment Options

### Learning Objectives
- Compare Azure container hosting services
- Deploy .NET containers to Azure Container Apps
- Deploy .NET containers to Azure App Service
- Deploy .NET containers to Azure Kubernetes Service (AKS)
- Implement CI/CD for container deployments
- Monitor and troubleshoot containerized applications

### Topics Covered

#### 4.1 Azure Container Services Overview

##### Theory: Comparing Azure Container Options

| Service | Use Case | Orchestration | Complexity | Scaling | Cost Model |
|---------|----------|---------------|------------|---------|------------|
| **Azure Container Instances (ACI)** | Simple, short-lived containers | None | Low | Manual/Scheduled | Per-second |
| **Azure Container Apps (ACA)** | Microservices, event-driven apps | Managed Kubernetes (KEDA) | Low-Medium | Auto (HTTP, Queue, CPU) | Consumption + Reserved |
| **Azure App Service (Containers)** | Web apps, APIs | App Service platform | Low | Auto (rules-based) | App Service Plan |
| **Azure Kubernetes Service (AKS)** | Complex orchestration, full control | Kubernetes | High | Auto (HPA, CA, KEDA) | Node-based |
| **Azure Container Apps Jobs** | Batch processing, scheduled tasks | Managed | Low | Job-based | Per execution |

##### Decision Matrix
- **Choose ACI**: Quick tests, batch jobs, burst scenarios
- **Choose Container Apps**: Modern microservices, serverless containers, event-driven
- **Choose App Service**: Traditional web apps, existing App Service users
- **Choose AKS**: Enterprise workloads, need Kubernetes features, multi-cloud strategy

##### Lab: Explore Azure Portal Container Services
- Review each service's capabilities in Azure Portal
- Compare pricing calculators
- Review SLA and availability options

#### 4.2 Azure Container Instances (ACI)

##### Lab: Deploy .NET Container to ACI
```bash
# Create container group
az container create \
  --resource-group rg-containers \
  --name myapi-aci \
  --image myuniqueregistry.azurecr.io/mywebapi:1.0 \
  --registry-login-server myuniqueregistry.azurecr.io \
  --registry-username <username> \
  --registry-password <password> \
  --dns-name-label myapi-unique \
  --ports 8080 \
  --cpu 1 \
  --memory 1.5 \
  --environment-variables ASPNETCORE_ENVIRONMENT=Production

# Get container details
az container show \
  --resource-group rg-containers \
  --name myapi-aci \
  --output table

# View logs
az container logs \
  --resource-group rg-containers \
  --name myapi-aci

# Execute command
az container exec \
  --resource-group rg-containers \
  --name myapi-aci \
  --exec-command "/bin/sh"
```

##### Lab: Multi-Container Groups with ACI
```yaml
# YAML definition for multi-container group
apiVersion: '2021-09-01'
location: eastus
name: myapp-group
properties:
  containers:
  - name: api
    properties:
      image: myuniqueregistry.azurecr.io/mywebapi:1.0
      resources:
        requests:
          cpu: 1.0
          memoryInGb: 1.5
      ports:
      - port: 8080
  - name: sidecar
    properties:
      image: myuniqueregistry.azurecr.io/logger:1.0
      resources:
        requests:
          cpu: 0.5
          memoryInGb: 0.5
  osType: Linux
  restartPolicy: Always
  imageRegistryCredentials:
  - server: myuniqueregistry.azurecr.io
    username: <username>
    password: <password>
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

```bash
# Deploy multi-container group
az container create --resource-group rg-containers --file aci-config.yaml
```

#### 4.3 Azure Container Apps (ACA)

##### Theory: Container Apps Architecture
- Dapr integration for microservices
- KEDA-based autoscaling
- Ingress and traffic splitting
- Revisions and versioning
- Environment variables and secrets
- Managed identity integration

##### Lab: Environment Setup for Container Apps
```bash
# Install Container Apps extension
az extension add --name containerapp --upgrade

# Create Container Apps environment
az containerapp env create \
  --name myenv \
  --resource-group rg-containers \
  --location eastus \
  --logs-workspace-id <log-analytics-workspace-id> \
  --logs-workspace-key <log-analytics-key>
```

##### Lab: Deploy .NET Container to Container Apps
```bash
# Create container app
az containerapp create \
  --name mywebapi \
  --resource-group rg-containers \
  --environment myenv \
  --image myuniqueregistry.azurecr.io/mywebapi:1.0 \
  --registry-server myuniqueregistry.azurecr.io \
  --registry-username <username> \
  --registry-password <password> \
  --target-port 8080 \
  --ingress external \
  --cpu 0.5 \
  --memory 1.0Gi \
  --min-replicas 1 \
  --max-replicas 10 \
  --env-vars ASPNETCORE_ENVIRONMENT=Production

# Get app URL
az containerapp show \
  --name mywebapi \
  --resource-group rg-containers \
  --query properties.configuration.ingress.fqdn \
  --output tsv

# Update with new revision
az containerapp update \
  --name mywebapi \
  --resource-group rg-containers \
  --image myuniqueregistry.azurecr.io/mywebapi:2.0

# View revisions
az containerapp revision list \
  --name mywebapi \
  --resource-group rg-containers \
  --output table
```

##### Lab: Autoscaling Configuration
```bash
# Configure HTTP-based autoscaling
az containerapp update \
  --name mywebapi \
  --resource-group rg-containers \
  --min-replicas 1 \
  --max-replicas 20 \
  --scale-rule-name http-rule \
  --scale-rule-type http \
  --scale-rule-http-concurrency 50

# Configure queue-based autoscaling (with Dapr)
az containerapp update \
  --name myworker \
  --resource-group rg-containers \
  --min-replicas 0 \
  --max-replicas 10 \
  --scale-rule-name queue-rule \
  --scale-rule-type azure-queue \
  --scale-rule-metadata queueName=myqueue accountName=mystorageaccount queueLength=5
```

##### Lab: Traffic Splitting and Blue-Green Deployment
```bash
# Split traffic between revisions
az containerapp ingress traffic set \
  --name mywebapi \
  --resource-group rg-containers \
  --revision-weight mywebapi--v1=80 mywebapi--v2=20

# Shift all traffic to new revision
az containerapp ingress traffic set \
  --name mywebapi \
  --resource-group rg-containers \
  --revision-weight mywebapi--v2=100
```

#### 4.4 Azure App Service with Containers

##### Lab: Deploy .NET Container to App Service
```bash
# Create App Service Plan (Linux)
az appservice plan create \
  --name myappplan \
  --resource-group rg-containers \
  --is-linux \
  --sku P1V2

# Create Web App with container
az webapp create \
  --resource-group rg-containers \
  --plan myappplan \
  --name mywebapi-app \
  --deployment-container-image-name myuniqueregistry.azurecr.io/mywebapi:1.0

# Configure ACR credentials
az webapp config container set \
  --name mywebapi-app \
  --resource-group rg-containers \
  --docker-registry-server-url https://myuniqueregistry.azurecr.io \
  --docker-registry-server-user <username> \
  --docker-registry-server-password <password>

# Enable continuous deployment
az webapp deployment container config \
  --name mywebapi-app \
  --resource-group rg-containers \
  --enable-cd true

# Get webhook URL for CI/CD
az webapp deployment container show-cd-url \
  --name mywebapi-app \
  --resource-group rg-containers
```

##### Lab: Configure App Service Settings
```bash
# Set environment variables
az webapp config appsettings set \
  --resource-group rg-containers \
  --name mywebapi-app \
  --settings ASPNETCORE_ENVIRONMENT=Production \
    ConnectionStrings__DefaultConnection="<connection-string>"

# Configure scaling
az webapp scale \
  --resource-group rg-containers \
  --name mywebapi-app \
  --instance-count 3

# Enable auto-scaling
az monitor autoscale create \
  --resource-group rg-containers \
  --resource mywebapi-app \
  --resource-type Microsoft.Web/sites \
  --name autoscale-rules \
  --min-count 1 \
  --max-count 10 \
  --count 2

# Add CPU-based scaling rule
az monitor autoscale rule create \
  --resource-group rg-containers \
  --autoscale-name autoscale-rules \
  --condition "CpuPercentage > 70 avg 5m" \
  --scale out 1
```

#### 4.5 Azure Kubernetes Service (AKS)

##### Theory: AKS Architecture and Concepts
- Kubernetes fundamentals (pods, services, deployments)
- AKS cluster architecture
- Node pools and scaling
- Networking (CNI, Kubenet)
- ACR integration with AKS
- Managed identity and RBAC

##### Lab: Provision AKS Cluster
```bash
# Create AKS cluster with ACR integration
az aks create \
  --resource-group rg-containers \
  --name myakscluster \
  --node-count 3 \
  --node-vm-size Standard_D2s_v3 \
  --enable-managed-identity \
  --attach-acr myuniqueregistry \
  --network-plugin azure \
  --enable-addons monitoring \
  --generate-ssh-keys

# Get credentials
az aks get-credentials \
  --resource-group rg-containers \
  --name myakscluster

# Verify connection
kubectl get nodes
```

##### Lab: Deploy .NET Application to AKS
```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mywebapi
  namespace: default
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mywebapi
  template:
    metadata:
      labels:
        app: mywebapi
    spec:
      containers:
      - name: mywebapi
        image: myuniqueregistry.azurecr.io/mywebapi:1.0
        ports:
        - containerPort: 8080
        env:
        - name: ASPNETCORE_ENVIRONMENT
          value: "Production"
        resources:
          requests:
            cpu: 250m
            memory: 512Mi
          limits:
            cpu: 500m
            memory: 1Gi
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /health/ready
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: mywebapi-service
  namespace: default
spec:
  type: LoadBalancer
  selector:
    app: mywebapi
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```

```bash
# Apply deployment
kubectl apply -f deployment.yaml

# Get service external IP
kubectl get service mywebapi-service

# View pods
kubectl get pods -l app=mywebapi

# View logs
kubectl logs -l app=mywebapi --tail=50

# Scale deployment
kubectl scale deployment mywebapi --replicas=5
```

##### Lab: Ingress and Helm Charts
```bash
# Install NGINX ingress controller
kubectl create namespace ingress-nginx
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-health-probe-request-path"=/healthz

# Create ingress resource
kubectl apply -f - <<EOF
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: mywebapi-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: mywebapi.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: mywebapi-service
            port:
              number: 80
EOF

# Package application as Helm chart
helm create mywebapi-chart
# Customize values.yaml and templates
helm package mywebapi-chart
helm install mywebapi ./mywebapi-chart-0.1.0.tgz
```

##### Lab: AKS Autoscaling
```bash
# Enable cluster autoscaler
az aks update \
  --resource-group rg-containers \
  --name myakscluster \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 10

# Create Horizontal Pod Autoscaler (HPA)
kubectl autoscale deployment mywebapi \
  --cpu-percent=70 \
  --min=2 \
  --max=20

# View HPA status
kubectl get hpa mywebapi --watch
```

#### 4.6 CI/CD for Container Deployments

##### Lab: GitHub Actions for ACR and AKS
```yaml
# .github/workflows/deploy-aks.yml
name: Build and Deploy to AKS

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

env:
  ACR_NAME: myuniqueregistry
  AKS_CLUSTER: myakscluster
  RESOURCE_GROUP: rg-containers
  IMAGE_NAME: mywebapi

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    
    - name: Build and push to ACR
      run: |
        az acr build \
          --registry ${{ env.ACR_NAME }} \
          --image ${{ env.IMAGE_NAME }}:${{ github.sha }} \
          --image ${{ env.IMAGE_NAME }}:latest \
          --file Dockerfile .
    
    - name: Get AKS credentials
      run: |
        az aks get-credentials \
          --resource-group ${{ env.RESOURCE_GROUP }} \
          --name ${{ env.AKS_CLUSTER }}
    
    - name: Deploy to AKS
      run: |
        kubectl set image deployment/mywebapi \
          mywebapi=${{ env.ACR_NAME }}.azurecr.io/${{ env.IMAGE_NAME }}:${{ github.sha }}
        kubectl rollout status deployment/mywebapi
```

##### Lab: Azure DevOps Pipeline
```yaml
# azure-pipelines.yml
trigger:
  branches:
    include:
    - main

pool:
  vmImage: 'ubuntu-latest'

variables:
  acrName: 'myuniqueregistry'
  imageName: 'mywebapi'
  aksCluster: 'myakscluster'
  resourceGroup: 'rg-containers'

stages:
- stage: Build
  jobs:
  - job: BuildAndPush
    steps:
    - task: Docker@2
      displayName: Build and Push
      inputs:
        containerRegistry: 'ACR-ServiceConnection'
        repository: $(imageName)
        command: 'buildAndPush'
        Dockerfile: '**/Dockerfile'
        tags: |
          $(Build.BuildId)
          latest

- stage: Deploy
  dependsOn: Build
  jobs:
  - deployment: DeployToAKS
    environment: 'production'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: KubernetesManifest@0
            displayName: Deploy to AKS
            inputs:
              action: 'deploy'
              kubernetesServiceConnection: 'AKS-ServiceConnection'
              manifests: |
                $(Pipeline.Workspace)/manifests/deployment.yaml
                $(Pipeline.Workspace)/manifests/service.yaml
              containers: |
                $(acrName).azurecr.io/$(imageName):$(Build.BuildId)
```

##### Lab: ACR Tasks for Automated Builds
```bash
# Create ACR task for automated build on git commit
az acr task create \
  --registry myuniqueregistry \
  --name build-on-commit \
  --image mywebapi:{{.Run.ID}} \
  --context https://github.com/myorg/myrepo.git \
  --file Dockerfile \
  --branch main \
  --git-access-token <PAT>

# Create multi-step task
az acr task create \
  --registry myuniqueregistry \
  --name multi-step-build \
  --context https://github.com/myorg/myrepo.git \
  --file acr-task.yaml \
  --git-access-token <PAT>

# Run task manually
az acr task run --registry myuniqueregistry --name build-on-commit

# View task runs
az acr task list-runs --registry myuniqueregistry --output table
```

### Hands-On Projects
- Deploy multi-tier .NET application across ACI, ACA, and AKS
- Implement blue-green deployment on Container Apps
- Set up GitOps with Flux for AKS
- Create CI/CD pipeline with automated testing and deployment
- Implement container monitoring with Application Insights
- Configure disaster recovery and backup strategies

---

## Module 5: Monitoring, Troubleshooting, and Best Practices

### Learning Objectives
- Monitor containerized .NET applications in Azure
- Troubleshoot common container issues
- Implement logging and observability
- Optimize container performance and cost

### Topics Covered

#### 5.1 Monitoring and Observability
- Azure Monitor and Container Insights
- Application Insights for containerized apps
- Log Analytics queries
- Prometheus and Grafana integration
- Distributed tracing with OpenTelemetry

#### 5.2 Troubleshooting Techniques
- Debugging containers locally
- Remote debugging in Azure
- Common deployment issues
- Performance optimization
- Resource constraints and limits

#### 5.3 Production Best Practices
- High availability and disaster recovery
- Secrets management (Azure Key Vault)
- Cost optimization strategies
- Security hardening checklist
- Backup and recovery procedures

---

## Assessment and Certification

### Lab Projects (60%)
- Containerize a multi-service .NET application
- Deploy to multiple Azure container services
- Implement complete CI/CD pipeline
- Demonstrate security best practices

### Written Exam (40%)
- Container fundamentals and Docker architecture
- .NET containerization concepts
- Azure container services comparison
- Security and compliance requirements

---

## Additional Resources

### Documentation
- [Docker Documentation](https://docs.docker.com/)
- [.NET Docker Samples](https://github.com/dotnet/dotnet-docker)
- [Azure Container Registry Documentation](https://docs.microsoft.com/azure/container-registry/)
- [Azure Kubernetes Service Documentation](https://docs.microsoft.com/azure/aks/)
- [Azure Container Apps Documentation](https://docs.microsoft.com/azure/container-apps/)
- [Docker Hardened Images Catalog](https://hub.docker.com/hardened-images/catalog)
- [ORAS Documentation](https://oras.land/)

### Tools
- Docker Desktop
- Visual Studio / VS Code with Docker extension
- Azure CLI
- kubectl
- Helm
- ORAS CLI
- Docker Scout / Trivy

### Sample Code Repositories
- [.NET Docker Samples](https://github.com/dotnet/dotnet-docker/tree/main/samples)
- [Azure Container Apps Samples](https://github.com/Azure-Samples/container-apps-store-api-microservice)
- [AKS Baseline Reference Architecture](https://github.com/mspnp/aks-baseline)

---

## Prerequisites

### Technical Skills
- Proficiency in C# and .NET development
- Basic understanding of cloud concepts
- Familiarity with command-line interfaces
- Basic networking knowledge

### Software Requirements
- Docker Desktop installed
- Azure subscription (with appropriate permissions)
- Code editor (VS Code or Visual Studio)
- Git client
- Azure CLI installed
- ORAS CLI installed

### Hardware Requirements
- 16GB RAM minimum (for running Docker Desktop)
- 50GB available disk space
- Stable internet connection

---

## Training Delivery Methods

### Instructor-Led Training
- Live demonstrations
- Hands-on labs with instructor support
- Q&A sessions
- Group discussions

### Self-Paced Learning
- Recorded video lectures
- Lab environment access
- Documentation and guides
- Community support forum

### Hybrid Approach
- Combination of live sessions and self-paced content
- Scheduled office hours for support
- Peer collaboration opportunities

---

## Success Metrics

### Participant Outcomes
- Successfully containerize existing .NET applications
- Deploy to at least two different Azure container services
- Implement automated CI/CD pipelines
- Apply security best practices in container development
- Troubleshoot common containerization issues

### Post-Training Support
- 30-day access to lab environments
- Community forum membership
- Monthly Q&A sessions
- Access to updated materials

---

**Document Version:** 1.0  
**Last Updated:** January 2026  
**Maintained by:** Progress Software Training Team
