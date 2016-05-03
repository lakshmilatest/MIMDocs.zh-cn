---
# required metadata

title: Azure 中的标识管理器混合报告 | Microsoft 标识管理器
description: Azure Active Directory 混合报告使你可以创建包括云和本地事件的自定义报告。
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Azure 中的身份管理器混合报告
如果你有 Azure 订阅，现在可以轻松地针对本地和云中的事件创建报告。 然后，可以在 Azure 门户中查看报告。 更好的做法是将这些报告与 Azure Active Directory 活动结合使用。 通过身份管理器混合报告，Azure AD 管理门户可以显示云和本地活动的身份管理活动报告。 该报告功能提供以下内容：

-   统一的体验：适用于 IAM 活动以及云和本地的统一的报告

-   降低成本：无需本地报告数据仓库基础结构

-   数据自有：可以轻松地从本地标识管理器或从 Azure AD 导出报告数据，并可以使用它来生成自定义视图报告

## 什么是 Azure AD 混合报告？
通过混合报告，Azure AD 管理门户可以显示统一的身份管理活动报告。 这不涉及开展活动的地点、身份管理器或 Azure AD。 例如，如果你想了解上个月谁注册了自助服务密码重置 (SSPR)，你可以在 Azure AD 管理门户中看到这些内容。 在此报告中，你将看到通过[应用程序访问面板](https://myapps.microsoft.com)和标识管理器注册了 SSPR 的用户。

![Azure 密码重置活动图像](media/MIM-Hybrid-passwordreset.jpg)

## 我为什么要使用它？
混合报告可帮助 IT 专业人员解决一些常见身份管理报告质询。

1.  报告在不同系统中所执行的身份管理活动：现在，你可以查看来自 Azure AD 上的活动以及 Azure AD 管理门户中的身份管理器的身份管理报告。

2.  导出报告数据并创建自定义报告：除了 Azure AD 中的报告，使用这项新功能，我们已添加反映身份管理器活动的 Windows 事件。 这使得比之前更易于集成到 SIEM 系统、查看身份管理器活动和创建自定义报告。

3.  将报告系统基础结构的成本降至最低：部署这项新功能将需要几分钟的时间。 你所要做的仅是在身份管理器服务器上安装报告代理。

从 Azure AD 管理门户下载报告代理，在目录配置屏幕中：

![MIM 报告代理下载图像](media/MIM-Hybrid-downloadReportAgent.jpg)

## BitLocker 是如何工作的？
安装了报告代理后，会将身份管理器的活动数据发送到 Windows 事件日志。 报告代理处理事件，并将它们上载到 Azure。 在 Azure 中，一般存储一个月的活动数据。 在检索报告时，将分析活动事件，并筛选出所需的报告。 最后，Azure 管理门户检索报告数据，并将此呈现为活动报告。

![混合报告图表](media/MIM-Hybrid-howitworks.png)


<!--HONumber=Apr16_HO2-->


