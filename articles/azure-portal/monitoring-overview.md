<properties
	pageTitle="Microsoft Azure 监视概述 | Azure"
	description="概述如何在 Azure 中进行监视和诊断，内容包括警报、webhook、自动缩放等。"
	authors="rboucher"
	manager=""
	editor=""
	services="monitoring-and-diagnostics"
	documentationCenter="monitoring-and-diagnostics"/>

<tags
	ms.service="monitoring-and-diagnostics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/06/2016"
	ms.author="robb"
	wacn.date="10/17/2016"/>  


# Azure 监视概述

本文从概念方面概述了如何监视 Azure 资源以及指向特定类型资源详细信息的指针。如需概要了解如何从非 Azure 角度监视应用程序，请参阅 [Monitoring and diagnostics guidance](/documentation/articles/best-practices-monitoring/)（监视和诊断指南）。

云应用程序很复杂，包含很多移动部件。监视可以为用户提供数据，确保应用程序始终处于健康运行状态。监视还有助于避免潜在问题，或者解决过去的问题。此外，还可以利用监视数据深入了解应用程序的情况。了解这些情况可以方便用户改进应用程序的性能或可维护性，或者实现某些本来需要手动干预的操作的自动化。

下图从概念方面介绍了 Azure 监视，包括可以收集的日志类型，以及可以对该数据执行的操作。

![非计算资源的监视和诊断逻辑模型](./media/monitoring-overview/monitoring-azure-resources-non-compute.png)  


图 1：非计算资源的监视和诊断概念模型

<br/>  


![计算资源的监视和诊断逻辑模型](./media/monitoring-overview/monitoring-azure-resources-compute.png)  


图 2：计算资源的监视和诊断概念模型


## 监视源
### 基础结构活动日志
可以搜索审核日志（有时称为操作日志或活动日志）中是否存在通过 Azure 基础结构查看的资源的相关信息。日志包含多种信息，例如创建或销毁资源的时间。

### 主 VM
**仅计算**

目前不能收集主 VM 的指标，此处将其列出只是出于完整性考虑。

某些计算资源（例如云服务、虚拟机和 Service Fabric）有一个与之交互的专用主 VM。主 VM 等同于 Hyper-V 虚拟机监控程序模型中的根 VM。

对于其他 Azure 服务，在用户的资源和特定主 VM 之间没有必然的 1:1 映射关系。


### 资源 - 指标和诊断日志
可收集的指标取决于资源类型。例如，虚拟机提供有关磁盘 IO 和 CPU 百分比的统计信息。但这些统计信息对于服务总线队列来说并不存在，后者提供的是队列大小和消息吞吐量之类的指标。

对于计算资源来说，用户可以获取有关来宾 OS 和诊断模块（例如 Azure 诊断）的指标。Azure 诊断有助于收集诊断数据，并可将收集的诊断数据路由到其他位置（包括 Azure 存储）。

### 应用程序 - 诊断日志、应用程序日志和指标
**仅计算**

在计算模型中，应用程序可以基于来宾 OS 运行。应用程序会发出自己的日志和指标集。

指标类型包括

- 性能计数器
- 应用程序日志
- Windows 事件日志
- .NET 事件源
- IIS Logs
- 基于清单的 ETW
- 故障转储
- 客户错误日志


## 用于监视数据

### 可视化
以图形和表格的形式将监视数据可视化可以更快地找出其中的趋势，其速度远非单纯查看数据可比。

可视化方法包括：

- 使用 Azure 门户
- 将数据路由到 Azure Application Insights
- 将数据路由到 Microsoft PowerBI
- 将数据路由到第三方可视化工具，可以使用实时传送视频流，也可以让工具从 Azure 存储中的存档读取。

### 存档
监视数据通常写入 Azure 存储并保存在那里，直至用户将其删除。

可以通过多种方式使用该数据：

- 写入以后，即可让 Azure 内外的其他工具读取和处理该数据。
- 可以将该数据下载到本地进行本地存档，也可以更改云中的保留策略，让数据保留更长的时间。
- 可以让数据在 Azure 存储中无限期保存。请注意，必须根据保存的数据量支付 Azure 存储费用。

### 查询
可以使用 Insights REST API、通用语言接口 (CLI) 命令、PowerShell cmdlet 或 .NET SDK 访问系统或 Azure 存储中的数据 示例包括：

-  获取所编写的自定义监视应用程序的数据
-  创建自定义查询，将该数据发送到第三方应用程序。

### 路由
可以将监视数据实时流式传输到其他位置。

示例包括：

- 发送到 Application Insights，以便使用其中的可视化工具。
- 发送到事件中心，以便将其路由到第三方工具进行实时分析。

### 自动化
可以使用监视数据触发事件甚至整个过程 示例包括：

- 使用数据根据应用程序负载自动缩放计算实例数。
- 当某个指标超出预定阈值时发送电子邮件。
- 调用 Web URL (webhook)，在 Azure 外部系统中执行操作。
- 在 Azure 自动化中启动 Runbook，执行各种任务



## 使用方法
一般情况下，可以使用下述方法之一操作数据的跟踪、路由和检索。并非所有方法都适用于所有操作或数据类型。

- [Azure 门户预览](https://portal.azure.cn)
- [PowerShell](/documentation/articles/insights-powershell-samples/)
- [跨平台命令行接口 (CLI)](/documentation/articles/insights-cli-samples/)
- [REST API](https://msdn.microsoft.com/zh-cn/library/dn931943.aspx)
- [.NET SDK](https://msdn.microsoft.com/zh-cn/library/dn802153.aspx)

## Azure 的监视产品/服务
Azure 的产品/服务可用于监视从裸机基础结构到应用程序遥测在内的各种服务。最佳监视策略综合使用了所有这三种方式，可以对服务的运行状况进行全面、细致的了解。

- [Azure Insights（平台）监视](http://aka.ms/azmondocs) – 提供了可视化、查询、路由、警报、自动缩放和自动化功能，可以对 Azure 基础结构（活动日志）和每个单独的 Azure 资源（诊断日志）的数据进行相关操作。本文是 Azure Insights 文档的一部分。
- [Application Insights](/documentation/services/application-insights/) – 针对用户服务的应用程序层的问题，提供各种检测和诊断方式，该服务基于 Azure 监视中的数据进行了良好的集成。
- [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) 的 [Log Analytics](/documentation/services/log-analytics/) 部分 – 提供全方位的 IT 管理解决方案，适用于本地基础结构和基于云的基础结构，包括从 Azure 资源进行日志搜索和分析。


## 后续步骤
详细了解以下内容

- [Azure Insights](http://aka.ms/azmondocs)：浏览左侧目录中的链接以及此链接中的视频。
- [Azure 诊断](/documentation/articles/azure-diagnostics/)：如果要尝试诊断云服务、虚拟机或 Service Fabric 应用程序中的问题。
- [Application Insights](/documentation/services/application-insights/)：如果要尝试诊断应用服务 Web 应用中的问题。
- [Azure 存储故障诊断](/documentation/articles/storage-e2e-troubleshooting/)：在使用存储 Blob、表或队列的情况下。
- [Log Analytics](/documentation/services/log-analytics/) 和 [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite)

<!---HONumber=Mooncake_1010_2016-->