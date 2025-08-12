---
title: Manage and resolve issues or impediments in Azure Boards
titleSuffix: Azure Boards
description: Learn how to track issues or impediments to more effectively execute plans or stay on schedule by using Azure Boards.
ms.custom: boards-backlogs
ms.service: azure-devops-boards
ms.assetid: 5B126205-599D-40EB-BC95-23CF1444EF2A
ms.author: chcomley
author: chcomley
ms.topic: tutorial
ms.date: 07/26/2022
---

# Manage issues or impediments in Azure Boards

[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

<a name="manage-impediments"></a>

If you have known issues you want to track, you can do so by defining an impediment (Scrum) or issue (Agile or CMMI). Impediments and issues represent unplanned activities. Resolving them requires more work beyond what's tracked for actual requirements. Use the impediment work item type to help you track and manage these issues until you can resolve and close them. 

Don't confuse impediments with bugs. You track impediments that may cause problems with delivering one or more requirements. For example, you may have to fix feature ambiguity, personnel or resource issues, problems with environments, or other risks that influence scope, quality, or schedule. Other issues that deserve tracking are decisions that require several stakeholders or product teams to weigh in on.

::: moniker range="<=azure-devops"

> [!IMPORTANT]  
> Issues and Impediments discussed in this article are defined for projects created with the [Agile](../work-items/guidance/agile-process.md), [Scrum](../work-items/guidance/scrum-process.md), or [CMMI](../work-items/guidance/cmmi-process.md) process. By default, these work item types don't appear on the product backlog or taskboard. 
> 
> If your project was created using the [Basic](../get-started/plan-track-work.md) process, which tracks work using Epics, Issues, and Tasks, then you track Issues using the product backlog. For more information, see [Track issues and tasks](../get-started/plan-track-work.md).

::: moniker-end

In this article you'll learn: 

::: moniker range="azure-devops"

> [!div class="checklist"]      
> * When to use issues versus tasks
> * How to capture issues or impediments as a work item  
> * Add issues or impediments to your product backlog  
 
::: moniker-end

::: moniker range="< azure-devops"

> [!div class="checklist"]      
> * When to use issues versus tasks
> * How to capture issues or impediments as a work item   
 
::: moniker-end

## Prerequisites

[!INCLUDE [temp](../includes/prerequisites-work-items.md)]   

[!INCLUDE [temp](../includes/image-differences-with-wits.md)]   

## Define a task

You use issues or impediments to track items that may block work from getting done. In general, you link these items to user stories or other work items using a Related link type.

Define tasks when you want to create a [checklist of tasks](../boards/add-task-checklists.md). You can also define tasks if you use Scrum methods and track work using the [Remaining Work](../sprints/task-board.md) field. By linking requirement work item types to tasks using the Parent-Child link type, the tasks appear on the taskboard for each linked user story.

::: moniker range="<azure-devops"

> [!NOTE]  
> If your project collection uses the On-premises XML process model to customize work tracking, you can enable work item types that you add to the Task Category to appear as a checklist on your product board.

::: moniker-end

## Add an issue or impediment 

::: moniker range="<=azure-devops"

Open **Boards>Work Items**, and choose the :::image type="icon" source="../../media/icons/blue-add.png" border="false"::: plus icon, and then select from the **New work item** menu of options. 

> [!div class="mx-imgBorder"]  
> ![Screenshot to add an Impediment from the New Work Item list.](media/manage-issues/add-issue-vert.png)   

Choose the  :::image type="icon" source="../media/icons/pin-icon.png" border="false":::  pin icon to have it show up within the add drop down menu. 

::: moniker-end   

<a id="customize"> </a>

## Customize issue tracking

[!INCLUDE [temp](../includes/customize-work-tracking.md)] 

::: moniker range="azure-devops"

Issues and impediments don't appear on your backlog by default. Instead, you track them using [queries](../queries/using-queries.md). To track them on a backlog, see the next section, [Add issues or impediments to your product backlog](#add-to-backlog). 

::: moniker-end

::: moniker range="<azure-devops"

Impediments and issues don't appear on your backlog. Instead, you track them using [queries](../queries/using-queries.md). They only appear on your backlog if your project is customized using the On-premises XML process model. For more information, see [Customize the On-premises XML process model](../../reference/on-premises-xml-process-model.md).

::: moniker-end

 
<a id="add-to-backlog"></a> 

## Add issues or impediments to your product backlog  

::: moniker range="azure-devops"
If you want to track issues or impediments along with your requirements or a portfolio backlog, you can track them by adding them to your custom Inherited process. For more information, see [Customize your backlogs or boards (Inheritance process)](../../organizations/settings/work/customize-process-backlogs-boards.md#edit-product-backlog).

::: moniker-end

::: moniker range="<azure-devops"
If you want to track issues or impediments along with your requirements or a portfolio backlog, you can track them by customizing your project's process. For more information, see the following: 
- **For the Inherited process**: [Customize your backlogs or boards (Inheritance process)](../../organizations/settings/work/customize-process-backlogs-boards.md#edit-product-backlog).
- **For the On-premise XML process**: [Process configuration XML element reference)](../../reference/xml/process-configuration-xml-element.md#configure-a-backlog).
::: moniker-end

## Related content 

- [Add work items](add-work-items.md)
- [Work item form controls](../work-items/about-work-items.md#work-item-form-controls)
- [Manage bugs or code defects](manage-bugs.md)
- [Create your backlog](create-your-backlog.md)
