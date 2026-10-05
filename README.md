# Advanced Dev Spaces Demo

OpenShift Dev Spaces is an open source "cloud"-based IDE that runs in your OpenShift cluster and it accessed via Browers (e.g. Chrome/Firefox) or remotely (VScode/Jetbrains/Kiro).

Using Dev Spaces gives you the ability to define your workspace and IDE environment "as code", to centrally manage common configuration (for example, a common Maven `settings.xml` file), and to provide sandboxed isolation for individual developers.

This drastically improves development environment consistency, insuring all developers on the team have the same runtime versions, cli tools, commands, and basic configuration.

Developer onboarding is greatly improved, as a developer can be up and running as soon as they have credentials to log into the environment and git.  No need to set up a laptop or a cloud-based operating system that will quickly drift away from the standard.

Here are a few examples of how OpenShift Dev Spaces can be configured to provide even more value to your organization.

## Centralized Devfile Management

The core file that defines the development enviornment for a project is the **devfile** (`.devfile.yaml` or `devfile.yaml`).  This file can exist in the root of your git repository, or it can be centrally managed in a "devfile" repository.

There are two main benefits to keeping your devfiles in a centrally managed git repository are the following:

* Authentication requirements:  If your git repositories are private, then Dev Spaces needs to have credentials to first read the `devfile` in your private repository.  This works fine if you're using one of the "big four" git services such as GitHub, GitLab, Bitbucket or Azure Repos, but if you're using a lesser known repository (for example, Gitea), this is a problem.  By keeping your devfiles (which don't contain sensitive information) in a public repository, Dev Spaces can read the devfile, start the workspace, then use your registered credentials to clone the private repository.
* Devfile management:  A central devfile git repository separates the devfiles from the project git repositories, making it easier to centrally manage/update devfiles and keep tighter control of them at the same time.  Memebers of a project team that have commit access on a project repo don't have commit rights on the devfile git repository.

## Central Configuration Management

It's normal to have common configuration that all developer requires.  A good example of this is a common `settings.xml` file that all developers should use.  Configuration like this can be centrally managed and distributed to all workspaces, greatly simplify workspace consistency.

## Container Tools

In some organizations, it's difficult or impossible to run local container tools such as docker or podman on local workspaces. Dev Spaces gives you the ability to run containers safely in your individual workspace.  This can be for use cases like "testcontainers", or running a container (or containers) to support microservice development.  The best part - no tools to install!

## Custome "Universal Developer Image"

When you need additional tools or cli's that aren't included in the default UDI image, what do you do?
You build your own and extened the official one!  This allows you to create tools images specific to projects that can be automatically updated and versioned for compatibility.

Do you have a cloud team doesn't really "code", but needs access to cloud provider tools?  Create a UDI image with the aws cli, azure cli, powershell, etc...

Do you have a team that wants to use an open source coding agent?  Create a UDI that has OpenCode pre-packaged and use the central config management feature of Dev Spaces to automatically connect it to your organizations vetted models.

The sky is the limit!

## Security and Compliance

