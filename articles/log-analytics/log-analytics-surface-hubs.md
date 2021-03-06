---
title: "使用 Azure Log Analytics 监视 Surface Hub | Microsoft 文档"
description: "使用 Surface Hub 解决方案来跟踪 Surface Hub 的运行状况并了解它们的使用情况。"
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: a0c8af30fbed064001c3fd393bf0440aa1cb2835
ms.openlocfilehash: d568c52a7cbbe593658fb95203bfa98af13a1554
ms.lasthandoff: 02/28/2017


---
# <a name="monitor-surface-hubs-with-log-analytics-to-track-their-health"></a>使用 Log Analytics 监视 Surface Hub 以跟踪其运行状况

本文介绍如何使用 Log Analytics 中的 Surface Hub 解决方案来监视具有 Microsoft Operations Management Suite (OMS) 的 Microsoft Surface Hub 设备。 Log Analytics 可帮助你同时了解 Surface Hub 的运行状况和使用情况。

每个 Surface Hub 都安装了 Microsoft Monitoring Agent。 通过代理，你可以将数据从 Surface Hub 发送到 OMS。 日志文件从 Surface Hub 读取，然后发送到 OMS 服务。 类似于服务器处于脱机状态、日历不同步，或设备帐户无法登录 Skype 等这些问题都将显示在 Surface Hub 仪表板的 OMS 中。 通过使用仪表板中的数据，你可以确定未运行或遇到其他问题的设备，并潜在解决检测到的问题。

## <a name="installing-and-configuring-the-solution"></a>安装和配置解决方案
使用以下信息来安装和配置解决方案。 为了从 Microsoft Operations Management Suite (OMS) 管理 Surface Hub，需要设置以下各项：

* 有效订阅 [OMS](http://www.microsoft.com/oms)。
* [OMS 订阅](https://azure.microsoft.com/pricing/details/log-analytics/)级别，用于支持你想要监视的设备数。 根据注册设备的数量以及处理的数据量，OMS 定价会有所不同。 在规划 Surface Hub 的部署时需要考虑这一点。

接下来，你需要将 OMS 订阅添加到现有的 Microsoft Azure 订阅，或者直接通过 OMS 门户新建一个工作区。 有关上述任一方法的详细使用说明，请参阅 [Log Analytics 入门](log-analytics-get-started.md)。 在设置 OMS 订阅后，有两种方法可注册 Surface Hub 设备：

* 通过 InTune 自动注册
* 通过 Surface Hub 设备上的“**设置**”手动注册。

## <a name="set-up-monitoring"></a>设置监视
可以使用 OMS 中的 Log Analytics 监视 Surface Hub 的运行状况和活动。 可以使用 InTune 在 OMS 中注册 Surface Hub，或在本地使用 Surface Hub 上的“**设置**”手动注册。

## <a name="connect-surface-hubs-to-oms-through-intune"></a>通过 InTune 将 Surface Hub 连接到 OMS
要管理 Surface Hub，你需要为 OMS 工作区提供工作区 ID 和工作区密钥。 这些信息可从 OMS 门户获得。

InTune 是 Microsoft 的一个产品，它允许你集中管理应用于一个或多个设备的 OMS 配置设置。 请按照以下步骤通过 InTune 来配置你的设备：

1. 登录到 InTune。
2. 导航到“**设置**” > “**连接源**”。
3. 创建或编辑基于 Surface Hub 模板的策略。
4. 导航到策略的 OMS（Azure 操作见解）部分，为策略添加“*工作区 ID*”和“*工作区密钥*”。
5. 保存策略。
6. 关联策略和相应的设备组。

   ![InTune 策略](./media/log-analytics-surface-hubs/intune.png)

InTune 然后会在 OMS 工作区中注册设备，从而将 OMS 设置与目标组中的设备同步。

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>使用“设置”应用将 Surface Hub 连接到 OMS
要管理 Surface Hub，你需要为 OMS 工作区提供工作区 ID 和工作区密钥。 这些信息可从 OMS 门户获得。

如果不使用 InTune 来管理你的环境，你可以通过每个 Surface Hub 上的“**设置**”手动注册设备：

1. 从 Surface Hub 打开“**设置**”。
2. 在出现提示时，输入设备管理凭据。
3. 单击“**此设备**”，然后单击“**监视**”下面的“**配置 OMS 设置**”。
4. 选择“**启用监视**”。
5. 在“OMS 设置”对话框中，键入“**工作区 ID**”和“**工作区密钥**”。  
   ![设置](./media/log-analytics-surface-hubs/settings.png)
6. 单击“**确定**”以完成配置。

将显示一条确认信息，告诉你是否已将 OMS 配置成功应用于设备。 如果是，则会出现一条消息，注明代理已成功连接到 OMS 服务。 然后，设备开始将数据发送到 OMS，你可以查看数据并采取相应操作。

## <a name="monitor-surface-hubs"></a>监视 Surface Hub
使用 OMS 监视 Surface Hub 非常类似于监视任何其他已注册的设备。

1. 登录到 OMS 门户。
2. 导航到 Surface Hub 解决方案包仪表板。
3. 此时将显示设备的运行状况。

   ![Surface Hub 仪表板](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

你可以根据现有或自定义的日志搜索创建[警报](log-analytics-alerts.md)。 通过使用 OMS 从 Surface Hub 收集的数据，你可以按照为设备定义的条件搜索问题和警报。

## <a name="next-steps"></a>后续步骤
* 使用 [Log Analytics 中的日志搜索](log-analytics-log-searches.md)，查看详细的 Surface Hub 数据。
* 创建[警报](log-analytics-alerts.md)，以便在 Surface Hub 出现问题时通知你。

