---
title: Use Personal Access Tokens
titleSuffix: Azure DevOps
ms.custom: ai-video-demo
ai-usage: ai-assisted
description: Learn how to create and manage personal access tokens (PATs) as alternate passwords to authenticate to Azure DevOps.
ms.subservice: azure-devops-security
ms.assetid: d980d58e-4240-47c7-977c-baaa7028a1d8
ms.topic: how-to
ms.author: chcomley
author: chcomley
ms.date: 06/27/2025
monikerRange: '<= azure-devops'
---

# Use personal access tokens

[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

A personal access token (PAT) serves as an alternative password for authenticating into Azure DevOps. This PAT identifies you and determines your accessibility and scope of access. Treat PATs with the same level of caution as passwords.

When you use Microsoft tools, your Microsoft account or Microsoft Entra ID is recognized and supported. If you use tools that don't support Microsoft Entra accounts, or if you prefer not to share your primary credentials, consider using PATs as an alternative authentication method. We recommend that you use [Microsoft Entra tokens](../../integrate/get-started/authentication/entra.md) instead of PATs whenever possible.

[!INCLUDE [use-microsoft-entra-reduce-pats](../../includes/use-microsoft-entra-reduce-pats.md)]

## Prerequisites

| Category | Requirements |
|--------------|-------------|
|Permissions |Permission to access and modify your user settings where PATs are managed. <br>- Go to your profile and select **User settings** > **Personal access tokens**. If you can see and manage your PATs here, you have the necessary permissions.<br>- Go to your project and select **Project settings** > **Permissions**. Find your user account in the list and check the permissions that are assigned to you. Look for permissions related to managing tokens or user settings.<br>- If your [organization has policies in place](manage-pats-with-policies-for-administrators.md), an administrator might need to grant you specific permissions or add you to an allowlist to create and manage PATs.<br>- PATs are connected to the user account that minted the token. Depending on the tasks the PAT performs, you might need more permissions yourself.|
|Access levels |At least Basic access.|
|Tasks|*Use PATs only when necessary and always rotate them regularly.* See the section [Best practices for using PATs](#best-practices-for-using-pats).|

## Create a PAT

1. Sign in to your organization (`https://dev.azure.com/{Your_Organization}`).
  
1. From your home page, open user settings :::image type="icon" source="../../media/icons/user-settings-gear.png" border="false"::: and select **Personal access tokens**.

   :::image type="content" source="media/pats/select-personal-access-tokens.png" alt-text="Screenshot that shows selecting Personal access tokens.":::

1. Select **+ New Token**.

   :::image type="content" source="media/pats/select-new-token.png" alt-text="Screenshot that shows selecting New Token.":::

1. Name your token, select the organization where you want to use the token, and then set your token to automatically expire after a set number of days.

   :::image type="content" source="media/pats/create-new-pat.png" alt-text="Screenshot that shows entry of basic token information.":::

1. Select the [scopes](../../integrate/get-started/authentication/oauth.md#available-scopes)
   for this token to authorize for *your specific tasks*.

      For example, to create a token for a [build and release agent](../../pipelines/agents/agents.md) to authenticate to Azure DevOps, set the token's scope to **Agent Pools (Read & manage)**. To read audit log events and manage or delete streams, select **Read Audit Log**, and then select **Create**.

   :::image type="content" source="media/pats/select-pat-scopes-preview.png" alt-text="Screenshot that shows selected scopes for a PAT.":::

   Your administrator might [restrict you from creating full-scoped PATs or limit you to packaging-scope PATs only](manage-pats-with-policies-for-administrators.md). Reach out to your admin to get on the allowlist if you need access to more scopes. Some scopes, for example, `vso.governance`, might not be available in the user interface (UI) if they aren't for widespread public use.

1. When you're finished, copy the token and store it in a secure location. For your security, it doesn't display again.

   :::image type="content" source="media/pats/copy-token-to-clipboard.png" alt-text="Screenshot that shows how to copy the token to your clipboard.":::

You can use your PAT anywhere that your user credentials are required for authentication in Azure DevOps. Remember:

- Treat a PAT with the same caution as your password, and keep it confidential. *Do not share PATS.*
- For organizations that are backed by Microsoft Entra ID, you must sign in with your new PAT within 90 days or it becomes inactive. For more information, see [User sign-in frequency for conditional access](/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime).

### Notifications

During a PAT's lifespan, users receive two notifications: one when the PAT is created and another seven days before it expires.

After you create a PAT, you might receive a notification similar to the following example. This notification serves as confirmation that your PAT was successfully added to your organization.

   :::image type="content" source="media/use-personal-access-tokens-to-authenticate/pat-creation.png" alt-text="Screenshot that shows PAT created notification.":::

An expiration notification email is sent three days before expiration. If your administrator [removed your ability to create PATs in the organization](manage-pats-with-policies-for-administrators.md), the email indicates that it's no longer possible for you to regenerate PATs. Reach out to your [project collection administrator](../security/look-up-project-collection-administrators.md) to be included in an allowlist for continued PAT creation permissions in that organization.

::: moniker range=" < azure-devops"

For more information, see [Configure an SMTP server and customize email for alerts and feedback requests](/azure/devops/server/admin/setup-customize-alerts).

::: moniker-end

#### Unexpected notification

If you receive an unexpected PAT notification, it might mean that an administrator or tool created a PAT for you. Here are some examples:

- A token named `git: https://dev.azure.com/{Your_Organization} on YourMachine` is created when you connect to an Azure DevOps Git repo via git.exe.
- A token named `Service Hooks: Azure App Service: Deploy web app` is created when you or an administrator sets up an Azure App Service web app deployment.
- A token named `WebAppLoadTestCDIntToken` is created when web load testing is set up as part of a pipeline by you or an administrator.
- A token named `Microsoft Teams Integration` is created when a Microsoft Teams Integration Messaging Extension is set up.

If you think the situation is serious:

- [Revoke the PAT](admin-revoke-user-pats.md) (and change your password) if you suspect that it exists in error.
- Check with your administrator if you're a Microsoft Entra user to see if an unknown source or location accessed your organization.
- Review the FAQ on [accidental PAT check-ins to public GitHub repositories](#what-happens-if-i-accidentally-check-my-pat-into-a-public-repository-on-github).

## Use a PAT

Your PAT serves as your digital identity, much like a password. You can use PATs as a quick way to do one-time requests or prototype an application locally. Use a PAT in your code to authenticate [REST API](/rest/api/azure/devops) requests and automate workflows by including the PAT in the authorization header of your request.

After your app code is working, switch to [Microsoft Entra OAuth to acquire tokens for your app's users](../../integrate/get-started/authentication/entra-oauth.md) or a [service principal or managed identity to acquire tokens as an application](../../integrate/get-started/authentication/service-principal-managed-identity.md). We don't recommend that you keep running apps or scripts with PATs long term. You can use Microsoft Entra tokens anywhere that a PAT is used.

Consider [acquiring a Microsoft Entra token via the Azure CLI](../../cli/entra-tokens.md) for ad-hoc requests.

### [Windows](#tab/Windows/)

To provide the PAT through an HTTP header, you must first convert it to a `Base64` string. It can then be provided as an HTTP header in the following format:

```cs

Authorization: Basic BASE64_USERNAME_PAT_STRING
```

### [Linux/macOS](#tab/Linux/)

The following sample gets a list of builds by using curl:

```curl

curl -u :{PAT} https://dev.azure.com/{organization}/_apis/build-release/builds
```

* * *

## Modify a PAT

Do the following steps to:

- Regenerate a PAT to create a new token, which invalidates the previous one.
- Extend a PAT to increase its validity period.
- Alter the [scope](../../integrate/get-started/authentication/oauth.md#available-scopes) of a PAT to change its permissions.

1. From your home page, open user settings :::image type="icon" source="../../media/icons/user-settings-gear.png" border="false"::: and select **Personal access tokens**.

1. Select the token that you want to modify, and then select **Edit**.

   :::image type="content" source="media/pats/select-edit-pat-current-view.png" alt-text="Screenshot that shows the highlighted Edit button to modify a PAT.":::

1. Edit the token name, token expiration, or the scope of access associated with the token, and then select **Save**.

   :::image type="content" source="media/pats/modify-pat.png" alt-text="Screenshot that shows a modified PAT.":::

## Revoke a PAT

You can revoke a PAT at any time for these and other reasons:

- **Security breach**: Revoke a PAT immediately if you suspect it was compromised, leaked, or exposed in logs or public repositories.
- **No longer needed**: Revoke a PAT when the project, service, or integration for which it was created is finished or discontinued.
- **Policy compliance**: Revoke a PAT to enforce security policies, compliance requirements, or organizational token rotation schedules.
- **User changes**: Revoke a PAT when a team member leaves the organization or changes roles and no longer needs access.
- **Scope reduction**: Revoke and re-create a PAT with reduced permissions when you need to limit its access capabilities.
- **Regular maintenance**: Revoke a PAT as part of routine security hygiene and token lifecycle management.

To revoke a PAT, follow these steps:

1. On your home page, open user settings :::image type="icon" source="../../media/icons/user-settings-gear.png" border="false"::: and select **Personal access tokens**.

1. Under **Security**, select **Personal access tokens**. Select the token for which you want to revoke access, and then select **Revoke**.

   :::image type="content" source="media/pats/revoke-personal-access-tokens-preview.png" alt-text="Screenshot that shows selecting to revoke a single token or all tokens.":::

1. In the **Confirmation** dialog, select **Revoke**.

   :::image type="content" source="media/pats/revoke-token-confirmation-dialog-preview.png" alt-text="Screenshot that shows the Confirmation dialog used to revoke a PAT.":::

## PAT Lifecycle Management APIs

The [PAT Lifecycle Management APIs](/rest/api/azure/devops/tokens) might be useful when maintaining large volumes of tokens through the UI is unsustainable. Managing PAT rotation programmatically also opens the opportunity to rotate PATs regularly and shorten their default lifespans. You can configure the [sample Python app](https://github.com/microsoft/azure-devops-auth-samples/tree/master/PersonalAccessTokenAPIAppSample) with your Microsoft Entra tenant and Azure DevOps organization.

Some things to note about these APIs:

* [Microsoft Entra access tokens](../../integrate/get-started/authentication/entra.md) are required to access this API. We recommend a stronger form of authentication when you mint new tokens.
* Only users or apps that use an "on-behalf-of user" flow can generate PATs. Apps that use "on-behalf-of application" flows or authentication flows that don't issue Microsoft Entra access tokens aren't valid for use with this API. As such, [service principals or managed identities](../../integrate/get-started/authentication/service-principal-managed-identity.md) can't create or manage PATs.
* Previously the PAT Lifecycle Management APIs supported only the `user_impersonation` scope, but now the `vso.pats` are available and are the recommended scope to use with these APIs. Downscope all apps that previously relied on `user_impersonation` to call these APIs.

## Changes to format

As of July 2024, we updated the format of PAT strings to improve secret detection in our [leaked PAT detection tooling](manage-pats-with-policies-for-administrators.md#revoke-leaked-pats-automatically-tenant-policy) and [partner offerings](../../repos/security/github-advanced-security-secret-scanning.md). This new PAT format includes more identifiable bits to improve the false positive detection rate in these detection tools and mitigate detected leaks faster.

* New tokens are now *84* characters long, with 52 characters being randomized data, which improves overall entropy. Tokens are now more resistant to brute force attacks.
* Tokens issued by our service include a fixed `AZDO` signature at positions 76-80.

If you're using a PAT issued before that data, regenerate your PAT. If you integrate with PATs and have PAT validation built in, update your validate code to accommodate both new and existing token lengths.

> [!WARNING]
> Both formats remain valid for the foreseeable future. As adoption of the new format increases, we might retire older 52-character PATs.

## Best practices for using PATs

### Consider alternatives

* Acquire a Microsoft Entra token via the [Azure CLI](../../cli/entra-tokens.md) for ad-hoc requests instead of minting a longer-lived PAT.
* Use credential managers like [Git Credential Manager](https://github.com/git-ecosystem/git-credential-manager) or [Azure Artifacts Credential Manager](https://github.com/microsoft/artifacts-credprovider) to simplify credential management, with authentication set to `oauth` or Microsoft Entra tokens.

### Create PATs

* Don't put personal data in the PAT name. Don't rename the PAT name to include some or all of the actual PAT token.
* Avoid creating global PATs unless necessary across all organizations.
* Use a different token per flow or user case.
* Select only the minimum scopes required for each PAT. Grant the least privilege necessary for your specific task. Create separate PATs with limited scopes for different workflows instead of using a single, broad-scoped token. If your PAT needs read-only permissions, don't provide write permissions until necessary.
* Keep PAT lifespans short. (Weekly is ideal, and even shorter is better.)

### Manage PATs

* *Don't share your PATs!*
* *Store your PATs in a secure key management solution*, like [Azure Key Vault](/azure/key-vault/general/overview).
* Regularly rotate or regenerate your PATs via the UI or by using PAT Lifecycle Management APIs.
* Revoke PATs when they're no longer needed.
* Rotate your PATs to use the [new PAT format](#changes-to-format) for better leaked-secret detection and revocation by our first-party tools.

### For admins

* Tenant admins can set [policies to restrict](manage-pats-with-policies-for-administrators.md) global PAT creation, full-scoped PAT creation, and long-lived PAT duration.
* Tenant admins can [revoke PATs for their organization users](admin-revoke-user-pats.md) if the PAT is compromised.
* Organization admins can [restrict PAT creation in an organization](manage-pats-with-policies-for-administrators.md). If PATs are still needed, limit their creation to only PATs that are on the allowlist.

## FAQs

### Why can't I edit or regenerate a PAT scoped to a single organization?

Sign in to the organization where your PAT is scoped. You can view your PATs when you're signed in to any organization in the same Microsoft Entra ID by changing the *Access scope* filter. You can edit only organization-scoped tokens when you're signed in to the specific organization.

### What happens to a PAT if a user account is disabled?

When a user is removed from Azure DevOps, the PAT invalidates within one hour. If your organization is connected to Microsoft Entra ID, the PAT also invalidates in Microsoft Entra ID because it belongs to the user. We recommend that you rotate the PAT to another user or service account to keep services running.

### Can I use PATs with all Azure DevOps REST APIs?

No. You can use PATs with most Azure DevOps REST APIs, but [organizations and profiles](/rest/api/azure/devops/) and the PAT Management Lifecycle APIs support only [Microsoft Entra tokens](../../integrate/get-started/authentication/entra-oauth.md).

### What happens if I accidentally check my PAT into a public repository on GitHub?

Azure DevOps scans for PATs that are checked in to public repositories on GitHub. When we find a leaked token, we immediately send a detailed email notification to the token owner and log an event in your Azure DevOps organization's [audit log](../audit/azure-devops-auditing.md#review-audit-log). We encourage affected users to mitigate the issue by [revoking the leaked token](use-personal-access-tokens-to-authenticate.md#revoke-a-pat) and replacing it with a new token.

Unless you disabled the *Automatically revoke leaked personal access tokens* policy, we immediately revoke the leaked PAT. For more information, see [Revoke leaked PATs automatically](manage-pats-with-policies-for-administrators.md#revoke-leaked-pats-automatically-tenant-policy).

### Can I use a personal access token as an API key to publish NuGet packages to an Azure Artifacts feed by using the dotnet/nuget.exe command line?

No. Azure Artifacts doesn't support passing a PAT as an API key. When you use a local development environment, we recommend that you install the [Azure Artifacts Credential Provider](https://github.com/microsoft/artifacts-credprovider) to authenticate with Azure Artifacts. For more information, see the following examples: [dotnet](../../artifacts/nuget/dotnet-exe.md) and [NuGet.exe](../../artifacts/nuget/publish.md). If you want to publish your packages by using Azure Pipelines, use the [NuGet Authenticate](/azure/devops/pipelines/tasks/reference/nuget-authenticate-v1) task to authenticate with your feed. For more information, see the example in [Publish NuGet packages with Azure Pipelines (YAML/Classic)](../../pipelines/artifacts/nuget.md).

### Why did my PAT stop working?

PAT authentication requires you to regularly sign in to Azure DevOps by using the full authentication flow. Signing in once every 30 days is sufficient for many users, but you might need to sign in more frequently depending on your Microsoft Entra configuration. If your PAT stops working, first try to sign in to your organization and complete the full authentication prompt. If your PAT still doesn't work, check if it expired.

Enabling IIS Basic Authentication invalidates using PATs for Azure DevOps Server. We recommend that you always keep [IIS Basic Authentication](/iis/configuration/system.webserver/security/authentication/basicauthentication) turned off.

> [!WARNING]
> If you use Git with IIS Basic Authentication, Git breaks because it requires PATs for user authentication. You can add an extra header to Git requests to use it with IIS Basic Authentication, but we don't recommend this action. The extra header must be used for all Azure DevOps Server installations because Windows Auth also prevents using PATs. The extra header must also include a Base 64 encoding of `user:PAT`.
>   ```
>   git -c http.extraheader='Authorization: Basic [base 64 encoding of "user:password"]' ls-remote http://tfsserver:8080/tfs/DefaultCollection/_git/projectName
>   ```

### How do I create access tokens that aren't tied to a person?

All PATs are associated with the user identity that created it. Applications can't create PATs.

In Azure DevOps, you can generate access tokens that aren't linked to a specific user. Use Microsoft Entra tokens that an [application service principal or managed identity](../../integrate/get-started/authentication/service-principal-managed-identity.md) issued. For pipelines, use [service connections](../../pipelines/library/service-endpoints.md) to securely authenticate and authorize automated tasks without relying on user-specific credentials.

### How can I regenerate/rotate PATs through the API? I saw that option in the UI, but I don't see a similar method in the API.

The **Regenerate** functionality available in the UI actually accomplishes a few actions, which you can replicate through an API.

To rotate your PAT, follow these steps:

1. See PAT metadata with a **GET** call.
1. Create a new PAT with the old PAT ID by using a **POST** call.
1. Revoke the old PAT by using a **DELETE** call.

### How long do expired, revoked, or inactive PATs remain visible in the Azure DevOps token list?

You can no longer use or regenerate PATs that are expired or revoked. These inactive tokens stay visible for several months after expiration or revocation before being automatically removed from the display.

### Why do I see a "Need admin approval" message when I try to use a Microsoft Entra app to call the PAT Lifecycle Management APIs?

Your tenant's security policies require admin consent before applications can access organization resources in the organization. Reach out to your tenant administrator.

### Can I use a service principal to create or manage PATs?

No. PATs belong to a user identity. Microsoft Entra [service principals or managed identities](../../integrate/get-started/authentication/service-principal-managed-identity.md) can generate short-lived Microsoft Entra tokens that you can use in most places where a PAT is accepted. Learn more about [our efforts to reduce PAT usage across Azure DevOps](https://devblogs.microsoft.com/devops/reducing-pat-usage-across-azure-devops/) and explore replacing PATs with Microsoft Entra tokens.

## Related content

* [Use policies to manage personal access tokens for users](manage-pats-with-policies-for-administrators.md)
* [Revoke user PATs (for admins)](admin-revoke-user-pats.md)
* [Authenticate with Microsoft Entra tokens](../../integrate/get-started/authentication/entra.md)
