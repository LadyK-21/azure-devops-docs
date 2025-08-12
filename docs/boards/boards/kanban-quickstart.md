---
title: Use your board 
titleSuffix: Azure Boards
description: Learn how to use your board to plan and track work in Azure Boards. 
ms.custom: boards-kanban 
ms.topic: quickstart
ms.service: azure-devops-boards
ms.author: chcomley
author: chcomley
monikerRange: '<= azure-devops'
ms.date: 09/11/2024
---

# Use your board

[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

Boards provide an intuitive and visual way to manage your projects, track work items, and collaborate with your team effectively. If you have a project, you already have a board. Let's get started!

> [!NOTE]  
>  You can only create or add boards to a project by adding another team. Boards only  get created when a project or team gets created. For more information, see [About teams and Agile tools](../../organizations/settings/about-teams-and-settings.md).

## Prerequisites

[!INCLUDE [temp](../includes/prerequisites-kanban.md)]

[!INCLUDE [temp](../includes/open-kanban-board.md)] 

## Map the flow of how your team works

When you first open your board, there's one column for each [workflow state](../work-items/guidance/choose-process.md#workflow-states). Your actual columns vary based on the [process](../work-items/guidance/choose-process.md) used to create your project.

1. Identify your team's workflow stages, which most likely don't map to the default states. For your team to have a functional board, configure the board to match your workflow stages.
   
   For example, for user stories, the **New**, **Active**, **Resolved**, and **Closed** states track progress from idea to completion.
    :::row:::
       :::column span="1":::
       ![Screenshot showing user story workflow states.](media/alm-kb-workflow.png)
       :::column-end:::
       :::column span="2":::
       ![Screenshot showing default board, Agile template.](media/alm-kb-empty.png)
       :::column-end:::
    :::row-end:::
    
2. [Manage your columns](add-columns.md), so they match your workflow stages. Keep the number of columns to a minimum while still representing the key handoffs that occur for your team.

    :::image type="content" source="media/ALM_KB_Board2.png" alt-text="Screenshot showing board, Columns customized.":::

## Set WIP limits

Set work in progress (WIP) limits for each workflow stage, so that when work items exceed the limit, the column count displays as red. Teams can use this color as a signal to focus immediately on activities to bring the number of items in the column down. For more information, see [Set WIP limits](wip-limits.md).

:::image type="content" source="media/alm-kb-wip-limits.png" alt-text="Screenshot showing WIP limit reached with red numbering.":::

## Track work in progress

See the estimated size of work for each item that displays at the bottom right of each card. Add items to your backlog in the first column. When priorities change, move items up and down within a column. And, as work completes in one stage, update the status of an item by moving it to a downstream stage.

:::image type="content" source="media/alm-cc-move-card.png" alt-text="Screenshot showing moving a card on board to update status.":::

Update your board as work progresses to help keep your team in sync. Also, you can see and share the value stream your team is delivering to customers.

[!INCLUDE [temp](../includes/note-kanban-boards-teams.md)]

## Add work items 

To add a work item, select the :::image type="icon" source="../media/icons/add_icon.png" border="false"::: plus sign, enter a title, and then select **Enter**. 

::: moniker range=">= azure-devops-2020"
:::image type="content" source="media/quickstart/add-new-item-agile-s155.png" alt-text="Screenshot showing adding a new item on a board, new nav.":::
::: moniker-end

The system automatically saves the work item with the title you entered. You can add as many work items you want by using this method. 

To add details to any work item, select the title. Or, you can directly modify any field that displays. For example, you can reassign a work item by selecting **Assigned To**. For a description of each field, see [Create your backlog, Add details, and estimates](../backlogs/create-your-backlog.md#estimates). You can also [add tasks or child items as checklists on your cards](add-task-checklists.md).

[!INCLUDE [temp](../includes/note-user-assigned.md)]

<a id="update-status">  </a>

## Update work item status

As work completes in one stage, update the status of an item by dragging it to a downstream stage.

[!INCLUDE [note-closed-items](../includes/note-closed-items.md)]

::: moniker range="< azure-devops-2022"
> [!NOTE]   
> Users assigned Stakeholder access can't use the drag-and-drop feature to update status. 
::: moniker-end

:::image type="content" source="media/alm-cc-move-card.png" alt-text="Screenshot showing update status of work item with arrow showing movement of card."::: 

## Update card fields

You can quickly update a field or reassign ownership directly from the board. If the field you want to update isn't showing, then customize the card, so it displays. You can also show other work item types besides bug and user stories, like change requests, incidents, issues, or other custom work item types. For more information, see [Customize cards](../../boards/boards/customize-cards.md) and [About configuring and customizing Azure Boards](../configure-customize.md). 

:::image type="content" source="media/alm-cc-update-card-field.png" alt-text="Screenshot showing update of card field.":::

### Filter your board with keywords, field values, or tags

You can apply filters interactively to focus on a subset of work. For example, you can filter the board to focus on work assigned to at team member for a specific sprint. To start filtering, choose **Filter** :::image type="icon" source="../../media/icons/filter-icon.png" border="false":::. For more information, see [Filter your backlogs, boards, and plans](../backlogs/filter-backlogs-boards-plans.md).

In the following example image, we filtered all items assigned to Jamal and Raisa.
::: moniker range=">= azure-devops-2020"
:::image type="content" source="../backlogs/media/filter-boards/filter-kb-filters-chosen-services.png" alt-text="Screenshot showing filtering on assignment field.":::
::: moniker-end

## Invite others to work on your board 

All members of a project can view and contribute to your board. To invite users to contribute, copy the URL of your board and send it to those users.

::: moniker range=">= azure-devops-2020"
:::image type="content" source="media/quickstart/kanban-board-url-s155.png" alt-text="Screenshot showing red square surrounding the URL for the board.":::
::: moniker-end

To add users to your project, see [Add users to a project](../../organizations/security/add-users-team-project.md).

## Monitor metrics

As with most Agile practices, you're encouraged to monitor key metrics to fine tune your processes. After your team uses the board for several weeks, check out your Cumulative Flow Diagram (CFD).

::: moniker range=">= azure-devops-2020"
Choose the **Analytics** tab, and then choose **View full report** for the CFD as shown in the following image. 

:::image type="content" source="media/quickstart/open-analytics.png" alt-text="Screenshot showing highlighted Analytics tab.":::

Use the interactive controls to choose the time frame, swimlanes, and workflow states or board columns. 

Hover over a point in time to show how many work items are in a particular state.

The following example image shows that on July 3, 101 items were in a *Researching* state.

:::image type="content" source="../../report/dashboards/media/cfd/analytics-cfd-azure-devops.png" alt-text="Screenshot showing opened CFD Analytics.":::

> [!TIP]
> The selections you make only get set for you, and persist across sessions until you change them. 
::: moniker-end

By monitoring these metrics, you can gain insight into how to optimize your processes and minimize lead time. For more information, see [Configure a cumulative flow chart](../../report/dashboards/cumulative-flow.md). 

You can also add Analytics widgets to your dashboard. The Analytics Service is in preview and provides access to several widgets. For more information, see the following articles: 
- [Widgets based on the Analytics Service](../../report/dashboards/analytics-widgets.md)
- [Add a widget to a dashboard](../../report/dashboards/add-widget-to-dashboard.md)
- [What is the Analytics Service?](../../report/powerbi/what-is-analytics.md)

## Can I view a board of work items defined by a query?  
The [Query Based Boards](https://marketplace.visualstudio.com/items?itemName=realdolmen.EdTro-AzureDevOps-Extensions-QueryBasedBoards-Public) Marketplace extension supports viewing a flat-list query of work items as a board. The query can contain different work item types and work items defined in different projects.  

## Next steps 

> [!div class="nextstepaction"]
> [Expedite work using swimlanes](expedite-work.md)  

## Related content

- [Filter backlogs, boards, queries, and plans](../backlogs/filter-backlogs-boards-plans.md)
- [Cumulative flow diagram](../../report/dashboards/cumulative-flow.md)
- [Cumulative flow, lead time, and cycle time guidance](../../report/dashboards/cumulative-flow-cycle-lead-time-guidance.md)
