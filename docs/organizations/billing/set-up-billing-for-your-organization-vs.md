---
title: Manage billing for your organization
titleSuffix: Azure DevOps Services
description: Use an Azure subscription to configure billing and pay for users, CI/CD concurrency, and extensions for Azure DevOps.
ms.subservice: azure-devops-billing
ms.assetid: 87b994d6-2a92-46e5-b667-afe4eb198e25
ms.topic: how-to
ms.author: chcomley
author: chcomley
ms.date: 07/25/2025
monikerRange: 'azure-devops'
ms.custom:
  - freshness-fy22q1
  - engagement-fy23
  - sfi-image-nochange
---

# Manage billing

[!INCLUDE [version-eq-azure-devops](../../includes/version-eq-azure-devops.md)]

This article guides you through managing billing in Azure DevOps, which is essential for controlling costs associated with users, CI/CD concurrency, and extensions. Learn how to [set up billing](#set-up-billing-for-your-organization), [change your Azure subscription](#change-your-billing-subscription), [remove your billing subscription](#remove-your-billing-subscription), and [give a user access to manage billing](#give-a-user-access-to-manage-billing).

*All services are billed via Azure*. You're not required to use any other Azure services.

> [!IMPORTANT]
> - When you remove the billing subscription from your organization, any paid quantities for the following types of users go back to the [free tier](billing-faq.yml) of service immediately:
>    - Basic
>    - Azure Artifacts
>    - Azure Test Plans
>    - Microsoft-hosted CI/CD
>    - Self-hosted CI/CD
> - Before you delete your organization resource in the Azure portal, ensure you changed the billing subscription. Deleting the resource before this change might result in reverting to the free tier of service for your organization. For more information, see [Billing FAQs](billing-faq.yml#azure-portal-integration).
> - When billing gets canceled or changed, a resource group gets left behind in the old Azure subscription. Despite Azure's warning that "Dependent resources will be deleted," it's safe to delete these resource groups. The Azure DevOps organization isn't deleted or harmed. These resource groups take the naming format, `VisualStudioOnline-<DEVOPS-ORG-HOSTID-WITHOUT-DASHES>`.

## Prerequisites

| Category | Requirements |
|--------------|-------------|
|**Permissions**| - Member of the [Project Collection Administrators group](../security/look-up-project-collection-administrators.md). Organization owners are automatically members of this group.<br> - To give another user access to manage billing: [Azure Account Administrator](/azure/cost-management-billing/manage/add-change-subscription-administrator).<br>- To change your organization's billing subscription:<br>- [Project Collection Administrators group](../security/look-up-project-collection-administrators.md). Organization owners are automatically members of this group.<br>- [Owner or Contributor permission for the Azure subscription](set-up-billing-for-your-organization-vs.md#give-a-user-access-to-manage-billing), which you can use to purchase.  |
|**Subscription**| An [Azure subscription](https://azure.microsoft.com/pricing/purchase-options/).|

> [!NOTE]
> Azure DevOps doesn't support the [Azure Free Trial](https://azure.microsoft.com/offers/ms-azr-0044p/).

<a name="set-up-billing"></a>

## Set up billing for your organization

Set up billing in Azure DevOps before making any purchases. This action is a one-time setup per organization. Once you select an Azure subscription for billing, you can add users and assign licenses.

1. Sign in to your organization (```https://dev.azure.com/{Your_Organization}```).

2. Select ![gear icon](../../media/icons/gear-icon.png) **Organization settings**.

   ![Screenshot showing highlighted Organization settings button.](../../media/settings/open-admin-settings-vert.png)
   
3. Select **Billing**.

   ![Screenshot showing highlighted Billing selection in Organization settings.](media/shared/select-billing-organization-settings.png)
   
4. Select **Set up billing**.

   ![Select Set up billing](media/shared/set-up-billing.png)
   
5. Select your Azure subscription, and then select **Save**.

   ![Select your Azure subscription](media/shared/select-azure-subscription.png)
   
> [!TIP]
> If your desired Azure Subscription doesn't display, it’s likely because the Azure subscription you’re trying to use is tied to a different Microsoft Entra ID tenant than the one you use to access your Azure DevOps organization. Do the following steps to ensure this Azure Subscription shows when you're configuring billing:
> 1. Go directly to your profile (`https://aex.dev.azure.com/`).
> 2. From the dropdown menu, select the Microsoft Entra ID tenant tied to the Azure Subscription you want to use for billing.
> 3. Sign in to your organization (`https://dev.azure.com/{Your_Organization}`).
> 4. Select **Organization settings** > **Billing**> **Set up billing**. The subscription should display.

Billing is set up for your Azure DevOps organization. You can [review your individual Azure subscription bill](/azure/cost-management-billing/understand/review-individual-bill) at any time.

<a id="change-subscription"></a>

## Change your billing subscription

You might need to change the Azure subscription for your Azure DevOps organization's billing if the subscription is linked to a former user. Consider switching to a shared subscription or removing the billing arrangement. You can [remove your billing subscription](#remove-your-billing-subscription) anytime. [Migrating between resource groups isn't supported](billing-faq.yml). If billing is set up for an organization that you don't recognize, you can [look up the owner for organizations connected to your Microsoft Entra ID](../accounts/get-list-of-organizations-connected-to-microsoft-entra-id.md).

1. Sign in to your organization (```https://dev.azure.com/{yourorganization}```).

2. Select ![gear icon](../../media/icons/gear-icon.png) **Organization settings**.

   :::image type="content" source="../../media/settings/open-admin-settings-vert.png" alt-text="Screenshot showing highlighted Organization settings button.":::

3. Select **Billing** > **Change billing**. If you don't see the **Change billing** button, you don't have the [right permissions for access](#prerequisites).

   :::image type="content" source="media/shared/select-change-billing.png" alt-text="Screenshot showing highlighted Billing and Change billing buttons.":::

4. Choose your Azure subscription, and then select **Save**.

   :::image type="content" source="media/shared/select-azure-subscription.png" alt-text="Screenshot showing Azure subscription selection.":::

Azure DevOps bills the newly saved subscription for your organization.

## Remove your billing subscription 

1. Sign in to your organization, choose ![gear icon](../../media/icons/gear-icon.png) **Organization settings** > **Billing** > **Change billing**, and do steps 1 through 4 of the [Change the subscription](#change-subscription) section.

2. Select **Remove billing** > **Save**. 

Azure DevOps removes your billing subscription from your organization.

<a name="add-backup-billing-managers"></a>

## Give a user access to manage billing

Assign the [Owner](/azure/role-based-access-control/built-in-roles#owner) or [Contributor](/azure/role-based-access-control/built-in-roles#contributor) role to a user under the subscription your organization uses for billing. For more information, see [Azure roles](/azure/role-based-access-control/rbac-and-directory-admin-roles).

1. [Sign in to the Azure portal](https://portal.azure.com/) as the Azure Account Administrator.
2. Enter *subscriptions* in the search box and select **Subscriptions**. 
3. Select the **subscription** > **Access control (IAM)** > **+ Add**.
4. Select the appropriate role from the dropdown menu.

   > [!div class="mx-imgBorder"]  
   > ![Screenshot of sequence of buttons to add role.](media/add-backup-billing-manager/add-role-to-subscription.png)

5. Select **+ Select members**, search for a user by name or email address, highlight the user, and then choose **Select**.

   > [!div class="mx-imgBorder"]  
   > ![Screenshot of Add role assignment pop-out pane.](media/add-backup-billing-manager/add-role-assignment.png)

6. Select **Review + assign**.

> [!NOTE]
> The user must accept their email invitation before they can access the Azure subscription, if they aren't in your directory.

## Next step

> [!div class="nextstepaction"]
> [Pay for users](buy-basic-access-add-users.md)

## Related content

* [Increase Azure Artifacts storage limit](../../artifacts/start-using-azure-artifacts.md#increase-artifacts-storage-limit)
* [Buy Azure Test Plans](buy-basic-access-add-users.md)
* [Learn about cost management and billing](/azure/cost-management-billing/cost-management-billing-overview)
