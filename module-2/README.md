# Module 2: .NET Containerization

## Progress Software Training - Container Migration

This module covers the complete journey of containerizing .NET applications, from environment setup to building optimized, secure container images.

---

## 📁 Module Contents

- **`presentation.html`** - Interactive lecture slides with web resources
- **`your-computer-setup.md`** - Complete tool installation guide (Mac & Windows)
- **`2.1-environment-setup.md`** - Docker Desktop and ORAS CLI installation lab
- **`2.2-dotnet-images.md`** - Understanding .NET container images
- **`2.3-creating-dockerfiles.md`** - Dockerfile creation and best practices
- **`2.4-building-running.md`** - Building and running .NET containers
- **`2.5-best-practices.md`** - Security and performance optimization

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
- ✅ Install and configure Docker for .NET development
- ✅ Create optimized Dockerfiles for .NET applications
- ✅ Build multi-stage Docker images for .NET
- ✅ Implement security best practices in .NET containers
- ✅ Debug containerized .NET applications

---

## 🚀 Getting Started

### Step 1: Setup Your Computer
**Start here!** Follow `your-computer-setup.md` to install all required tools:
- Docker Desktop
- .NET SDK 8.0
- ORAS CLI
- Azure CLI
- VS Code (optional)

### Step 2: Review the Presentation
Open `presentation.html` in your web browser for the lecture content.

### Step 3: Complete the Labs
Work through each lab file in order:
1. `2.1-environment-setup.md`
2. `2.2-dotnet-images.md`
3. `2.3-creating-dockerfiles.md`
4. `2.4-building-running.md`
5. `2.5-best-practices.md`

---

## ⏱️ Estimated Time

- **Tool Installation:** 1-2 hours
- **Presentation:** 1.5 hours
- **Hands-on Labs:** 6-8 hours
- **Total:** 8-11 hours

---

## 📋 Prerequisites

- Basic .NET development knowledge (C#)
- Familiarity with command-line interfaces
- Understanding of Module 1 concepts (containers, images, registries)
- Active Azure subscription (for later modules)

---

## 🛠️ Required Tools

| Tool | Purpose | Installation Guide |
|------|---------|-------------------|
| Docker Desktop | Container runtime | `your-computer-setup.md` |
| .NET SDK 10.0 | Build .NET applications | `your-computer-setup.md` |
| ORAS CLI | OCI artifact management | `your-computer-setup.md` |
| Azure CLI | Azure resource management | `your-computer-setup.md` |
| VS Code | Code editor (optional) | `your-computer-setup.md` |

---

## 📚 Key Concepts Covered

### Environment Setup
- Docker Desktop installation and configuration
- WSL2 setup for Windows users
- Resource allocation and optimization
- ORAS CLI for artifact management

### .NET Container Images
- Microsoft Container Registry (MCR)
- SDK vs. Runtime images
- Alpine vs. Debian variants
- Version tagging strategies
- Docker Hardened Images

### Dockerfile Creation
- Multi-stage builds
- Layer caching optimization
- Security hardening
- Non-root user implementation
- Health checks

### Building & Running
- Docker build commands
- Running containers
- Docker Compose for multi-container apps
- Logging and debugging
- Container lifecycle management

### Best Practices
- Security scanning with Docker Scout and Trivy
- Performance optimization
- SBOM generation
- Image signing
- ReadyToRun and AOT compilation

---

## 🎓 Hands-On Projects

Throughout this module, you'll complete these practical exercises:

1. **Basic Console App** - Containerize a simple .NET console application
2. **ASP.NET Core API** - Build a multi-stage Dockerfile for a web API
3. **Hardened Container** - Implement security best practices
4. **Multi-Container App** - Use Docker Compose with API + SQL Server
5. **Optimization Challenge** - Reduce image size and build time
6. **Security Scan** - Scan and remediate vulnerabilities

---

## 🔗 Additional Resources

### Official Documentation
- [Docker Documentation](https://docs.docker.com/)
- [.NET Docker Samples](https://github.com/dotnet/dotnet-docker)
- [Microsoft: .NET and Docker](https://learn.microsoft.com/dotnet/core/docker/introduction)
- [ORAS Documentation](https://oras.land/)

### Tools
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Docker Hub .NET Images](https://hub.docker.com/_/microsoft-dotnet)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Docker Extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)

### Learning Resources
- [Docker Getting Started Tutorial](https://docs.docker.com/get-started/)
- [Play with Docker](https://labs.play-with-docker.com/)
- [Microsoft Learn: Containerize .NET Apps](https://learn.microsoft.com/training/modules/intro-to-containers/)

---

## ❓ Troubleshooting

### Common Issues

**Docker won't start**
- Windows: Ensure WSL 2 is installed and updated
- Mac: Check that you have sufficient permissions
- Both: Try restarting Docker Desktop or your computer

**Permission errors**
- Windows: Run PowerShell as Administrator
- Mac/Linux: Use `sudo` for system operations or add your user to docker group

**Build failures**
- Check `.dockerignore` is configured correctly
- Verify network connectivity for package downloads
- Ensure sufficient disk space (50GB+ recommended)

**Port conflicts**
- Check for services already using ports 8080, 1433, etc.
- Use `docker ps` to see running containers
- Use different ports with `-p` flag

### Getting Help

- Check the troubleshooting section in each lab file
- Review [Docker Desktop Troubleshooting](https://docs.docker.com/desktop/troubleshoot/overview/)
- Consult [Stack Overflow - Docker Tag](https://stackoverflow.com/questions/tagged/docker)
- Ask your instructor or training coordinator

---

## 📝 Lab Completion Checklist

Track your progress through the module:

- [ ] Installed Docker Desktop successfully
- [ ] Installed .NET SDK 8.0
- [ ] Installed ORAS CLI
- [ ] Installed Azure CLI
- [ ] Completed 2.1: Environment Setup
- [ ] Completed 2.2: Understanding .NET Images
- [ ] Completed 2.3: Creating Dockerfiles
- [ ] Completed 2.4: Building and Running
- [ ] Completed 2.5: Best Practices
- [ ] Containerized a sample .NET application
- [ ] Created a multi-container application
- [ ] Scanned images for vulnerabilities
- [ ] Optimized image size

---

## 🎯 Next Steps

Once you've completed Module 2, proceed to:
- **Module 3:** Azure Container Registry (ACR)
- Push your container images to ACR
- Use ORAS CLI for artifact management
- Implement security and access controls

---

## 📞 Support

If you encounter issues or have questions:
- **Email:** training@progress.com
- **Slack:** #container-training channel
- **Office Hours:** Monday-Friday, 2-4 PM EST

---

**Last Updated:** January 2026  
**Version:** 1.0  
**Maintained by:** Progress Software Training Team
