---
# required metadata

title: 部署 MIM 2016 | Microsoft 标识管理器
description: 获取部署 Microsoft 标识管理器 2016 的完整步骤列表，包括从准备环境到配置门户的全部步骤。
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# 部署 MIM 2016
本部分中的文章提供部署 Microsoft 标识管理器 (MIM) 2016 的分步说明，这些说明适用于此前未部署过 FIM 或 MIM 的新服务器上的最终用户自助服务方案。

> [!NOTE]
> 本部分中所述部署拓扑仅用于开始使用和了解 MIM。  [容量计划指南](/microsoft-identity-manager/plan-design/capacity-planning-guide)提供有关生产部署拓扑的详细信息。  我们建议应在查看该文档后，再部署 MIM 生产规模或使用。

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## 第一步：准备域
MIM 与 Active Directory (AD) 协同工作，因此请按照以下步骤来配置你的 AD 域控制器。
- [域设置](preparing-domain.md)

## 第二步：准备标识管理服务器
设置并配置好域后，准备你的企业标识管理服务器。 这包括设置：
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md)（可选）

## 第三步：安装 Microsoft 标识管理器 2016 组件
设置好域和服务器后，便可立即安装 MIM 组件，并将其配置为与 AD 同步。
- [MIM 同步服务](install-mim-sync.md)
- [MIM 服务和门户](install-mim-service-portal.md)
- [同步 Active Directory 和 MIM 服务数据库](install-mim-sync-ad-service.md)


<!--HONumber=Apr16_HO4-->


