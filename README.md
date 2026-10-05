# Advanced Dev Spaces Demo

OpenShift Dev Spaces is an open source "cloud"-based IDE that runs in your OpenShift cluster and is accessed via browsers (e.g. Chrome/Firefox) or remotely (VS Code/JetBrains/Kiro).

Using Dev Spaces gives you the ability to define your workspace and IDE environment "as code", to centrally manage common configuration (for example, a common Maven `settings.xml` file), and to provide sandboxed isolation for individual developers.

This drastically improves development environment consistency, ensuring all developers on the team have the same runtime versions, CLI tools, commands, and basic configuration.

Developer onboarding is greatly improved, as a developer can be up and running as soon as they have credentials to log into the environment and git.  No need to set up a laptop or a cloud-based operating system that will quickly drift away from the standard.

Here are a few examples of how OpenShift Dev Spaces can be configured to provide even more value to your organization.

## Centralized Devfile Management

The core file that defines the development environment for a project is the **devfile** (`.devfile.yaml` or `devfile.yaml`).  This file can exist in the root of your git repository, or it can be centrally managed in a "devfile" repository.

There are two main benefits to keeping your devfiles in a centrally managed git repository:

* Authentication requirements:  If your git repositories are private, then Dev Spaces needs to have credentials to first read the `devfile` in your private repository.  This works fine if you're using one of the "big four" git services such as GitHub, GitLab, Bitbucket or Azure Repos, but if you're using a lesser known repository (for example, Gitea), this is a problem.  By keeping your devfiles (which don't contain sensitive information) in a public repository, Dev Spaces can read the devfile, start the workspace, then use your registered credentials to clone the private repository.
* Devfile management:  A central devfile git repository separates the devfiles from the project git repositories, making it easier to centrally manage/update devfiles and keep tighter control of them at the same time.  Members of a project team that have commit access on a project repo don't have commit rights on the devfile git repository.

## Dev Space Administrative Controls

There are a number of default configuration settings that can be changed in order to best suit your organization.

Here is a partial `CheCluster` custom resource to highlight a few such controls:

```
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

The settings above are managed by a "cluster admin", or a user that is delegated permission to manage this resource.

## Central Configuration Management

It's normal to have common configuration that all developers require.  A good example of this is a common `settings.xml` file that all developers should use.  Configuration like this can be centrally managed and distributed to all workspaces, greatly simplifying workspace consistency.

## Container Tools

In some organizations, it's difficult or impossible to run local container tools such as docker or podman on local workstations. Dev Spaces gives you the ability to run containers safely in your individual workspace.  This can be for use cases like "testcontainers", or running containers to support microservice development.  The best part - no tools to install!

## Custom "Universal Developer Image"

When you need additional tools or CLIs that aren't included in the default UDI image, what do you do?
You build your own and extend the official one!  This allows you to create tools images specific to projects that can be automatically updated and versioned for compatibility.

Do you have a cloud team doesn't really "code", but needs access to cloud provider tools?  Create a UDI image with the aws cli, azure cli, powershell, etc...

Do you have a team that wants to use an open source coding agent?  Create a UDI that has OpenCode pre-packaged and use the central config management feature of Dev Spaces to automatically connect it to your organizations vetted models.

The sky is the limit!

## Security and Compliance

OpenShift Dev Spaces shines when it comes to security and compliance, bringing a number of very valuable capabilities to the table.

* Source code doesn't leave the network: Since the code resides in your workspace pod in your OpenShift cluster, your source code never lands on a developer laptop.  This means a lost or stolen laptop doesn't contain sensitive information.
* Sandboxed development environments:  As development teams adopt AI tools such as coding assistants and agents, the importance of developing in a sandbox environment becomes critical.  Not only is this important in the event that a code assistant or agent decides to delete your home directory (there are many well documented instances of this) or attempts to escalate privileges on your machine.  In both cases, Dev Spaces provides security constraints and mitigations.
    * If an agent decides to delete your home directory, simply delete and re-start your workspace to be back up and running in a minute or two.
    * Role based access controls, security context constraints, network policies and resource quotas add layers of protection against a potential rogue agent that tries to access systems or resources that it's not supposed to access.
    * The "as code" nature of Dev Spaces makes it more difficult for a user to bypass security controls and install unvetted extensions or use "shadow" models.

## Cost and Maintenance

OpenShift Dev Spaces is a cost effective development environment option.  Developers no longer require powerful laptops, as the computing power needed to support development is on the server side.  Many developer workspaces can be efficiently bin-packed on worker nodes, scaling pods and nodes up and down on demand.
Developer workspace configuration is controlled by the devfile, and the underlying tools/runtimes are contained in the tools image, removing the IDE maintenance burden from individual developers.

OpenShift Dev Spaces is a supported capability of Red Hat OpenShift Container Platform (as well as Azure Red Hat OpenShift, Red Hat OpenShift Service on AWS, and OpenShift Dedicated on GCP), meaning there is nothing to procure to use Dev Spaces, just additional "worker node" capacity.