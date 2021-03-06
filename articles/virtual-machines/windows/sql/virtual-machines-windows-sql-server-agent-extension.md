---
title: "在 SQL VM (Resource Manager) 上自动完成管理任务 | Microsoft 文档"
description: "本主题介绍如何管理可以自动执行特定 SQL Server 管理任务的 SQL Server 代理扩展。 这些任务包括自动备份、自动修补和 Azure 密钥保管库集成。 本主题使用 Resource Manager 部署模型。"
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/24/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 328b7cff01b485a908be65f52425ff4e81a96b53
ms.lasthandoff: 04/27/2017

---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-resource-manager"></a>使用 SQL Server 代理扩展 (Resource Manager) 在 Azure 虚拟机上自动完成管理任务
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [经典](../classic/sql-server-agent-extension.md)
> 
> 

Azure 虚拟机上运行的 SQL Server IaaS 代理扩展 (SQLIaaSExtension) 可以自动执行管理任务。 本主题概述了该扩展支持的服务以及有关安装、状态及删除的说明。

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

若要查看这篇文章的经典版，请参阅[适用于 SQL Server 经典 VM 的 SQL Server 代理扩展](../classic/sql-server-agent-extension.md)。

## <a name="supported-services"></a>支持的服务
SQL Server IaaS 代理扩展支持以下管理任务：

| 管理功能 | 说明 |
| --- | --- |
| **SQL 自动备份** |对 VM 中的 SQL Server 默认实例自动执行所有数据库的备份计划。 有关详细信息，请参阅 [Azure 虚拟机中 SQL Server 的自动备份 (Resource Manager)](virtual-machines-windows-sql-automated-backup.md)。 |
| **SQL 自动修补** |配置维护时段，可在此时段更新 VM，避开工作负荷的高峰期。 有关详细信息，请参阅 [Azure 虚拟机中 SQL Server 的自动修补 (Resource Manager)](virtual-machines-windows-sql-automated-patching.md)。 |
| **Azure 密钥保管库集成** |可让你在 SQL Server VM 上自动安装和配置 Azure 密匙保管库。 有关详细信息，请参阅 [为 Azure VM 上的 SQL Server 配置 Azure 密钥保管库集成 (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md)。 |

## <a name="prerequisites"></a>先决条件
在 VM 上使用 SQL Server IaaS 代理扩展的要求：

**操作系统**：

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server 版本**：

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell**：

* [下载和配置最新 Azure PowerShell 命令](/powershell/azure/overview)

## <a name="installation"></a>安装
当你预配某个 SQL Server 虚拟机库映像时，系统会自动安装 SQL Server IaaS 代理扩展。

还可在仅有 OS 的 Windows Server 虚拟机上安装 SQL Server IaaS 代理扩展。 此操作仅适用于还在此计算机上手动安装了 SQL Server 的情况。 然后使用 Set-AzureVMSqlServerExtension PowerShell cmdlet 手动安装扩展。 例如，以下命令将在仅限操作系统的 Windows Server VM 上安装扩展，并将其命名为“SQLIaaSExtension”。

    Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"

> [!NOTE]
> 若手动安装 SQL Server IaaS 代理扩展，则无法通过 Azure 门户管理 SQL Server 配置设置。 在此方案中，必须使用 PowerShell 进行所有更改。

如果更新到最新版本的 SQL IaaS 代理扩展，则必须在更新该扩展后重启虚拟机。

## <a name="status"></a>状态
验证是否已安装扩展的方法之一是在 Azure 门户中查看代理状态。 在虚拟机边栏选项卡中选择“所有设置”，然后单击“扩展”。 随后将列出“SQLIaaSExtension”扩展。

![Azure 门户中的 SQL Server IaaS 代理扩展](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

也可以使用 **Get-AzureVMSqlServerExtension** Azure Powershell cmdlet。

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

上一个命令确认已安装代理并提供常规状态信息。 还可使用以下命令获取有关自动备份和修补的特定状态信息。

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>删除
在 Azure 门户中，可以通过单击虚拟机属性的“扩展”边栏选项卡中的省略号来卸载扩展。 然后单击“删除”。

![在 Azure 门户中卸载 SQL Server IaaS 代理扩展](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

也可以使用 **Remove-AzureRmVMSqlServerExtension** Powershell cmdlet。

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>后续步骤
开始使用扩展支持的服务之一。 有关详细信息，请参阅本文的[支持的服务](#supported-services)部分中提到的主题。

有关在 Azure 虚拟机中运行 SQL Server 的详细信息，请参阅 [Azure 虚拟机中的 SQL Server 概述](virtual-machines-windows-sql-server-iaas-overview.md)。


