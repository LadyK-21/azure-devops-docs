---
title: About access levels
titleSuffix: Azure DevOps
description: Understand how access levels control the features and functions of Azure DevOps. Learn about assigning and changing access levels for users and groups.
ms.subservice: azure-devops-security
ms.assetid: E2C63C7B-6273-41D7-BD14-BFB340DF8D65
ms.topic: conceptual
ms.author: chcomley
author: chcomley
monikerRange: '<= azure-devops'
ms.date: 04/04/2025
---

# About access levels

[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

Access levels in Azure DevOps control which web portal features are available or not. Access levels supplement security groups, which allow or deny specific tasks. Administrators ensure that their user base has access to the features they need and only pay for those specific features. It’s an efficient way to manage costs while providing the necessary functionality to users. For more information, see [Stakeholder access quick reference](stakeholder-access.md) and [Manage users and access](../accounts/add-organization-users.md).

[!INCLUDE [temp](../../includes/version-selector.md)] 

When you add a user or group to a team or project, they automatically gain access to the features associated with the default access level and security group. For most users, assigning them to the **Basic** access level and the **Contributors** security group provides access to most features. For a simplified overview of the permissions assigned to the most common groups **Readers**, **Contributors**, and **Project Administrators**, see [Default permissions](permissions-access.md).

## Supported access levels

Assign users or groups of users to one of the following access levels:

::: moniker range="azure-devops"

- **Basic**: Provides access to most features. Assign to users with a Visual Studio Professional subscription, an Azure DevOps Server CAL, and to users for whom you're paying for Basic access in an organization.
- **Basic + Test Plans**: Provides access to all features included in **Basic** and Azure Test Plans. Assign to users with a Visual Studio Test Professional or MSDN Platforms subscription, and to users for whom you're paying for Basic + Test Plans access in an organization.
- **Stakeholder**: Provides limited access to private projects and nearly full access to public projects. Assign to an unlimited number of users without requiring a license or subscription, ideal for those needing access to a restricted set of features. Stakeholders can perform various, including viewing work items, participating in discussion, and accessing dashboards. 
- **Visual Studio subscriber**: Assign to users who already have a Visual Studio subscription. The system automatically recognizes the user's subscription&mdash;Visual Studio Enterprise, Visual Studio Professional, Visual Studio Test Professional, or MSDN Platform&mdash;and enables any other features included in their subscription level. If you assign **Basic** or **Stakeholder**, they also receive their Visual Studio subscription benefits upon sign-in.
   > [!TIP]
   > As a best practice when adding new users, we recommend assigning the **Visual Studio Subscriber** level when appropriate (as opposed to Basic) to prevent being charged the **Basic** rate before the user signs in for the first time.
- **GitHub Enterprise**: The system automatically recognizes users with a GitHub Enterprise license the next time they sign in to Azure DevOps. Regardless of a user's assigned access level (for example, they could be assigned **Stakeholder** access), they receive **Basic** access when they're associated with a GitHub Enterprise license.

::: moniker-end  

::: moniker range="<azure-devops"

- **Stakeholder**: Provides partial access, can assign to unlimited users for free. Assign to users with no license or subscriptions who need access to a limited set of features.
- **Basic**: Provides access to most features. Assign to users with an Azure DevOps Server CAL, with a Visual Studio Professional subscription, and to users for whom you're paying for Basic access in an organization.
- **Basic + Test Plans**: Provides access for users who have a monthly Test Manager subscription, Visual Studio Test Professional, or MSDN Platforms subscription. 
- **VS Enterprise**: Provides access to premium features. Assign to users with a subscription to Visual Studio Enterprise.

::: moniker-end  

The following table indicates those features available for each supported access level. Visual Studio Test Professional and MSDN Platform subscriptions grant access to the same features as Visual Studio Enterprise. 

<a id="access-level-settings"></a>

---
::: moniker range="azure-devops"

:::row:::
   :::column span="3":::
      **Feature**
   :::column-end:::
   :::column span="1":::
      **Stakeholder**
   :::column-end:::
   :::column span="1":::
      **Basic**, **GitHub Enterprise**, &<br/>**Visual Studio Professional**
   :::column-end:::
   :::column span="1":::
      **Basic + Test Plans &**<br/>**Visual Studio Enterprise**
   :::column-end:::
:::row-end:::
---
::: moniker-end  

::: moniker range=" < azure-devops"

:::row:::
   :::column span="3":::
      **Feature**
   :::column-end:::
   :::column span="1":::
      **Stakeholder**
   :::column-end:::
   :::column span="1":::
      **Basic** &<br/>**Visual Studio Professional**
   :::column-end:::
   :::column span="1":::
      **Basic + Test Plans** &<br/>**Visual Studio Enterprise**
   :::column-end:::
:::row-end:::
---
::: moniker-end  
  
:::row:::
   :::column span="3":::
      [**Administer organization**](../settings/about-settings.md)  
      Can configure resources when also added to a security group or role: team administrator, Project Administrator, or Project Collection Administrator.
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **Advanced backlog and sprint planning tools**  
      Includes full access to all [backlog](../../boards/backlogs/backlogs-overview.md) and [sprint planning](../../boards/sprints/scrum-overview.md) tools.
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **Advanced home page**  
      Includes [access to projects, work items, and pull requests defined across projects you work in](../../project/navigation/work-across-projects.md).
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **Advanced portfolio management**  
      Includes full access to define features and epics from a [portfolio backlog](../../boards/backlogs/define-features-epics.md) or [board](../../boards/boards/kanban-epics-features-stories.md).  
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
::: moniker range=">= azure-devops-2020"

:::row:::
   :::column span="3":::
      **Agile boards**  
      Stakeholders get limited access to [boards](../../boards/boards/kanban-quickstart.md) and [Taskboards](../../boards/sprints/task-board.md). Stakeholders use drag-and-drop to create and change work items, but only change the State field on cards. They only view [the sprint capacity settings](../../boards/sprints/set-capacity.md).
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
::: moniker-end  
 
:::row:::
   :::column span="3":::
      **Agile Portfolio Management**  
      Includes limited access to [portfolio backlogs](../../boards/backlogs/define-features-epics.md) and [ boards](../../boards/boards/kanban-epics-features-stories.md). Stakeholders can't change the backlog priority order, can't assign items to an iteration, use the mapping pane, or exercise forecasting.  
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="3":::
      [**Artifacts**](../../artifacts/start-using-azure-artifacts.md)  
      Includes full access to all Azure Artifacts features, up to 2-GiB free storage.
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **Author Release Pipelines and Manage Releases**  
      Includes defining [release pipelines](../../pipelines/release/index.md), [multi-stage continuous deployment (CD) pipelines](../../pipelines/release/define-multistage-release-process.md), and [using approvals and gates to control deployments](../../pipelines/release/deploy-using-approvals.md).
   :::column-end:::
   :::column span="1":::
      ✔️ 
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="3":::
      **Basic backlog and sprint planning tools**  
      Includes limited access to add and modify items on [backlogs](../../boards/backlogs/backlogs-overview.md) and [sprint backlogs and Taskboards](../../boards/sprints/scrum-overview.md). Stakeholders can't assign items to an iteration, use the mapping pane, or forecasting.
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="3":::
      **Build**  
      Includes full access to all features to [manage continuous integration and continuous delivery of software](../../pipelines/get-started/what-is-azure-pipelines.md).
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="3":::
      **Chart Authoring**  
      Can create work tracking [query charts](../../report/dashboards/charts.md).
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **Chart Viewing**  
      Can only view work tracking query charts. Stakeholders can't view query charts from the Queries page. They can view them when added to a dashboard.  
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **Code**  
      Includes full access to all features to manage code using [Git repositories](../../repos/git/index.yml) or using [Team Foundation Version Control (TFVC)](../../repos/tfvc/index.yml) Team Foundation Version Control (TFVC). 
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
::: moniker range="azure-devops"
:::row:::
   :::column span="3":::
      [**Delivery Plans**](../../boards/plans/review-team-plans.md)  
      Includes full access to add and view Delivery plans. 
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
::: moniker-end
::: moniker range="< azure-devops"
:::row:::
   :::column span="3":::
      [**Delivery Plans**](../../boards/plans/review-team-plans.md)  
      Includes full access to add and view Delivery plans. 
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
::: moniker-end
:::row:::
   :::column span="3":::
      [**Request and Manage Feedback**](/previous-versions/azure/devops/project/feedback/get-feedback)
      Includes full access to request and manage feedback on working software.  
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **Standard Features**  
      Includes [working across projects](../../project/navigation/work-across-projects.md), [View dashboards](../../report/dashboards/dashboards.md), [View wikis](../../project/wiki/filter-print-wiki.md), and [Manage personal notifications](../../organizations/notifications/manage-your-personal-notifications.md). Stakeholders can't view Markdown README files defined for repositories and can only view wiki pages. Access might be restricted based on specific project or organization permissions. For full functionality, including the ability to view all wiki features, a **Basic** license is recommended.  
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **Test services in build and release**  
      Includes [running unit tests with your builds](../../pipelines/ecosystems/dotnet-core.md#run-your-tests), [reviewing](../../pipelines/test/review-continuous-test-results-after-build.md), and [analyzing](../../pipelines/test/test-analytics.md) test results.  
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **Test Case Management**  
      Includes [adding test plans and test suites](../../test/create-a-test-plan.md), [creating manual test cases](../../test/create-test-cases.md), [deleting test artifacts](../../boards/backlogs/delete-test-artifacts.md), and [testing different configurations](../../test/test-different-configurations.md).  
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **Test Execution and Test Analysis**  
      Includes running [manual](../../test/run-manual-tests.md), [tracking test status](../../test/track-test-status.md), and [automated tests](../../test/run-automated-tests-from-test-hub.md).
   :::column-end:::
   :::column span="1":::
       
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **Test summary access to Stakeholder license**  
      Includes [requesting Stakeholder feedback using the Test & Feedback extension](../../test/request-stakeholder-feedback.md). 
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **View My Work Items**  
      Access to [add and modify work items](../../boards/work-items/about-work-items.md), [follow work items](../../boards/work-items/follow-work-items.md), [view and create queries](../../boards/queries/view-run-query.md), and [submit, view, and change feedback responses](/previous-versions/azure/devops/project/feedback/give-feedback). Stakeholders can only assign existing tags to work items (can't add new tags) and can only save queries under My Queries (can't save under Shared Queries). 
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="3":::
      **View Releases and Manage Approvals**  
      Includes [viewing releases](../../pipelines/release/approvals/index.md) and [approving releases](../../pipelines/release/approvals/index.md). 
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
   :::column span="1":::
      ✔️
   :::column-end:::
:::row-end:::
---

<a id="basic"></a>

::: moniker range="azure-devops"

## Visual Studio subscription access

Visual Studio subscribers get **Visual Studio subscription** features as a subscriber benefit. When you add those users, be sure to assign them the **Visual Studio subscription** access level. 

The system automatically recognizes their subscription and enables any other features included, based on their subscription level.

::: moniker-end  

::: moniker range="< azure-devops"

## VS Enterprise access

Visual Studio Enterprise subscribers get **VS Enterprise** access as a subscriber benefit. When you add those users, be sure to assign them the **VS Enterprise** access level. 

With Visual Studio Enterprise (VS Enterprise) access, users gain access to any fee-based, Marketplace extension published by Microsoft that is included for active Visual Studio Enterprise subscribers.

::: moniker-end

::: moniker range="< azure-devops"

Advanced access gives users all the Basic features, plus [web-based test case management tools](../../test/create-a-test-plan.md). You can [buy monthly access](../billing/buy-access-tfs-test-hub.md#buy-monthly-access-to-test-plans) or add users who already have a Visual Studio Test Professional with MSDN or MSDN Platforms subscription. 
 
::: moniker-end

<a id="mapping"></a> 

## Programmatic mapping of access levels

::: moniker range="azure-devops"

You can manage access levels programmatically using the [`az devops user add` (Azure DevOps Services only)](../accounts/add-organization-users.md) or the [User Entitlement - Add REST API](/rest/api/azure/devops/memberentitlementmanagement/user-entitlements/add). The following table provides a mapping of the access level selected through the user interface and the `AccountLicenseType`, `licensingSource`, and `msdnLicenseType` parameters.

| Access level (user interface)<br/>licenseDisplayName  | accountLicenseType | licensingSource | msdnLicenseType | 
| ------------------------------------------------------|--------------------|----------------|------------------|
| Basic                                                 | express            | account        | none             | 
| Basic + Test Plans                                    | advanced           | account        | none             | 
| Visual Studio Subscriber                              | none               | msdn           | eligible         | 
| Stakeholder                                           | stakeholder        | account        | none             | 
| Visual Studio Enterprise subscription                 | none               | msdn           | enterprise|
| GitHub Enterprise                                     | express   | account                  |enterprise       | 

 > [!NOTE]   
 > The `earlyAdopter` accountLicenseType is an internal value used solely by Microsoft.  

::: moniker-end

::: moniker range="<azure-devops"

You can manage access levels programmatically using the [User Entitlement - Add REST API](/rest/api/azure/devops/memberentitlementmanagement/user-entitlements/add). The following table provides a mapping of the access level selected through the user interface and the `AccountLicenseType`, `licensingSource`, and `msdnLicenseType` parameters.

| Access level (user interface)<br/>licenseDisplayName  | accountLicenseType | licensingSource | msdnLicenseType | 
| ------------------------------------------------------|--------------------|----------------|------------------|
| Basic                                                 | express            | account        | none             | 
| Basic + Test Plans                                    | advanced           | account        | none             | 
| Visual Studio Subscriber                              | none               | msdn           | eligible         | 
| Stakeholder                                           | stakeholder        | account        | none             | 
| VS Enterprise                                         | none               | msdn           | enterprise       | 

::: moniker-end

::: moniker range="< azure-devops"

## What features are available to users who added to two different access levels?

If a user belongs to a group that has **Basic** access and another group that has **VS Enterprise** access, the user has access to all features available through **VS Enterprise**, which is a superset of **Basic**.

## Service account access

Azure DevOps [service accounts](/azure/devops/server/admin/service-accounts-dependencies) added to the default access level. If you make Stakeholder the default access level, you must add the service accounts to Basic or Advanced/VS Enterprise access.  

Service accounts don't require a CAL or other purchase.  

::: moniker-end

## Related content  

::: moniker range="azure-devops"

- [Stakeholder access quick reference](stakeholder-access.md)
- [Get started as a Stakeholder](get-started-stakeholder.md)
- [Export a list of users and their access levels](export-users-audit-log.md)
- [Default permissions and access](permissions-access.md)

::: moniker-end

::: moniker range="< azure-devops"
- [Stakeholder access quick reference](stakeholder-access.md)
- [Change access levels](change-access-levels.md)
- [Get started as a Stakeholder](get-started-stakeholder.md)
- [Compare features between plans](https://azure.microsoft.com/services/devops/compare-features/)

::: moniker-end
