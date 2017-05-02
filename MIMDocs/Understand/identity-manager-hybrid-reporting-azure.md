---
title: "什么是混合报告 | Microsoft Docs"
description: "使用 Azure Active Directory 中的混合审核活动报告可以查看云端和本地审核的活动。"
keywords: 
author: kgremban
ms.author: fimguy
manager: femila
ms.date: 04/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3144ee195675df5dc120896cc801a7124ee12214
ms.openlocfilehash: 8ca0af93f2d72ccf2e314b20d13323b631eb02bc
ms.lasthandoff: 04/27/2017


---

# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory"></a>Azure Active Directory 中的混合身份管理审核报告
借助 Azure Active Directory (AD) 审核活动报告，你可以查看单个报表，以监视发生在本地或云中的身份管理活动。 此功能允许你在一个位置管理所有标识和访问数据，从而节省时间并降低总体成本。

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>什么是 Azure Active Directory 混合报告？
混合审核报告可帮助 IT 专业人员解决常见的身份管理报告质询。

1. **收集不同系统中的标识管理活动。** 混合报告为你显示 Azure AD 和 Identity Manager 中的标识管理活动。

2. **导出报告数据并创建自定义报告。** 除了可在 Azure 门户中查看报表，还可导出数据以自行生成自定义视图。

3. **降低报告系统基础结构成本。** 借助云中的混合报告，可消除本地报告数据仓库基础结构。

## <a name="how-does-it-work"></a>BitLocker 是如何工作的？

若要收集本地数据，首先要在 Identity Manager 2016 服务器上安装报告代理。 可从[此处](https://www.microsoft.com/en-us/download/details.aspx?id=####/)的 Microsoft 下载页面下载报告代理。

混合报告的过程将按照以下步骤执行：
1. 安装报告代理后，Identity Manager 的活动数据将发送到 Windows 事件日志。
2. 报告代理会处理 Windows 事件日志中的事件，并将其上传到 Azure 门户。
3. 活动数据在 Azure 中将存储 1 个月。
4. 在请求报告时，系统将分析活动事件，并筛选出所需的报告。
5. Azure 将检索审核报告数据并将此呈现为活动报告中的审核。

## <a name="see-also"></a>另请参阅
- 获取有关[使用 Identity Manager 混合报告](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)的更多详细信息
- 获取有关[Azure Active Directory 门户中的“审核活动”报表](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)的更多详细信息

