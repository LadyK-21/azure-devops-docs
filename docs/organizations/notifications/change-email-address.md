---
title: Change your preferred notification email address
titleSuffix: Azure DevOps
description: Change the email address used to receive alerts or email  notifications managed in Azure DevOps.  
ms.subservice: azure-devops-notifications
ms.topic: how-to
ms.author: chcomley
author: chcomley
monikerRange: '<= azure-devops'
ms.date: 07/28/2025
---

# Change your preferred email address for notifications

[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

Change your preferred email address for notifications through your organization profile settings. By default, Azure DevOps sends notifications to the email address associated with your organization profile, which is typically the email you used to sign in.

> [!TIP]
> **Quick access**: Go to **User settings** > **Profile** to update your notification email address.

::: moniker range="azure-devops"

> [!IMPORTANT]
> - Your preferred email address applies to all organizations in your Azure DevOps account and can't be set per organization
> - Notifications might take up to 24 hours to get delivered to the updated email addresses
> - If you're using the Microsoft Entra profile information preview, your profile is managed through Microsoft Entra ID and can't be edited directly in Azure DevOps. See [Set user preferences, Microsoft Entra profile preview](../settings/set-your-preferences.md#microsoft-entra-profile-preview)

::: moniker-end

## Change your email address

Follow these steps to update your preferred notification email address:

::: moniker range="azure-devops"

1. From your Azure DevOps home page, select **User settings** :::image type="icon" source="../../media/icons/user-settings-gear.png" border="false":::, then select **Profile**.

   :::image type="content" source="../../media/open-user-settings-profile-preview.png" alt-text="Screenshot showing how to open user settings and select Profile.":::

2. Update your contact information and select **Save**.

   :::image type="content" source="media/edit-contact-info-preview.png" alt-text="Screenshot showing the contact information page with save option.":::

> [!TIP]
> You can also update other profile information like your display name and avatar from this same page.

::: moniker-end

::: moniker range="<azure-devops"

### For Azure DevOps Server

1. Select your profile menu, then select **My profile**.  

	:::image type="content" source="../settings/media/user-preferences/open-profile-menu-2020.png" alt-text="Screenshot showing how to select your profile menu and then My profile.":::

2. Update your **Preferred email** and select **Save changes**.  

	:::image type="content" source="../settings/media/user-preferences/user-profile-dialog-general-tab.png" alt-text="Screenshot of the User Profile dialog showing the General tab with email settings.":::

For additional settings, see [Set your preferences](../../organizations/settings/set-your-preferences.md).

::: moniker-end

## Troubleshooting

| Issue | Solution |
|-------|----------|
| **Email change not taking effect** | Wait up to 24 hours for changes to propagate across all Azure DevOps services |
| **Still receiving notifications at old email** | Check if you have organization-specific notification subscriptions that override your profile settings |
| **Can't edit email in Microsoft Entra preview** | Contact your Microsoft Entra ID administrator to update your profile information |
| **Not receiving any notifications** | Verify your [notification subscription settings](./manage-your-personal-notifications.md) are configured correctly |

## Related content

- [Set your preferences](../../organizations/settings/set-your-preferences.md)
- [Manage personal notifications](./manage-your-personal-notifications.md)
- [Manage team and organization notifications](manage-team-group-global-organization-notifications.md)
- [About notifications](./about-notifications.md)
