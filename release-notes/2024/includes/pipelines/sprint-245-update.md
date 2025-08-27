---
author: ckanyika
ms.author: ckanyika
ms.date: 9/26/2024
ms.topic: include
---

### Azure Pipeline agent v4 runs on .NET 8


The Azure Pipeline agent v3 currently uses .NET 6, but with the end-of-life for .NET 6 approaching, we're upgrading the agent to .NET 8. This update will be rolled out over the coming weeks.

If you're using self-hosted agents on an operating system that isn't supported by .NET 8, your agent won’t be upgraded to v4. Instead, pipelines running on unsupported operating systems display warnings in the pipeline logs. You can use the [QueryAgentPoolsForCompatibleOS.ps1 script](https://github.com/microsoft/azure-pipelines-agent/tree/master/tools/FindAgentsNotCompatibleWithAgent) to identify any pipeline agents running on outdated operating systems proactively.

The following operating system versions won't be supported by the updated v4 agent:

- Alpine Linux 3.13 - 3.16
- Debian 10
- Fedora 36 - 38
- macOS 10 & 11
- openSUSE 15.0 - 15.4
- Oracle Linux 7
- Red Hat Enterprise Linux 7
- SUSE Enterprise Linux 12
- Ubuntu, 16.04, 18.04
- Windows 7, 8 & 10 up to 21H2

