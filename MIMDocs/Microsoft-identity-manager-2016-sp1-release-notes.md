---
title: Microsoft Identity Manager 2016 Service Pack 1 | Microsoft Docs
description: "了解 MIM 2016 如何在云中和本地创建更安全、更方便的标识管理体验。"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 69d44af5eaef3665f3a55ea91f48d3658cd5e65c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# Microsoft Identity Manager 2016 Service Pack 1 新增功能
<a id="whats-new-for-microsoft-identity-manager-2016-service-pack-1" class="xliff"></a> #

作为用于维护和更新 Microsoft Identity Manager 的定期发布周期的一部分，我们非常高兴地宣布推出 [Microsoft Identity Manager (MIM) 2016 Service Pack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212)。 本文档概述了更新、增强功能、功能以及此版本中所包含的更改。

如果在 MIM SP1 的生产部署过程中遇到问题，请与 Microsoft 客户支持联系。

我们也希望倾听你的意见！ 如有任何与产品团队相关的反馈、意见或问题，请发送电子邮件到 [mim2016@microsoft.com.](mailto:mim2016@microsoft.com) 与我们联系



## Service Pack 更新
<a id="updates-in-this-service-pack" class="xliff"></a> #

### MIM
<a id="mim" class="xliff"></a>

- **最终用户自助服务的 MIM 门户跨浏览器兼容性：**在此 Service Pack 中，我们引入了对大多数主流浏览器的支持。 现在，用户可以访问并与 MIM 门户交互，以从 Microsoft Edge、Chrome 和 Safari 实现自助服务组和配置文件管理。

- **Exchange Online 的 MIM 服务支持：**MIM 服务长期以来一直支持发送和接收电子邮件来进行审批和通知。 在 SP1 MIM 之前，仅支持 Exchange Server 或 SMTP。 有了 Service Pack 1，MIM 服务可以使用 Office365 Exchange 联机帐户发送和接收请求以及电子邮件通知。

- **图像文件格式上载验证：**将图像文件格式上载到门户时，MIM 现在可进行验证。

### 特权访问管理 (PAM)
<a id="privileged-access-managementpam" class="xliff"></a>

- **PAM“PRIV”（堡垒）林支持 Windows Server 2016 功能级别：**MIM PAM 服务可能在域控制器在 Windows Server 2016 的 Active Directory 域服务林功能级别运行的环境中进行配置。 配置时，用户 Kerberos 票证角色激活的剩余时间将会受到限制。

    >[!Note]
    如果选择维护 CORP 域中的 Windows Server 2012 R2 林功能级别，建议在 CORP 域控制器上安装 [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) 和 [KB 2919355](https://support.microsoft.com/en-us/kb/2919355)。

- **到“PRIV”（堡垒）林独有组的特权帐户提升：**现在，管理员可以通知“PRIV”林独有的组和用户的 MIM 服务。 这样做允许这些组和用户包括在 PAM 角色中。  随后可对它们进行角色激活，并将成员关系分配到“PRIV”林中的组。

- **PAM 部署脚本：**PAM 部署脚本使管理员能够简化 PAM 环境的安装。

- **用于身份验证策略接收器配置的 PAM Cmdlet：** Service pack 1 引入新的 Cmdlet 来加强堡垒林的安全性。 这些 Cmdlet 会自动创建身份验证策略接收器，绑定到身份验证策略模板。

    >[!Note]
    这些 Cmdlet 会作为部署脚本的一部分自动运行。


## 平台支持
<a id="platform-support" class="xliff"></a>
更新后的平台支持信息可在名为 [MIM 2016 支持的平台](microsoft-identity-manager-2016-supported-platforms.md)文档中找到。  此 Service Pack 中支持的新平台包括 SQL Server 2016、SharePoint 2016

## 自 MIM 2016 公开发行以来该版本中修复的问题
<a id="issues-fixed-in-this-release-from-mim-2016-general-availability" class="xliff"></a>

### PAM
<a id="pam" class="xliff"></a>
- New-PAMGroup 未在 PRIV 林中为域本地组创建 MIM 对象
- New-PAMDomainConfiguration 将失败，并显示“netdom”错误消息
- PAM 监视服务记录 PRIV 林中的组的警告

## 如何升级到 Service Pack 1
<a id="how-to-upgrade-to-service-pack-1" class="xliff"></a>

升级到 Microsoft Identity Manager 2016 Service Pack 1 的客户应遵循以下有关适用于其部署的所有服务的指南。

>[!Note]
>运行 Forefront Identity Manager 2010 R2 SP1 或更早版本的客户必须先将环境升级到 2015 年 8 月发布的 Microsoft Identity Manager 2016，然后按照下面的步骤进行操作。

在开始之前

在升级 MIM 服务和门户之前，需要升级 MIM 同步引擎。
需要备份 MIMService 和 MIM 同步数据库。

  1. 卸载正在升级的 Microsoft Identity Manager 组件
  2. 卸载完成后，打开位于安装介质“FIMSplash.htm”上的初始页
  3. 选择要升级的 MIM 组件
  4. 按照提示继续进行安装
    * MIM 服务和门户的安装：选择 Exchange Online 作为邮件帐户时，在下一个屏幕上输入 Exchange Online 帐户的电子邮件地址和凭据。
