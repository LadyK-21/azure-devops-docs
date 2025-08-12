---
title: Conditional Access policies on Azure DevOps
titleSuffix: Azure DevOps Services
description: Set up Microsoft Entra Conditional Access policies to grant or restrict access to tenant resources
ms.subservice: azure-devops-organizations
ms.assetid: 2fdfbfe2-b9b2-4d61-ad3e-45f11953ef3e
ms.topic: how-to
ms.author: chcomley
author: chcomley
ms.date: 06/27/2025
monikerRange: 'azure-devops'
---

# Set up Conditional Access policies on Azure DevOps

Microsoft Entra ID lets tenant admins control which users can access Microsoft resources using [Conditional Access policies](/azure/active-directory/conditional-access/overview). Admins set specific conditions users must meet to gain access, such as:

- Membership in a specific Microsoft Entra security group
- Location or network requirements
- Use of a particular operating system
- Use of a managed and enabled device

Based on these conditions, you can grant access, require more checks like multifactor authentication, or block access entirely. Learn more about [Conditional Access policies](/azure/active-directory/active-directory-conditional-access) in the Microsoft Entra documentation.

## Create a Conditional Access policy for Azure DevOps

| Category | Requirements |
|--------------|-------------|
|**Permissions**| You must be at least a **Conditional Access Administrator** to set up a Conditional Access policy in your tenant. Learn more in the ["Create a Conditional Access policy" Entra docs](/entra/identity/authentication/tutorial-enable-azure-mfa#create-a-conditional-access-policy). |

1. Go to the [Azure portal](https://portal.azure.com) and find the **"Microsoft Entra Conditional Access"** service.
2. Select **"Policies"** on the right sidebar.
3. Select the **"+ New policy"** button. Provide the policy a name. 
5. For the **"Target resources"** assignments, toggle **"Select resources"** and add the _"Azure DevOps"_ or _"Microsoft Visual Studio Team Services"_ resource (resource id: 499b84ac-1321-427f-aa17-267ca6975798) to the list of target resources.
6. Configure other settings as desired.
7. Select **Save** to apply this new policy.

 :::image type="content" source="./media/setup-ado-cap.png" alt-text="Screenshot showing how to add Azure DevOps as a target resource on a new Conditional Access policy in Microsoft Entra portal.":::

## Conditional Access behavior on web

When you sign in to the web portal of a Microsoft Entra ID-backed organization, Microsoft Entra ID validates all Conditional Access policies set by tenant administrators. After [modernizing our web authentication stack to use Microsoft Entra tokens](https://devblogs.microsoft.com/devops/full-web-support-for-conditional-access-policies-across-azure-devops-and-partner-web-properties/), Azure DevOps now enforces Conditional Access policy validation on all interactive (web) flows.

- Meet sign-in policies when using [Personal Access Tokens (PATs)](use-personal-access-tokens-to-authenticate.md) on REST API calls that rely on Microsoft Entra.
- Remove Azure DevOps as a resource from the Conditional Access policy, which prevents Conditional Access policies from applying.
- Enforce MFA policies on web flows only; block access for non-interactive flows if users don't meet a Conditional Access policy.

## IP-based conditions

| Category | Requirements |
|--------------|-------------|
|**Permissions**| You must be an [Project Collection Administrator](../security/look-up-project-collection-administrators.md) to enable this policy.</li></ul>|

If the **IP Conditional Access policy validation on non-interactive flows** organization policy is enabled on the **Organization Settings** page, Azure DevOps checks IP fencing policies on non-interactive flows, such as when you use a PAT to make a REST API call.

Azure DevOps supports IP-fencing Conditional Access policies for both IPv4 and IPv6 addresses. If Conditional Access policies block your IPv6 address, ask your tenant administrator to update the policy to allow your IPv6 address. Also, consider including the IPv4-mapped address for any default IPv6 address in all Conditional Access policy conditions.

If users access the Microsoft Entra sign-in page from a different IP address than the one used to access Azure DevOps resources (which can happen with VPN tunneling), review your VPN configuration or networking setup. Make sure your tenant administrator includes all relevant IP addresses in the Conditional Access policies.

## Azure Resource Manager audience

> [!NOTE]
> These changes will go into effect starting Sep 2, 2025. Learn more in [our blog post](https://devblogs.microsoft.com/devops/removing-azure-resource-manager-reliance-on-azure-devops-sign-ins/).

Azure DevOps doesn't depend on the Azure Resource Manager (ARM) resource (`https://management.azure.com`) when you sign in or refresh Microsoft Entra access tokens. Previously, Azure DevOps required the ARM audience during sign-in and token refresh flows. This requirement meant that administrators had to allow all Azure DevOps users to bypass ARM Conditional Access policies to ensure access. 

If you previously set up a Conditional Access policy for Azure Resource Manager or the associated [Windows Azure Service Management API application](/entra/identity/conditional-access/concept-conditional-access-cloud-apps#windows-azure-service-management-api), this policy no longer covers Azure DevOps sign-ins. Set up a new Azure DevOps Conditional Access policy for continued coverage of Azure DevOps.

The following groups still require continued access to ARM. You may want to consider adding them as exclusions to any ARM or Windows Azure Service Management API Conditional Access policies.
- **Billing administrators** need access to ARM to set up billing and access subscriptions.
- **Service Connection creators** require access to ARM for ARM role assignments and updates to managed service identities (MSIs).

## Continuous Access Evaluation

Azure DevOps now supports [Continuous Access Evaluation (CAE) via Microsoft Entra ID](/entra/identity/conditional-access/concept-continuous-access-evaluation), enabling near real-time enforcement of Conditional Access policies. With CAE, access tokens can be revoked immediately when critical events occur—such as user disablement, password changes, or location/IP shifts—without waiting for token expiration. This enhances security, reduces operational overhead, and improves resilience during identity service outages.

App developers using our latest [.NET client library version](../../integrate/concepts/dotnet-client-libraries.md) (20.259.0-preview and beyond) are recommended to provide support for CAE-enabled tokens by [gracefully handling claims challenges](/entra/identity-platform/claims-challenge?tabs=dotnet). CAE support will be coming to Python and Go client libraries in the latter half of 2025.

## Related content
* [Change application connection & security policies for your organization](change-application-access-policies.md)
