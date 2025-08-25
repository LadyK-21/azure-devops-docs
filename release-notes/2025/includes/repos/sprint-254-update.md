---
author: ckanyika
ms.author: ckanyika
ms.date: 4/3/2025
ms.topic: include
---

### TFVC check-in policies changes

#### New version (19.255.0-preview) of Microsoft.TeamFoundationServer.ExtendedClient NuGet package
NuGet `Microsoft.TeamFoundationServer.ExtendedClient` package was updated with new TFVC policy classes and methods.


#### Policy changes

We're making changes to how TFVC check-in policies are stored in Azure DevOps, which also means updates to how the NuGet Microsoft.TeamFoundationServer.ExtendedClient communicates with the service. 


If your TFVC project uses check-in policies, migrate those policies to the new format. There are two ways to do this:

1. Using Visual Studio.

> [!WARNING]
> Dangerous certain consequences of an action.: Ensure you updated Visual Studio to the latest version before proceeding (VS 2022, VS 2019, and VS 2017 with minimal versions `17.14 Preview 3`, `17.13.6`, `17.12.7`, `17.10.13`, `17.8.20`, `16.11.46`, `15.9.72` are supporting the new policies).

To create new policies using Visual Studio project administrator should open **Settings** -> Team Project -> Source Control -> Check-in Policy and add new policy (without "Obsolete" mark) with the same parameters as old one:

> [!div class="mx-imgBorder"]
> [![Screenshot of before fix](../../media/254-repos-01.png "Screenshot of before fix")](../../media/254-repos-01.png#lightbox)

2. If you're using custom implementation of `Microsoft.TeamFoundationServer.ExtendedClient` to communicate with server, please follow the [migration guide](/azure/devops/repos/tfvc/tfvc-check-in-policy-migrate-guide).



The migration is required for keeping TFVC check-in compatible with the future Azure DevOps versions. For the time being, both old (Obsolete) and new policies remain valid and functional.
For information on the Future Plans, see our [blog post](https://devblogs.microsoft.com/devops/?p=70556&preview=true).


### Enhancement to GetRepository API

We have added `creationDate` property to the response of Repositories - Get Repository API returning repository creation date. The property is available on the API versions `7.2-preview` and higher.

### Enhancement to Pull Requests Query API

We have introduced a new `Label` property in the response of Pull Request Query - Get API. You can now specify whether to include labels (tags) for related pull requests in every query.
A new `Include` property is available - if set to Labels, the response includes labels for the specified PRs.
If left as `null`, labels won't be included.
To prevent unintended errors, ensure that `NotSet` isn't explicitly assigned - this will result in `Bad Request`.

> [!NOTE]
> Label enrichment resource utilization depends on the number of assigned labels and their length. Requesting labels can impact throttling and increase network load. To optimize the performance, we recommend avoiding unnecessary label requests.

**Request payload example :**
```json
{
    "queries": [
        {
            "type": "lastMergeCommit",
            "include": "Labels",
            "items": [ 
                "0d6c9b2b524113113fced41aecbf8631a4649dec"
            ]
        },
        {
            "type": "lastMergeCommit",
            "items": [
                "b524113113f0dd41aecbf8631a4649dec6c9b2ce"
            ]
        }
    ]
}
```