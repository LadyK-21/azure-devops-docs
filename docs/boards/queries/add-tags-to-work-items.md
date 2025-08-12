---
title: Add Tags to Work Items to Categorize Lists and Boards 
titleSuffix: Azure Boards
description: Learn how to add work item tags to categorize and filter lists & boards when working in Azure Boards.
ms.custom: boards-queries
ms.service: azure-devops-boards
ms.assetid: 79A08F31-BB8A-48BD-AD17-477EE0B76BC7
ms.author: chcomley
author: chcomley
ms.topic: how-to
monikerRange: '<= azure-devops'
ms.date: 07/31/2025
#customer intent: As a team member with organizational responsibilities, I want to use tags to organize team activities.
---

# Add work item tags to categorize and filter lists and boards  
 
[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

Tag work items to quickly filter the product backlog or a work item query by categories that you define. A tag corresponds to a one or two keyword phrase that you define. A tag supports your needs to filter a backlog or query, or define a query.

You can add and modify tags from the web portal or from Team Explorer plug-in for Visual Studio. Also, you can open a query in [Excel](../backlogs/office/bulk-add-modify-work-items-excel.md) to modify tags in bulk.

Tags are a better choice for filtering work items than using text strings as described in [Define a work item query](using-queries.md). Tags are a shared resource associated with a project and not a team. If your project contains multiple teams, all teams add to and work from the same set of tags.

## Prerequisites

[!INCLUDE [temp](../includes/prerequisites-work-items.md)] 

<a id="assign"></a>

## Add tags to a work item

Tags should be 400 characters or less and not contain separators such as a `,` (comma), `;` (semicolon), or other formatting character. 

**Recommendation:** Don't use the `@` character in a tag. Tags that start with the `@` character can't be used in a work item query. The `@` character signifies a macro within a query and therefore the tag isn't recognized as a tag.  

From the web portal, open a work item and add a tag. Choose **Add tag** and type your keyword. Or, select from the list of previously assigned tags.  

:::image type="content" source="media/add-tags/add-tag-vsts.png" alt-text="Screenshot shows adding one or more tags to a work item.":::

To add several tags at one time, type a comma between tags. Tags are case sensitive.  

Tags that appear in the tag bar are already assigned to the work item. To unassign a tag, choose the **x** on the tag, :::image type="icon" source="media/add-tags/unassign-a-tag.png":::.   

<a id="bulk-modify"></a>

## Bulk add or remove tags

You can bulk update work items to add or remove tags from the web portal. You bulk modify tags in the same way as you [bulk modify other fields using the web portal](../backlogs/bulk-modify-work-items.md#tags). Or, you can use [Excel](../backlogs/office/bulk-add-modify-work-items-excel.md) to bulk add or remove tags.   

:::image type="content" source="media/add-tags/bulk-add-tags.png" alt-text="Screenshot shows the Edit work items dialog, with Tags value to be added."::: 

> [!NOTE]   
> Boards doesn't support modifying tags in bulk from Visual Studio or other clients.

<a id="query"></a>

## Query for work items based on tags

To query work items based on tags, add a clause for each tag you want to use to support your query.

[!INCLUDE [temp](../includes/query-clause-tip.md)]

You can use the **Contains** or **Does Not Contain** operators. Tags that start with the `@` character can't be used in a work item query because the query editor interprets the `@` character as a macro. For more information, see [Define a work item query](using-queries.md). 

For example, here's a query for all work items that are tagged as either `Web` or `Service`. 

:::image type="content" source="media/add-tags/query-tags-add-or.png" alt-text="Screenshot shows the Query Editor with a query on tags."::: 

<a id="no-tags"></a>

> [!NOTE]
> You can't query for work items that don't have any tags attached to them. If you'd like to up vote the request to support this feature, you can do so on our Developer Community page, [Be able to search for empty tags](https://developercommunity.visualstudio.com/t/be-able-to-search-for-empty-tags/907425). 

<a id="show-tags"></a>

## Show tags in your backlog or query results

Choose **Column Options** to add the **Tags** field to the product backlog or a work item query. If the option doesn't appear, choose **More commands** :::image type="icon" source="../media/icons/actions-icon.png" border="false":::  to select it from the menu of options.    

:::image type="content" source="media/add-tags/column-options-add-tags-field.png" alt-text="Screenshot shows the Column options dialog, with the Tags option.":::

All tags added to the listed work items appear.

:::image type="content" source="media/add-tags/backlog-with-tags.png" alt-text="Screenshot shows the Product backlog where you can see the Tags column."::: 

<a id="filter"></a>

## Filter lists using tags  

From the web portal, you can filter backlogs, boards, and query results using tags. 

Begin by choosing **Filter** :::image type="icon" source="../media/icons/filter-icon.png" border="false":::. 

Select the tags that you want to filter on. Keep the **OR** selection to run a logical OR for all the tags you selected. Or, choose the **AND** option to run a logical AND on all the selected tags. 

:::image type="content" source="media/add-tags/filter-backlog-tags.png" alt-text="Screenshot shows the Product backlog filtered by tags."::: 

## Delete, remove, or manage tags

You can't delete tags using the Azure DevOps Web UI. 

If you need to remove a tag, create a script or application capable of a delete using the [Azure DevOps REST API](/rest/api/azure/devops/wit/tags?view=azure-devops-rest-7.2&preserve-view=true) or the [.NET client libraries](/azure/devops/integrate/concepts/dotnet-client-libraries?view=azure-devops&preserve-view=true). For more examples, see [Azure-DevOps-Admin-CLI](https://github.com/danhellem/azure-devops-admin-cli).

Another option is to install the [Marketplace Tags Manager](https://marketplace.visualstudio.com/items?itemName=YodLabs.TagsManager2), which adds a **Tags** page under **Boards** or **Work** to manage tags, including deletes. 

## Color-code tags on boards

Highlight tags on board cards by color-coding them. These colors only appear on the board that you configure. They don't appear on backlogs or Taskboards. For more information, see [Assign tag colors](../boards/customize-cards.md#assign-tag-colors). 

:::image type="content" source="media/add-tags/color-code-tags.png" alt-text="Screenshot shows the Tag colors page for setting colors for tags.":::

<a id="group-by-tags"></a> 
::: moniker range="azure-devops"

## Chart work items and group by tags

You can't group a query-based chart by tags, but, you can group a **Chart for Work Items** widget by tags that you add to a dashboard. This feature is in public preview. To enable it, see [Manage or enable features](../../project/navigation/preview-features.md) and turn on **Enable group by tags for work item chart widget on dashboard**. 

To group a **Chart for Work Items** widget by tags, complete the same steps provided in [Add a chart to a dashboard](../../report/dashboards/charts.md#add-a-chart-to-a-dashboard). Make sure that your flat-list query contains **Tags** in the query clause or as a column option. Then, choose **Tags** for the **Group by** selection. To filter the chart to show only some tags, choose the **Selected tags** radio button and then choose the tags you want the chart to display.  

:::image type="content" source="../../report/dashboards/media/charts/configure-chart-widget-tags.png" alt-text="Screenshot shows Chart by Work Items configured to group by tags.":::

::: moniker-end

### Limits on the number of tags

While no hard limit exists, creating more than 100,000 tags for a project collection can negatively affect performance. Also, the autocomplete dropdown menu for the tag control displays a maximum of 200 tags. When more than 200 tags are defined, begin entering to cause the tag control to display relevant tags.  

You can't assign more than 100 tags to a work item or you receive the following message:  

```
TF401243: Failed to save work item because too many new tags were added to the work item.
```

Save the work item with the tags (100 or less) that you added, and then you can add more tags. 

Limit queries to fewer than 25 tags. More than that amount and the query likely times out.  

::: moniker range="< azure-devops"

### Add tags to the default column view on the product backlog 

To add the **Tags** field as a column field for the product backlog, modify the ProcessConfiguration file to include `System.Tags` For more information, see the [Process configuration XML element reference](../../reference/xml/process-configuration-xml-element.md).

::: moniker-end

## Related content

- [Define a work item query](using-queries.md) 
- [Customize cards on a board](../../boards/boards/customize-cards.md)
- [Bulk modify work items from the web portal](../backlogs/bulk-modify-work-items.md)  
- [Bulk modify work items from Excel](../backlogs/office/bulk-add-modify-work-items-excel.md)  

### Marketplace extension

- [Marketplace Tags Manager](https://marketplace.visualstudio.com/items?itemName=YodLabs.TagsManager2)
