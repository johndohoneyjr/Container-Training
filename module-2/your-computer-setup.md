# Your Computer Setup Guide

## Module 2: .NET Containerization - Tool Installation

This guide provides step-by-step instructions for installing all tools needed for Module 2. Follow the instructions for your operating system (Windows or macOS).

---

## 📋 Overview

You will install the following tools:
1. **Docker Desktop** - Container runtime and management
2. **.NET SDK 10.0** - Build and run .NET applications
3. **ORAS CLI** - OCI artifact management
4. **Azure CLI** - Azure resource management
5. **Visual Studio Code** (Optional) - Code editor with Docker extensions
6. **Git** (Optional) - Source control

---

## 🐳 1. Docker Desktop

### Windows Installation

#### Prerequisites
- Windows 10 64-bit: Pro, Enterprise, or Education (Build 19044 or higher)
- OR Windows 11 64-bit: Pro, Enterprise, or Education
- Hardware virtualization enabled in BIOS
- 4GB RAM minimum (8GB+ recommended)
- 50GB free disk space

#### Step 1: Enable WSL 2

1. Open **PowerShell as Administrator**:
   - Press `Win + X`
   - Select "Windows PowerShell (Admin)" or "Terminal (Admin)"

2. Run the following command:
   ```powershell
   wsl --install
   ```

3. **Restart your computer** when prompted

4. After restart, verify WSL 2 installation:
   ```powershell
   wsl --list --verbose
   ```
   
5. Ensure WSL 2 is the default version:
   ```powershell
   wsl --set-default-version 2
   ```

#### Step 2: Download Docker Desktop

1. Visit [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
2. Click **"Download for Windows"**
3. Save the `Docker Desktop Installer.exe` file

#### Step 3: Install Docker Desktop

1. Double-click the installer file
2. When prompted, ensure **"Use WSL 2 instead of Hyper-V"** is checked
3. Check **"Add shortcut to desktop"** (optional)
4. Click **"Ok"** to start installation
5. Wait for installation to complete (may take several minutes)

#### Step 4: First Launch

1. Launch Docker Desktop from the Start menu or desktop shortcut
2. Accept the Docker Subscription Service Agreement
3. Complete the optional tutorial or skip it
4. Docker Desktop will start automatically

#### Step 5: Configure Docker Desktop

1. Click the **Settings** icon (gear) in Docker Desktop
2. Navigate to **Resources > Advanced**:
   - **CPUs:** Set to at least 2 (4 recommended)
   - **Memory:** Set to at least 4GB (8GB recommended)
   - **Disk image size:** Ensure at least 50GB
3. Click **"Apply & Restart"**

#### Step 6: Verify Installation

Open PowerShell and run:
```powershell
# Check Docker version
docker --version

# Run test container
docker run hello-world

# Check Docker info
docker info
```

You should see output confirming Docker is running correctly.

#### Troubleshooting Windows

**Issue: "WSL 2 installation is incomplete"**
```powershell
# Update WSL
wsl --update

# Set WSL 2 as default
wsl --set-default-version 2
```

**Issue: Docker Desktop won't start**
- Ensure virtualization is enabled in BIOS/UEFI
- Check Windows Hyper-V is enabled:
  ```powershell
  Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
  ```
- Restart your computer

**Issue: Permission errors**
- Run PowerShell as Administrator
- Add your user to the `docker-users` group:
  ```powershell
  net localgroup docker-users "YOUR_USERNAME" /add
  ```

---

### macOS Installation

#### Prerequisites
- macOS 11 (Big Sur) or newer
- 4GB RAM minimum (8GB+ recommended)
- 50GB free disk space

#### Step 1: Download Docker Desktop

1. Visit [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
2. Click **"Download for Mac"**
3. Choose the correct version:
   - **Mac with Apple chip** (M1/M2/M3) - For Apple Silicon
   - **Mac with Intel chip** - For Intel processors
4. Save the `.dmg` file

#### Step 2: Install Docker Desktop

1. Double-click the downloaded `.dmg` file
2. Drag the **Docker** icon to the **Applications** folder
3. Open **Applications** folder
4. Double-click **Docker** to launch

#### Step 3: Grant Permissions

1. Docker will request privileged access
2. Enter your macOS password when prompted
3. Click **"OK"** to grant permissions

#### Step 4: First Launch

1. Accept the Docker Subscription Service Agreement
2. Complete the optional tutorial or skip it
3. Docker Desktop will appear in your menu bar (whale icon)

#### Step 5: Configure Docker Desktop

1. Click the Docker icon in the menu bar
2. Select **Preferences** or **Settings**
3. Navigate to **Resources**:
   - **CPUs:** Set to at least 2 (4 recommended)
   - **Memory:** Set to at least 4GB (8GB recommended)
   - **Disk image size:** Ensure at least 50GB
4. Click **"Apply & Restart"**

#### Step 6: Verify Installation

Open **Terminal** and run:
```bash
# Check Docker version
docker --version

# Run test container
docker run hello-world

# Check Docker info
docker info
```

#### Troubleshooting macOS

**Issue: Permission denied errors**
```bash
# Fix Docker socket permissions
sudo chown -R $(whoami) ~/.docker
```

**Issue: Rosetta 2 needed (Apple Silicon)**
```bash
# Install Rosetta 2 for x86 compatibility
softwareupdate --install-rosetta
```

**Issue: Docker daemon not running**
- Ensure Docker Desktop is running (check menu bar)
- Restart Docker Desktop from the menu
- Restart your computer if issues persist

---

## 🔧 2. .NET SDK 10.0

### Windows Installation

#### Step 1: Download

1. Visit [https://dotnet.microsoft.com/download](https://dotnet.microsoft.com/download)
2. Click **".NET 10.0"** (or latest LTS version)
3. Under **"SDK"**, click **"Download .NET SDK x64""

#### Step 2: Install

1. Run the downloaded installer (`dotnet-sdk-8.x.xxx-win-x64.exe`)
2. Follow the installation wizard
3. Accept the license agreement
4. Click **"Install"**
5. Wait for installation to complete

#### Step 3: Verify

Open PowerShell:
```powershell
# Check .NET version
dotnet --version

# List installed SDKs
dotnet --list-sdks

# List installed runtimes
dotnet --list-runtimes
```

---

### macOS Installation

#### Option 1: Using Homebrew (Recommended)

1. Install Homebrew (if not already installed):
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. Install .NET SDK:
   ```bash
   brew install --cask dotnet-sdk
   ```

3. Verify installation:
   ```bash
   dotnet --version
   dotnet --list-sdks
   ```

#### Option 2: Manual Installation

1. Visit [https://dotnet.microsoft.com/download](https://dotnet.microsoft.com/download)
2. Click **".NET 10.0"**
3. Under **"SDK"**, choose your architecture:
   - **Arm64** for Apple Silicon (M1/M2/M3)
   - **x64** for Intel Macs
4. Download the `.pkg` installer
5. Double-click to install
6. Follow the installation wizard

#### Verify Installation

```bash
dotnet --version
dotnet --list-sdks
dotnet --info
```

---

## 📦 3. ORAS CLI

ORAS (OCI Registry As Storage) is used for managing OCI artifacts in container registries.

### Windows Installation

#### Option 1: Using Winget (Recommended)

```powershell
# Open PowerShell
winget install oras
```

#### Option 2: Using Chocolatey

```powershell
# Install Chocolatey if not already installed
# Visit https://chocolatey.org/install

# Install ORAS
choco install oras
```

#### Option 3: Manual Installation

1. Visit [https://github.com/oras-project/oras/releases](https://github.com/oras-project/oras/releases)
2. Download `oras_x.x.x_windows_amd64.zip` (latest version)
3. Extract the ZIP file
4. Move `oras.exe` to a folder in your PATH (e.g., `C:\Program Files\oras\`)
5. Add to PATH if necessary:
   - Search "Environment Variables" in Start menu
   - Click "Environment Variables" button
   - Under "System variables", find and edit "Path"
   - Add the folder containing `oras.exe`
   - Click OK

#### Verify Installation

```powershell
oras version
```

---

### macOS Installation

#### Option 1: Using Homebrew (Recommended)

```bash
brew install oras
```

#### Option 2: Manual Installation

```bash
# Download latest release (check GitHub for current version)
VERSION="1.1.0"
curl -LO "https://github.com/oras-project/oras/releases/download/v${VERSION}/oras_${VERSION}_darwin_amd64.tar.gz"

# For Apple Silicon, use:
# curl -LO "https://github.com/oras-project/oras/releases/download/v${VERSION}/oras_${VERSION}_darwin_arm64.tar.gz"

# Extract
tar -xzf oras_${VERSION}_darwin_*.tar.gz

# Move to PATH
sudo mv oras /usr/local/bin/

# Make executable
sudo chmod +x /usr/local/bin/oras

# Clean up
rm oras_${VERSION}_darwin_*.tar.gz
```

#### Verify Installation

```bash
oras version
oras help
```

---

## ☁️ 4. Azure CLI

### Windows Installation

#### Option 1: MSI Installer (Recommended)

1. Download from [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows)
2. Run the downloaded MSI installer
3. Follow the installation wizard
4. Accept the license agreement
5. Click "Install"

#### Option 2: Using Winget

```powershell
winget install Microsoft.AzureCLI
```

#### Verify Installation

```powershell
# Check version
az --version

# Login to Azure
az login

# List subscriptions
az account list --output table
```

---

### macOS Installation

#### Option 1: Using Homebrew (Recommended)

```bash
# Update Homebrew
brew update

# Install Azure CLI
brew install azure-cli
```

#### Option 2: Manual Script Installation

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

#### Verify Installation

```bash
# Check version
az --version

# Login to Azure
az login

# List subscriptions
az account list --output table
```

---

## 💻 5. Visual Studio Code (Optional but Recommended)

### Windows Installation

1. Visit [https://code.visualstudio.com/](https://code.visualstudio.com/)
2. Click **"Download for Windows"**
3. Run the installer
4. Accept the agreement
5. Choose installation options:
   - ✅ Add "Open with Code" to context menu
   - ✅ Add to PATH
6. Click **"Install"**

### macOS Installation

1. Visit [https://code.visualstudio.com/](https://code.visualstudio.com/)
2. Click **"Download for Mac"**
3. Open the downloaded `.zip` file
4. Drag **Visual Studio Code.app** to **Applications** folder
5. Launch from Applications

### Install Recommended Extensions

Open VS Code and install these extensions:

```bash
# From command line:
code --install-extension ms-dotnettools.csdevkit
code --install-extension ms-azuretools.vscode-docker
code --install-extension ms-vscode.azure-account
code --install-extension ms-azuretools.vscode-azurecontainerapps
```

Or manually from VS Code:
1. Click Extensions icon (or `Ctrl+Shift+X` / `Cmd+Shift+X`)
2. Search and install:
   - **C# Dev Kit** (ms-dotnettools.csdevkit)
   - **Docker** (ms-azuretools.vscode-docker)
   - **Azure Account** (ms-vscode.azure-account)
   - **Azure Container Apps** (ms-azuretools.vscode-azurecontainerapps)

---

## 🔗 6. Git (Optional)

### Windows Installation

1. Download from [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Run the installer
3. Recommended settings:
   - Editor: Visual Studio Code
   - PATH: "Git from the command line and also from 3rd-party software"
   - SSH: "Use bundled OpenSSH"
   - HTTPS: "Use the OpenSSL library"
   - Line endings: "Checkout Windows-style, commit Unix-style"

4. Verify:
   ```powershell
   git --version
   ```

### macOS Installation

#### Option 1: Xcode Command Line Tools (Included)

```bash
xcode-select --install
```

#### Option 2: Homebrew

```bash
brew install git
```

#### Verify

```bash
git --version
```

---

## ✅ Final Verification Checklist

Run these commands to verify all tools are installed correctly:

### Windows (PowerShell)
```powershell
# Docker
docker --version
docker run hello-world

# .NET
dotnet --version
dotnet --list-sdks

# ORAS
oras version

# Azure CLI
az --version

# VS Code (if installed)
code --version

# Git (if installed)
git --version
```

### macOS (Terminal)
```bash
# Docker
docker --version
docker run hello-world

# .NET
dotnet --version
dotnet --list-sdks

# ORAS
oras version

# Azure CLI
az --version

# VS Code (if installed)
code --version

# Git (if installed)
git --version
```

---

## 🎯 Expected Output

You should see version numbers for each tool, similar to:

```
Docker version 24.0.7, build afdd53b
dotnet SDK 10.0.100
oras version 1.1.0
azure-cli 2.55.0
Visual Studio Code 1.85.0
git version 2.43.0
```

---

## 🐛 Common Issues and Solutions

### Issue: Command not found

**Solution:** Restart your terminal/PowerShell or computer. If the issue persists, check that the tool's directory is in your PATH.

### Issue: Permission Denied (macOS/Linux)

**Solution:** Use `sudo` for system-level installations or add your user to the docker group:
```bash
sudo usermod -aG docker $USER
```
Then log out and log back in.

### Issue: Docker daemon not running

**Solution:**
- Ensure Docker Desktop is running
- Check system tray (Windows) or menu bar (macOS) for Docker icon
- Restart Docker Desktop

### Issue: Azure CLI login fails

**Solution:**
```bash
# Clear cached credentials
az account clear

# Try device code login
az login --use-device-code
```

---

## 📚 Additional Resources

- [Docker Desktop Documentation](https://docs.docker.com/desktop/)
- [.NET Documentation](https://docs.microsoft.com/dotnet/)
- [ORAS Documentation](https://oras.land/)
- [Azure CLI Documentation](https://docs.microsoft.com/cli/azure/)
- [VS Code Docker Extension](https://code.visualstudio.com/docs/containers/overview)

---

## 🆘 Getting Help

If you encounter issues not covered here:

1. Check the official documentation for each tool
2. Search Stack Overflow
3. Contact your training coordinator
4. Email: training@progress.com

---

## ✨ You're Ready!

Once all tools are installed and verified, proceed to the Module 2 presentation (`presentation.html`) and labs.

**Next Step:** Open `presentation.html` in your web browser to begin Module 2!

---

**Last Updated:** January 2026  
**Version:** 1.0
