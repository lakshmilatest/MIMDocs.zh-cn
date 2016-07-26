---
title: "什么是混合报告 | Microsoft Identity Manager"
description: "Azure Active Directory 混合报告使你可以创建包括云和本地事件的自定义报告。"
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: a074f3361e6d5be497b1a3c25d56aaa7008b128e


---

# Azure 中的混合标识管理报告
借助 Azure Active Directory (AD)，你可以创建单个报表，以监视发生在本地或云中的身份管理活动。 此功能允许你在一个位置管理所有标识和访问数据，从而节省时间并降低总体成本。

## 什么是 Azure AD 混合报告？
混合报告可帮助 IT 专业人员解决常见的身份管理报告质询。

1. **收集不同系统中的身份管理活动。** 混合报告为你显示 Azure AD 和 Identity Manager 中的标识管理活动。

2. **导出报告数据并创建自定义报告。** 除了可在 Azure 门户中查看报表，还可导出数据以自行生成自定义视图。

3. **降低报告系统基础结构成本。** 借助云中的混合报告，可消除本地报告数据仓库基础结构。

## BitLocker 是如何工作的？

若要收集本地数据，首先要在 Identity Manager 服务器上安装报告代理。 报告代理可从 [Azure 经典门户](https://manage.windowsazure.com/)的目录配置页中直接下载。

混合报告的过程将按照以下步骤执行：
1. 安装报告代理后，Identity Manager 的活动数据将发送到 Windows 事件日志。
2. 报告代理会处理 Windows 事件日志中的事件，并将其上传到 Azure。
3. 活动数据在 Azure 中将存储 1 个月。
4. 在请求报告时，系统将分析活动事件，并筛选出所需的报告。
5. Azure 经典门户将检索报告数据，并将此呈现为活动报告。

## 另请参阅
- 获取有关[使用 Identity Manager 混合报告](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)的更多详细信息



<!--HONumber=Jul16_HO3-->


