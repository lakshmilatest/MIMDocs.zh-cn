---
title: "从 Forefront 标识管理器 2010 R2 升级 | Microsoft 标识管理器"
description: "了解如何升级你的 FIM 2010 R2 组件，然后安装 MIM 2016 中的新增组件。"
keywords: 
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 7e61e201b277a2e8ec9fee785e9e34fca3b1cb29
ms.openlocfilehash: 24a7bf5bfb0a7450becd08be6743ed7ab1755559


---

# 从 Forefront 标识管理器 2010 R2 升级

如果你具有Forefront Identity Manager (FIM) 2010 R2 环境，且想要试用 Microsoft Identity Manager (MIM) 2016，请以本文作为指南。 在此升级过程中有 3 个阶段：

1.  在已加入你的 Active Directory (AD) 域的服务器上安装 MIM 同步服务 (Sync)。 这将替换同步的 FIM 2010 R2 实例。

2.  安装 MIM 服务和门户 此时，你还可以选择安装自助服务密码重置 (SSPR) 注册门户和服务门户。 排除 Privileged Access Management 功能集，随后其他将被安装。

3.  将 MIM 加载项和扩展部署到单独的客户端计算机。 这包括集成了 SSPR Windows 登录的客户端。


本指南假定你已设置了以下内容：
- 测试环境中部署的 FIM 2010 R2
- 在 Windows Server 2012、Windows Server 2012 R2 或 Windows Server 2008 R2 上运行的服务器
- 为 FIM 2010 R2 配置的本地和环境先决条件（SQL Server、Exchange Server、SharePoint Services 等）。


## 准备

1.  备份 FIM 服务数据库、FIM 同步数据库、FIM 同步和服务配置和软件。

2.  在已安装 FIM 2010 R2 组件的每台服务器上（例如 *CORPIDM*），以 Contoso\Administrator 身份登录。 在此部署示例中，将 FIM 2010 R2 升级到 **MIM**需要具有管理权限。

3.  下载或解压缩 MIM 软件。

## 升级同步服务

1.  以管理员身份登录到已部署 FIM 2010 R2 同步服务（“Sync”）的服务器。

2.  在开始此步骤之前，请确保备份你的数据库。

3.  打开“服务”  控制台，找到 **Forefront Identity Manager 同步服务**，并将其停止。

    ![服务控制台图像](media/MIM-UpgFIM1.PNG)

4.  运行“MIM 同步服务安装程序” 。 安装程序将检测现有的同步版本并建议升级。 单击“更新”  按钮以继续。

5.  如果接受许可条款，则单击“下一步”  以继续。

6.  输入同步使用的服务帐户的密码，并单击“下一步” 。

    ![配置 MIM 同步服务账户图像](media/MIM-UpgFIM3.png)

7.  验证该安全组名称是否正确，并单击“下一步” 。

    ![配置 MIM 同步安全组图像](media/MIM-UpgFIM4.png)

8.  对于入站 RPC 通信的防火墙规则，请勿更改复选框状态。

9. 安装程序已准备好将同步从 FIM 2010 R2 升级到 MIM。 单击“升级”  以开始升级过程。

10. 正在进行升级。 升级过程中，请勿退出安装程序或重新启动计算机。

    ![MIM 同步安装状态图像](media/MIM-UpgFIM7.png)

11. 升级期间，将显示有关同步数据库升级的警告。 建议在开始此操作之前备份数据库。

12. 升级成功完成时，单击“完成” 。

    ![MIM 同步安装成功图像](media/MIM-UpgSP1.png)

13. 请注意，“同步服务”  已重新启动。

## 升级服务和门户

1.  以管理员身份登录到已部署 FIM 2010 R2 服务和门户的服务器。

2.  打开“服务”  控制台，找到 **Forefront Identity Manager 服务**，并将其停止。

    ![服务控制台图像](media/MIM-UpgFIM9.PNG)

3.  运行 MIM 服务和门户安装程序。 单击“安装”按钮以继续。

    ![安装 MIM 服务和门户映像](media/MIM-UpgSP2.png)

4.  如果接受许可条款，则单击“下一步”  以继续。

5.  在“MIM 客户体验改善计划”屏幕上，单击“下一步”  以继续。

6.  选择想要安装的 MIM 功能和组件，完成后单击“下一步”。

    ![自定义安装图像](media/MIM-UpgSP4.png)

    1.  **MIM 服务：**至少在一台服务器上需要安装此功能，并且在同一台服务器或另一台服务器上需要安装 SQL Server 数据库服务器。

    2.  **MIM 门户：** 至少在一台服务器上需要安装此功能，并且需要安装 SharePoint 2013 Foundation。

    3.  **MIM 密码注册门户：**自助服务密码重置需要此功能。

    4.  **MIM 密码重置门户：** 密码重置需要此功能。

7.  提供用于 FIM 服务数据库的 SQL Server 的详细信息。 选择选项以重新使用现有的数据库并保留数据。 单击“下一步”继续。 

8. 如果选择了重复使用现有的数据库选项，将显示一条备份数据库的提示。

9. 输入邮件服务器的详细信息。 如果邮件服务器位于当前服务器上，请输入“localhost”作为邮件服务器位置。 单击“下一步”继续。 

    ![配置邮件服务器连接图像](media/MIM-UpgSP6.png)

10. 选择要使用的服务的证书，以便验证客户端。 你应使用先前 FIM 服务使用的本地证书存储中的现有证书。

    ![配置服务证书图像](media/MIM-UpgSP7.png)

    1.  如果选择本地证书存储选项，请单击“选择证书”按钮，然后从弹出窗口中的列表选择一个证书。 单击“确定”，然后单击“下一步”。

        ![选择证书弹出窗口图像](media/MIM-UpgSP8.PNG)

11. 配置 MIM 服务的服务帐户凭据。 请注意，服务帐户不能为同步服务所使用的相同服务帐户。 这应该与 FIM 服务使用的帐户相同。

    ![配置 MIM 服务帐户图像](media/MIM-UpgSP9.png)

12. 根据你上一步中配置的 MIM 服务部署，配置 MIM 同步服务器的详细信息。

    ![配置 MIM 服务和门户同步图像](media/MIM-UpgSP10.png)

13. 安装 MIM 门户时，提供 MIM 服务服务器的地址。 单击“下一步”。

14. 安装 MIM 门户时，提供 FIM 门户当前托管在其中的 SharePoint 站点集的 URL。 单击“下一步” 。

## 安装 MIM 密码注册门户

1. 如果正在安装 MIM 密码注册门户，提供针对密码注册门户请求的 URL。 单击“下一步” 。

2. 配置客户端和最终用户使用服务和门户的能力。

    1.  检查是否希望 **打开防火墙中的端口 5725 和 5726**。

    2.  检查是否希望**授予验证的用户访问 MIM 门户站点的权限**。

    3.  单击“下一步” 。

3. 为 MIM 密码注册提供访问详细信息和凭据。

    ![配置 MIM 密码注册门户图像](media/MIM-UpgSP15.png)

    1.  提供 MIM 密码注册的服务帐户名（包括域）和密码。

    2.  提供密码注册门户的主机的详细信息 – 名称和端口号（例如 8080）。

    3.  选中“打开防火墙中的端口”  选项。

    4.  单击“下一步” 。

4. 在下一个 MIM 密码注册配置屏幕中：

    1.  告诉 MIM 密码注册 MIM 服务的安装位置，通常是同一系统。

    2.  确定此门户是否可由外联网用户和局域网用户访问，还是仅限局域网用户访问，如先前针对 FIM 密码重置进行的配置。

## 安装 MIM 密码重置门户

1. 如果正在安装 MIM 密码重置门户，提供 MIM 密码重置的访问详细信息和凭据。

    ![配置 FIM 密码重置门户图像](media/MIM-UpgSP17.png)

    1.  提供 MIM 密码重置的服务帐户名（包括域）和密码。

    2.  提供密码重置门户的主机的详细信息 – 名称和端口号（例如 8088）。

    3.  选中“打开防火墙中的端口”  选项。

    4.  单击“下一步” 。

2. 在下一个 MIM 密码重置配置屏幕中：

    1.  告诉 MIM 密码重置 MIM 服务的安装位置。

    2.  指定此门户是否可以由外联网用户和局域网用户访问，还是仅限局域网用户访问。

## 完成安装和升级

1. 成功完成所有配置定义后，会显示安装页面。 单击“安装”  以开始安装和升级 MIM 服务和门户。

2. 正在进行 MIM 服务和门户的安装和升级。 安装期间，请勿取消安装程序或重新启动计算机。

3. 成功完成 MIM 服务和门户的安装（升级）后，将显示确认屏幕。 单击“完成”  以结束安装并退出安装程序。

4. 请注意， **Forefront Identity Manager 服务** 已重新启动。

注意：如果当前在用户的计算机上部署了 FIM 外接程序和扩展以实现 SSPR，那么请务必先将所有 FIM 外接程序和扩展升级到 MIM 2016，再配置新的 MFA 电话入口以用于密码重置。  由于 FIM 2010 和 FIM 2010 R2 外接程序和扩展不识别新的入口，它们会给出错误信息，用户便无法完成密码重置。



<!--HONumber=Jun16_HO4-->


