---
title: Run Git commands in a script
description: Learn how you can run a Git command in a build script for your workflow with Azure Pipelines
ms.topic: conceptual
ms.assetid: B5481254-F39C-4F1C-BE98-44DC0A95F2AD
ms.date: 07/03/2023
monikerRange: '<= azure-devops'
---

# Run Git commands in a script

[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

For some workflows, you need your build pipeline to run Git commands. For example, after a CI build on a feature branch is done, the team might want to merge the branch to main.

Git is available on [Microsoft-hosted agents](../agents/hosted.md) and on [on-premises agents](../agents/agents.md).

<a name="enable"></a>

## Enable scripts to run Git commands

> [!NOTE]
> Before you begin, be sure your account's default identity is set with the following code.
> This must be done as the very first step after checking out your code.
> ```
> git config --global user.email "you@example.com"
> git config --global user.name "Your Name"
> ```

<a name="version-control"></a>

### Grant version control permissions to the build service

::: moniker range=">= azure-devops-2020"

1. Go to the project settings page for your organization at **Organization Settings** > **General** > **Projects**.

    :::image type="content" source="media/organization-project-settings.png" alt-text="Select your organization settings. ":::

1. Select the project you want to edit. 

    :::image type="content" source="media/select-project.png" alt-text="Select your project. ":::

1. Within **Project Settings**, select **Repositories**. Select the repository you want to run Git commands on.

1. Select **Security** to edit your repository security. 

    :::image type="content" source="media/modify-repo-security.png" alt-text="Choose Security to edit your repository security. ":::

1. Search for **Project Collection Build Service**. Choose the identity **{{your project name}} Build Service ({your organization})** (not the group **Project Collection Build Service Accounts ({your organization})**). By default, this identity can read from the repo but can’t push any changes back to it. Grant permissions needed for the Git commands you want to run. Typically you'll want to grant:

      * **Create branch:**  Allow
      * **Contribute:**  Allow
      * **Read:**  Allow
      * **Create tag:**  Allow

::: moniker-end

### Allow scripts to access the system token

::: moniker range=">=azure-devops-2020"

# [YAML](#tab/yaml)

Add a `checkout` section with `persistCredentials` set to `true`.

```yaml
steps:
- checkout: self
  persistCredentials: true
```

Learn more about [`checkout`](/azure/devops/pipelines/yaml-schema/steps-checkout).

# [Classic](#tab/classic)

On the [options tab](../build/options.md), select **Allow scripts to access OAuth token**.

---

::: moniker-end

## Make sure to clean up the local repo

Certain kinds of changes to the local repository aren't automatically cleaned up by the build pipeline. So make sure to:

* Delete local branches you create.
* Undo git config changes.

If you run into problems using an on-premises agent, make sure the repo is clean:

::: moniker range=">=azure-devops-2020"

# [YAML](#tab/yaml)

Make sure `checkout` has `clean` set to `true`.

```yaml
steps:
- checkout: self
  clean: true
```

# [Classic](#tab/classic)

* On the [repository tab](../repos/pipeline-options-for-git.md#clean-the-local-repo-on-the-agent) set **Clean** to true.

---

::: moniker-end

::: moniker range="< azure-devops"

* On the [repository tab](../repos/pipeline-options-for-git.md#clean-the-local-repo-on-the-agent), set **Clean** to true.

* On the [variables tab](../build/variables.md), create or modify the ```Build.Clean``` variable and set it to ```source```

::: moniker-end

## Examples

### List the files in your repo

On the [build tab](../tasks/index.md), add this task:

| Task | Arguments |
| ---- | --------- |
|  :::image type="icon" source="../tasks/utility/media/command-line.png"::: <br/>[Utility: Command Line](/azure/devops/pipelines/tasks/reference/cmd-line-v2)<br />List the files in the Git repo. | **Tool**: `git`<br /><br />**Arguments**: `ls-files` |

### Merge a feature branch to main

You want a CI build to merge to main if the build succeeds.

On the [Triggers tab](../build/triggers.md), select **Continuous integration (CI)** and include the branches you want to build.

Create ```merge.bat``` at the root of your repo:

```bat
@echo off
ECHO SOURCE BRANCH IS %BUILD_SOURCEBRANCH%
IF %BUILD_SOURCEBRANCH% == refs/heads/main (
   ECHO Building main branch so no merge is needed.
   EXIT
)
SET sourceBranch=origin/%BUILD_SOURCEBRANCH:refs/heads/=%
ECHO GIT CHECKOUT MAIN
git checkout main
ECHO GIT STATUS
git status
ECHO GIT MERGE
git merge %sourceBranch% -m "Merge to main"
ECHO GIT STATUS
git status
ECHO GIT PUSH
git push origin
ECHO GIT STATUS
git status
```

On the [build tab](../tasks/index.md) add this as the last task:

| Task | Arguments |
| ---- | --------- |
| :::image type="icon" source="../tasks/utility/media/batch-script.png"::: <br/>[Utility: Batch Script](/azure/devops/pipelines/tasks/reference/batch-script-v1)<br />Run merge.bat. | **Path**: `merge.bat` |

## FAQ

<!-- BEGINSECTION class="md-qanda" -->

### Can I run Git commands if my remote repo is in GitHub or another Git service such as Bitbucket Cloud?

Yes

### Which tasks can I use to run Git commands?

[Batch Script](/azure/devops/pipelines/tasks/reference/batch-script-v1)

[Command Line](/azure/devops/pipelines/tasks/reference/cmd-line-v2)

[PowerShell](/azure/devops/pipelines/tasks/reference/powershell-v2)

[Shell Script](/azure/devops/pipelines/tasks/reference/shell-script-v2)

### How do I avoid triggering a CI build when the script pushes?

::: moniker range="<=azure-devops"

Add `[skip ci]` to your commit message or description. Here are examples:
* ```git commit -m "This is a commit message [skip ci]"```
* ```git merge origin/features/hello-world -m "Merge to main [skip ci]"```

You can also use any of these variations for commits to Azure Repos Git, Bitbucket Cloud, GitHub, and GitHub Enterprise Server.

- `[skip ci]` or `[ci skip]`
- `skip-checks: true` or `skip-checks:true`
- `[skip azurepipelines]` or `[azurepipelines skip]`
- `[skip azpipelines]` or `[azpipelines skip]`
- `[skip azp]` or `[azp skip]`
- `***NO_CI***`

::: moniker-end

[!INCLUDE [temp](../includes/qa-agents.md)]

::: moniker range="< azure-devops"
[!INCLUDE [temp](../includes/qa-versions.md)]
::: moniker-end

<!-- ENDSECTION -->
