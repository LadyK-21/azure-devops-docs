---
title: Query trend data
titleSuffix: Azure DevOps 
description: Learn how to query Analytics trend data and consume it in a client tool when working from Azure DevOps.
ms.subservice: azure-devops-analytics
ms.assetid: FEF88D72-32D7-4DE8-B11E-BCB1A491C3FC
ms.author: chcomley
author: chcomley
ms.topic: tutorial
monikerRange: "<=azure-devops"
ms.date: 02/12/2025
---

# Query trend data

[!INCLUDE [version-gt-eq-2019](../../includes/version-gt-eq-2019.md)]

Examining trends in data and making period-over-period comparisons are important aspects of reporting and data analysis. Analytics supports these capabilities.

[!INCLUDE [temp](../includes/analytics-preview.md)]

Trend data is exposed in the WorkItemSnapshot and WorkItemBoardSnapshot entity sets. They're constructed so every work item, from the day it was created until today, exists for each day. For an organization with only one work item that was created a year ago, there are 365 rows in this entity. For large projects, these entities would be impractical to use with client tools.

What is the solution? Use the [Aggregation extensions](aggregated-data-analytics.md).      

Using the OData Aggregation Extensions, you can return aggregated data from Azure DevOps that is conducive to reporting. For example, you could show bug trend for the month of March. Bug trends are a common and critical part of managing any project so you can put it to good use immediately.

::: moniker range=" < azure-devops"

> [!NOTE]
> The examples shown in this document are based on an Azure DevOps Services URL. Substitute your Azure DevOps Server URL as needed.

> [!div class="tabbedCodeSnippets"]
> ```OData
> https://{servername}:{port}/tfs/{OrganizationName}/{ProjectName}/_odata/{version}/
> ```

::: moniker-end

## Prerequisites

[!INCLUDE [prerequisites-simple](../includes/analytics-prerequisites-simple.md)]

<a id="trend-data"></a>

## Construct a basic query for trend data   
 
To effectively query the WorkItemSnapshot table, follow these basic requirements:
* Filter the data by date.
* Group the aggregation by at least the date. If not, the response includes a warning.

The query to create a bug trend report looks like the following example:

> [!div class="tabbedCodeSnippets"]
> ```OData
> https://analytics.dev.azure.com/{OrganizationName}/{ProjectName}/_odata/{version}//WorkItemSnapshot?
>   $apply=
>     filter(DateValue ge 2016-03-01Z and DateValue le 2016-03-31Z and WorkItemType eq 'Bug')/
>     groupby((DateValue,State), aggregate($count as Count))
>   &$orderby=DateValue
> ```

It returns a result similar to the following example:

> [!div class="tabbedCodeSnippets"]
> ```JSON
> {
>   "@odata.context": "https://analytics.dev.azure.com/{OrganizationName}/{ProjectName}/_odata/{version}//$metadata#WorkItemSnapshot(DateValue,State,Count)",
>   "value": [
>     {
>       "@odata.id": null,
>       "State": "Active",
>       "DateValue": "2016-03-01T00:00:00-08:00",
>       "Count": 2666
>     },
>     {
>       "@odata.id": null,
>       "State": "Closed",
>       "DateValue": "2016-03-01T00:00:00-08:00",
>       "Count": 51408
>     }
>   ]
> }
> ```

This query produces at most `31 * (number of bug states)`. The default bug has three states:
- Active
- Resolved
- Closed

 At most, this query returns 93 rows no matter how many thousands of records actually exist. It provides a much more compact form of returning data.

Let's look at a variation on this example. You want to see the bug trend for an iteration or a release that starts with one iteration and ends with another.  

To construct that query, do the following example:

> [!div class="tabbedCodeSnippets"]
> ```OData
> https://analytics.dev.azure.com/{OrganizationName}/{ProjectName}/_odata/{version}//WorkItemSnapshot?
>   $apply=
>     filter(WorkItemType eq 'Bug')/
>     filter(Iteration/IterationName eq 'Sprint 99')/
>     filter(DateValue ge Iteration/StartDate and (Iteration/EndDate eq null or DateValue le Iteration/EndDate))/
>     groupby((DateValue, State), aggregate($count as Count))
>   &$orderby=DateValue
> ```

It returns a result similar to the following example:

> [!div class="tabbedCodeSnippets"]
> ```JSON
> {
>   "@odata.context": "https://analytics.dev.azure.com/{OrganizationName}/{ProjectName}/_odata/{version}//$metadata#WorkItemSnapshot(DateValue,State,Count)",
>   "value": [
>     {
>       "@odata.id": null,
>       "State": "Active",
>       "DateValue": "2016-04-04T00:00:00-07:00",
>       "Count": 320
>     },
>     {
>       "@odata.id": null,
>       "State": "Closed",
>       "DateValue": "2016-04-04T00:00:00-07:00",
>       "Count": 38
>     }
>   ]
> }
> ```

In this query, there are two key differences. We added a filter clause to filter the data to a specific iteration and the dates are now being compared to the iteration start and end dates versus a hard-coded date.  
 
> [!NOTE]
> If your query on snapshot tables doesn't use aggregation, the response displays the warning: "The specified query doesn't include a `$select` or `$apply` clause which is recommended for all queries." 

## Related articles

- [Construct aggregate data queries](aggregated-data-analytics.md) to count and analyze groups of related data.
