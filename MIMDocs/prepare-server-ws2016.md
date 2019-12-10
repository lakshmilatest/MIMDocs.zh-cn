---
title: 为 MIM 2016 SP2 配置 Windows Server 2016 或 Windows Server 2019 | Microsoft Docs
description: 获取准备 Windows Server 2016 或 Windows Server 2019 以使用 MIM 2016 SP2 的步骤和最低要求。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c6d5d5081f0e932b9c60d8f2025b54e47dc352d5
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "73568039"
---
# <a name="set-up-an-identity-management-server-windows-server-2016-or-2019"></a>设置身份管理服务器：Windows Server 2016 或 Windows Server 2019

> [!div class="step-by-step"]
> [« 准备域](preparing-domain.md)
> [SQL Server »](prepare-server-sql2016.md)
> 

> [!NOTE]
> Windows Server 2019 设置过程与 Windows Server 2016 设置过程没有区别。


> [!NOTE]
> 本演练使用名为 Contoso 的公司中的示例名和值。 将其替换为你自己的。 例如：
> - 域控制器名称 - corpdc 
> - 域名 - **contoso**
> - MIM 服务服务器名称 - corpservice 
> - MIM 同步服务器名称 - corpsync 
> - SQL Server 名称 - corpsql 
> - 密码 - <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>将 Windows Server 2016 加入到域中

开始使用 Windows Server 2016 计算机（具有最少 8-12 GB 的 RAM）。 安装时，请指定“Windows Server 2016 标准/数据中心（带有 GUI 的服务器）x64”版本。

1. 以其管理员身份登录到新计算机。

2. 使用控制面板，为计算机提供一个网络上的静态 IP 地址。 配置该网络接口以便将 DNS 查询发送到上一步骤中的域控制器的 IP 地址，并将计算机名称设置为“CORPSERVICE”  。  此操作需要重启服务器。

3. 打开控制面板，然后将计算机加入到上一步配置的域 contoso.com  中。  此操作包括提供域管理员的用户名和凭据（例如 Contoso\Administrator  ）。  显示欢迎使用消息后，关闭该对话框并再次重新启动此服务器。

4. 使用本地计算机管理员的域帐户（如 Contoso\MIMINSTALL  ）登录到计算机 CORPSERVICE  。


5. 以管理员身份启动 PowerShell 窗口，并键入以下命令以使用组策略设置更新计算机。

    ```
    gpupdate /force /target:computer
    ```

    之后不会超过一分钟，它将完成并显示消息“计算机策略更新已成功完成”。

6. 添加 **Web 服务器 (IIS)** 和 **应用程序服务器** 角色、 **.NET Framework** 3.5、4.0 和 4.5 功能，以及 **Windows PowerShell 的 Active Directory 模块**。

    ![PowerShell 功能映像](media/MIM-DeployWS2.png)

7. 在 PowerShell 中，键入以下命令。 请注意，可能需要为 **.NET Framework** 3.5 功能的源文件指定其他位置。 这些功能通常在安装 Windows Server 时并不存在，但在操作系统安装磁盘源文件夹上的并行 (SxS) 文件夹中可见，例如，“*d:\Sources\SxS\*”。

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>配置服务器安全策略

将服务器安全策略设置为允许新创建的帐户以服务身份运行。
> [!NOTE] 
> 根据配置，只需根据像同步服务器这样的成员计算机角色来添加单一服务器（一体化）或分布式服务器。 

1. 启动本地安全策略程序

2. 导航到“本地策略”>“用户权限分配”  。

3. 在详细信息窗格中，右键单击“作为服务登录”  ，然后选择“属性”  。

    ![本地安全策略映像](media/MIM-DeployWS3.png)

4. 单击“添加用户或组”  ，然后在文本框中键入以下基于角色 `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR` 的内容，单击“检查名称”  ，然后单击“确定”  。

5. 单击“确定”  以关闭“作为服务登录”属性  窗口。

6.  在“详细信息”窗格中，右键单击“拒绝从网络访问该计算机”  ，并选择“属性”  。>

7. 单击“添加用户或组”  ，在文本框中键入 `contoso\MIMSync; contoso\MIMService`，然后单击“确定”  。

8. 单击“确定”  以关闭“拒绝从网络属性访问此计算机”属性  窗口。

9. 在详细信息窗格中，右键单击“拒绝本地登录”  ，然后选择“属性”  。

10. 单击“添加用户或组”  ，在文本框中键入 `contoso\MIMSync; contoso\MIMService`，然后单击“确定”  。

11. 单击“确定”  以关闭“拒绝本地登录”属性  窗口。

12. 关闭本地安全策略窗口。

## <a name="software-prerequisites"></a>软件必备项

请先确保已安装所有软件必备项，然后再安装 MIM 2016 SP2 组件：

13. 安装 [Visual C++ 2013 Redistributable Packages](https://www.microsoft.com/download/details.aspx?id=40784)。

14. 安装 .NET Framework 4.6。

15. 在托管 MIM 同步服务的服务器上，MIM 同步服务需要 [SQL Server Native Client](https://www.microsoft.com/download/details.aspx?id=50402)。

16. 在托管 MIM 服务的服务器上，MIM 服务需要 .NET Framework 3.5。

17. （可选）若使用的是 TLS 1.2 或 FIPS 模式，请参阅[“仅 TLS 1.2 ”或 FIPS 模式环境中的 MIM 2016 SP2](preparing-tls.md)。

## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>更改 IIS Windows 身份验证模式（如果需要）

1.  打开 PowerShell 窗口。

2.  使用命令 *iisreset /STOP* 停止 IIS

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [« 准备域](preparing-domain.md)
> [SQL Server »](prepare-server-sql2016.md)
