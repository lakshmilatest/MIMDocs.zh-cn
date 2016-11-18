---
title: "配置 Windows Server | Microsoft Docs"
description: "获取准备 Windows Server 2012 RS 以使用 MIM 2016 的步骤和最低要求。"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 08/11/2016
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 06d2365808858dc09d04ab37eafa9d6e3212cbcf


---

# <a name="set-up-an-identity-management-server-windows-server-2012-r2"></a>设置标识管理服务器：Windows Server 2012 R2

>[!div class="step-by-step"]
[« 准备域](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

> [!NOTE]
> 本演练使用名为 Contoso 的公司中的示例名和值。 将其替换为你自己的。 例如：
> - 域控制器名称 - **mimservername**
> - 域名 - **contoso**
> - 密码 - **Pass@word1**

## <a name="join-windows-server-2012-r2-to-your-domain"></a>将 Windows Server 2012 R2 加入到你的域中

开始使用 Windows Server 2012 R2 计算机（具有最少 8GB 的 RAM）。 安装时，请指定“Windows Server 2012 R2 标准（带有 GUI 的服务器）x64”版本。

1. 以其管理员身份登录到新计算机。

2. 使用控制面板，为计算机提供一个网络上的静态 IP 地址。 配置该网络接口以便将 DNS 查询发送到上一步骤中的域控制器的 IP 地址，并将计算机名称设置为 **CORPIDM**。  这将需要重新启动服务器。

3. 打开控制面板，然后将计算机加入到上一步配置的域 *contoso.local* 中。  这包括提供域管理员的用户名和凭据（例如 *Contoso\Administrator*）。  显示欢迎使用消息后，关闭该对话框并再次重新启动此服务器。

4. 以域管理员的身份（例如 *Contoso\Administrator*）登录到计算机 *CorpIDM*。

5. 以管理员身份启动 PowerShell 窗口，并键入以下命令以使用组策略设置更新计算机。

    ```
    gpupdate /force /target:computer
    ```

    之后不会超过一分钟，它将完成并显示消息“计算机策略更新已成功完成”。

6. 添加 **Web 服务器 (IIS)** 和 **应用程序服务器** 角色、**.NET Framework** 3.5、4.0 和 4.5 功能，以及 **Windows PowerShell 的 Active Directory 模块**。

    ![PowerShell 功能映像](media/MIM-DeployWS2.png)

7. 在 PowerShell 中，键入以下命令。 请注意，可能需要为 **.NET Framework** 3.5 功能的源文件指定其他位置。 这些功能通常在安装 Windows Server 时并不存在，但在操作系统安装磁盘源文件夹上的并行 (SxS) 文件夹中可见，例如，“*d:\Sources\SxS\*”。

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>配置服务器安全策略

将服务器安全策略设置为允许新创建的帐户以服务身份运行。

1. 启动本地安全策略程序

2. 导航到“本地策略”>“用户权限分配”。

3. 在详细信息窗格中，右键单击“作为服务登录” ，然后选择“属性” 。

    ![本地安全策略映像](media/MIM-DeployWS3.png)

4. 单击“添加用户或组”，然后在文本框中键入 `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`，单击“检查名称”，然后单击“确定”。

5. 单击“确定”以关闭“作为服务登录”属性窗口。

6.  在详细信息窗格中，右键单击“拒绝从网络访问该计算机”，并选择“属性”。

7. 单击“添加用户或组”，在文本框中键入 `contoso\MIMSync; contoso\MIMService`，然后单击“确定”。

8. 单击“确定”以关闭“拒绝从网络属性访问此计算机”属性窗口。

9. 在详细信息窗格中，右键单击“拒绝本地登录”，然后选择“属性”。

10. 单击“添加用户或组”，在文本框中键入 `contoso\MIMSync; contoso\MIMService`，然后单击“确定”。

11. 单击“确定”以关闭“拒绝本地登录”属性窗口。

12. 关闭本地安全策略窗口。


## <a name="change-the-iis-windows-authentication-mode"></a>更改 IIS Windows 身份验证模式

1.  打开 PowerShell 窗口。

2.  使用命令 *iisreset /STOP* 停止 IIS

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>[!div class="step-by-step"]  
[« 准备域](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)



<!--HONumber=Nov16_HO2-->


