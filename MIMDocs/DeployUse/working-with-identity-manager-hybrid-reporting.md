---
title: "使用标识管理器混合报告 | Microsoft 标识管理器"
description: "了解如何将本地和云中的数据结合到 Azure 中的混合报告内，以及如何管理和查看这些报告。"
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f9b01ac2cee2b96f64a9fda917f4f4146ca2eeda
ms.openlocfilehash: e2d0bd6120628d4fd2a14718fc205cde976c7785


---

# 使用标识管理器混合报告

## 可用混合报告
Azure AD 中可用的前三个 Microsoft 标识管理器 (MIM) 报告是“密码重置活动”、“密码重置注册”和“自助服务组活动”。

-   当用户使用 SSPR 执行密码重置并提供用于身份验证的入口或 **方法** 时，密码重置活动将显示每个实例。

    ![Azure 混合报告 - 密码重置活动图像](media/MIM-Hybrid-passwordreset.jpg)

-   每当用户注册用于身份验证的 SPR 和 **方法** （例如移动电话号码或问题和回答）时，密码重置注册将显示。
    请注意，对于密码重置注册，不区分 SMS 入口和 MFA 入口，两者都视为“移动电话” 。

-   自助服务组活动显示某人为在组和组创建中添加或删除自己所作的每次尝试。

> [!NOTE]
> 报告当前显示最多一个月前的数据。
>
> 如果要卸载混合报告，请卸载 MIMreportingAgent.msi 代理。

## 必备条件

1.  安装 Microsoft 标识管理器 2016，包括 MIM 服务。

2.  请确保目录中有一个带有许可管理员的 Azure AD 高级租户。

3.  请确保你具有从 Microsoft 标识管理器服务器到 Azure 的传出 Internet 连接。

## 在 Azure AD 中安装 Microsoft 标识管理器报告。
安装报告代理后，Microsoft 标识管理器活动中的数据从 MIM 导出到 Windows 事件日志。 MIM 报告代理处理这些事件，并将其上传到 Azure。 在 Azure 中，针对所需的报告解析、解密并筛选这些事件。

1.  安装 Microsoft 标识管理器 2016。

2.  下载 Microsoft 标识管理器报告代理：

    1.  登录到 Azure AD 管理门户并单击 Active Directory 图标。

    2.  双击你担任全局管理员的目录，你将拥有一个 Azure AD 高级订阅。

    3.  单击“配置”  ，然后下载报告代理。

3.  安装 Microsoft 标识管理器报告代理：

    1.  在计算机上创建一个目录。

    2.  将文件 `MIMHybridReportingAgent.msi` 和 `tenant.cert` 提取到目录中。

    3.  运行代理安装程序。

    4.  请确保 MIM 报告代理服务正在运行

    5.  重新启动 MIM 服务。

4.  验证 Microsoft 标识管理器报告是否在 Azure 中工作。

    可以通过使用 Microsoft 标识管理器自助服务密码重置门户来重置用户的密码，从而创建报告数据。 请确保密码重置已成功完成，然后检查该数据是否在 Azure AD 管理门户中显示。

## 在 Azure 经典门户中查看混合报告

1.  为租户使用全局管理员帐户登录到 [Azure 经典门户](https://manage.windowsazure.com/)。

2.  单击“Active Directory”  图标。

3.  从订阅的可用目录列表中选择租户目录。

4.  依次单击“报告”  和“密码重置活动” 。

5.  请确保在源下拉菜单中选择“标识管理器”  。

> [!WARNING]
> Microsoft 标识管理器数据需要一些时间才能显示在 Azure AD 中。

## 停止创建混合报告
如果要停止将报告数据从 Microsoft 标识管理器上传到 Azure Active Directory，请卸载混合报告代理。 使用 Windows“添加或删除程序”工具，卸载 Microsoft 标识管理器混合报告。

## 用于混合报告的 Windows 事件
由 Microsoft Identity Manager 生成的事件记录在 Windows 事件日志中，并且在事件查看器中可见，位于以下位置：“应用程序和服务”日志 -&gt;“Identity Manager 请求日志”。 每个 MIM 请求以 JSON 结构导出为 Windows 事件日志中的事件。 这可以导出到你的 SIEM。

|事件类型|ID|事件详细信息|
|--------------|------|-----------------|
|信息|4121|包括所有请求数据的 MIM 事件数据。|
|信息|4137|MIM 事件 4121 扩展名，用于单个事件具有过多数据的情况。 本事件中的头采用以下格式： `"Request: <GUID> , message <xxx> out of <xxx>`|



<!--HONumber=Jun16_HO4-->


