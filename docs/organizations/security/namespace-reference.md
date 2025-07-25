---
title: Namespace reference  
titleSuffix: Azure DevOps
description: Learn about the security namespaces used to manage access control in Azure DevOps, including associated permissions and detailed information.
ms.subservice: azure-devops-security
ms.custom: quarterly-review
ms.topic: reference
ms.author: chcomley
author: chcomley
monikerRange: '<= azure-devops'
ms.date: 06/23/2025  
---

# Security namespace and permission reference for Azure DevOps 

[!INCLUDE [version-lt-eq-azure-devops](../../includes/version-lt-eq-azure-devops.md)]

This article describes the valid security namespaces, lists the associated permissions, and provides links to more information. Security namespaces store access control lists ([ACLs](/rest/api/azure/devops/security/access-control-lists/query)) on tokens, determining the level of access various entities have to perform specific actions on specific resources. These entities include:

- Azure DevOps users
- Azure DevOps organization owners
- Members of Azure DevOps security groups
- Azure DevOps service accounts
- Azure DevOps service principals

Each family of resources, such as work items or Git repositories, is secured through a unique namespace. Each security namespace contains zero or more ACLs. An ACL includes a token, an inherit flag, and a set of zero or more access control entries (ACEs). Each ACE consists of an identity descriptor, an allowed permissions bitmask, and a denied permissions bitmask. Tokens are arbitrary strings representing resources in Azure DevOps.

> [!NOTE]   
> Namespaces and tokens are valid for all versions of Azure DevOps. Those listed here are valid for Azure DevOps 2019 and later versions. Namespaces are subject to change over time. To get the latest list of namespaces, exercise one of the command line tools or REST API. Some namespaces have been deprecated as listed in the [Deprecated and read-only namespaces](#deprecated-namespaces) section later in this article. For more information, see [Security namespaces query](/rest/api/azure/devops/security/security-namespaces/query)

## Permission management tools 

The recommended method for managing permissions is through the web portal. However, to set permissions that are unavailable through the portal or to manage granular permissions, use command line tools or the REST API:

- For Azure DevOps Services, use the `az devops security permission` commands.
- For Azure DevOps Server, use the TFSSecurity commands.
- For Azure DevOps Git repositories, use the [tf git permission command-line tool](../../repos/tfvc/git-permission-command.md).
- For Team Foundation Version Control (TFVC) repositories, use the [TFVC permission command-line tool](../../repos/tfvc/permission-command.md).

For all Azure DevOps instances, you can also use the [Security REST API](/rest/api/azure/devops/security). 

## Security namespaces and their IDs

Many security namespaces correspond to permissions you set through a **Security** or **Permissions** web portal page. Other namespaces or specific permissions aren't visible through the web portal and grant access by default to members of security groups or Azure DevOps service principals. These namespaces are grouped into the following categories based on how they're managed through the web portal: 

- Object-level 
- Project-level
- Organization or collection-level 
- Server-level (on-premises only) 
- Role-based 
- Internal only 

### Hierarchy and tokens 

A security namespace can be either hierarchical or flat. In a hierarchical namespace, tokens exist in a hierarchy where effective permissions are inherited from parent tokens to child tokens. In contrast, tokens in a flat namespace have no concept of a parent-child relationship between any two tokens.

Tokens in a hierarchical namespace either have a fixed length for each path part, or variable length.
If the tokens have variable-length path parts, then a separator character is used to distinguish where one path part ends and another begins.

Security tokens are case-insensitive. Example of tokens for different namespaces are provided in the following sections. 

## Object-level namespaces and permissions

The following table describes the namespaces that manage object-level permissions. Most of these permissions are managed through the web portal page for each object. Permissions are set at the project level and inherited at the object level unless explicitly changed.

---
:::row:::
   :::column span="1":::
      **Namespace**
   :::column-end:::
   :::column span="1":::
      **Permissions**
   :::column-end:::
   :::column span="2":::
      **Description**
   :::column-end:::
:::row-end:::
---
::: moniker range="<=azure-devops"
:::row:::
   :::column span="1":::
      <a id="analytic-views-namespace"></a>  AnalyticsViews
   :::column-end:::
   :::column span="1":::
      `Read`                
      `Edit`                
      `Delete`              
      `Execute`             
      `ManagePermissions`   
   :::column-end:::
   :::column span="2":::
      [Manages Analytics views permissions at the project-level and object-level](permissions.md#analytics-views-object-level) to read, edit, delete, and generate reports. You can manage these permissions for each [analytics view from the user interface](../../report/powerbi/analytics-security.md).  
      <br/>
      **Token format for project level permissions**: `$/Shared/PROJECT_ID`  
      **Example**: `$/Shared/00001111-aaaa-2222-bbbb-3333cccc4444`   
      <br/>
      **ID:** `d34d3680-dfe5-4cc6-a949-7d9c68f73cba`
   :::column-end:::
:::row-end:::
---
::: moniker-end
:::row:::
   :::column span="1":::
      Build  
   :::column-end:::
   :::column span="1":::
      `ViewBuilds`                       
      `EditBuildQuality`                 
      `RetainIndefinitely`               
      `DeleteBuilds`                     
      `ManageBuildQualities`             
      `DestroyBuilds`                    
      `UpdateBuildInformation`           
      `QueueBuilds`                      
      `ManageBuildQueue`                 
      `StopBuilds`                       
      `ViewBuildDefinition`              
      `EditBuildDefinition`              
      `DeleteBuildDefinition`            
      `OverrideBuildCheckInValidation`   
      `AdministerBuildPermissions`       
   :::column-end:::
   :::column span="2":::
      [Manages build permissions at the project-level and object-level](permissions.md#build-object-level).  
      <br/>
      **Token format for project-level build permissions**: `PROJECT_ID`  
      If you need to update permissions for a particular build definition ID, for example, 12, security token for that build definition looks like the following example:  
      **Token format for project-level, specific build permissions**: `PROJECT_ID/12`  
      **Example**: `00001111-aaaa-2222-bbbb-3333cccc4444/12`  
      <br/>
      **ID:** `33344d9c-fc72-4d6f-aba5-fa317101a7e9`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      CSS  
   :::column-end:::
   :::column span="1":::
      `GENERIC_READ`         
      `GENERIC_WRITE`        
      `CREATE_CHILDREN`      
      `DELETE`               
      `WORK_ITEM_READ`       
      `WORK_ITEM_WRITE`      
      `MANAGE_TEST_PLANS`    
      `MANAGE_TEST_SUITES`   
   :::column-end:::
   :::column span="2":::
      [Manages area path object-level permissions](permissions.md#area-path-object-level) to create, edit, and delete child nodes and set permissions to view or edit work items in a node. For more information, see [Set permissions and access for work tracking,  Create child nodes, modify work items under an area path](set-permissions-access-work-tracking.md#set-permissions-area-path).  
      **Token format example:** `POST https://dev.azure.com/{organization}/_apis/securitynamespaces/{namespaceId}/permissions?api-version=6.0 { "token": "vstfs:///Classification/Node/{area_node_id}", "permissions": { "allow": 1, "deny": 0 } }` 
      <br/>
      **ID:** `83e28ad4-2d72-4ceb-97b0-c7726d5502c3`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      <a id="dashboard-previleges-namespace"></a>  DashboardsPrivileges  
   :::column-end:::
   :::column span="1":::
      `Read`              
      `Create`                  
      `Edit`                    
      `Delete`                  
      `ManagePermissions`       
      `MaterializeDashboards`   
   :::column-end:::
   :::column span="2":::
      [Manages dashboard object-level permissions](permissions.md#dashboards-object-level) to edit and delete dashboards and manage permissions for a project dashboard. You can manage these permissions through the [dashboards user interface](../../report/dashboards/dashboard-permissions.md#set-permissions-for-a-project-dashboard).   
      <br/>
      **Token format for Team Dashboard permissions**: `$/PROJECT_ID/Team_ID/Dashboard_ID`  
      **Example**: `$/66667777-aaaa-8888-bbbb-9999cccc0000/00001111-aaaa-2222-bbbb-3333cccc4444/55556666-ffff-7777-aaaa-8888bbbb9999` 
      <br/><br/>
    **Token format for Project Dashboard permissions**: `$/PROJECT_ID/00001111-aaaa-2222-bbbb-3333cccc4444/Dashboard_ID`  
      **Example**: `$/66667777-aaaa-8888-bbbb-9999cccc0000/00001111-aaaa-2222-bbbb-3333cccc4444/55556666-ffff-7777-aaaa-8888bbbb9999`
      <br/><br/>
      **ID:** `8adf73b7-389a-4276-b638-fe1653f7efc7`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      Git Repositories <a id="git-repositories" />    
   :::column-end:::
   :::column span="1":::
      `Administer`                
      `GenericRead`               
      `GenericContribute`         
      `ForcePush`                 
      `CreateBranch`              
      `CreateTag`                 
      `ManageNote`                
      `PolicyExempt`             
      `CreateRepository`          
      `DeleteRepository`          
      `RenameRepository`          
      `EditPolicies`              
      `RemoveOthersLocks`         
      `ManagePermissions`         
      `PullRequestContribute`    
      `PullRequestBypassPolicy`  
   :::column-end:::
   :::column span="2":::
      [Manages Git repository permissions at the project-level and object-level](permissions.md#git-repository-object-level). You can manage these permissions through the [Project settings, Repositories administrative interface](../../repos/git/set-git-repository-permissions.md).  
      <br/>
      The `Administer` permission was divided into several more granular permissions [in 2017](/previous-versions/azure/devops/2017/jan-25-team-services#repo-admin-permission-changes), and shouldn't be used.
      <br/>
      **Token format for project-level permissions**: `repoV2/PROJECT_ID`  
      <br/>
      You need to append `RepositoryID` to update repository-level permissions.  
      <br/>
      **Token format for repository-specific permissions**: `repoV2/PROJECT_ID/REPO_ID`  
      <br/>
      **Token format for branch-level permissions** is described in [Git repo tokens for the security service](https://devblogs.microsoft.com/devops/git-repo-tokens-for-the-security-service/).
      <br/><br/>
      **ID:** `2e9eb7ed-3c0a-47d4-87c1-0ffdd275fd87`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      Iteration 
   :::column-end:::
   :::column span="1":::
      `GENERIC_READ`      
      `GENERIC_WRITE`     
      `CREATE_CHILDREN`   
      `DELETE`          
   :::column-end:::
   :::column span="2":::
      [Manages iteration path object-level permissions](permissions.md#iteration-path-permissions) to create, edit, and delete child nodes and view child node permissions. To manage through the web portal, see [Set permissions and access for work tracking, Create child nodes](set-permissions-access-work-tracking.md#set-permissions-area-path). <br/>
      **Token format**: `'vstfs:///Classification/Node/Iteration_Identifier/'`  
      Suppose, you have the following iterations configured for your team.  
      &ndash; ProjectIteration1  
      &nbsp;&nbsp;TeamIteration1  
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&ndash; TeamIteration1ChildIteration1  
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&ndash; TeamIteration1ChildIteration2  
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&ndash; TeamIteration1ChildIteration3  
      &nbsp;&nbsp;TeamIteration2  
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&ndash; TeamIteration2ChildIteration1  
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&ndash; TeamIteration2ChildIteration2  
      <br/>
      To update permissions for `ProjectIteration1\TeamIteration1\TeamIteration1ChildIteration1`, the security token looks like the following example:  
      `vstfs:///Classification/Node/ProjectIteration1_Identifier:vstfs:///Classification/Node/TeamIteration1_Identifier:vstfs:///Classification/Node/TeamIteration1ChildIteration1_Identifier`  
      <br/>
      **ID:** `bf7bfa03-b2b7-47db-8113-fa2e002cc5b1`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      MetaTask  
   :::column-end:::
   :::column span="1":::
      `Administer`  
      `Edit`         
      `Delete`      
   :::column-end:::
   :::column span="2":::
      [Manages task group permissions](permissions.md) to edit and delete task groups, and administer task group permissions. To manage through the web portal, see [Pipeline permissions and security roles, Task group permissions](../../pipelines/policies/permissions.md#task-group-permissions). 
      <br/><br/>
      **Token format for project-level permissions**: `PROJECT_ID`
      <br/>
      **Token format for metaTask-level permissions**: `PROJECT_ID/METATASK_ID`
      <br/><br/>
      If MetaTask has parentTaskId, then the Security token looks like the following example: 
      <br/>
      **Token Format**: `PROJECT_ID/PARENT_TASK_ID/METATASK_ID`
      <br/><br/>
      **ID:** `f6a4de49-dbe2-4704-86dc-f8ec1a294436`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      Plan  
   :::column-end:::
   :::column span="1":::
      `View`     
      `Edit`   
      `Delete`   
      `Manage`   
   :::column-end:::
   :::column span="2":::
      [Manages permissions for Delivery Plans](permissions.md) to view, edit, delete, and manage delivery plans. You can manage these permissions through the [web portal for each plan](set-permissions-access-work-tracking.md).      
      <br/>
      **Token Format**: `Plan/PROJECT_ID/DeliveryPlan_ID` 
      <br/><br/>
      **Example**: `Plan/00001111-aaaa-2222-bbbb-3333cccc4444/55556666-ffff-7777-aaaa-8888bbbb9999`   
      <br/>
      **ID:** `bed337f8-e5f3-4fb9-80da-81e17d06e7a8`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      ReleaseManagement 
   :::column-end:::
   :::column span="1":::
      `ViewReleaseDefinition`        
      `EditReleaseDefinition`        
      `DeleteReleaseDefinition`       
      `ManageReleaseApprovers`        
      `ManageReleases`  
      `ViewReleases`                   
      `CreateReleases`               
      `EditReleaseEnvironment`         
      `DeleteReleaseEnvironment`      
      `AdministerReleasePermissions`   
      `DeleteReleases`                 
      `ManageDeployments`             
      `ManageReleaseSettings`         
      `ManageTaskHubExtension`         
   :::column-end:::
   :::column span="2":::
      [Manages release definition permissions at the project and object-level](permissions.md#release-management).  
      <br/>
      **Token format for project-level permissions**: `PROJECT_ID`  
      **Example**: `00001111-aaaa-2222-bbbb-3333cccc4444`  
      If you need to update permissions for a particular release definition ID, for example, 12, security token for that release definition looks as follows:  
      <br/>
      **Token format for specific release definition permissions**: `PROJECT_ID/12`  
      **Example**: `00001111-aaaa-2222-bbbb-3333cccc4444/12`  
      If the release definition ID lives in a folder, then the security tokens look as follows:  
      **Token format**: `PROJECT_ID/{folderName}/12`  
      For stages, tokens look like: `PROJECT_ID/{folderName}/{DefinitionId}/Environment/{EnvironmentId}`.  
      <br/>
      **ID:** `c788c23e-1b46-4162-8f5e-d7585343b5de`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      WorkItemQueryFolders 
   :::column-end:::
   :::column span="1":::
      `Read`    
      `Contribute`   
      `Delete`                    
      `ManagePermissions`          
      `FullControl`                
      `RecordQueryExecutionInfo`   
   :::column-end:::
   :::column span="2":::
      [Manages permissions for work item queries and query folders](permissions.md#query). To manage these permissions through the web portal, see [Set permissions on queries or query folders](set-permissions-access-work-tracking.md#work-item-queries). 
      **Token format example:** `POST https://dev.azure.com/{organization}/_apis/securitynamespaces/{namespaceId}/permissions?api-version=6.0 { "token": "/{project_id}/{shared_queries_id}", "permissions": { "allow": 1, "deny": 0 } }`.
      <br/>
      **ID:** `71356614-aad7-4757-8f2c-0fb3bff6f680`
   :::column-end:::
:::row-end:::
---

## Project-level namespaces and permissions

The following table describes the namespaces that manage project-level permissions. Most of the listed permissions are managed through the [web portal admin context](change-project-level-permissions.md). Project Administrators are granted all project-level permissions, while other project-level groups have specific permission assignments.

---
:::row:::
   :::column span="1":::
      **Namespace**
   :::column-end:::
   :::column span="1":::
      **Permissions**
   :::column-end:::
   :::column span="2":::
      **Description**
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      Project
   :::column-end:::
   :::column span="1":::
      `GENERIC_READ`                   
      `GENERIC_WRITE`                  
      `DELETE`                         
      `PUBLISH_TEST_RESULTS`          
      `ADMINISTER_BUILD`               
      `START_BUILD`                    
      `EDIT_BUILD_STATUS`              
      `UPDATE_BUILD`                   
      `DELETE_TEST_RESULTS`            
      `VIEW_TEST_RESULTS`              
      `MANAGE_TEST_ENVIRONMENTS`       
      `MANAGE_TEST_CONFIGURATIONS`     
      `WORK_ITEM_DELETE`               
      `WORK_ITEM_MOVE`                 
      `WORK_ITEM_PERMANENTLY_DELETE`   
      `RENAME`                         
      `MANAGE_PROPERTIES`             
      `MANAGE_SYSTEM_PROPERTIES`       
      `BYPASS_PROPERTY_CACHE`         
      `BYPASS_RULES`                   
      `SUPPRESS_NOTIFICATIONS`         
      `UPDATE_VISIBILITY`              
      `CHANGE_PROCESS`                 
      `AGILETOOLS_BACKLOG`             
      `AGILETOOLS_PLANS` <!--- TBD, this doesn't get listed --> 
   :::column-end:::
   :::column span="2":::
      [Manages Project-level permissions](permissions.md#project-level-permissions).  
      The `AGILETOOLS_BACKLOG` permission manages access to Azure Boards backlogs. This setting is an internal permission setting and shouldn't be changed.  
      <br/>
      **Root token format**: `$PROJECT`  
      Token to secure permissions for each project in your organization.  
      `$PROJECT:vstfs:///Classification/TeamProject/PROJECT_ID`.  
      Assume you have a project named `Test Project 1`.  
      You can get the project ID for this project by using the [`az devops project show` command](../projects/create-project.md).  
      `az devops project show --project "Test Project 1"`  
      The command returns a project-id, for example, `00001111-aaaa-2222-bbbb-3333cccc4444`.  
      Therefore, the token to secure project-related permissions for `Test Project 1` is:  
      `'$PROJECT:vstfs:///Classification/TeamProject/00001111-aaaa-2222-bbbb-3333cccc4444'`
      <br/>
      **ID:** `52d39943-cb85-4d7f-8fa8-c6baac873819`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      <a id="tagging"></a>Tagging
   :::column-end:::
   :::column span="1":::
      `Enumerate`   
      `Create`      
      `Update`      
      `Delete`      
   :::column-end:::
   :::column span="2":::
      Manages permissions to create, delete, enumerate, and use work item tags. You can manage the **Create tag definition** permission through the [permissions administrative interface](change-project-level-permissions.md).  
      <br/>
      **Token format for project-level permissions**: `/PROJECT_ID`  
      **Example**: `/00001111-aaaa-2222-bbbb-3333cccc4444`  
      <br/>
      **ID:** `bb50f182-8e5e-40b8-bc21-e8752a1e7ae2`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      VersionControlItems
   :::column-end:::
   :::column span="1":::
      `Read`                 
      `PendChange`           
      `Checkin`            
      `Label`                
      `Lock`                 
      `ReviseOther`         
      `UnlockOther`          
      `UndoOther`           
      `LabelOther`           
      `AdminProjectRights`   
      `CheckinOther`         
      `Merge`                
      `ManageBranch`         
   :::column-end:::
   :::column span="2":::
      Manages permissions for a [Team Foundation Version Control (TFVC) repository](permissions.md#tfvc). There's only one TFVC repository for a project. You can manage these permissions through the [repositories administrative interface](../../repos/tfvc/set-tfvc-repository-permissions.md).  
      <br/>
      **ID:** `a39371cf-0841-4c16-bbd3-276e341bc052`
   :::column-end:::
:::row-end:::
---

::: moniker range="azure-devops"

## Organization-level namespaces and permissions 

The following table describes the namespaces that manage organization-level permissions. Most of the listed permissions are managed through the web portal **Organization settings** context. The **Organization owner** and members of the **Project Collection Administrators** group are granted most of these permissions. For more information, see [Change project collection-level permissions](change-organization-collection-level-permissions.md). 
::: moniker-end

::: moniker range="< azure-devops"

## Collection-level namespaces and permissions 

The following table describes the namespaces that manage organization-level permissions. Most of the listed permissions are managed through the web portal **Collection settings** context. Members of the **Project Collection Administrators** group are granted most of these permissions.   For more information, see [Change project collection-level permissions](change-organization-collection-level-permissions.md). 

::: moniker-end

<a id="process"></a>

---
:::row:::
   :::column span="1":::
      **Namespace**
   :::column-end:::
   :::column span="1":::
      **Permissions**
   :::column-end:::
   :::column span="2":::
      **Description**
   :::column-end:::
:::row-end:::
---
::: moniker range="azure-devops"
:::row:::
   :::column span="1":::
      AuditLog  
   :::column-end:::
   :::column span="1":::
      `Read`             
      `Write`            
      `Manage_Streams`   
      `Delete_Streams`   
   :::column-end:::
   :::column span="2":::
      [Manages auditing permissions](permissions.md#audit-streams-permissions) to read or write to the audit log and manage or delete audit streams.  
      <br/>
      **Token format**: `/AllPermissions`
      <br/>
      **ID:** `a6cc6381-a1ca-4b36-b3c1-4e65211e82b6`
   :::column-end:::
:::row-end:::
---
::: moniker-end
:::row:::
   :::column span="1":::
      BuildAdministration  
   :::column-end:::
   :::column span="1":::
      `ViewBuildResources`                   
      `ManageBuildResources`                 
      `UseBuildResources`                    
      `AdministerBuildResourcePermissions`   
      `ManagePipelinePolicies`  
   :::column-end:::
   :::column span="2":::
      [Manages access to view, manage, use, or administer permissions for build resources](permissions.md#collection-level).  
      <br/>
      **ID:** `302acaca-b667-436d-a946-87133492041c`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      Collection  
   :::column-end:::
   :::column span="1":::
      `GENERIC_READ`                 
      `GENERIC_WRITE`                
      `CREATE_PROJECTS`              
      `TRIGGER_EVENT`                
      `MANAGE_TEMPLATE`              
      `DIAGNOSTIC_TRACE`             
      `SYNCHRONIZE_READ`             
      `MANAGE_TEST_CONTROLLERS`      
      `DELETE_FIELD`                 
      `MANAGE_ENTERPRISE_POLICIES`   
   :::column-end:::
   :::column span="2":::
      [Manages permissions at the organization or collection-level](permissions.md#collection-level).    
      <br/>
      **ID:** `3e65f728-f8bc-4ecd-8764-7e378b19bfa7`
   :::column-end:::
:::row-end:::
---
::: moniker range="<=azure-devops"
:::row:::
   :::column span="1":::
      Process 
   :::column-end:::
   :::column span="1":::
      `Edit`                           
      `Delete`                         
      `Create`                         
      `AdministerProcessPermissions`   
      `ReadProcessPermissions`         
   :::column-end:::
   :::column span="2":::
      [Manages permissions to create, delete, and administer processes](permissions.md#administer-process-permissions).
      <br/>
      **ID:** `2dab47f9-bd70-49ed-9bd5-8eb051e59c02`
   :::column-end:::
:::row-end:::
---
::: moniker-end
:::row:::
   :::column span="1":::
      Workspaces 
   :::column-end:::
   :::column span="1":::
      `Read`         
      `Use`         
      `Checkin`      
      `Administer`   
   :::column-end:::
   :::column span="2":::
      Manages permissions for administering shelved changes, workspaces, and the ability to create a workspace at the organization or collection level. The Workspaces namespace applies to the TFVC repository.    
      <br/>
      **Root token format**: `/`  
      **Token format for a specific workspace**: `/{workspace_name};{owner_id}`  
      <br/>
      **ID:** `93bafc04-9075-403a-9367-b7164eac6b5c`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      VersionControlPrivileges
   :::column-end:::
   :::column span="1":::
      `CreateWorkspace`      
      `AdminWorkspaces`      
      `AdminShelvesets`      
      `AdminConnections`     
      `AdminConfiguration`   
   :::column-end:::
   :::column span="2":::
      Manages permissions for [Team Foundation Version Control (TFVC) repository](permissions.md#tfvc).  
      ::: moniker range="< azure-devops"
      > The `AdminConfiguration` permission grants users the ability to edit server-level permissions for users and groups. 
      > The `AdminConnections` permission grants users the ability to read the contents of a file or folder of an on-premises, server-level repository.
      ::: moniker-end
      **ID:** `66312704-deb5-43f9-b51c-ab4ff5e351c3`
   :::column-end:::
:::row-end:::
---

::: moniker range="< azure-devops"

## Server-level namespaces and permissions  

The following table describes those security namespaces and permissions defined for on-premises instances of Azure DevOps Server. You can manage these permissions, which are granted to members of the Team Foundation Administrators group, through the [Azure DevOps Server administration console](/azure/devops/server/admin/add-administrator). For descriptions of these permissions, see [Permissions and groups, Server-level permissions](permissions.md#server-permissions).  
 

---
:::row:::
   :::column span="1":::
      **Namespace**
   :::column-end:::
   :::column span="1":::
      **Permissions**
   :::column-end:::
   :::column span="2":::
      **Description**
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      CollectionManagement 
   :::column-end:::
   :::column span="1":::
      `CreateCollection`        
      `DeleteCollection`           
   :::column-end:::
   :::column span="2":::
      Manages permissions set at the server-level to create and delete project collections.  
      <br/>
      **ID:** `52d39943-cb85-4d7f-8fa8-c6baac873819`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      Server
   :::column-end:::
   :::column span="1":::
      `GenericRead`    
      `GenericWrite`  
      `Impersonate`    
      `TriggerEvent`   
   :::column-end:::
   :::column span="2":::
      Manages permissions set at the server-level. Includes permissions to edit instance-level information, make requests on behalf of others, and trigger events.  
      <br/>
      **ID:** `1f4179b3-6bac-4d01-b421-71ea09171400`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      Warehouse
   :::column-end:::
   :::column span="1":::
      `Administer`                  
   :::column-end:::
   :::column span="2":::
      Grants permission to process or change settings for the data warehouse or SQL Server Analysis cube by using the [Warehouse Control Web Service](/previous-versions/azure/devops/report/admin/manage-reports-data-warehouse-cube).   
      <br/>
      **ID:** `b8fbab8b-69c8-4cd9-98b5-873656788efb`
   :::column-end:::
:::row-end:::
---
 
::: moniker-end

## Role-based namespaces and permissions

The following table describes the security namespaces and permissions used to manage role-based security. You can manage role assignments through the web portal for pipeline resources as described [Pipeline permissions and security roles](../../pipelines/policies/permissions.md).  
 
---
:::row:::
   :::column span="1":::
      **Namespace**
   :::column-end:::
   :::column span="1":::
      **Permissions**
   :::column-end:::
   :::column span="2":::
      **Description**
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      DistributedTask 
   :::column-end:::
   :::column span="1":::
      `View`                    
      `Manage`                  
      `Listen`                  
      `AdministerPermissions`   
      `Use`                     
      `Create`                  
   :::column-end:::
   :::column span="2":::
      Manages permissions to access agent pool resources. By default, the following roles and permissions are assigned at the project level and inherited for each agent pool that is created: 
      - **Reader** role (`View` permissions only) to all members of the Project Valid Users group 
      - **Administrator** role (all permissions) to members of the Build Administrators, Project Administrators, and Release Administrators groups. 
      - **User** role (`View`, `Use`, and `Create` permissions) to all members of the Contributor group 
      - **Creator** role (`View`, `Use`, and `Create` permissions) to all members of the Contributor group<br/><br/>
      **ID:** `101eae8c-1709-47f9-b228-0e476c35b3ba`  
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      Environment
   :::column-end:::
   :::column span="1":::
     ` View`                    
      `Manage`                  
      `ManageHistory`   
      `Administer`     
      `Use`                     
      `Create`                  
   :::column-end:::
   :::column span="2":::
      Manages permissions to create and manage Environments. By default, the following permissions are assigned: 
      - **Reader** role (`View` permissions only) to all members of the Project Valid Users group 
      - **Creator** role (`View`, `Use`, and `Create` permissions) to all members of the Contributor group 
      - **Creator** role (`View`, `Use`, and `Create` permissions) to all members of the Project Administrators group 
      - **Administrator** role (all permissions) to the user who created a specific Environment.<br/><br/>
      **ID:** `83d4c2e6-e57d-4d6e-892b-b87222b7ad20`  
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      ExtensionManagement
   :::column-end:::
   :::column span="1":::
      `ViewExtensions`     
      `ManageExtensions`   
      `ManageSecurity`    
   :::column-end:::
   :::column span="2":::
      The **Manager** role is the only role used to manage the security of Marketplace extensions. Members of the Manager role can install extensions and respond to requests for extensions to be installed. The other permissions are assigned automatically to members of default security groups and service principals. To add users to the Manager role, see [Manage extension permissions](../../marketplace/grant-permissions.md).    
      <br/>
      **ID:** `5d6d7b80-3c63-4ab0-b699-b6a5910f8029`  
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      Library
   :::column-end:::
   :::column span="1":::
      `View`          
      `Administer`    
      `Create`       
      `ViewSecrets`   
      `Use`           
      `Owner`         
   :::column-end:::
   :::column span="2":::
      Manages permissions to create and manage library items, which include secure files and variable groups. Role memberships for individual items are automatically inherited from the Library. By default, the following permissions are assigned: 
      - **Reader** role (`View` permissions only) to all members of the Project Valid Users group and the Project Collection Build Service account
      - **Creator** role (`View`, `Use`, and `Create` permissions) to all members of the Contributors group 
      - **Creator** role (`View`, `Use`, `Create`, and `Owner` permissions) to the member who created the library item
      - **Administrator** role (all permissions) to members of the Build Administrators, Project Administrators, and Release Administrators groups.  
      For more information, see [Library asset security roles](../../pipelines/library/index.md).<br/><br/>
      **ID:** `b7e84409-6553-448a-bbb2-af228e07cbeb`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      ServiceEndpoints
   :::column-end:::
   :::column span="1":::
      `Use`                 
      `Administer`          
      `Create`              
      `ViewAuthorization`   
      `ViewEndpoint`        
   :::column-end:::
   :::column span="2":::
      Manages permissions to create and manage service connections. Role memberships for individual items are automatically inherited from the project-level roles. By default, the following roles are assigned: 
      - **Reader** role (`View` permissions only) to all members of the Project Valid Users group and the Project Collection Build Service account
      - **Creator** role (`View`, `Use`, and `Create` permissions) to members of the Endpoint Creators service security group.  
      - **Administrator** role (all permissions) to members of the Endpoint Administrators service security group.  
      Roles are assigned through [Service connection security roles](about-security-roles.md#service-endpoint-roles).<br/><br/>
      **ID:** `49b48001-ca20-4adc-8111-5b60c903a50c`
   :::column-end:::
:::row-end:::
---

<a id="internal"></a> 

## Internal namespaces and permissions

The following table describes the security namespaces and permissions that aren't surfaced through the web portal. They're primarily used to grant access to members of default security groups or to internal resources. We strongly recommend that you don't alter these permission settings in any way.    
 
---
:::row:::
   :::column span="1":::
      **Namespace**
   :::column-end:::
   :::column span="1":::
      **Permissions**
   :::column-end:::
   :::column span="2":::
      **Description**
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="1":::
      AccountAdminSecurity
   :::column-end:::
   :::column span="1":::
      `Read`       
      `Create`    
      `Modify`     
   :::column-end:::
   :::column span="2":::
      Manages permissions to read or modify the organization account owner. These permissions are assigned to the organization owner and members of the Project Collection Administrator group.  
      <br/>
      **ID:** `11238e09-49f2-40c7-94d0-8f0307204ce4`
   :::column-end:::
:::row-end:::
---
::: moniker range="<=azure-devops"
:::row:::
   :::column span="1":::
      Analytics
   :::column-end:::
   :::column span="1":::
      `Read`                       
      `Administer`                 
      `Stage`                      
      `ExecuteUnrestrictedQuery`   
      `ReadEuii`                   
   :::column-end:::
   :::column span="2":::
      Manages permissions to read, administer permissions, and execute queries against the Analytics service.  
      <br/>
      **Token format for project-level permissions**: `$/PROJECT_ID`  
      **Example**: `$/00001111-aaaa-2222-bbbb-3333cccc4444`  
      <br/>
      **ID:** `58450c49-b02d-465a-ab12-59ae512d6531`
   :::column-end:::
:::row-end:::
---
::: moniker-end
:::row:::
   :::column span="":::
      BlobStoreBlobPrivileges
   :::column-end:::
   :::column span="":::
      `Read`   
      `Delete`   
      `Create`   
      `SecurityAdmin`   
   :::column-end:::
   :::column span="2":::
      Sets permissions to read, delete, create, and manage the security of the data store. These permissions are assigned to several Azure DevOps service principals.  
      <br/>
      **ID:** `19F9F97D-7CB7-45F7-8160-DD308A6BD48E` 
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="":::
      Boards
   :::column-end:::
   :::column span="":::
      `View`             
      `Create`          
      `ChangeMetadata`   
      `MoveCard`         
      `Delete`           
      `Manage`          
   :::column-end:::
   :::column span="2":::
      Manages permissions and access to boards.  
      <br/>
      **ID:** `251e12d9-bea3-43a8-bfdb-901b98c0125e`
   :::column-end:::
:::row-end:::
---
::: moniker range=">= azure-devops-2020"
:::row:::
   :::column span="":::
      BoardsExternalIntegration
   :::column-end:::
   :::column span="":::
      `Read`    
      `Write`    
   :::column-end:::
   :::column span="2":::
      Manages read/write permissions of external integrations with Azure Boards.   
      <br/>
     **ID:** `5ab15bc8-4ea1-d0f3-8344-cab8fe976877`
   :::column-end:::
:::row-end:::
---
::: moniker-end
::: moniker range=">= azure-devops-2020"
:::row:::
   :::column span="":::
      Chat
   :::column-end:::
   :::column span="":::
      `ReadChatRoomMetadata`      
      `UpdateChatRoomMetadata`    
      `CreateChatRoom`           
      `CloseChatRoom`            
      `DeleteChatRoom`            
      `AddRemoveChatRoomMember`   
      `ReadChatRoomMessage`      
      `WriteChatRoomMessage`    
      `UpdateChatRoomMessage`     
      `DeleteChatRoomMessage`    
      `ReadChatRoomTranscript`    
      `ManageChatPermissions`     
   :::column-end:::
   :::column span="2":::
      Manages permissions for chat services integrated with Azure DevOps, such as Slack and Microsoft Teams. For more information, see [Azure Boards with Slack](../../boards/integrations/boards-slack.md), [Azure Boards with Microsoft Teams](../../boards/integrations/boards-teams.md), [Azure Pipelines with Slack](../../pipelines/integrations/slack.md), [Azure Pipelines with Microsoft Teams](../../pipelines/integrations/microsoft-teams.md), [Azure Repos with Slack](../../repos/integrations/repos-slack.md), and [Azure Repos with Microsoft Teams](../../repos/integrations/repos-teams.md).  
      <br/>
      **ID:** `bc295513-b1a2-4663-8d1a-7017fd760d18`
   :::column-end:::
:::row-end:::
---
::: moniker-end
::: moniker range="azure-devops"
:::row:::
   :::column span="":::
      Discussion Threads
   :::column-end:::
   :::column span="":::
      `Administer`          
      `GenericRead`         
      `GenericContribute`   
      `Moderate`            
   :::column-end:::
   :::column span="2":::
      Manages permissions to view, manage, moderate, and contribute to [code review discussions setup for Azure Pipelines](../../pipelines/process/environments-kubernetes.md#set-up-review-app).  
      <br/>
      **ID:** `0d140cae-8ac1-4f48-b6d1-c93ce0301a12`
   :::column-end:::
:::row-end:::
---
::: moniker-end
:::row:::
   :::column span="":::
      EventPublish
   :::column-end:::
   :::column span="":::
      `Read`  
      `Write`  
   :::column-end:::
   :::column span="2":::
      Grants read and write access for notification handler.  
      <br/>
      **ID:** `7cd317f2-adc6-4b6c-8d99-6074faeaf173`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="":::
      EventSubscriber
   :::column-end:::
   :::column span="":::
      `GENERIC_READ`  
      `GENERIC_WRITE`  
   :::column-end:::
   :::column span="2":::
      Grants read and write access for notification subscribers.  
      <br/>
      **ID:** `2bf24a2b-70ba-43d3-ad97-3d9e1f75622f`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="":::
      EventSubscription  
   :::column-end:::
   :::column span="":::
      `GENERIC_READ`  
      `GENERIC_WRITE`  
      `UNSUBSCRIBE`  
      `CREATE_SOAP_SUBSCRIPTION`  
   :::column-end:::
   :::column span="2":::
      Manages member permissions to view, edit, and unsubscribe from notifications or create a SOAP subscription.  
      <br/>
      **ID:** `58b176e7-3411-457a-89d0-c6d0ccb3c52b` 
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      Identity
   :::column-end:::
   :::column span="":::
      `Read`     
      `Write`  
      `Delete`  
      `ManageMembership`  
      `CreateScope`  
      `RestoreScope`  
   :::column-end:::
   :::column span="2":::
      Manages permissions to read, write, and delete user account identity information; manage group membership and create and restore identity scopes. The `ManageMembership` permission is automatically granted to members of the Project Administrators and Project Collection Administrators groups.   
      
      **Token format for project-level permissions**: `PROJECT_ID`  
      **Example**: `00001111-aaaa-2222-bbbb-3333cccc4444`  
      To modify group level permissions for Group Origin ID [11112222-bbbb-3333-cccc-4444dddd5555]:  
      **Token**: `00001111-aaaa-2222-bbbb-3333cccc4444\11112222-bbbb-3333-cccc-4444dddd5555`  
      <br/>
      **ID:** `5a27515b-ccd7-42c9-84f1-54c998f03866` 
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="":::
      Licensing
   :::column-end:::
   :::column span="":::
      `Read`     
      `Create`  
      `Modify`  
      `Delete`  
      `Assign`     
      `Revoke`  
   :::column-end:::
   :::column span="2":::
      Manages the ability to view, add, modify, and remove license levels. These permissions are automatically granted to members of the Project Collection Administrators groups.    
      <br/>
      **ID:** `453e2db3-2e81-474f-874d-3bf51027f2ee` 
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="":::
      PermissionLevel
   :::column-end:::
   :::column span="":::
      `Read`     
      `Create`  
      `Update`  
      `Delete`  
   :::column-end:::
   :::column span="2":::
      Manages the ability to create and download permission reports.  
      <br/>
      **ID:** `25fb0ed7-eb8f-42b8-9a5e-836a25f67e37` 
   :::column-end:::
:::row-end:::
---
::: moniker range="azure-devops"
:::row:::
   :::column span="1":::
      OrganizationLevelData
   :::column-end:::
   :::column span="1":::
      `Project-Scoped Users`                       
   :::column-end:::
   :::column span="2":::
      Applies a system-level deny permission on the namespace, which supports the Project-Scoped User Group. Members of the group receive limited visibility to organization-level data. For more information, see [Manage your organization, Limit  user visibility for projects and more](../../user-guide/manage-organization-collection.md#project-scoped-user-group). <br/> 
      **ID:** `F0003BCE-5F45-4F93-A25D-90FC33FE3AA9`
   :::column-end:::
:::row-end:::
---
::: moniker-end  
:::row:::
   :::column span="":::
      PipelineCachePrivileges
   :::column-end:::
   :::column span="":::
      `Read`  
      `Write`  
   :::column-end:::
   :::column span="2":::
      Manages permissions to read and write [pipeline cache entries](../../pipelines/release/caching.md). These permissions are only assigned to internal Azure DevOps service principles. 
      <br/>
      **ID:** `62a7ad6b-8b8d-426b-ba10-76a7090e94d5` 
   :::column-end:::
:::row-end:::
---
<!---
:::row:::
   :::column span="":::
      Proxy
   :::column-end:::
   :::column span="":::
      `Read`  
      `Manage`  
   :::column-end:::
   :::column span="2":::
      Grants `Read` permissions to members of the Project Valid Users group and `Manage` permissions to members of the Project Administrators group.  
      <br/>
      ID: `cb4d56d2-e84b-457e-8845-81320a133fbb` 
   :::column-end:::
:::row-end:::
---
-->
:::row:::
   :::column span="":::
      ReleaseManagement
   :::column-end:::
   :::column span="":::
      `ViewTaskEditor`                         
      `ViewCDWorkflowEditor`                   
      `ExportReleaseDefinition`                    
      `ViewLegacyUI`                              
      `DeploymentSummaryAcrossProjects`             
      `ViewExternalArtifactCommitsAndWorkItems`     
   :::column-end:::
   :::column span="2":::
      Manages access to Release Management user interface elements.  
      <br/>
      **ID:** `7c7d32f7-0e86-4cd6-892e-b35dbba870bd` 
   :::column-end:::
:::row-end:::
---
::: moniker range="azure-devops"
:::row:::
   :::column span="":::
      SearchSecurity
   :::column-end:::
   :::column span="":::
      `ReadMembers`
      `ReadAnonymous`  
   :::column-end:::
   :::column span="2":::
      This security namespace is used to know if a user is valid or anonymous/public.  
      <br/>
      **ID:** `ca535e7e-67ce-457f-93fe-6e53aa4e4160` 
   :::column-end:::
:::row-end:::
---
::: moniker-end
:::row:::
   :::column span="":::
      ServiceHooks
   :::column-end:::
   :::column span="":::
      `ViewSubscriptions`  
      `EditSubscriptions`    
      `DeleteSubscriptions`  
      `PublishEvents`  
   :::column-end:::
   :::column span="2":::
      Manages permissions to view, edit, and delete service hook subscriptions and publish service hook events. These permissions are automatically assigned to members of the Project Collection Administrators group. `DeleteSubscriptions` is no longer used; `EditSubscriptions` can delete service hooks.  
      <br/>
      **ID:** `cb594ebe-87dd-4fc9-ac2c-6a10a4c92046` 
   :::column-end:::
:::row-end:::
---
::: moniker range="azure-devops"
:::row:::
   :::column span="1":::
      UtilizationPermissions
   :::column-end:::
   :::column span="1":::
      `QueryUsageSummary`
   :::column-end:::
   :::column span="2":::
      Manages permissions to query usage. By default, all members of the Project Collection Administrators groups and users granted Stakeholder access are granted permission to query usage summary for everyone. For more information, see [Rate limits](../../integrate/concepts/rate-limits.md).
      <!--- Update link when new article on Utilization is published. --> 
      **Token format**: `/`
      <br/>
      **ID:** `83abde3a-4593-424e-b45f-9898af99034d`
   :::column-end:::
:::row-end:::
---
::: moniker-end
:::row:::
   :::column span="":::
      WorkItemTrackingAdministration
   :::column-end:::
   :::column span="":::
      `ManagePermissions`  
      `DestroyAttachments`  
   :::column-end:::
   :::column span="2":::
      Manages permissions for administrating work tracking and destroying attachments.
      <br/>
      **ID:** `445d2788-c5fb-4132-bbef-09c4045ad93f`
   :::column-end:::
:::row-end:::
---
:::row:::
   :::column span="":::
      WorkItemTrackingProvision
   :::column-end:::
   :::column span="":::
      `Administer`  
      `ManageLinkTypes`  
   :::column-end:::
   :::column span="2":::
      Manages permissions for changing work tracking processes and managing link types. The WorkItemTrackingProvision namespace is an older security namespace that is mostly used for earlier on-premises versions. The [Process](#process) namespace replaces this namespace for managing processes in Azure DevOps Server 2019 and later versions.   
      <br/>
      **Root token format**: `/$`   
      **Token format for a specific project**: `$/PROJECT_ID`   
      <br/>
      **ID:** `5a6cd233-6615-414d-9393-48dbb252bd23`
   :::column-end:::
:::row-end:::
---

<a id="deprecated-namespaces"></a> 

## Deprecated and read-only namespaces

The following namespaces are either deprecated or read-only. You shouldn't use them. 

:::row:::
   :::column span="2":::
      - `CrossProjectWidgetView`
      - `DataProvider`
      - `Favorites`
      - `Graph`
      - `Identity2`
      - `IdentityPicker`
      - `Job`
      - `Location`
      - `ProjectAnalysisLanguageMetrics`
      - `Proxy`
      - `Publish` 
      - `Registry` 
      - `Security`
   :::column-end:::
   :::column span="2":::
      - `ServicingOrchestration`  
      - `SettingEntries`  
      - `Social`  
      - `StrongBox`  
      - `TeamLabSecurity`  
      - `TestManagement`
      - `VersionControlItems2`  
      - `ViewActivityPaneSecurity` 
      - `WebPlatform`  
      - `WorkItemsHub` 
      - `WorkItemTracking` 
      - `WorkItemTrackingConfiguration` 
   :::column-end:::
:::row-end:::
---
 

## Related content

- [Learn about security, authentication, and authorization](about-security-identity.md)
- [Use the security Namespaces REST API](/rest/api/azure/devops/security/)
- [Use TFSSecurity](/azure/devops/server/command-line/tfssecurity-cmd) 
- [Understand security terms with the security glossary](security-glossary.md)

