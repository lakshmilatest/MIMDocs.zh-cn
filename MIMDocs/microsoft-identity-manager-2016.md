---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: MIM 包括 FIM 2010 的访问权限管理功能，有助于管理用户、凭据、策略及组织中的访问权限。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/02/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: bd483ecb0abc3e4bb4444c87715971a3fba9820b
ms.sourcegitcommit: 5405ed10fea6f50b711eca1153446c04d4faff7a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/25/2018
ms.locfileid: "36927181"
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

Microsoft Identity Manager (MIM) 2016 以 [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx) 的标识和访问管理功能为基础。 如同其前身，MIM 有助于管理组织中的用户、凭据、策略和访问。  此外，MIM 2016 增加了混合体验、特权访问管理功能，并支持新的平台。

除 [FIM](https://technet.microsoft.com/library/jj133868) 中包含的现有标识管理功能外， MIM 2016 还提供了新增功能和增强功能，如：

- 特权标识管理
- 证书管理中的新增功能
  - [证书管理 REST API 参考](./reference/certificate-management-rest-api-reference.md)
  - 对多林拓扑的支持
  - [适用于虚拟智能卡的 Windows 应用](working-with-mim-certificate-manager.md)
  - 更新的事件和故障排除功能 
- [自助式方案](working-with-self-service-password-reset.md)现在包括帐户解锁和用于密码重置的 Azure MFA（多重身份验证）入口。

## <a name="hybrid-experience"></a>混合体验

Microsoft Identity Manager 2016 与 [Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) 协同工作，以便用户能够控制整个环境。 Azure AD 中的混合报表在一个地方集中显示云数据和本地数据。 此外，[自助服务密码重置门户](working-with-self-service-password-reset.md)支持 Azure 多重身份验证 (MFA)。

## <a name="privileged-identity-management"></a>特权标识管理

借助 Privileged Identity Management，可基于任务暂时性地访问敏感资源，从而对管理访问权限进行控制和管理。 这意味着你可仅授予用户所需的足够权限，从而降低网络攻击者获取完全管理访问权限的几率。 此外，特权标识管理还可从现有 Active Directory 林提取和隔离管理帐户。

MIM 支持用于管理 Active Directory 的本地 Privileged Identity Management 解决方案。 请从[使用 Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md) 开始。

## <a name="related-topics"></a>相关主题

- Microsoft Identity Manager 与其前身 Forefront Identity Manager 仍然紧密相关。 如果仍然使用 FIM，或者想要引用其他文档，请参阅 [FIM 2010 R2 文档路线图](https://technet.microsoft.com/library/jj133885.aspx)。
- [部署 MIM 的拓扑注意事项](topology-considerations.md) 本文介绍了多种可以考虑实施的部署拓扑。
- [容量计划指南](capacity-planning-guide.md) 可以利用本指南以及测试环境，了解针对你的部署的适当范围。