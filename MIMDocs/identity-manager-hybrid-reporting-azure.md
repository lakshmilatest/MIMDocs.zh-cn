---
title: Azure 中的混合报告是什么？ | Microsoft Docs
description: 使用 Azure Active Directory 中的混合审核活动报告可以在云中和本地查看已审核的事件。
keywords: ''
author: davidste
ms.author: davidste
manager: bhu
ms.date: 02/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: eb9725df484fb5ac2ee44bd9a0423bdb4fbe7e86
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2018
ms.locfileid: "29370372"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Azure Active Directory 中的混合身份管理审核报告
借助 Azure Active Directory (Azure AD) 审核活动报告，你可以在本地或云中监视身份管理活动。 通过在单个报告中管理所有身份和访问数据，可以节省时间并降低总体成本。

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>什么是 Azure Active Directory 混合报告？
混合审核报告可帮助 IT 专业人员解决常见的身份管理报告质询，例如：

* **收集不同系统中的身份管理活动**。 混合报告为你显示 Azure AD 和 Identity Manager 中的标识管理活动。

* **导出报告数据并创建自定义报告**。 除了可在 Azure 门户中查看报告，还可导出数据以自行生成自定义视图。

* **降低报告系统基础结构成本**。 云中的混合报告意味着有助于你削减与本地数据仓库基础结构相关联的成本。

## <a name="how-does-it-work"></a>BitLocker 是如何工作的？

若要收集本地数据，首先要在 Identity Manager 2016 服务器上安装报告代理。 [下载 Microsoft Identity Manager 混合报告代理](https://www.microsoft.com/download/details.aspx?id=55112)。

混合报告会历经以下过程：
1. 安装报告代理后，Identity Manager 活动数据将被发送到 Windows 事件日志。
2. 报告代理每 10 分钟或在 Windows 事件日志服务重启时处理增量事件。 代理随后会将事件上传到 Azure 门户。
3. Azure 门户在接收到数据 1 小时内处理收到的数据。
4. 活动数据在 Azure 中将存储 1 个月。
5. Azure 门户会检索审核报告数据，并将其显示在 Azure 审核报告窗口中。

## <a name="next-steps"></a>后续步骤
了解详细信息：
- [使用 Identity Manager 混合报告](working-with-identity-manager-hybrid-reporting.md)
- [Azure Active Directory 门户中的审核活动报告](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [报告保留策略](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure 日志集成 (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Azure Active Directory 报告 API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
