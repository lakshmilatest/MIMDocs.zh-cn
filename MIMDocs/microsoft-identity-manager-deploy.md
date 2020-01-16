---
title: Microsoft Identity Manager 2016 的必需部署步骤 | Microsoft Docs
description: 获取部署 Microsoft 标识管理器 2016 的完整步骤列表，包括从准备环境到配置门户的全部步骤。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/17/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: ea9caef07c2496c5d2e040f5d88764938231220b
ms.sourcegitcommit: 80cdfd782cc6e2a4c4698decd54342f0e1460f5f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75756169"
---
# <a name="deploy-microsoft-identity-manager-2016-sp2"></a>部署 Microsoft Identity Manager 2016 SP2
本部分中的文章提供部署 Microsoft 标识管理器 (MIM) 2016 的分步说明，这些说明适用于此前未部署过 FIM 或 MIM 的新服务器上的最终用户自助服务方案。

> [!NOTE]
> 本部分中所述部署拓扑仅用于开始使用和了解 MIM。  [容量计划指南](capacity-planning-guide.md)提供有关生产部署拓扑的详细信息。  我们建议应在查看该文档后，再部署 MIM 生产规模或使用。

部署特许访问权限管理方案与其他 MIM 方案不同，因为其需要专用堡垒林环境。  若要了解有关针对 Privileged Identity Management 部署 MIM 的详细信息，请参阅[为 Privileged Access Management 配置 MIM 环境](./pam/configuring-mim-environment-for-pam.md)。

部署 MIM 的过程与部署其前身 FIM 2010 R2 的过程非常类似。 如果想引用 FIM 文档，请参阅 [Forefront Identity Manager 2010 R2 部署指南](https://technet.microsoft.com/library/jj134310)。

## <a name="first-prepare-a-domain"></a>第 1 步：准备域
MIM 与 Active Directory (AD) 协同工作，因此请按照以下步骤来配置你的 AD 域控制器。
- [域设置](preparing-domain.md)或
- [组托管服务帐户的域设置](preparing-domain-gmsa.md)


## <a name="next-prepare-an-identity-management-servers"></a>第 2 步：准备标识管理服务器
设置并配置好域后，准备你的企业标识管理服务器。

有关支持的平台的详细信息，请参阅 [MIM 2016 或更高版本支持的平台](microsoft-identity-manager-2016-supported-platforms.md)

 这包括设置：
- [Windows Server](prepare-server-ws2016.md)
- [SQL Server](prepare-server-sql2016.md)
- [SharePoint Server](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md)（可选）

## <a name="finally-install-microsoft-identity-manager-2016-sp2-components"></a>第 3 步：安装 Microsoft 标识管理器 2016 SP2 组件
设置好域和服务器后，便可立即安装 MIM 组件，并将其配置为与 AD 同步。
- [MIM 同步服务](install-mim-sync.md)
- [MIM 服务和门户](install-mim-service-portal.md)
- [同步 Active Directory 和 MIM 服务数据库](install-mim-sync-ad-service.md)
