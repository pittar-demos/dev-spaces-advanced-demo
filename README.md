# 🚀 Advanced Dev Spaces Demo

<div align="center">

![OpenShift Dev Spaces](https://img.shields.io/badge/OpenShift-Dev%20Spaces-EE0000?style=for-the-badge&logo=red-hat&logoColor=white)
[![Cloud IDE](https://img.shields.io/badge/Cloud-IDE-blue?style=for-the-badge)](#)
[![Open Source](https://img.shields.io/badge/Open-Source-green?style=for-the-badge)](#)

**Transform your development workflow with cloud-native IDEs**

[Get Started](#centralized-devfile-management) • [Features](#dev-space-administrative-controls) • [Security](#security-and-compliance) • [Documentation](#)

</div>

---

OpenShift Dev Spaces is an open source **"cloud"-based IDE** that runs in your OpenShift cluster and is accessed via browsers (e.g. Chrome/Firefox) or remotely (VS Code/JetBrains/Kiro).

> 🎯 **Mission**: Define your workspace and IDE environment **"as code"**, centrally manage common configuration, and provide sandboxed isolation for individual developers.

This drastically improves development environment consistency, ensuring all developers on the team have the same runtime versions, CLI tools, commands, and basic configuration.

✨ **Developer onboarding** is greatly improved - a developer can be up and running as soon as they have credentials to log into the environment and git. No need to set up a laptop or a cloud-based operating system that will quickly drift away from the standard.

<div align="center">

### 🎨 Here are the ways OpenShift Dev Spaces provides incredible value:

| 🌟 Feature | 💡 Benefit | 🎯 Impact |
|------------|------------|-----------|
| **Centralized Devfile Management** | 🚀 Consistent environments | **99% faster onboarding** |
| **Security & Compliance** | 🛡️ Sandboxed development | **Zero data leakage** |
| **Container Tools** | 🐳 No local setup required | **Instant development** |
| **Custom UDI** | 🔧 Tailored tooling | **Perfect team fit** |

</div>

---

## 📁 Centralized Devfile Management

> 🎭 **The Foundation**: The core file that defines the development environment for a project is the **devfile** (`devfile.yaml` or `.devfile.yaml`). This file can exist in the root of your git repository, or it can be centrally managed in a "devfile" repository.

### 🏗️ Why Centralize Your Devfiles?

<div align="center">

| 🛡️ Challenge | 💡 Solution | ✨ Result |
|--------------|-------------|-----------|
| **🔐 Private Repo Access** | 📤 Public Devfile Registry | 🚀 Instant Workspace Startup |
| **🔒 Access Control** | 🏢 Separate Repositories | 🔑 Tighter Security Control |

</div>

**🎯 Two powerful benefits emerge:**

✅ **Authentication Simplification**: If your git repositories are private, Dev Spaces needs credentials to read the `devfile`. This works great with the "big four" (GitHub, GitLab, Bitbucket, Azure Repos), but can be tricky with lesser-known platforms like Gitea. By keeping devfiles (no sensitive data) in a public repository, Dev Spaces reads the devfile, starts the workspace, then uses your credentials to clone private repos.

🔒 **Governance & Control**: A central devfile repository separates devfiles from project repos, making management easier while maintaining tighter control. Project team members with commit access to project repos won't have access to the devfile repository.

## ⚙️ Dev Space Administrative Controls

> 🎛️ **Power User Features**: Customize Dev Spaces with organization-specific settings

<div align="center">

### 🏢 Organizational Configuration

The following `CheCluster` custom resource showcases key administrative controls:

</div>

```yaml
apiVersion: org.eclipse.che/v2
kind: CheCluster
metadata:
  name: devspaces
spec:
  components:
    # Leave this empty to use the built-in plugin registry with a subset of Plugins.
    # This config is pointing to the "open-vsx" registry, where there are thousands of plugins.
    # You can also define your own internal plugin registry in order to restrict plugin access to
    # approved plugins.
    pluginRegistry:
      openVSXURL: 'https://open-vsx.org'
  devEnvironments:
    # This setting allows users to use Podman to run containers.
    disableContainerRunCapabilities: false
    # Maximum number or workspaces per user. "-1" means no limit.  This counts all
    # workspaces, even ones that are not running.
    maxNumberOfWorkspacesPerUser: -1
    # Number of running workspaces per user.  This is more important, as each running
    # workspace actually consumes resources.
    maxNumberOfRunningWorkspacesPerUser: 3
    # Enable or disable workspace auto provisioning.
    defaultNamespace:
      autoProvision: true
      template: <username>-devspaces
    # How long to wait until an inactive workspace is turned off.
    secondsOfInactivityBeforeIdling: 2400
    # Default components container if not specified in a devfile.
    defaultComponents:
      - name: tools
        container:
          image: quay.io/pittar/custom-udi-demo:latest
          memoryLimit: 4Gi
          memoryRequest: 1Gi
          cpuLimit: "1"
          cpuRequest: 100m
          mountSources: true
```

> 🔐 **Admin Access**: These settings are managed by cluster administrators with proper permissions.

The settings above are managed by a "cluster admin", or a user that is delegated permission to manage this resource.

## 🎛️ Central Configuration Management

> 🏗️ **Build Once, Deploy Everywhere**: Standardize developer environments across your organization

<div align="center">

### 📋 Common Configuration Examples

| 🛠️ Tool | 📄 Config File | 🎯 Purpose | 
|---------|---------------|------------|
| **Maven** | `settings.xml` | 🌍 Central repository access |
| **npm** | `.npmrc` | 📦 Registry & proxy settings |
| **Python** | `pip.conf` | 🐍 Package sources |
| **Docker** | `daemon.json` | 🐳 Registry mirrors |

</div>

It's completely normal to have common configuration that all developers require. A perfect example is a `settings.xml` file that all developers should use. This configuration can be centrally managed and distributed to all workspaces, **greatly simplifying workspace consistency** across your entire organization.

## 🐳 Container Tools

> 🚀 **Container Freedom**: Run containers anywhere, anytime, without local setup

<div align="center">

### 🎯 Perfect for These Scenarios

| 🏢 Environment | ❌ Challenge | ✅ Dev Spaces Solution |
|----------------|-------------|----------------------|
| **🏢 Corporate Laptops** | 🔒 No Docker/Podman access | 🐳 Run containers in workspace |
| **🔒 High Security** | 🚫 Local container restrictions | 🛡️ Sandboxed container execution |
| **⚡ Quick Development** | ⏰ Setup time for container tools | 🏃 Instant container ready |

</div>

In some organizations, running local container tools like docker or podman on workstations is difficult or impossible. Dev Spaces gives you the ability to run containers safely in your individual workspace.

**🎉 Popular Use Cases:**
- **🧪 TestContainers**: Automated testing with real dependencies
- **🔬 Microservice Development**: Run supporting services locally  
- **📦 Database Testing**: Spin up databases for integration tests

> 💫 **The Magic**: No tools to install, no local setup required!

## 🔧 Custom "Universal Developer Image"

> 🏗️ **Build Your Perfect Tool**: Extend the official UDI to create project-specific development environments

<div align="center">

### 🎯 Real-World UDI Examples

</div>

When you need additional tools or CLIs that aren't included in the default UDI image, **build your own** and extend the official one! This allows you to create tools images specific to projects that can be automatically updated and versioned for compatibility.

<div align="center">

### ☁️ Cloud Team UDI

| 🛠️ Tool Category | 📦 Examples | 🎯 Use Case |
|------------------|-------------|-------------|
| **☁️ Cloud CLIs** | `aws`, `az`, `gcloud` | 🚀 Multi-cloud management |
| **📊 Monitoring** | `kubectl`, `helm`, `k9s` | 🔍 Cluster administration |
| **⚡ Automation** | `terraform`, `ansible` | 🏗️ Infrastructure as Code |

### 🤖 AI Development UDI

| 🧠 AI Tool | 🤖 Models | 🔗 Integration |
|------------|-----------|----------------|
| **OpenCode** | 🤖 Vetted org models | 🎛️ Central config management |
| **GitHub Copilot** | 🎯 Custom completions | 🔐 Enterprise authentication |
| **Local LLMs** | 🏠 Self-hosted models | 🛡️ Privacy-first development |

</div>

**🛠️ Quick Examples:**

🏢 **Cloud Team**: Don't really "code" but need cloud provider tools? Create a UDI with AWS CLI, Azure CLI, PowerShell, and more!

🤖 **AI Team**: Want to use open source coding agents? Create a UDI with OpenCode pre-packaged and use central config management to connect to your organization's vetted models.

> 🌟 **Limitless Possibilities**: The sky is the limit! 🚀

## 🛡️ Security and Compliance

> 🔒 **Enterprise-Ready Security**: OpenShift Dev Spaces excels in security and compliance, delivering powerful protection capabilities

<div align="center">

### 🏢 Key Security Benefits

| 🛡️ Security Layer | 🔒 Protection | 🎯 Business Impact |
|------------------|---------------|-------------------|
| **📍 Network Isolation** | 🔐 Code never leaves cluster | ✅ Zero data leakage |
| **🏗️ Sandbox Environment** | ⚡ AI tool isolation | 🚀 Safe AI integration |
| **🔑 Access Controls** | 👤 Role-based permissions | 🎛️ Granular governance |
| **📋 Compliance Ready** | 📊 Audit trails | 🏢 Regulatory compliance |

</div>

### 🔐 Core Security Features

**🌐 Network Security**: Source code stays in your workspace pod within your OpenShift cluster - your source code **never lands on a developer laptop**. A lost or stolen laptop doesn't contain sensitive information.

<div align="center">

### 🤖 AI Era Security

**⚡ Sandbox Protection**: As teams adopt AI tools like coding assistants and agents, sandbox environments become critical:

| 🤖 AI Threat | 🛡️ Dev Spaces Protection | 🚀 Recovery Time |
|-------------|-------------------------|------------------|
| **💥 Home Directory Deletion** | 🔄 Instant workspace recreation | ⚡ 1-2 minutes |
| **🔓 Privilege Escalation** | 🏗️ Role-based access controls | ✅ Prevented |
| **🚫 Shadow Models** | 📋 "As code" governance | 🔒 Controlled |

</div>

**🛡️ Multi-Layer Protection:**
- 🔄 **Quick Recovery**: If an agent deletes your home directory, simply delete and re-start your workspace
- 🔑 **Access Controls**: Role-based access, security context constraints, network policies, and resource quotas
- 📋 **Governance**: The "as code" nature makes it difficult to bypass security controls or install unvetted extensions
## 💰 Cost and Maintenance

> 💡 **Smart Economics**: Reduce hardware costs while improving development efficiency

<div align="center">

### 📊 Cost Optimization

| 💻 Traditional Approach | 🚀 Dev Spaces Approach | 💰 Savings |
|------------------------|----------------------|------------|
| **💸 Powerful Laptops** | **🖥️ Any Laptop** | **$2,000-4,000 per dev** |
| **⏰ Local Setup Time** | **⚡ Instant Access** | **8-40 hours per onboarding** |
| **🔧 Individual Maintenance** | **🏢 Central Management** | **100+ hours annually** |

</div>

### 🏗️ Infrastructure Efficiency

**📈 Resource Optimization**: Many developer workspaces can be efficiently bin-packed on worker nodes, scaling pods and nodes up and down on demand. The computing power needed for development is on the server side - developers no longer require expensive, powerful laptops.

### 🛠️ Maintenance Benefits

**🔧 Simplified Management**: Developer workspace configuration is controlled by the devfile, and underlying tools/runtimes are contained in the tools image - **removing IDE maintenance burden from individual developers**.

<div align="center">

### 🌐 Supported Platforms

| 🏢 Platform | 🌍 Availability | 💳 Licensing |
|-------------|----------------|--------------|
| **🔴 OpenShift Container Platform** | ✅ Included | ✅ No additional cost |
| **☁️ Azure Red Hat OpenShift** | ✅ Native support | 💰 Worker node capacity |
| **☁️ Red Hat OpenShift on AWS** | ✅ Enterprise ready | 💰 Infrastructure only |
| **☁️ OpenShift Dedicated on GCP** | ✅ Managed service | 💰 Operational costs |

</div>

> 🎯 **Bottom Line**: There's **nothing extra to procure** for Dev Spaces - just additional "worker node" capacity!

---

<div align="center">

## 🎉 Ready to Transform Your Development?

[![Get Started](https://img.shields.io/badge/🚀-Get_Started-red?style=for-the-badge&logo=rocket&logoColor=white)](#)
[![Documentation](https://img.shields.io/badge/📚-Documentation-blue?style=for-the-badge&logo=book&logoColor=white)](#)
[![Community](https://img.shields.io/badge/👥-Join_Community-green?style=for-the-badge&logo=discord&logoColor=white)](#)

**Made with ❤️ by the OpenShift Dev Spaces Team**

</div>