---
title: Microsoft Identity Manager 2016 的必需部署步骤 | Microsoft Docs
description: 获取部署 Microsoft 标识管理器 2016 的完整步骤列表，包括从准备环境到配置门户的全部步骤。
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3460436682054acf5e9e1b186c3fa39faaa40a43
ms.sourcegitcommit: 8316fa41f06f137dba0739a8700910116b5575d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/04/2018
---
# <a name="deploy-microsoft-identity-manager-2016-sp1"></a>部署 Microsoft Identity Manager 2016 SP1
本部分中的文章提供部署 Microsoft 标识管理器 (MIM) 2016 的分步说明，这些说明适用于此前未部署过 FIM 或 MIM 的新服务器上的最终用户自助服务方案。

> [!NOTE]
> 本部分中所述部署拓扑仅用于开始使用和了解 MIM。  [容量计划指南](capacity-planning-guide.md)提供有关生产部署拓扑的详细信息。  我们建议应在查看该文档后，再部署 MIM 生产规模或使用。

部署特许访问权限管理方案与其他 MIM 方案不同，因为其需要专用堡垒林环境。  若要了解有关针对 Privileged Identity Management 部署 MIM 的详细信息，请参阅[为 Privileged Access Management 配置 MIM 环境](./pam/configuring-mim-environment-for-pam.md)。

部署 MIM 的过程与部署其前身 FIM 2010 R2 的过程非常类似。 如果想引用 FIM 文档，请参阅 [Forefront Identity Manager 2010 R2 部署指南](https://technet.microsoft.com/library/jj134310)。

## <a name="first-prepare-a-domain"></a>第一步：准备域
MIM 与 Active Directory (AD) 协同工作，因此请按照以下步骤来配置你的 AD 域控制器。
- [域设置](preparing-domain.md)

## <a name="next-prepare-an-identity-management-servers"></a>第二步：准备标识管理服务器
设置并配置好域后，准备你的企业标识管理服务器。 这包括设置：
- [Windows Server 2016](prepare-server-ws2016.md)
- [SQL Server 2016](prepare-server-sql2016.md)
- [SharePoint 2016](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md)（可选）

## <a name="finally-install-microsoft-identity-manager-2016-sp1-components"></a>第三步：安装 Microsoft 标识管理器 2016 SP1 组件
设置好域和服务器后，便可立即安装 MIM 组件，并将其配置为与 AD 同步。
- [MIM 同步服务](install-mim-sync.md)
- [MIM 服务和门户](install-mim-service-portal.md)
- [同步 Active Directory 和 MIM 服务数据库](install-mim-sync-ad-service.md)
- [MIM 2016 或更高版本支持的平台](microsoft-identity-manager-2016-supported-platforms.md)
