---
title: "使用 Service Fabric Explorer 可视化群集 | Microsoft 文档"
description: "Service Fabric Explorer 是一个用于检验和管理 Microsoft Azure Service Fabric 群集中的云应用程序和节点的 Web 工具。"
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 2bf6bd76b653e30f38595631eff576d8eeb8efda
ms.contentlocale: zh-cn
ms.lasthandoff: 04/27/2017


---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>使用 Service Fabric Explorer 可视化群集
Service Fabric Explorer 是一个用于检验和管理 Azure Service Fabric 群集中的应用程序和节点的 Web 工具。 Service Fabric Explorer 管理器直接托管在群集内，因此，无论群集在何处运行，它都始终可供使用。

## <a name="video-tutorial"></a>视频教程

若要了解如何使用 Service Fabric Explorer，请观看下面的 Microsoft 虚拟大学视频：

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-to-service-fabric-explorer"></a>连接到 Service Fabric Explorer
如果已根据说明[准备开发环境](service-fabric-get-started.md)，则可以通过导航到 http://localhost:19080/Explorer 来启动本地群集上的 Service Fabric Explorer。

> [!NOTE]
> 如果你使用 Internet Explorer 配合 Service Fabric Explorer 来管理远程群集，则需要配置一些 Internet Explorer 设置。 转到“**工具**” > “**兼容性视图设置**”，然后取消选中“**在兼容性视图中显示 Intranet 站点**”，以确保正确加载所有信息。
>
>

## <a name="understand-the-service-fabric-explorer-layout"></a>了解 SService Fabric Explorer 的布局
可以使用左侧的树来导航 Service Fabric Explorer。 在树根中，群集仪表板提供了群集的概述，包括应用程序和节点运行状况的摘要。

![Service Fabric Explorer 群集仪表板][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>查看群集的布局
Service Fabric 群集中的节点横跨容错域和升级域的二维网格放置。 这种放置可确保应用程序在发生硬件故障及应用程序升级时仍然可用。 你可以使用群集图查看当前群集的布局方式。

![Service Fabric Explorer 群集图][sfx-cluster-map]

### <a name="view-applications-and-services"></a>查看应用程序和服务
群集包含两个子树：一个用于应用程序，另一个用于节点。

你可以使用应用程序视图来导航 Service Fabric 的逻辑层次结构：应用程序、服务、分区和副本。

在以下示例中，应用程序 **MyApp** 由两个服务 **MyStatefulService** 与 **WebService** 组成。 由于 **MyStatefulService** 是有状态的，因此它包含一个分区，其中有一个主副本和两个辅助副本。 相反，WebSvcService 是无状态的，只包含单个实例。

![Service Fabric Explorer 应用程序视图][sfx-application-tree]

在树的每个级别，主窗格显示有关项目的信息。 例如，你可以看到特定服务的运行状况和版本。

![Service Fabric Explorer 基本信息窗格][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>查看群集的节点
节点视图显示群集的物理布局。 对于给定的节点，你可以检查已在该节点上部署代码的应用程序。 更具体地说，你可以看到当前在那里运行的副本。

## <a name="actions"></a>操作
Service Fabric Explorer 提供用于对群集中的节点、应用程序和服务快速调用操作的方式。

例如，若要删除某应用程序实例，只需从左侧树中选择该应用程序，然后选择“操作” > “删除应用程序”。

![Service Fabric Explorer 中删除应用程序][sfx-delete-application]

> [!TIP]
> 可以通过单击每个元素旁边的省略号来执行相同的操作。
>
>

下表列出了可对每个实体执行的操作：

| **实体** | **操作** | **说明** |
| --- | --- | --- |
| 应用程序类型 |取消预配类型 |从群集的映像存储中删除应用程序包。 需要先删除该类型的所有应用程序。 |
| 应用程序 |删除应用程序 |删除应用程序，包括其所有的服务以及状态（如果有）。 |
| 服务 |删除服务 |删除服务及其状态（如果有）。 |
| 节点 |激活 |激活节点。 |
| 节点 | 停用（暂停） | 在其当前的状态中暂停节点。 服务继续运行，但 Service Fabric 并不主动在节点上移入或移出任何项目，除非需要避免服务中断或数据不一致的问题。 此操作通常用于在特定节点上启用调试服务，以确保它们不在检查期间移动。 | |
| 节点 | 停用（重新启动） | 从节点中安全删除所有内存服务，并关闭永久性服务。 通常在需要重新启动主机进程或计算机时使用。 | |
| 节点 | 停用（删除数据） | 在构建足够的备用副本之后，安全关闭节点上运行的所有服务。 通常在永久性淘汰某个节点（或至少其存储）时使用。 | |
| 节点 | 删除节点状态 | 从群集中删除节点副本的信息。 通常在发生故障的节点肯定无法恢复时使用。 | |
| 节点 | 重新启动 | 重启节点可模拟节点故障。 详细信息请参见[此处](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps) | |

由于许多操作都具有破坏性，因此在完成该操作之前，系统可能会请求你确认意图。

> [!TIP]
> 可以通过 Service Fabric Explorer 执行的每个操作也可以通过 PowerShell 或 REST API 执行，以实现自动化。
>
>

还可使用 Service Fabric Explorer 为给定应用程序类型和版本创建应用程序实例。 在树视图中选择应用程序类型，在右窗格中单击想要的版本旁边的“**创建应用实例**”链接。

![在 Service Fabric Explorer 中创建应用程序实例][sfx-create-app-instance]

> [!NOTE]
> 当前无法对通过 Service Fabric Explorer 创建的应用程序实例进行参数化。 它们是使用默认参数值创建的。
>
>

## <a name="connect-to-a-remote-service-fabric-cluster"></a>连接到远程 Service Fabric 群集
如果知道群集的终结点并具有足够的权限，则可从任一浏览器访问 Service Fabric Explorer。 这是因为 Service Fabric Explorer 只是在该群集中运行的另一个服务。

### <a name="discover-the-service-fabric-explorer-endpoint-for-a-remote-cluster"></a>发现远程群集的 Service Fabric Explorer 终结点
若要访问给定群集的 Service Fabric Explorer，请将浏览器指向：

http://&lt;your-cluster-endpoint&gt;:19080/Explorer

对于 Azure 群集，Azure 门户的群集基本信息窗格中也提供了 Azure 群集的完整 URL。

### <a name="connect-to-a-secure-cluster"></a>连接到安全群集
可以使用证书或 Azure Active Directory (AAD) 控制客户端对 Service Fabric 群集的访问。

如果尝试在安全群集上连接到 Service Fabric Explorer，则需提供客户端证书或使用 AAD 登录，具体取决于群集的配置。

## <a name="next-steps"></a>后续步骤
* [可测试性概述](service-fabric-testability-overview.md)
* [在 Visual Studio 中管理 Service Fabric 应用程序](service-fabric-manage-application-in-visual-studio.md)
* [使用 PowerShell 部署 Service Fabric 应用程序](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png

