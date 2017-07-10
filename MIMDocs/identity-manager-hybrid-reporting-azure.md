---
title: "什么是混合报告 | Microsoft Docs"
description: "使用 Azure Active Directory 中的混合审核活动报告可以查看云端和本地审核的活动。"
keywords: 
author: fimguy
ms.author: fimguy
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 678626e7c32659570de88d8178c16821cceaf7ee
ms.contentlocale: zh-cn
ms.lasthandoff: 07/10/2017


---

<a id="hybrid-identity-management-audit-reports-in-azure-active-directory---public-previewrefresh" class="xliff"></a>
# Azure Active Directory 中的混合身份管理审计报告 - 公开预览版（新版）
借助 Azure Active Directory (AD) 审核活动报告，你可以查看单个报表，以监视发生在本地或云中的身份管理活动。 此功能允许你在一个位置管理所有标识和访问数据，从而节省时间并降低总体成本。

<a id="what-is-azure-active-directory-hybrid-reporting" class="xliff"></a>
## 什么是 Azure Active Directory 混合报告？
混合审核报告可帮助 IT 专业人员解决常见的身份管理报告质询。

1. **收集不同系统中的标识管理活动。** 混合报告为你显示 Azure AD 和 Identity Manager 中的标识管理活动。

2. **导出报告数据并创建自定义报告。** 除了可在 Azure 门户中查看报表，还可导出数据以自行生成自定义视图。

3. **降低报告系统基础结构成本。** 借助云中的混合报告，无需使用本地报告数据仓库基础结构。

<a id="how-does-it-work" class="xliff"></a>
## BitLocker 是如何工作的？

若要收集本地数据，首先要在 Identity Manager 2016 服务器上安装报告代理。 可从[此处](https://www.microsoft.com/en-us/download/details.aspx?id=55112)的 Microsoft 下载页面下载报告代理。

混合报告的过程将按照以下步骤执行：
1. 安装报告代理后，Identity Manager 的活动数据将发送到 Windows 事件日志。
2. 报告代理每 10 分钟或在服务重启时处理“Windows 事件日志”中的增量事件，然后将它们上载到 Azure 门户中。
3. Azure 门户会在收到数据后的 1 小时内处理这些数据。
4. 活动数据在 Azure 中将存储 1 个月。
5. Azure 门户会检索审计报告数据，并将此呈现为 Azure 审计报告边栏选项卡中的审计数据。

<a id="see-also" class="xliff"></a>
## 另请参阅
- 获取有关[使用 Identity Manager 混合报告](working-with-identity-manager-hybrid-reporting.md)的更多详细信息
- 获取有关[Azure Active Directory 门户中的“审核活动”报表](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)的更多详细信息
