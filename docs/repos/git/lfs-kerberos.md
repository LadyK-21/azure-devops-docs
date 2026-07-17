---
title: Kerberos and Git LFS
titleSuffix: Azure Repos
description: Using Git LFS versions older than 2.4.0 with TFS
ms.service: azure-devops-repos
ms.topic: overview
ms.date: 07/15/2026
monikerRange: '< azure-devops'
ms.subservice: azure-devops-repos-git
ms.custom: sfi-image-nochange
---

# Kerberos authentication

[!INCLUDE [version-lt-azure-devops](../../includes/version-lt-azure-devops.md)]

If you use Azure DevOps to manage your Git repository, Git might use the Kerberos protocol for authentication. 

> [!NOTE]
> This guidance applies only to Azure DevOps Server. Azure DevOps Services uses a different authentication mechanism.

Git LFS supports Kerberos authentication starting with Git LFS 2.10.0.

Earlier Git LFS releases supported NTLM authentication. However, NTLM support was removed starting with Git LFS 3.0.0. Customers using Git LFS 3.0.0 or later and connecting to an Azure DevOps Server instance configured for Windows Authentication should use Kerberos authentication.

> [!IMPORTANT]
> Upgrade to Git LFS 2.10.0 or later to take advantage of Kerberos authentication support. For Git LFS 3.0.0 and later, Kerberos is the supported authentication mechanism when connecting to Azure DevOps Server configured for Windows Authentication.
