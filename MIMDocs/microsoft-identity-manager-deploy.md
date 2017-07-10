---
title: "Microsoft Identity Manager 2016 的必需部署步骤 | Microsoft Docs"
description: "获取部署 Microsoft 标识管理器 2016 的完整步骤列表，包括从准备环境到配置门户的全部步骤。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: fa200bb18871387420743af64ca196565397e5d5
ms.contentlocale: zh-cn
ms.lasthandoff: 07/10/2017


---

<a id="deploy-mim-2016" class="xliff"></a>
# 部署 MIM 2016
本部分中的文章提供部署 Microsoft 标识管理器 (MIM) 2016 的分步说明，这些说明适用于此前未部署过 FIM 或 MIM 的新服务器上的最终用户自助服务方案。

> [!NOTE]
> 本部分中所述部署拓扑仅用于开始使用和了解 MIM。  [容量计划指南](capacity-planning-guide.md)提供有关生产部署拓扑的详细信息。  我们建议应在查看该文档后，再部署 MIM 生产规模或使用。

部署特许访问权限管理方案与其他 MIM 方案不同，因为其需要专用堡垒林环境。  若要了解有关针对 Privileged Identity Management 部署 MIM 的详细信息，请参阅[为 Privileged Access Management 配置 MIM 环境](./pam/configuring-mim-environment-for-pam.md)。

部署 MIM 2016 的过程与部署其前身 FIM 2010 R2 的过程非常类似。 如果想引用 FIM 文档，请参阅 [Forefront Identity Manager 2010 R2 部署指南](https://technet.microsoft.com/library/jj134310)。

<a id="first-prepare-a-domain" class="xliff"></a>
## 第一步：准备域
MIM 与 Active Directory (AD) 协同工作，因此请按照以下步骤来配置你的 AD 域控制器。
- [域设置](preparing-domain.md)

<a id="next-prepare-an-identity-management-server" class="xliff"></a>
## 第二步：准备标识管理服务器
设置并配置好域后，准备你的企业标识管理服务器。 这包括设置：
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md)（可选）

<a id="finally-install-microsoft-identity-manager-2016-components" class="xliff"></a>
## 第三步：安装 Microsoft 标识管理器 2016 组件
设置好域和服务器后，便可立即安装 MIM 组件，并将其配置为与 AD 同步。
- [MIM 同步服务](install-mim-sync.md)
- [MIM 服务和门户](install-mim-service-portal.md)
- [同步 Active Directory 和 MIM 服务数据库](install-mim-sync-ad-service.md)

