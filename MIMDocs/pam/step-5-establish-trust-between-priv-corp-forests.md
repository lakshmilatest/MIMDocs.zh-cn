---
title: "步骤 5 - 在 PRIV 和 CORP 林之间建立信任关系 | Microsoft Identity Manager"
description: 
keywords: 
author: 
manager: femila
ms.date: 06/16/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: 06319438d93d8d92edc833e2d0bf6492dd5919a6
ms.openlocfilehash: 1260b4e1792bf43a7288866cf3afa41fabe79fe9


---

# 步骤 5 - 在 PRIV 和 CORP 林之间建立信任关系

>[!div class="step-by-step"][«步骤 4](step-4-install-mim-components-on-pam-server.md)
[步骤 6»](step-6-transition-group-to-pam.md)


对于每个 CORP 域（如 contoso.local），都需要在 PRIV 和 CONTOSO 域控制器之间建立信任关系。 以便 PRIV 域中的用户能够访问 CORP 域中的资源。

## 将每个域控制器连接到其对应项

建立信任关系前，根据其他域控制器/DNS 服务器的 IP 地址，每个域控制器必须为其对应项的 DNS 名称解析进行配置。

1.  如果将域控制器或安装有 MIM 软件的服务器部署为虚拟机，请确保没有任何其他 DNS 服务器为这些计算机提供域命名服务。
    - 如果虚拟机具有包括连接到公用网络的多个网络接口，则可能需要临时禁用这些连接或替代 Windows 网络接口设置。 请确保 DHCP 提供的 DNS 服务器地址没有被任何虚拟机使用过，这一点至关重要。

2.  验证每个现有的 CORP 域控制器是否能够按指定路线将名称发送到 PRIV 林。 在 PRIV 林外的每个域控制器（例如 CORPDC）上，启动 PowerShell 并键入以下命令：

    ```
    nslookup -qt=ns priv.contoso.local.
    ```
    检查输出结果是否指示为一个具有正确 IP 地址的 PRIV 域的名称服务器记录。

3.  如果域控制器无法按指定路线发送 PRIV 域，则使用 **DNS 管理器**（位于“启动” > “应用程序工具” > “DNS”下）为 PRIV 域配置 DNS 名称转发，以转发至 PRIVDC 的 IP 地址。 如果它是一个高级域（例如 contoso.local），则展开此域控制器及其域的节点，例如“CORPDC” > “正向查找区域” > “contoso.local”，确保名为 **priv** 的密钥显示为名称服务器 (NS) 类型。

    ![私钥的文件结构 - 屏幕截图](./media/PAM_GS_DNS_Manager.png)

## 在 PAMSRV 中建立信任关系

在 PAMSRV 中，与每个域（如 CORPDC）建立单向信任关系，以使 CORP 域控制器信任 PRIV 林。

1. 以 PRIV 域管理员 (PRIV\Administrator) 身份登录 PAMSRV。

2.  启动 PowerShell。

3.  为每个现有的林键入以下 PowerShell 命令。 出现提示时输入 CORP 域管理员 (CONTOSO\Administrator) 的凭据。

    ```
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  为现有林中的每个域键入以下 PowerShell 命令。 出现提示时输入 CORP 域管理员 (CONTOSO\Administrator) 的凭据。

    ```
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## 授予林对 Active Directory 的读取访问权限

对于现有的每个林，启用 PRIV 管理员和监视服务对 AD 的读取访问权限。

1.  登录到现有 CORP 林域控制器 (CORPDC)，作为此林的顶级域的域管理员 (Contoso\Administrator)。  
2.  启动“Active Directory 用户和计算机” 。  
3.  右键单击域“contoso.local”并选择“委托控件”。  
4.  在“选定的用户和组”选项卡上，单击“添加”。  
5.  在“选择用户、计算机或组”窗口上，单击“位置”并将位置更改为 priv.contoso.local。  在“对象名称”上，键入“Domain Admins”并单击“检查名称”。 当出现弹出窗口时，输入用户名 *priv\administrator* 及其密码。  
6.  在“Domain Admins”后添加“; MIMMonitor”。 为名称 **Domain Admins** 和 **MIMMonitor** 加上下划线后，单击“确定”，然后单击“下一步”。  
7.  在常见任务列表中，选择“读取所有用户信息”，然后依次单击“下一步”和“完成”。  
8.  关闭“Active Directory 用户和计算机”。

9.  打开 PowerShell 窗口。  
10.  使用 `netdom` 确保已启用 SID 历史记录并已禁用 SID 筛选功能。 键入：  
    ```
    netdom trust contoso.local /quarantine /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    输出应指示“为此信任关系启用 SID 历史记录”或“已经为此信任关系启用了 SID 历史记录”。

    输出还应指示“没有为此信任关系启用 SID 筛选功能”。 有关详细信息，请参阅 [Disable SID filter quarantining](http://technet.microsoft.com/library/cc772816.aspx)（禁用 SID 筛选隔离功能）。

## 启动监控和组件服务

1.  以 PRIV 域管理员 (PRIV\Administrator) 身份登录 PAMSRV。

2.  启动 PowerShell。

3.  键入以下 PowerShell 命令。

    ```
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

下一步将介绍向 PAM 移动组。

>[!div class="step-by-step"][«步骤 4](step-4-install-mim-components-on-pam-server.md)
[步骤 6»](step-6-transition-group-to-pam.md)



<!--HONumber=Jun16_HO3-->


