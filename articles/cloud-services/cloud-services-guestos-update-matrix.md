<properties 
   pageTitle="了解最新的 Azure 来宾 OS 版本 | Windows Azure" 
   description="有关 Azure 云服务来宾 OS 的最新发行新闻以及 SDK 兼容性。" 
   services="cloud-services" 
   documentationCenter="na" 
   authors="yuemlu" 
   manager="markkie" 
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.date="08/14/2015"
   wacn.date=""/>

# Azure 来宾 OS 版本和 SDK 兼容性对照表
提供适用于云服务的最新 Azure 来宾 OS 版本的最新信息。此信息将帮助你在来宾 OS 停用之前规划升级路径。

> [AZURE.IMPORTANT]本页面适用于在来宾 OS 顶层运行的云服务 Web 角色和辅助角色，而不适用于 IaaS 虚拟机。如果你根据 [Azure 来宾 OS 更新设置][]中所述将角色配置为使用自动进行来宾 OS 更新，则不一定要阅读本页面。

<!-- -->
<br />

> [AZURE.TIP]订阅[来宾 OS 更新 RSS 源][rss]，以接收有关所有来宾 OS 更改的最新通知。大约每周会将该源上提及的更改集成到本页面中一次。


## 新闻更新

###### **2015 年 8 月 14 日**
8 月版来宾 OS 将从今天（2015 年 8 月 14 日）开始推出，预计于 2015 年 9 月 11 日正式发行。

###### **2015 年 8 月 7 日**
2015 年 8 月 7 日已发行来宾 OS 4.22、3.29、2.41 版。

###### **2015 年 7 月 14 日**
7 月版来宾 OS 将从今天（2015 年 7 月 14 日）开始推出，预计于 2015 年 8 月 14 日正式发行。

2015 年 7 月 9 日已发行来宾 OS 4.21、3.28、2.40 版。

###### **2015 年 6 月 15 日**
6 月版来宾 OS 将从今天（2015 年 6 月 15 日）开始推出，预计于 2015 年 7 月 9 日正式发行。

2015 年 6 月 12 日已发行来宾 OS 4.20、3.27、2.39 版。

###### **2015 年 5 月 20 日**
5 月版来宾 OS 将从今天（2015 年 5 月 20 日）开始推出，预计于 2015 年 6 月 12 日正式发行。

###### **2015 年 4 月 17 日**
今日已发行来宾 OS 4.19、3.26、2.38 版。

此版次包含 Windows HTTP Server 的**关键**修补程序 [MS15-034](https://technet.microsoft.com/zh-cn/library/security/MS15-034)。

来宾 OS 4.17、3.24、2.36 版将于 2015 年 5 月 17 日停用。

###### **2015 年 4 月 6 日**
2015 年 4 月 2 日已发行来宾 OS 4.18、3.25、2.37 版。

来宾 OS 4.16、3.23、2.35 版将于 2015 年 5 月 2 日停用。


###### **2015 年 3 月 11 日**
2015 年 3 月 9 日已发行来宾 OS 4.17、3.24 和 2.36 版。

来宾 OS 4.15、3.22 和 2.34 版的停用日期已设为 2015 年 4 月 9 日。


###### **2015 年 1 月 29 日**
来宾 OS 4.14、4.13、3.21、3.20、2.33、2.32（11 月发行）的停用日期已后延。已更新以下来宾 OS 版本对照表。


###### **2015 年 1 月 13 日，2015 年 1 月 15 日更新**
2015 年 1 月 14 日已发行 11 月版来宾 OS。


###### **2015 年 1 月 13 日**
1 月版来宾 OS 部署预计于 2015 年 1 月 19 日当天或之后开始，将更新自动更新上运行的云服务。1 月版来宾 OS 将于 2 月某个时间发行部署。1 月安全更新中已提到，将按默认停用 SSL v3.0。有更多相关信息时，本文将随之更新。

根据[以前的通告][ssl3 announcement]，PasS 来宾 OS 的 1 月安全更新将按默认根据 [Microsoft 安全顾问 3009008][] 的建议停用 SSL v3.0。此更改将随着其他每月安全更新一起发布。当运行此[修复脚本][ssl3-fixit]（主动停用 SSL v3.0 支持）而禁用 SSL v3.0 时，已启用自动更新的 PaaS 客户可以验证应用程序兼容性是否受到影响。

###### **2014 年 12 月 16 日2015 年 1 月 7 日更新**
12 月版来宾 OS 版本预计于 2015 年 1 月 9 日当天或之后推出。




## 来宾 OS 版本信息

本部分列出当前支持的来宾 OS 版本。来宾 OS 系列和版本具有发布日期、停用日期和到期日期。截至发布日期，可以在管理门户中手动选择来宾 OS 版本。来宾 OS 将在“停用”日期当日或之后从管理门户中删除。然后它处于“过渡中”状态，但受到有限的更新部署功能的支持。到期日期是计划彻底从 Azure 系统中删除某个版本或系列的日期。仍在过期的版本上运行的云服务将被停止、删除或强制升级到较新的版本，如 [Azure 来宾 OS 可支持性和停用策略][retirepolicy]中所述。

Microsoft 支持至少两个最新版本的受支持来宾 OS 系列。现有来宾 OS 版本的停用日期可能会改为更后的日期，以确保至少启用两个已发布版本进行部署。

> [AZURE.WARNING]来宾 OS 系列 1 的停用期从 2013 年 6 月 1 日开始，并计划在短时间内结束。请勿使用此来宾 OS 系列创建新的安装及升级旧的安装。有关详细信息，请参阅 [Azure 来宾 OS 系列 1 停用信息][fam1retire]

来宾 OS 包含的配置不同于默认安装的 Windows Server。有关详细信息，请参阅 [Azure 来宾 OS 与默认 Windows Server 之间的差别][server and gos]。

### 来宾 OS 系列、版本和发行说明
来宾 OS 系列基于发布的 Microsoft Windows Server 版本。来宾 OS 是运行 Azure 云服务的基本操作系统。每个来宾 OS 都具有系列、版本和版本号。

**来宾 OS 系列**与来宾 OS 所基于的 Windows Server 操作系统发行版相对应。例如，系列 3 基于 Windows Server 2012。

**来宾 OS 版本**是指系列 OS 映像加上在生成新的来宾 OS 版本时提供的相关 [Microsoft 安全响应中心 (MSRC)][msrc] 修补程序。并非提供了所有修补程序。版本号从 0 开始，并在每次添加新的一组更新时增加 1。仅在比较重要时，才会显示尾随零。即，2.10 版是与 2.1 版不同的版本，并且比它晚得多。

**来宾 OS 发行版**是指来宾 OS 版本的再发行版。如果 Microsoft 在测试期间发现需要更改的问题，就会出现再发行版。最新的发行版始终会取代任何以前的发行版（无论是否公开）。管理门户将只允许用户选取给定版本的最新发行版。通常，不会对运行在以前版本上的部署进行强制升级，具体取决于 Bug 的严重性。

在下面的示例中，2 是系列，12 是版本，而“rel2”是发行版本。

**来宾 OS 发行版本** - 2.12 rel2

**此发行版本的配置字符串** - WA-GUEST-OS-2.12\_201208-02

来宾 OS 的配置字符串嵌入了该相同信息，以及显示考虑为该发行版本包括哪些 MSRC 修补程序的日期。在此示例中，考虑包括 2012 年 8 月之前（含该日期）为 Windows Server 2008 R2 开发的 MSRC 修补程序。仅包括专门应用于该版本的 Windows Server 的修补程序。例如，如果某个 MSRC 修补程序应用于 Microsoft Office，则它将不包括在内，因为该产品不属于 Windows Server 基本映像。

## 发行版本

## 系列 4 发行版本
**Windows Server 2012 R2**

支持 .NET 4.0、4.5、4.5.1、4.5.2（注释 2）

| 来宾 OS 版本 | 配置字符串 | 发布日期 | 停用日期 | 到期日期 |
| ---------------- | -------------------------- | ---------------------- | ------------ | --- |
| 4\.23 | WA-GUEST-OS-4.23\_201508-01 | 预计于 2015 年 9 月 11 日 | 将在 4.25 发行时更新 | TBD |
| 4\.22 | WA-GUEST-OS-4.22\_201507-01 | 2015 年 8 月 7 日 | 将在 4.24 发行时更新 | TBD |
| 4\.21 | WA-GUEST-OS-4.21\_201506-01 | 2015 年 7 月 9 日 | 将在 4.23 发行时更新 | TBD |
| 4\.20 | WA-GUEST-OS-4.20\_201505-02 | 2015 年 6 月 12 日 | 2015 年 9 月 7 日 | TBD |
| 4\.19 | WA-GUEST-OS-4.19\_201504-01 | 2015 年 4 月 17 日 | 2015 年 8 月 9 日 | TBD |
| 4\.18 | WA-GUEST-OS-4.18\_201503-01 | 2015 年 4 月 2 日 | 2015 年 7 月 12 日 | TBD |
| 4\.17 | WA-GUEST-OS-4.17\_201502-01 | 2015 年 3 月 9 日 | 2015 年 5 月 17 日 | TBD |
| 4\.16 | WA-GUEST-OS-4.16\_201501-01 | 2015 年 1 月 29 日 | 2015 年 5 月 2 日 | TBD |
| 4\.15 | WA-GUEST-OS-4.15\_201412-01 | 2015 年 1 月 14 日 | 2015 年 4 月 9 日 | TBD |
| 4\.14 | WA-GUEST-OS-4.14\_201411-01 | 2014 年 11 月 11 日 | 2015 年 2 月 28 日 | TBD |
| 4\.13 | WA-GUEST-OS-4.13\_201410-01 | 2014 年 11 月 3 日 | 2015 年 2 月 14 日 | TBD |
| 4\.12（注释 1） | WA-GUEST-OS-4.12\_201409-02 | 2014 年 10 月 6 日 | 2014 年 10 月 12 日 | 2015 年 3 月 23 日 |
| 4\.11（注释 1） | WA-GUEST-OS-4.11\_201408-02 | 2014 年 8 月 25 日 | 2014 年 9 月 11 日 | 2015 年 3 月 23 日 |
| 4\.10 | WA-GUEST-OS-4.10\_201407-01 | 2014 年 7 月 18 日 | 2014 年 12 月 1 日 | 2015 年 3 月 23 日 |
| 4\.9 | WA-GUEST-OS-4.9\_201406-01 | 2014 年 6 月 16 日 | 2014 年 11 月 10 日 | 2015 年 3 月 23 日 |
| 4\.8 | WA-GUEST-OS-4.8\_201405-01 | 2014 年 6 月 1 日 | 2014 年 8 月 1 日 | 2015 年 3 月 23 日 |

## 系列 3 发行版本

**Windows Server 2012**

支持 .NET 4.0、4.5

| 来宾 OS 版本 | 配置字符串 | 发布日期 | 停用日期 | 到期日期 |
| ---------------- | -------------------------- | ---------------------- | ------------ | --- |
| 3\.30 | WA-GUEST-OS-3.30\_201508-01 | 预计于 2015 年 9 月 11 日 | 将在 3.32 发行时更新 | TBD |
| 3\.29 | WA-GUEST-OS-3.29\_201507-01 | 2015 年 8 月 7 日 | 将在 3.31 发行时更新 | TBD |
| 3\.28 | WA-GUEST-OS-3.28\_201506-01 | 2015 年 7 月 9 日 | 将在 3.30 发行时更新 | TBD |
| 3\.27 | WA-GUEST-OS-3.27\_201505-02 | 2015 年 6 月 12 日 | 2015 年 9 月 7 日 | TBD |
| 3\.26 | WA-GUEST-OS-3.26\_201504-01 | 2015 年 4 月 17 日 | 2015 年 8 月 9 日 | TBD |
| 3\.25 | WA-GUEST-OS-3.25\_201503-01 | 2015 年 4 月 2 日 | 2015 年 7 月 12 日 | TBD |
| 3\.24 | WA-GUEST-OS-3.24\_201502-01 | 2015 年 3 月 9 日 | 2015 年 5 月 17 日 | TBD |
| 3\.23 | WA-GUEST-OS-3.23\_201501-01 | 2015 年 1 月 29 日 | 2015 年 5 月 2 日 | TBD |
| 3\.22 | WA-GUEST-OS-3.22\_201412-01 | 2015 年 1 月 14 日 | 2015 年 4 月 9 日 | TBD |
| 3\.21 | WA-GUEST-OS-3.21\_201411-01 | 2014 年 11 月 11 日 | 2015 年 2 月 28 日 | TBD |
| 3\.20 | WA-GUEST-OS-3.20\_201410-01 | 2014 年 11 月 3 日 | 2015 年 2 月 14 日 | TBD |
| 3\.19（注释 1） | WA-GUEST-OS-3.19\_201409-02 | 2014 年 10 月 6 日 | 2014 年 10 月 12 日 | 2015 年 3 月 23 日 |
| 3\.18（注释 1） | WA-GUEST-OS-3.18\_201408-02 | 2014 年 8 月 25 日 | 2014 年 9 月 11 日 | 2015 年 3 月 23 日 |
| 3\.17 | WA-GUEST-OS-3.17\_201407-01 | 2014 年 7 月 18 日 | 2014 年 12 月 1 日 | 2015 年 3 月 23 日 |
| 3\.16 | WA-GUEST-OS-3.16\_201406-01 | 2014 年 6 月 16 日 | 2014 年 11 月 10 日 | 2015 年 3 月 23 日 |
| 3\.15 | WA-GUEST-OS-3.15\_201405-01 | 2014 年 6 月 1 日 | 2014 年 8 月 1 日 | 2015 年 3 月 23 日 |


## 系列 2 发行版本

**Windows Server 2008 R2 SP1**

支持 .NET 3.5、4.0

| 来宾 OS 版本 | 配置字符串 | 发布日期 | 停用日期 | 到期日期 |
| ---------------- | -------------------------- | ---------------------- | ------------ | --- |
| 2\.42 | WA-GUEST-OS-2.42\_201508-01 | 预计于 2015 年 9 月 11 日 | 将在 2.44 发行时更新 | TBD |
| 2\.41 | WA-GUEST-OS-2.41\_201507-01 | 2015 年 8 月 7 日 | 将在 2.43 发行时更新 | TBD |
| 2\.40 | WA-GUEST-OS-2.40\_201506-01 | 2015 年 7 月 9 日 | 将在 2.42 发行时更新 | TBD |
| 2\.39 | WA-GUEST-OS-2.39\_201505-02 | 2015 年 6 月 12 日 | 2015 年 9 月 7 日 | TBD |
| 2\.38 | WA-GUEST-OS-2.38\_201504-01 | 2015 年 4 月 17 日 | 2015 年 8 月 9 日 | TBD |
| 2\.37 | WA-GUEST-OS-2.37\_201503-01 | 2015 年 4 月 2 日 | 2015 年 7 月 12 日 | TBD |
| 2\.36 | WA-GUEST-OS-2.36\_201502-01 | 2015 年 3 月 9 日 | 2015 年 5 月 17 日 | TBD |
| 2\.35 | WA-GUEST-OS-2.35\_201501-01 | 2015 年 1 月 29 日 | 2015 年 5 月 2 日 | TBD |
| 2\.34 | WA-GUEST-OS-2.34\_201412-01 | 2015 年 1 月 14 日 | 2015 年 4 月 9 日 | TBD |
| 2\.33 | WA-GUEST-OS-2.33\_201411-01 | 2014 年 11 月 11 日 | 2015 年 2 月 28 日 | TBD |
| 2\.32 | WA-GUEST-OS-2.32\_201410-01 | 2014 年 11 月 3 日 | 2015 年 2 月 14 日 | TBD |
| 2\.31（注释 1） | WA-GUEST-OS-2.31\_201409-02 | 2014 年 10 月 6 日 | 2014 年 10 月 12 日 | 2015 年 3 月 23 日 |
| 2\.30（注释 1） | WA-GUEST-OS-2.30\_201408-02 | 2014 年 8 月 25 日 | 2014 年 9 月 11 日 | 2015 年 3 月 23 日 |
| 2\.29 | WA-GUEST-OS-2.29\_201407-01 | 2014 年 7 月 18 日 | 2014 年 12 月 1 日 | 2015 年 3 月 23 日 |
| 2\.28 | WA-GUEST-OS-2.28\_201406-01 | 2014 年 6 月 16 日 | 2014 年 11 月 10 日 | 2015 年 3 月 23 日 |
| 2\.27 | WA-GUEST-OS-2.27\_201405-01 | 2014 年 6 月 1 日 | 2014 年 8 月 1 日 | 2015 年 3 月 23 日 |




### 系列 1 发行版本
**系列 1**[已停用][fam1retire]。

#### 注释 1：
2014 年 8 月版和 9 月版仅部分推出，因为在发布过程中发现 [MSRC 修补程序][MS14-046]有问题。由于该问题，无法在系列 3 和 4 上手动安装 .NET 3.5.1。为了安全起见，已停用了该版本，因此，客户无法手动选择它。

#### 注释 2：
截至 2014 年 9 月 19 日，.NET 4.5.2 尚未在 Azure 来宾 OS 上经过专门测试。但是，该来宾 OS 实质上等同于 Windows Server。因此，适用于 Windows Server 产品的兼容性规则同样适用于等同的来宾 OS 系列。如果你在使用此策略时遇到意外情况，请与 [Azure 支持][azuresupport]联系。Microsoft 将提供商业上合理的措施来解决你的问题。[.NET 4.5.2 的手动安装包][net install pkg]。

## 来宾 OS 中包含的 MSRC 更新
[此处][patches]提供了每月来宾 OS 版本随附的修补程序列表。

## SDK 支持

此表显示了哪个来宾 OS 系列与哪些 Azure SDK 版本兼容。有关超出此表范围的详细信息，请参阅 [Azure SDK for .NET 和 API 的支持与停用信息][retire policy sdk]。此列表中的任何信息都将取代下面的信息。

> [AZURE.IMPORTANT]若要确保你的服务正常运行，必须将其部署到与用于开发该服务的 Azure SDK 版本兼容的来宾 OS 版本中。否则，所部署的服务可能会在云中出现未曾在开发环境中出现的错误。

| 来宾 OS 系列 | 支持的 SDK 版本 |
| --------------- | ---------------------- |
| 4 | 2\.1 和更高版本 |
| 3 | 1\.8 和更高版本 |
| 2 | 1\.3 和更高版本 |
| 1 | 1\.0 和更高版本 |


## 来宾 OS 系统更新过程
本页包含有关即将发布的来宾 OS 版本的信息。客户已表明想知道什么时候发行版本，因为如果未设为“自动”更新，他们的云服务角色将重新启动。来宾 OS 发行版本通常会在每月第二个星期二发布 MSRC 更新之后的至少 5 天发布。新版本包含针对每个来宾 OS 系列的所有相关 MSRC 修补程序。

Microsoft Azure 不断地发布更新。来宾 OS 只不过是此类更新的其中一种。一个发行版本会受到许多因素影响，不胜列举。此外，Azure 实际上在成千上万的计算机上运行。这意味着无法提供重新启动你角色的准确日期和时间。我们将使用最新信息更新[来宾 OS 更新 RSS 源][rss]，但会考虑到其时间和近似时间范围。我们意识到这对于客户构成问题，并正在致力于限制重新启动或为重新启动定时的计划。

在发布新的来宾 OS 发行版本时，可能需要一段时间才能在 Azure 中完全传播。在将服务更新为新的来宾 OS 时，将重新启动这些服务以满足更新域限制。设置为使用“自动”更新的服务将先获取发行版本。在更新后，将会在 Azure 管理门户中看到为你的服务列出新的来宾 OS 版本。在此期间，可能会发布再发行版本。某些版本可能需要较长的部署时间，可能不会在正式发行日期后的几个星期内执行自动升级重新启动。在来宾 OS 可用后，你可以从门户或配置文件中显式选择该版本。有关详细信息，请参阅[从管理门户更新 Azure 来宾 OS][update guest os portal] 和[通过修改服务配置文件来更新 Azure 来宾 OS][update guest os svc]。

有关重新启动和指针的大量宝贵信息以及有关来宾和主机操作系统更新的更多技术详情，请阅读 MSDN 博客文章[角色实例因操作系统升级而重新启动][restarts]。

如果你手动更新来宾 OS，请阅读[来宾 OS 停用策略][retirepolicy]。


## 来宾 OS 可支持性和停用策略
[此处][retirepolicy]解释了来宾 OS 可支持性和停用策略。
 
## 新闻存档

###### **2014 年 11 月 11 日**

11 月版（4.14、3.21 和 2.33）已于 11 月 11 日推出。之所以提前推出此更新，是因为它包含 MSRC 更新 [Microsoft 安全公告 MS14-066 - 关键][MS14-066]。在接下来的几天内，你的 Web 角色和辅助角色应该在自动更新时重新启动一次，然后接收此修补程序。

###### **2014 年 11 月 10 日**
根据客户的反馈，我们更新了 10 月版（4.13、3.20 和 2.32）的停用日期。停用日期始终在从发布日期算起的至少两个月后。

###### **2014 年 11 月 4 日**
10 月版（4.13、 3.20 和 2.32）已于 2014 年 11 月 4 日推出。它包含的 MSRC 修补程序会导致 8 月和 9 月版出现问题。为了避免此问题，10 月版预装了 .NET 3.5 和 3.5.1（但已停用）。使用尝试安装 .NET 3.5 或 3.5.1 的脚本可以有效地重新启用该组件，并为 .NET 安装返回“成功”，同时还能避免 MSRC 修补程序造成的完全安装问题。

**2014 年 10 月 20 日。2014 年 11 月 4 日更新** – 由于相同的 [MSRC 修补程序 MS14-046][MS14-046] 导致尝试在系列 3 或 4 上安装 .NET 3.5 或 3.5.1 的用户遇到失败，因此已部分推出 9 月版（4.12、3.19、2.31 和 1.39）。这两个系列都不正式支持 .NET 3.5.x，但 Microsoft 正在对行为更改做出响应，因为某些客户的安装不依赖于该组件，并且已取消宣布该项更改。以前的来宾 OS（6 月和 7 月）的停用日期将会相应地延期，以便最少有两个完全发布的来宾 OS 受支持且可用。2014 年 10 月版中提供了 .NET 安装问题的解决方案。

10 月版包含预装的 .NET 3.5 和 3.5.1（但已禁用），另外还包含前面列出的 MSRC 修补程序。使用尝试安装 .NET 3.5 或 3.5.1 的脚本可以有效地重新启用该组件，并为 .NET 安装返回“成功”，同时还能避免 MSRC 修补程序造成的安装问题。

由于后两个版本是部分推出的，因此使用自动更新的用户或者部署了新安装的用户可能会使用其中的任何一种来宾 OS。下表列出了哪些来宾 OS 版本允许在系列 3 和 4 上安装 .NET 3.5 或 3.5.1。目前，如果某个版本允许相应的安装，则表示该版本未安装 MSRC 修补程序 MS14-046。

| OS 版本。 | 可以安装 .NET 3.5 | 包含 MSRC 修补程序 [MS14-046][] |
| --- | --- | --- |
| 所有更高的来宾 OS 版本 | 是 | 是 |
| WA-GUEST-OS-4.13\_201410-01 | 是 | 是 |
| WA-GUEST-OS-4.12\_201409-02 | 否 | 是 |
| WA-GUEST-OS-4.12\_201409-01 | 否 | 是 |
| WA-GUEST-OS-4.11\_201408-02 | 是 | 否 |
| WA-GUEST-OS-4.11\_201408-01 | 否 | 是 |
| WA-GUEST-OS-4.10\_201407-01 | 是 | 否 |

**2014 年 10 月 20 日** – 由于 8 月版和 9 月版仅部分推出，因此应注意以下事项：

1. “Azure 来宾 OS 与默认 Windows Server 之间的差别”中概述的密码更改尚未部署到整个 Azure。未使用 8 月版或 9 月版的客户将在 10 月版中获得这些更改。 

2. 8 月版和 9 月版来宾 OS 已在管理门户中禁用。你不能手动选择这些版本。这是为了防止你在选择此来宾 OS 版本时出现问题。

3. 先前某些版本的停用日期已向前调整。这是为了确保门户持续可用，并且持续为每个系列中的至少两个已发布来宾 OS 版本提供支持。

## 版本存档

### 系列 4

| 来宾 OS 版本 | 配置字符串 | 发布日期 | 停用日期 | 到期日期 |
| ---------------- | -------------------------- | ------------ | ------------ | --- |
| 4\.7 | WA-GUEST-OS-4.7\_201404-01 | 2014 年 5 月 2 日 | 2014 年 7 月 2 日 | 2014 年 8 月 18 日 |
| 4\.6 | WA-GUEST-OS-4.6\_201403-01 | 2014 年 3 月 28 日 | 2014 年 6 月 9 日 | 2014 年 8 月 18 日 |
| 4\.5 | WA-GUEST-OS-4.5\_201402-01 | 2014 年 3 月 21 日 | 2014 年 5 月 21 日 | 2014 年 8 月 18 日 |
| 4\.4 | WA-GUEST-OS-4.4\_201401-01 | 2014 年 2 月 8 日 | 2014 年 4 月 8 日 | 2014 年 5 月 14 日 |
| 4\.3 | WA-GUEST-OS-4.3\_201312-01 | 2014 年 1 月 6 日 | 2014 年 3 月 6 日 | 2014 年 5 月 14 日 |
| 4\.2 | WA-GUEST-OS-4.2\_201311-01 | 2013 年 12 月 12 日 | 2014 年 2 月 12 日 | 2014 年 5 月 14 日 |
| 4\.1 | WA-GUEST-OS-4.1\_201310-01 | 2013 年 10 月 29 日 | 不适用 | 2014 年 5 月 14 日 |
| 4\.0 rel3 | WA-GUEST-OS-4.0\_201309-03 | 2013 年 10 月 9 日10 月 18 日公开。 | 不适用 | 2014 年 5 月 14 日 |
 


### 系列 3

| 来宾 OS 版本 | 配置字符串 | 发布日期 | 停用日期 | 到期日期 |
| ---------------- | -------------------------- | ------------ | ------------ | --- |
| 3\.14 | WA-GUEST-OS-3.14\_201404-01 | 2014 年 5 月 2 日 | 2014 年 7 月 2 日 | 2014 年 8 月 18 日 |
| 3\.13 | WA-GUEST-OS-3.13\_201403-01 | 2014 年 3 月 28 日 | 2014 年 6 月 9 日 | 2014 年 8 月 18 日 |
| 3\.12 | WA-GUEST-OS-3.12\_201402-01 | 2014 年 3 月 21 日 | 2014 年 5 月 21 日 | 2014 年 8 月 18 日 |
| 3\.11 | WA-GUEST-OS-3.11\_201401-01 | 2014 年 2 月 8 日 | 2014 年 4 月 8 日 | 2014 年 5 月 14 日 |
| 3\.10 | WA-GUEST-OS-3.10\_201312-01 | 2014 年 1 月 6 日 | 2014 年 3 月 6 日 | 2014 年 5 月 14 日 |
| 3\.9 | WA-GUEST-OS-3.9\_201311-01 | 2013 年 12 月 12 日 | 2014 年 2 月 12 日 | 2014 年 5 月 14 日 |
| 3\.8 | WA-GUEST-OS-3.8\_201310-01 | 2013 年 10 月 29 日 | 不适用 | 2014 年 5 月 14 日 |
| 3\.7 rel3 | WA-GUEST-OS-3.7\_201309-03 | 2013 年 10 月 9 日 | 不适用 | 2014 年 5 月 14 日 |
| 3\.7 rel1 | WA-GUEST-OS-3.7\_201309-01 | 2013 年 9 月 23 日 | 不适用 | 2014 年 5 月 14 日 |

### 系列 2

| 来宾 OS 版本 | 配置字符串 | 发布日期 | 停用日期 | 到期日期 |
| ---------------- | -------------------------- | ------------ | ------------ | --- |
| 2\.26 | WA-GUEST-OS-2.26\_201404-01 | 2014 年 5 月 2 日 | 2014 年 7 月 2 日 | 2014 年 8 月 18 日 |
| 2\.25 | WA-GUEST-OS-2.25\_201403-01 | 2014 年 3 月 28 日 | 2014 年 6 月 9 日 | 2014 年 8 月 18 日 |
| 2\.24 | WA-GUEST-OS-2.24\_201402-01 | 2014 年 3 月 21 日 | 2014 年 5 月 21 日 | 2014 年 8 月 18 日 |
| 2\.23 | WA-GUEST-OS-2.23\_201401-01 | 2014 年 2 月 8 日 | 2014 年 4 月 8 日 | 2014 年 5 月 14 日 |
| 2\.22 | WA-GUEST-OS-2.22\_201312-01 | 2014 年 1 月 6 日 | 2014 年 3 月 6 日 | 2014 年 5 月 14 日 |
| 2\.21 | WA-GUEST-OS-2.21\_201311-01 | 2013 年 12 月 12 日 | 2014 年 2 月 12 日 | 2014 年 5 月 14 日 |
| 2\.20 | WA-GUEST-OS-2.20\_201310-01 | 2013 年 10 月 29 日 | 不适用 | 2014 年 5 月 14 日 |
| 2\.19 rel3 | WA-GUEST-OS-2.19\_201309-03 | 2013 年 10 月 9 日 | 不适用 | 2014 年 5 月 14 日 |
| 2\.19 rel1 | WA-GUEST-OS-2.19\_201309-01 | 2013 年 9 月 23 日 | 不适用 | 2014 年 5 月 14 日 |


[Azure 来宾 OS 更新设置]: https://msdn.microsoft.com/zh-cn/library/azure/ff729420.aspx
[rss]: http://sxp.microsoft.com/feeds/3.0/msdntn/WindowsAzureOSUpdates
[ssl3 announcement]: http://azure.microsoft.com/blog/2014/12/09/azure-security-ssl-3-0-update/
[Microsoft 安全顾问 3009008]: https://technet.microsoft.com/zh-cn/library/security/3009008.aspx
[ssl3-fixit]: http://go.microsoft.com/?linkid=9863266
[MS14-066]: https://technet.microsoft.com/zh-cn/library/security/ms14-066.aspx
[MS14-046]: https://technet.microsoft.com/zh-cn/library/security/ms14-046.aspx
[retire policy sdk]: https://msdn.microsoft.com/zh-cn/library/dn479282.aspx
[server and gos]: https://msdn.microsoft.com/zh-cn/library/dn775043.aspx
[azuresupport]: http://azure.microsoft.com/support/options/
[net install pkg]: http://www.microsoft.com/download/details.aspx?id=42643
[msrc]: http://www.microsoft.com/security/msrc/default.aspx
[update guest os portal]: https://msdn.microsoft.com/zh-cn/library/gg433101.aspx
[update guest os svc]: https://msdn.microsoft.com/zh-cn/library/gg456324.aspx
[restarts]: http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx
[patches]: /documentation/articles/cloud-services-guestos-msrc-releases
[retirepolicy]: /documentation/articles/cloud-services-guestos-retirement-policy
[fam1retire]: /documentation/articles/cloud-services-guestos-family1-retirement
 

<!---HONumber=69-->