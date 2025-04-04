---
title: Sign in with a Personal Access Token (PAT), Azure DevOps CLI 
titleSuffix: Azure DevOps 
description: Use a Personal Access Token (PAT) with Azure DevOps CLI 
ms.topic: how-to
ms.subservice: azure-devops-reference
ms.manager: mijacobs 
ms.author: chcomley  
author: chcomley
monikerRange: 'azure-devops'
ms.date: 01/08/2025
---

# Sign in with a personal access token (PAT)

[!INCLUDE [version-eq-azure-devops](../includes/version-eq-azure-devops.md)] 

You can sign in using an Azure DevOps personal access token (PAT). To create a PAT, see [Use personal access tokens](../organizations/accounts/use-personal-access-tokens-to-authenticate.md#create-a-pat).

[!INCLUDE [use-microsoft-entra-reduce-pats](../includes/use-microsoft-entra-reduce-pats.md)]

To use a PAT with the Azure DevOps CLI, use one of these options:

* Use `az devops login` and be [prompted for the PAT token](#user-prompted-to-use-az-devops-login).
* Pipe the [PAT token on StdIn](#pipe-pat-on-stdin-to-az-devops-login) to `az devops login`. 

  > [!NOTE]
  > This option works only in a non-interactive shell.

* Set the `AZURE_DEVOPS_EXT_PAT` [environment variable]((#use-the-azuredevopsextpat-environment-variable), and don't use `az devops login`.

## User prompted to use az devops login

You're prompted to enter a PAT after you run the `az devops login` command:

```azurecli
$az devops login --organization https://dev.azure.com/contoso
Token:
```

> [!NOTE]   
> If you have already signed in with `az login` interactively or if you're using a user name and password, you're not required to provide a token because the `az devops` commands now support sign-in through `az login`.

When you're successfully signed in, this command also can set your default organization to Contoso, provided no default organization is configured.

> [!NOTE]   
> For Guest Users, only the `az devops login` is supported.


## Pipe PAT on StdIn to az devops login

### From a variable

This option is useful in pipelines in which you can replace `#####` with `$(System.AccessToken)` or another pipeline variable:

```azurecli
echo  "######" | az devops login --organization https://dev.azure.com/contoso/
```

### From a file

```azurecli
cat my_pat_token.txt | az devops login --organization https://dev.azure.com/contoso/
```

## Use the AZURE_DEVOPS_EXT_PAT environment variable 

To gain access in a non-interactive manner for automation scenarios, you can use environment variables or fetch a PAT from a file. 

If `az login` or `az devops login` haven't been used, all `az devops` commands try to sign in using a PAT stored in the `AZURE_DEVOPS_EXT_PAT` environment variable.

To use a PAT, set the `AZURE_DEVOPS_EXT_PAT` environment variable at the process level.

#### [Windows](#tab/windows)


```powershell
# set environment variable for current process
$env:AZURE_DEVOPS_EXT_PAT = 'xxxxxxxxxx'
```

##### [macOS and Linux](#tab/unix)


```bash
export AZURE_DEVOPS_EXT_PAT=xxxxxxxxxx
```

Replace *xxxxxxxxxx* with your PAT.

