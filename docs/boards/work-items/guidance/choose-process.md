---
title: Default processes and process templates
titleSuffix: Azure Boards
description: Learn about choosing a process or process template based on the process model you use in your Azure Boards project.
ms.custom: work-items 
ms.service: azure-devops-boards
ms.assetid: 702EE9E5-7AEA-49B6-9DB0-B12A882979C8
ms.topic: overview
ms.author: chcomley
author: chcomley
monikerRange: '<= azure-devops'
ms.date: 10/11/2024
#customer intent: As a team leader, I want to understand the processes that Azure Boards uses to manage work items for my project.
---

# Default processes and process templates

[!INCLUDE [version-lt-eq-azure-devops](../../../includes/version-lt-eq-azure-devops.md)]

Azure Boards offers various processes for managing work items. Selecting the right process helps optimize project workflow and ensure your project's success. In this article, explore the different processes available with Azure Boards. This article provides guidance on how to choose the most suitable process for your project.

[!INCLUDE [process terms](../../../includes/choose-process-introduction.md)]

The default process types are *Basic*, *Agile*, *Capability Maturity Model Integration (CMMI)*, and *Scrum*. The work tracking objects in the default processes and process templates are the same. They're summarized in this article.

[!INCLUDE [process templates](../../includes/get-latest-process-templates.md)]

## Default processes

The default processes differ mainly in the work item types they provide for planning and tracking work. The default processes are:

- **Basic**: Is the most lightweight and is in a selective preview.
- **Scrum**: Is the next most lightweight.
- **Agile**: Supports many Agile method terms.
- **CMMI**: Provides the most support for formal processes and change management.

:::row:::
   :::column span="2":::

   **Basic**

   Choose [Basic](../../get-started/plan-track-work.md) when your team wants the simplest model that uses Issue, Task, and Epic work item types to track work.

   Tasks support tracking Remaining Work.

   :::column-end:::
   :::column span="2":::

   :::image type="content" source="../../get-started/media/about-boards/basic-process-epics-issues-tasks-2.png" alt-text="Diagram shows Basic work item types in a hierarchy.":::
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="2":::

   **Agile**

   Choose [Agile](agile-process.md) when your team uses Agile planning methods, including Scrum, and tracks development and test activities separately. This process works great for tracking User Stories and, optionally, bugs on the board. You can also track bugs and tasks on the taskboard.

   For more information about Agile methodologies, see [Agile Alliance](https://www.agilealliance.org/).

   Tasks support tracking Original Estimate, Remaining Work, and Completed Work.

   :::column-end:::
   :::column span="2":::

   :::image type="content" source="media/ALM_PT_Agile_WIT_Artifacts.png" alt-text="Diagram shows Agile work item types in a hierarchy.":::
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="2":::

   **Scrum**

   Choose [Scrum](scrum-process.md) when your team practices Scrum. This process works great for tracking product backlog items and bugs on the board. You can also break down product backlog items and bugs into tasks on the taskboard.

   This process supports the Scrum methodology as defined by the [Scrum organization](https://www.scrum.org/).

   Tasks support tracking Remaining Work only.

   :::column-end:::
   :::column span="2":::

   :::image type="content" source="media/ALM_PT_Scrum_WIT_Artifacts.png" alt-text="Diagram shows Scrum work item types in a hierarchy.":::
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="2":::

   **CMMI**

   Choose [CMMI](cmmi-process.md) when your team follows more formal project methods that require a framework for process improvement and an auditable record of decisions. With this process, you can track requirements, change requests, risks, and reviews.

   This process supports [formal change management activities](./cmmi/guidance-background-to-cmmi.md). Tasks support tracking Original Estimate, Remaining Work, and Completed Work.

   :::column-end:::
   :::column span="2":::

   :::image type="content" source="media/ALM_PT_CMMI_WIT_Artifacts.png" alt-text="Screenshot shows CMMI work item types in a hierarchy.":::
   :::column-end:::
  :::row-end:::
---

If you need more than two or three backlog levels, add more based on the process model that you use:

- **Inheritance**: [Customize your backlogs or boards for a process](../../../organizations/settings/work/customize-process-backlogs-boards.md)
- **Hosted XML or On-premises XML**: [Add portfolio backlogs](../../../reference/add-portfolio-backlogs.md)

<a id="main-distinctions"></a>

## Main distinctions among the default processes

The default processes are designed to meet the needs of most teams. If your team has unusual needs and connects to an on-premises server, customize a process and then create the project. You can also create a project from a process and then customize the project.

The following table summarizes the main distinctions between the work item types and states used by the four default processes.

:::row:::
   :::column span="1":::
   **Tracking area**
   :::column-end:::
   :::column span="1":::
   **Basic**
   :::column-end:::
   :::column span="1":::
   **Agile**
   :::column-end:::
   :::column span="1":::
   **Scrum**
   :::column-end:::
   :::column span="1":::
   **CMMI**
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::

   Workflow states
   :::column-end:::
   :::column span="1":::

   - To Do
   - Doing
   - Done

   :::column-end:::
   :::column span="1":::

   - New
   - Active
   - Resolved
   - Closed
   - Removed

   :::column-end:::
      :::column span="1":::

   - New
   - Approved
   - Committed
   - Done
   - Removed
   :::column-end:::   
   :::column span="1":::

   - Proposed
   - Active
   - Resolved
   - Closed

   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::

   Product planning (see Note 1)
   :::column-end:::
   :::column span="1":::

   - Issue

   :::column-end:::
   :::column span="1":::

   - User Story
   - Bug (optional)

   :::column-end:::
   :::column span="1":::

   - Product backlog item
   - Bug (optional)

   :::column-end:::
   :::column span="1":::

   - Requirement
   - Bug (optional)

   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::

   Portfolio backlogs (see Note 2)
   :::column-end:::
   :::column span="1":::

   - Epic

   :::column-end:::
   :::column span="1":::

   - Epic
   - Feature

   :::column-end:::
   :::column span="1":::

   - Epic
   - Feature

   :::column-end:::
   :::column span="1":::

   - Epic
   - Feature

   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::

   Task and sprint planning (see Note 3)
   :::column-end:::
   :::column span="1":::

   - Task

   :::column-end:::
   :::column span="1":::

   - Task
   - Bug (optional)

   :::column-end:::
      :::column span="1":::

   - Task
   - Bug (optional)

   :::column-end:::
   :::column span="1":::

   - Task
   - Bug (optional)

   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::

   Bug backlog management (see Note 1)
   :::column-end:::
   :::column span="1":::

   - Issue

   :::column-end:::
   :::column span="1":::

   - Bug

   :::column-end:::
   :::column span="1":::

   - Bug

   :::column-end:::
   :::column span="1":::

   - Bug

   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::

   Issue and risk management
   :::column-end:::
   :::column span="1":::

   - Issue

   :::column-end:::
   :::column span="1":::

   - Issue

   :::column-end:::
   :::column span="1":::

   - Impediment

   :::column-end:::
   :::column span="1":::

   - Issue
   - Risk
   - Review

   :::column-end:::
:::row-end:::
---

Notes:

1. Add work items from the [product backlog](../../backlogs/create-your-backlog.md) or [board](../../boards/kanban-overview.md). The product backlog shows a single view of the current backlog of work that can be dynamically reordered and grouped. Product owners can prioritize work and outline dependencies and relationships. Each team can configure how they want [bugs to show up on their backlogs and boards](../../../organizations/settings/show-bugs-on-backlog.md).
1. Define a hierarchy of portfolio backlogs to understand the scope of work across several teams and see how that work rolls up into broader initiatives. Each team configures which [portfolio backlogs appear for their use](../../../organizations/settings/select-backlog-navigation-levels.md).
1. Define tasks from the [sprint backlog and taskboard](../../sprints/assign-work-sprint.md). With capacity planning, teams can determine if they're over capacity or under capacity for a sprint.

<a id="workflow-states"></a>

### Workflow states, transitions, and reasons

Workflow states support tracking the status of work as it moves from a `New` state to a `Closed` or a `Done` state. Each workflow consists of a set of states, the valid transitions between the states, and the reasons for transitioning the work item to the selected state.

> [!IMPORTANT]
> **Workflow transitions:** The default workflow transitions support any state to any state transition for Azure DevOps Services and Azure DevOps Server 2020 and later versions. You can customize these workflows to restrict specific transitions based on your team's requirements. For more information, see [Customize your work tracking experience](../../../reference/customize-work.md).
>
> **Visualizing workflows:** To view the supported workflow transitions for each work item type, install the [State Model Visualization](https://marketplace.visualstudio.com/items?itemName=taavi-koosaar.StateModelVisualization) Marketplace extension. This extension adds a **State Visualizer** hub under **Boards** where you can select a work item type and view its complete workflow state model.

The following diagrams show the typical forward progression of those work item types used to track work and code defects for the three default processes. They also show some of the regressions to former states and transitions to removed states.

Each image shows only the default reason associated with the transition.

#### [Agile process](#tab/agile-process)

:::row:::
   :::column span="1":::

   #### User Story
   :::image type="content" source="media/ALM_PT_Agile_WF_UserStory.png" alt-text="Diagram that shows User Story workflow states by using the Agile process.":::
   :::column-end:::
   :::column span="1":::

   #### Feature
   :::image type="content" source="media/ALM_PT_Agile_WF_Feature.png" alt-text="Diagram that shows Feature workflow states by using the Agile process.":::
   :::column-end:::
   :::column span="1":::

   #### Epic
   :::image type="content" source="media/ALM_PT_Agile_WF_Epic.png" alt-text="Diagram that shows Epic workflow states by using the Agile process.":::
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::

   #### Bug
   :::image type="content" source="media/ALM_PT_Agile_WF_Bug.png" alt-text="Diagram that shows Bug workflow states by using the Agile process.":::
   :::column-end:::
   :::column span="1":::

   #### Task
   :::image type="content" source="media/ALM_PT_Agile_WF_Task.png" alt-text="Diagram that shows Task workflow states by using the Agile process.":::
   :::column-end:::
   :::column span="1":::

   :::column-end:::
:::row-end:::

#### [Basic process](#tab/basic-process)

> [!NOTE]
> The Basic process is available with Azure DevOps Services and Azure DevOps Server 2020 and later versions. For earlier on-premises deployments, use the Agile, Scrum, or CMMI process instead.

:::row:::
   :::column span="1":::

   #### Epic, Issue, and Task hierarchy
   :::image type="content" source="../../get-started/media/track-issues/basic-process-epics-issues-tasks.png" alt-text="Diagram that shows the Basic process work item hierarchy.":::
   :::column-end:::
   :::column span="1":::

   #### Epic, Issue, and Task workflow 
   :::image type="content" source="../../get-started/media/track-issues/basic-process-workflow.png" alt-text="Diagram that shows the Basic process workflow.":::
   :::column-end:::
   :::column span="1":::

   :::column-end:::
:::row-end:::

#### [Scrum process](#tab/scrum-process)

:::row:::
   :::column span="1":::

   #### Product backlog item
   :::image type="content" source="media/alm-pt-scrum-wf-pbi.png" alt-text="Diagram that shows Product backlog item workflow states by using the Scrum process.":::
   :::column-end:::
   :::column span="1":::

   #### Feature
   :::image type="content" source="media/ALM_PT_Scrum_WF_Feature.png" alt-text="Diagram that shows Feature workflow states by using the Scrum process.":::
   :::column-end:::
   :::column span="1":::

   #### Epic
   :::image type="content" source="media/ALM_PT_Scrum_WF_Epic.png" alt-text="Diagram that shows Epic workflow states by using the Scrum process.":::
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::

   #### Bug
   :::image type="content" source="media/alm-pt-scrum-wf-bug.png" alt-text="Diagram that shows Bug workflow states by using the Scrum process.":::
   :::column-end:::
   :::column span="1":::

   #### Task
   :::image type="content" source="media/alm-pt-scrum-wf-task.png" alt-text="Diagram that shows Task workflow states by using the Scrum process.":::
   :::column-end:::
   :::column span="1":::

   :::column-end:::
:::row-end:::

#### [CMMI process](#tab/cmmi-process)

:::row:::
   :::column span="1":::

   #### Requirement
   :::image type="content" source="media/ALM_PT_CMMI_WF_Requirement.png" alt-text="Diagram that shows Requirement workflow states by using the CMMI process.":::
   :::column-end:::
   :::column span="1":::

   #### Feature
   :::image type="content" source="media/ALM_PT_CMMI_WF_Feature.png" alt-text="Diagram that shows Feature workflow states by using the CMMI process.":::
   :::column-end:::
   :::column span="1":::

   #### Epic
   :::image type="content" source="media/ALM_PT_CMMI_WF_Epic.png" alt-text="Diagram that shows Epic workflow states by using the CMMI process.":::
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::

   #### Bug
   :::image type="content" source="media/ALM_PT_CMMI_WF_Bug.png" alt-text="Diagram that shows Bug workflow states by using the CMMI process.":::
   :::column-end:::
   :::column span="1":::

   #### Task
   :::image type="content" source="media/ALM_PT_CMMI_WF_Task.png" alt-text="Diagram that shows Task workflow states by using the CMMI process.":::
   :::column-end:::
   :::column span="1":::

   :::column-end:::
:::row-end:::

* * *

Most work item types used by Agile tools, the ones that appear on backlogs and boards, support any-to-any transitions. Update the status of a work item by using the board or the taskboard. Drag a work item to its corresponding state column.

Change the workflow to support other states, transitions, and reasons. For more information, see [Customize your work tracking experience](../../../reference/customize-work.md).

<a id="removed-closed-done"></a>

### Work item states

When you change the state of a work item to `Removed`, `Closed`, or `Done`, the system responds as follows:

- `Closed` or `Done`: Work items in this state don't appear on the portfolio backlog and backlog pages. They do appear on the sprint backlog pages, board, and taskboard. When you change the portfolio backlog view to **Show backlog items**, for example, to view features to product backlog items, work items in the `Closed` and `Done` state appear.
- `Removed`: Work items in this state don't appear on any backlog or board.

Your project maintains work items as long as the project is active. Even if you set work items to `Closed`, `Done`, or `Removed`, the data store keeps a record. Use a record to create queries or reports.

[!INCLUDE [closed items](../../includes/note-closed-items.md)]

If you need to permanently delete work items, see [Remove or delete work items](../../backlogs/remove-delete-work-items.md).

<a id="wits-all"></a>

## Work item types added to all processes

The following work item types are added to all processes except the Basic process.

:::image type="content" source="media/ALM_PT_WITS_shared.png" alt-text="Diagram that shows work item types used by Test Plans, Microsoft Test Managers, My Work, and Feedback.":::

Your team can create and work with these types by using the corresponding tool.

| Tool  | Work item types |
|:---------|:---------|
| Microsoft Test Manager | `Test Plan`, `Test Suite`, `Test Case Shared Steps`, `Shared Parameters` |
| Request Feedback | `Feedback Request`, `Feedback Response` |
| My Work (from Team Explorer), Code Review | `Code Review Request`, `Code Review Response` |

Work items from these type definitions aren't meant to be created manually and are then added to the `Hidden Types` category. Work item types added to the `Hidden Types` category don't appear on the menus that create new work items.

<a id="test-experience"></a>

### Work item types that support the test experience

Work item types that support the test experience and work with Test Manager and the web portal are linked together by using the link types shown in the following image.

:::image type="content" source="media/ALM_PT_WITS_TestExperience.png" alt-text="Diagram that shows test management work item types.":::

From the web portal or Microsoft Test Manager, view which test cases are defined for a test suite and view which test suites are defined for a test plan. However, these objects aren't connected to each other through link types. Customize these work item types as you would any other work item types. For more information, see [Customize your work tracking experience](../../../reference/customize-work.md).

If you change the workflow for the test plan and test suite, you might need to update the process configuration as described here. For definitions of each test field, see [Create a query based on build and test integration fields](../../queries/build-test-integration.md).

## Related content

<a id="term-note"></a>

- [Customize your work tracking experience](../../../reference/customize-work.md)
- [Upload and download process templates](manage-process-templates.md)
