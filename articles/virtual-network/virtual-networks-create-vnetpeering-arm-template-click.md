---
title: "Azure 虚拟网络对等互连 - 模板 | Microsoft Docs"
description: "了解如何使用 Azure Resource Manager 模板创建虚拟网络对等互连。"
services: virtual-network
documentationcenter: 
author: narayanannamalai
manager: jefco
editor: 
tags: azure-resource-manager
ms.assetid: 75f8d10e-23e8-44bd-9972-aab74048cf38
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: narayan;annahar
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 86eec91025ae8ae553fa1f76a687e440784e7855
ms.lasthandoff: 04/27/2017


---
# <a name="create-a-virtual-network-peering-using-an-azure-resource-manager-template"></a>使用 Azure Resource Manager 模板创建虚拟网络对等互连
[!INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[!INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

若要使用 Resource Manager 模板创建 VNet 对等互连，请完成以下步骤：

1. 如果你从未使用过 Azure PowerShell，请参阅 [How to Install and Configure Azure PowerShell](/powershell/azure/overview) （如何安装和配置 Azure PowerShell），并始终按照说明进行操作，以登录到 Azure 并选择你的订阅。

   > [!NOTE]
   > 用于管理 VNet 对等互连的 PowerShell cmdlet 随附于 [Azure PowerShell 1.6](http://www.powershellgallery.com/packages/Azure/1.6.0)
   > 
   > 
2. 以下文本显示基于上述方案的从 VNet1 到 VNet2 的 VNet 对等互连链接定义。 复制以下内容并将其保存到名为 VNetPeeringVNet1.json 的文件中。

    ```json
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }
    ```
3. 以下部分显示基于上述方案的从 VNet2 到 VNet1 的 VNet 对等互连链接定义。  复制以下内容并将其保存到名为 VNetPeeringVNet2.json 的文件中。

    ```json
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }
    ```
    如以上模板所示，VNet 对等互连有几个可配置属性：
   
   | 选项 | 说明 | 默认 |
   |:--- |:--- |:--- |
   | 允许虚拟网络访问 |是否将对等 VNet 的地址空间包括为 Virtual_network 标记的一部分。 |是 |
   | 允许转发的流量 |是否接受或丢弃不是源自对等 VNet 的流量。 |否 |
   | 允许网关传输 |允许对等 VNet 使用你的 VNet 网关。 |否 |
   | 使用远程网关 |使用对等方的 VNet 网关。 对等 VNet 必须已配置网关并且已选中“允许网关传输”。 如果你已配置了网关，则无法使用此选项。 |否 |
   
    VNet 对等互连中的每个链接都有上述属性集。 例如，可针对 VNet1 到 VNet2 的 VNet 对等互连链接将“允许虚拟网络访问”设置为“Ture”，而针对另一方向的 VNet 对等互连链接设置为“False”。
4. 若要部署模板文件，可以运行 `New-AzureRmResourceGroupDeployment` 创建或更新部署。 有关使用 Resource Manager 模板的详细信息，请参阅此 [文章](../azure-resource-manager/resource-group-template-deploy.md)。

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all
    ```

   > [!NOTE]
   > 请根据需要替换资源组名称和模板文件。
   > 
   > 
   
    以下是基于前一方案的一个示例：

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all
    ```

    返回的输出：
   
        DeploymentName        : VNetPeeringVNet1
        ResourceGroupName    : VNet101
        ProvisioningState        : Succeeded
        Timestamp            : MM/DD/YEAR 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters            :
        Outputs            :
        DeploymentDebugLogLevel : RequestContent, ResponseContent
   
    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all
    ```

    返回的输出：
   
        DeploymentName        : VNetPeeringVNet2
        ResourceGroupName    : VNet101
        ProvisioningState        : Succeeded
        Timestamp            : MM/DD/YEAR 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters            :
        Outputs            :
        DeploymentDebugLogLevel : RequestContent, ResponseContent
5. 完成部署后，运行以下 cmdlet 查看对等互连状态：

    ```powershell
    Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2
    ```

    返回的输出：
   
        Name            : LinkToVNet2
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : VNet101
        VirtualNetworkName    : VNet1
        ProvisioningState        : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null
   
    此方案中的对等互连建立好后，应该能够启动这两个 VNet 中的任何 VM 之间的连接。 默认情况下，`AllowVirtualNetworkAccess` 为 *True*，且 VNet 对等互连将预配正确的 ACL 以允许 VNet 之间的通信。 仍可应用网络安全组 (NSG) 规则来阻止特定子网或虚拟机之间的连接，从而实现两个虚拟网络之间访问权限的细粒度控制。 阅读[网络安全组](virtual-networks-create-nsg-arm-ps.md)一文，了解有关 NSG 的详细信息。

[!INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

若要跨订阅创建 VNet 对等互连，请完成以下步骤：

1. 使用订阅 A 的特权用户 A 帐户登录到 Azure，并运行以下 cmdlet：

    ```powershell
    New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5
    ```

    这不是必须的。 只要请求匹配，即使用户分别提出针对各个 Vnet 的对等互连请求，也可建立对等互连。 添加另一个 VNet 的特权用户作为本地 VNet 用户可以更轻松地执行安装程序。
2. 使用订阅 B 的特权用户 B 帐户登录到 Azure，并运行以下 cmdlet：

    ```powershell
    New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3
    ```

    > [!IMPORTANT]
    > 如果要在通过 Azure Resource Manager 部署模型创建的两个 VNet 之间创建对等互连，请继续执行本部分中的剩余步骤。 如果两个 VNet 是通过不同部署模型创建的，请跳过本部分的剩余步骤并完成本文的[将通过不同部署模型创建的虚拟网络对等互连](#x-model)部分中所述的步骤。

3. 在用户 A 的登录会话中，运行以下 cmdlet：

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all
    ```
   
    json 如下所示：

    ```json
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }
    ```
4. 在用户 B 的登录会话中，运行以下 cmdlet：

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all
    ```

    JSON 如下所示：

    ```json
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }
    ```

    此方案中的对等互连建立好后，应该能够跨订阅启动两个 Vnet 中的任何虚拟机之间的连接。

[!INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. 在此方案中，可以部署以下示例模板以建立 VNet 对等互连。 需要将 `AllowForwardedTraffic` 属性设置为 *True*，以允许对等 VNet 中的网络虚拟设备发送和接收流量。

    以下 JSON 是用于创建从 HubVNet 到 VNet1 的 VNet 对等互连的模板内容。 请注意，AllowForwardedTraffic 已设置为 False。

    ```json
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }
    ```
2. 以下 JSON 是用于创建从 VNet1 到 HubVnet 的 VNet 对等互连的模板内容。 请注意，AllowForwardedTraffic 已设置为 True。

    ```json
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }
    ```
3. 建立对等互连后，可以参考此[文章](virtual-network-create-udr-arm-ps.md)来定义用户定义的路由 (UDR)，以便通过虚拟设备重定向 VNet1 流量以使用其功能。 在路由中指定下一个跃点地址时，可以在对等 VNet HubVNet 中将其设置为虚拟设备的 IP 地址。

[!INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

若要通过不同的部署模型在虚拟网络之间创建对等互连，请完成以下步骤：

1. 如果要在*同一*订阅中创建通过不同部署模型部署的 VNet 之间的对等互连，请跳到步骤 2。 在**预览**版中，能够在*不同*订阅中创建通过不同部署模型部署的 VNet 之间的对等互连。 预览版功能不具有与正式版功能相同级别的可靠性和服务级别协议。 如果要在不同订阅中创建通过不同部署模型部署的 VNet 之间的对等互连，必须先完成以下任务：
    - 通过从 PowerShell 输入以下命令在 Azure 订阅中注册预览版功能：`Register-AzureRmProviderFeature -FeatureName AllowClassicCrossSubscriptionPeering -ProviderNamespace Microsoft.Network` 和 `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network`
    - 完成本文的[跨订阅对等互连](#x-sub)部分中的步骤 1-2。
2. 以下文本介绍了此方案中 VNET1 与 VNET2 之间的 VNet 对等互连链接的定义。 只需创建一个链接即可在经典虚拟网络和 Azure Resource Manager 虚拟网络之间实现对等互连。

    请务必将订阅 ID 放入经典虚拟网络或 VNET2 所在的位置，并将 MyResouceGroup 更改为相应的资源组名称。

    ```json
       {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [

            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNET1/LinkToVNET2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.ClassicNetwork/virtualNetworks', 'VNET2')]"
        }

            }
            }
        ]
        }
    ```
3. 若要部署模板文件，请运行以下 cmdlet 创建或更新部署：

    ```powershell
        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all
    ```

    返回的输出：
   
        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent
4. 成功完成部署后，运行以下 cmdlet 查看对等互连状态：

    ```powershell
    Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2
    ```

    返回的输出：
   
        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    在经典 VNet 和 Resource Manager VNet 之间建立对等互连后，应该可以开始在 VNET1 中的任何虚拟机与 VNET2 中的任何虚拟机之间建立连接，反之亦然。


