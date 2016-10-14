---
title: "部署 PAM 步骤 3 - PAM 服务器 | Microsoft Identity Manager"
description: "准备 PAM 服务器，它将为你的 Privileged Access Management 部署托管 SQL 和 SharePoint。"
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 1a21399df9528f689b811400a660543853d88472


---

# 步骤 3 - 准备 PAM 服务器

>[!div class="step-by-step"]
[« 步骤 2](step-2-prepare-priv-domain-controller.md)
[步骤 4 »](step-4-install-mim-components-on-pam-server.md)

## 安装 Windows Server 2012 R2
在第三台虚拟机上，安装 Windows Server 2012 R2，尤其是 Windows Server 2012 R2 Standard（带有 GUI 的服务器）x64，以使该计算机为“PAMSRV”。 由于 SQL Server 和 SharePoint 2013 将安装在此计算机上，因此它需要至少 8 GB 的 RAM。

1. 选择“Windows Server 2012 R2 Standard（带有 GUI 的服务器）x64”。

    ![选择带有 GUI 的 Windows Server Standard - 屏幕截图](media/PAM_GS_Select_WS2012.png)

2. 查看并接受许可条款。

3.  因为磁盘将为空，所以请选择“自定义: 仅安装 Windows”  并使用 **未初始化的磁盘空间**。

4.  以其管理员身份登录到此新计算机。  使用“控制面板”，为其提供一个虚拟网络上的静态 IP 地址、配置该网络接口以将 DNS 查询发送到 PRIVDC 的 IP 地址并将计算机名称设置为“PAMSRV”。  这将需要重新启动服务器。

5.  如果虚拟网络不提供 Internet 连接，请将其他网络接口添加到提供 Internet 连接的计算机。  它是 SharePoint 安装所必需的，并且可在完成此步骤后禁用。

6.  重启服务器后，以管理员身份登录。 使用“控制面板”，配置计算机以检查更新，并安装所需的任何更新。  这可能需要重新启动服务器。

7.  重启服务器后以管理员身份登录，打开“控制面板”并将 PAMSRV 加入到 PRIV 域 (priv.contoso.local)。  此操作将需要提供 PRIV 域管理员 (PRIV\Administrator) 的用户名和凭据。 在欢迎消息显示后，请关闭对话框并重新启动此服务器。


### 添加 Web 服务器 (IIS) 和应用程序服务器角色
添加 Web 服务器 (IIS) 和应用程序服务器角色、.NET Framework 3.5 功能、适用于 Windows PowerShell 的 Active Directory 模块和 SharePoint 所需的其他功能

1.  以 PRIV 域管理员 (PRIV\Administrator) 身份登录并启动 PowerShell。

2.  键入下列命令。 请注意，可能需要为 .NET Framework 3.5 功能的源文件指定不同的位置。 这些功能通常在安装 Windows Server 时并不存在，但在操作系统安装磁盘源文件夹上的并行 (SxS) 文件夹中可见，例如，“*d:\Sources\SxS”。

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### 配置服务器安全策略
将服务器安全策略配置为允许新创建的帐户以服务身份运行。

1.  启动“本地安全策略”  程序。   
2.  导航到“本地策略” > “用户权限分配”。  
3.  在详细信息窗格中，右键单击“作为服务登录” ，然后选择“属性” 。  
4.  单击“添加用户或组”，并在用户和组名称中，键入“priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer”。 单击“检查名称”，然后单击“确定”。  

5.  单击“确定”以关闭“属性”窗口。  
6.  在详细信息窗格中，右键单击“拒绝从网络访问该计算机”，并选择“属性”。  
7.  单击“添加用户或组”，在用户和组名称中，键入“priv\mimmonitor; priv\MIMService; priv\mimcomponent”并单击“确定”。  
8.  单击“确定”以关闭“属性”窗口。  

9. 在详细信息窗格中，右键单击“拒绝本地登录”，然后选择“属性”。  
10. 单击“添加用户或组”，在用户和组名称中，键入“priv\mimmonitor; priv\MIMService; priv\mimcomponent”并单击“确定”。  
11. 单击“确定”以关闭“属性”窗口。  
12. 关闭本地安全策略窗口。  

13. 打开“控制面板”并切换至“用户帐户”。  
14. 单击“允许其他人访问这台计算机” 。  
15. 单击“添加”，在域 PRIV 中输入用户 MIMADMIN，然后在向导的下一屏幕中，单击“以管理员身份添加此用户”。  
16. 单击“添加”，在域 PRIV 中输入用户 SharePoint，然后在向导的下一屏幕中，单击“以管理员身份添加此用户”。  
17. 关闭“控制面板”。  

### 更改 IIS 配置
有两种方式可以更改 IIS 配置以允许应用程序使用 Windows 身份验证模式。 请确保以 MIMAdmin 身份登录，然后按照以下选项之一操作。

想要使用 PowerShell：
1.  右键单击“PowerShell”，然后选择“以管理员身份运行”。  
2.  使用这些命令停止 IIS 并解锁应用程序主机设置  
    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```  

想要使用文本编辑器，如“记事本”：   
1. 打开文件 **C:\Windows\System32\inetsrv\config\applicationHost.config**   
2. 向下滚动到该文件的第 82 行。 标记值 **overrideModeDefault** 应为**<section name="windowsAuthentication" overrideModeDefault="Deny" />**  
3. 将值 **overrideModeDefault** 更改为 Allow  
4. 保存该文件，然后使用 PowerShell 命令重启 IIS `iisreset /START`

## 安装 SQL Server
如果 SQL Server 尚未在堡垒环境中，请安装 SQL Server 2012（Service Pack 1 或更高版本）或 SQL Server 2014。 以下步骤假设 SQL 2014。

1. 请确保以 MIMAdmin 身份登录。
2. 右键单击“PowerShell”，然后选择“以管理员身份运行”。   
3. 导航到 SQL Server 安装程序所在的目录。  
4. 键入下列命令。  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## 安装 SharePoint Foundation 2013

使用 SharePoint Foundation 2013 SP1 安装程序，在 PAMSRV 上安装 SharePoint 的软件必备组件。

> [!NOTE]
> 此安装程序需要 Internet 连接才能下载必备组件。 安装所需组件后，服务器将重启。

1. 右键单击“PowerShell”，然后选择“以管理员身份运行”。  
2. 更改为 SharePoint 解压缩所在的目录。  
3. 键入命令 `.\prerequisiteinstaller.exe`。

SharePoint 必备组件安装完成后，请安装 SharePoint Foundation 2013 SP1。

1.  右键单击“PowerShell”，然后选择“以管理员身份运行”。  
2.  更改为 SharePoint 解压缩所在的目录。  
3.  键入命令 `.\setup.exe`。  
4.  选择 **完整的服务器** 类型。  
5.  安装完成后，选择运行该向导。  

### 配置 SharePoint
运行 SharePoint 产品配置向导以配置 SharePoint。

1.  在“连接到服务器场”选项卡上，将其更改为“创建新服务器场”。  
2.  指定 **PAMSRV** 作为配置数据库的数据库服务器，指定 **PRIV\SharePoint** 作为 SharePoint 使用的数据库访问帐户。  
3.  指定一个密码作为场安全密码（此演练后面将不会再使用它）。  
4.  现在，接受其余的 SharePoint 配置向导默认设置以创建单服务器场。    
5.  当配置向导完成配置任务 10（共 10 项）后，单击“完成”，将打开 Web 浏览器。  
6.  在 Internet Explorer 弹出窗口中，以域管理员 (PRIV\MIMAdmin) 身份进行验证以继续。  
7.  在 Web 应用中启动向导以配置 SharePoint 场。  
8.  选择使用现有托管帐户 (PRIV\SharePoint)，取消选择禁用任何可选服务，然后单击“下一步”。  
9. 出现“创建网站集”窗口后，单击“跳过”，然后单击“完成”。  

## 创建 SharePoint Foundation 2013 Web 应用程序
在向导完成后，请使用 PowerShell 创建 SharePoint Foundation 2013 Web 应用程序来承载 MIM 门户。 由于本演练是出于演示目的，所以将不会启用 SSL。

1.  右键单击“SharePoint 2013 命令行管理程序”，选择“以管理员身份运行”，然后运行以下 PowerShell 脚本：

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"            -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. 将显示一条警告消息，指示 Windows 经典身份验证方法正在使用中，最后的命令可能需要几分钟才能返回。  完成后，输出将提供新门户的 URL。

> [!NOTE]
> 保持 SharePoint 2013 命令行管理程序窗口处于打开状态，以供下一步使用。

## 创建 Sharepoint 站点集合
接下来，创建一个与该 Web 应用程序相关联的 SharePoint 站点集合来承载 MIM 门户。

1.  启动“SharePoint 2013 命令行管理程序”，如果尚未打开，运行以下 PowerShell 脚本：

    ```
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    确保 **CompatibilityLevel** 变量设置为 14。 如果它返回 15，则网站集不是针对 2010 体验版本创建的；删除该网站集并重新创建它。

2.  在 **SharePoint 2013 命令行管理程序**中运行以下 PowerShell 命令： 这将禁用 SharePoint 服务器端视图状态和 SharePoint 任务**运行状况分析作业(每小时、Microsoft SharePoint Foundation 计时器、所有服务器)**。

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

## 更改更新设置

1. 打开“控制面板”，导航到“Windows 更新”，然后单击“更改设置”。  
2. 更改设置以便从 Window 更新接收更新和从 Microsoft 更新接收其他产品的更新。  
3. 检查新的更新，确保安装所有挂起的重要更新后再继续。

## 将网站设置为本地 Intranet

1. 启动“Internet Explorer”并打开一个新的 Web 浏览器选项卡。
2. 导航到 http://pamsrv.priv.contoso.local:82/ 并以 PRIV\MIMAdmin 身份登录。  将显示名为“MIM 门户”的空 SharePoint 站点。  
3. 在“Internet Explorer”中，打开“Internet 选项”，更改为“安全”选项卡，选择“本地 Intranet”，然后添加 URL `http://pamsrv.priv.contoso.local:82/`。

如果登录失败，则可能需要更新之前在[步骤 2](step-2-prepare-priv-domain-controller.md) 中创建的 Kerberos SPN。

## 启动 SharePoint 管理服务

使用“服务”（位于管理工具中），启动 **SharePoint 管理**服务（如果尚未运行）。

在步骤 4 中，将在 PAM 服务器上开始安装 MIM 组件。

>[!div class="step-by-step"]
[« 步骤 2](step-2-prepare-priv-domain-controller.md)
[步骤 4 »](step-4-install-mim-components-on-pam-server.md)



<!--HONumber=Oct16_HO2-->


