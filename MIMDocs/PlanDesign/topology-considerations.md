---
title: "用于部署的拓扑指南 | Microsoft Identity Manager"
description: "了解 MIM 2016 组件，并获取有关如何在你的环境中部署它们的建议。"
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: 8efb513bfe7dfb67d240a17b39535270f0c7fab6


---


# 拓扑注意事项
你可以在同一服务器上或在多个配置的多个服务器之间部署 Microsoft 标识管理器 (MIM) 组件。 为部署选择的拓扑会影响你所能实现的 MIM 性能。 本文介绍多种你可以考虑实施的部署拓扑。

## MIM 组件
在设计部署拓扑时，应了解每个组件的作用以及它们之间的交互方式，这一点很重要。

- **MIM 门户** - 用于密码重置、组管理和管理操作的接口。
    -
- **MIM 服务** - 实现 MIM 2016 标识管理功能的 Web 服务。
- **MIM 同步服务** - 同步数据与其他标识系统。
- **Microsoft SQL Server** - MIM 服务和 MIM 同步服务都将数据存储在 SQL 数据库中。

下表显示用于承载每个 MIM 组件的选项。 这些组件可承载于同一台计算机上，也可分布在多个服务器和群集中。

| | MIM 门户 | MIM 服务 | MIM 同步服务 | SQL Server |
| --- | --- | --- | --- | --- |
| 同一台计算机 | 是 | 是 | 是 | 是 |
| 单独服务器 | 是 | 是 | 是 | 是 |
| 网络负载平衡群集 | 是 | 是 | | |
| 服务器群集 | | | | 是 |


## 多层拓扑
多层拓扑是最常用的拓扑。 它提供最大程度的灵活性。 MIM 门户、MIM 服务和数据库分为不同层级并部署在多台计算机上。 此拓扑可增加扩展不同 MIM 组件时的灵活性。 例如，可以通过在网络负载平衡 (NLB) 群集中添加更多服务器来横向扩展 MIM 门户。 同样，也可以通过使用 NLB 群集和按需增加群集中的计算机（节点）数量来扩展 MIM 服务。

在多层拓扑中，分配有专用计算机来承载每个 SQL 数据库（一个用于 MIM 服务，另一个用于 MIM 同步服务）。 可通过添加或升级硬件来提高承载 SQL 数据库的计算机的性能的可扩展性。例如，通过升级 CPU、添加更多 CPU、增加随机存取内存 (RAM) 或升级 RAM，或升级硬盘驱动器配置来提高读取和写入访问速度并减少延迟。

![MIM 多层拓扑图](media/MIM-topo-multitier.png)

在这种配置中，MIM 同步服务及其数据库位于同一台计算机上。 然而，当它们位于不同的计算机上时，如果在 MIM 同步服务和它的数据库之间存在千兆位的专用网络连接，你也可获得相似的性能。


## 具有多个 MIM 服务的多层拓扑
与外部系统进行数据同步会花费很长时间，在此期间还将极大地增加系统负载。 如果同步配置导致触发工作流策略，这些策略会与最终用户工作流争用资源。 身份验证工作流（例如密码重置）可能会出现这类问题，这些工作流为实时进行并有最终用户等待进程的完成。 通过提供用于最终用户操作的 MIM 服务的一个实例和用于管理数据同步的单独门户，你可以提供更好的最终用户操作响应性。

![多 MIM 多层拓扑图](media/MIM-topo-multitier-multiservice.png)

使用标准多层拓扑，可通过使用 NLB 群集和按需增加群集中的节点数来提高 MIM 门户性能。

如果计算机运行承载了 MIM 同步服务和 MIM 服务数据库的 SQL Server，则其将极大影响 MIM 部署的整体性能。 因此，请遵循 SQL Server 文档中有关优化数据库性能的建议。 有关详细信息，请参阅以下文档：

## 另请参阅
- [Forefront 标识管理器 (FIM) 2010 容量规划指南](http://go.microsoft.com/fwlink/?LinkId=200180)（可下载）详尽介绍了有关测试版本和性能测试结果的相关信息。



<!--HONumber=Jul16_HO3-->


