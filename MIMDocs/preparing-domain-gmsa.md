---
title: 设置 Microsoft Identity Manager 2016 的 gMSA | Microsoft Docs
description: 在 Microsoft Identity Manager 2016 的域中设置组托管服务帐户
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 1/7/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: a74f4074d9a0cf8378fd4972b7f51f723bd2f1c6
ms.sourcegitcommit: 80cdfd782cc6e2a4c4698decd54342f0e1460f5f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75756252"
---
# <a name="configure-a-domain-for-group-managed-service-accounts-gmsa-scenario"></a>为组托管服务帐户 (gMSA) 方案配置域

> [!div class="step-by-step"]
> [Windows Server »](prepare-server-ws2016.md)

> [!IMPORTANT]
> 本文仅适用于 MIM 2016 SP2。

Microsoft Identity Manger (MIM) 适用于你的 Active Directory (AD) 域。 应已安装了 AD，并确保你的环境中有一个你能够管理的域的域控制器。  本文介绍如何在 MIM 使用的域中设置组托管服务帐户。

## <a name="overview"></a>概述

组托管服务帐户无需定期更改服务帐户密码。 在版本 MIM 2016 SP2 中，可使用以下 MIM 组件配置 gMSA 帐户，使其在安装过程中使用：

-   MIM 同步服务 (FIMSynchronizationService)
-   MIM 服务 (FIMService)
-   MIM 密码注册网站应用程序池
-   MIM 密码重置网站应用程序池
-   PAM REST API 网站应用程序池
-   PAM 监视服务 (PamMonitoringService)
-   PAM 组件服务 (PrivilegeManagementComponentService)

以下 MIM 组件不支持作为 gMSA 帐户运行：

-   MIM 门户。 这是因为 MIM 门户是 SharePoint 环境的一部分。 但你可以在场模式下部署 SharePoint，并[在 SharePoint Server 中配置自动密码更改](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)。
-   所有管理代理
-   Microsoft 证书管理
-   BHOLD


可以在下面的文章中找到有关 gMSA 的详细信息：
-   [组托管服务帐户概述](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)

-   [创建密钥分发服务 (KDS) 根密钥](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx)

## <a name="create-user-accounts-and-groups"></a>创建用户帐户和组

MIM 部署的所有组件都需要在域中具有自己的标识。 这包括服务和同步等 MIM 组件，以及 SharePoint 和 SQL。


> [!NOTE]
> 本演练使用名为 Contoso 的公司中的示例名和值。 将其替换为你自己的。 例如：
> - 域控制器名称 - **dc**
> - 域名 - **contoso**
> - MIM 服务服务器名称 - **mimservice**
> - MIM 同步服务器名称 - **mimsync**
> - SQL Server 名称 - **sql**
> - 密码 - <strong>Pass@word1</strong>

1. 以域管理员的身份登录到域控制器（*例如：Contoso\Administrator*）。

2. 为 MIM 服务创建以下用户账户。 启动 PowerShell 并键入以下 PowerShell 脚本，以创建新的 AD 域用户（并非所有帐户都是必需的，尽管脚本仅用于提供信息，但最佳做法是使用专用 MIMAdmin  帐户安装 MIM 和 SharePoint 进程）。

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

    New-ADUser –SamAccountName MIMAdmin –name MIMAdmin
    Set-ADAccountPassword –identity MIMAdmin –NewPassword $sp
    Set-ADUser –identity MIMAdmin –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcSharePoint –name svcSharePoint
    Set-ADAccountPassword –identity svcSharePoint –NewPassword $sp
    Set-ADUser –identity svcSharePoint –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMSql –name svcMIMSql
    Set-ADAccountPassword –identity svcMIMSql –NewPassword $sp
    Set-ADUser –identity svcMIMSql –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMAppPool –name svcMIMAppPool
    Set-ADAccountPassword –identity svcMIMAppPool –NewPassword $sp
    Set-ADUser –identity svcMIMAppPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  为所有组创建安全组。

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordSet –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordSet
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupMember -identity MIMSyncAdmins -Members MIMAdmin
    ```

4.  添加 SPN，以便为服务账户启用 Kerberos 身份验证

    ```PowerShell
    Set-ADServiceAccount -Identity svcMIMAppPool -ServicePrincipalNames @{Add="http/mim.contoso.com"}
    ```

5.  请务必注册以下 DNS“A”记录以进行专有名称解析（假设 MIM 服务、MIM 门户、密码重置和密码注册网站将托管在同一台计算机上）

    - mim.contoso.com - 指向 MIM 服务和门户服务器物理 IP 地址
    - passwordreset.contoso.com - 指向 MIM 服务和门户服务器物理 IP 地址
    - passwordregistration.contoso.com - 指向 MIM 服务和门户服务器物理 IP 地址

## <a name="create-key-distribution-service-root-key"></a>创建密钥分发服务根密钥

请确保以管理员身份登录到域控制器，以便准备组密钥分发服务。

如果已存在域的根密钥（可使用 Get-KdsRootKey  进行检查），则继续下一部分。

6.  如果需要，请创建密钥分发服务 (KDS) 根密钥（每个域仅一次）。 域控制器上的 KDS 服务使用根密钥（以及其他信息）来生成密码。 以管理员身份键入以下 PowerShell 命令：

    ```PowerShell
    Add-KDSRootKey –EffectiveImmediately
    ```
    –EffectiveImmediately  可能需要最长延迟 \~10 小时，因为需要将其复制到所有域控制器。 对于两个域控制器来说，该延迟大约花费 1 小时。

    ![](media/7fbdf01a847ea0e330feeaf062e30668.png)

    >[!NOTE]
    >在实验室或测试环境中，你可以通过运行以下命令来避免 10 小时的复制延迟：
    ><br/>
    >Add-KDSRootKey -EffectiveTime ((Get-Date).AddHours(-10))

## <a name="create-mim-synchronization-service-account-group-and-service-principal"></a>创建 MIM 同步服务帐户、组和服务主体
-----------------------

对于要在其中安装 MIM 软件的计算机，请确保其所有计算机帐户都已加入域。  然后，以域管理员身份在 PowerShell 中执行这些步骤。

7.  创建名为“MIMSync_Server”的组并将所有 MIM 同步服务器添加到此组  。
    键入以下内容，为 MIM 同步服务器创建新的 AD 组。 然后，将 MIM 同步服务器 Active Directory 计算机帐户（如 contoso\MIMSync $）添加到该组中  。

    ```PowerShell
    New-ADGroup –name MIMSync_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMSync_Servers
    Add-ADGroupmember -identity MIMSync_Servers -Members MIMSync$
    ```

8.  创建 MIM 同步服务 gMSA。 键入以下 PowerShell 命令。

    ```PowerShell
    New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"
    ```

    检查通过执行 Get-ADServiceAccount  PowerShell 命令创建的 GSMA 的详细信息：

    ![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

9.  如果计划运行密码更改通知服务，则需要通过执行以下 PowerShell 命令来注册服务主体名称：

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames @{Add="PCNSCLNT/mimsync.contoso.com"}
    ```

10. 重新启动 MIM 同步服务器以刷新与服务器关联的 Kerberos 令牌，因为“MIMSync_Server”组成员身份已更改。

## <a name="create-mim-service-management-agent-service-account"></a>创建 MIM 服务管理代理服务帐户

11. 通常在安装 MIM 服务时，需要为 MIM 服务管理代理（MIM MA 帐户）创建一个新帐户。  gMSA 有两个可用选项。

- 使用 MIM 同步服务组托管服务帐户，而不创建单独的帐户

    可以跳过创建 MIM 服务管理代理服务帐户的步骤。 在这种情况下，请在安装 MIM 服务时使用 MIM 同步服务 gMSA 名称（例如 contoso\MIMSyncGMSAsvc $），而不是使用 MIM MA 帐户  。 随后在“MIM 服务管理代理配置”中，启用“使用 MIMSync 帐户”选项  。

    请勿为 MIM 同步服务 gMSA 启用“拒绝从网络登录”权限，因为 MIM MA 帐户需要“允许网络登录”权限。

- 请为 MIM 服务管理代理服务帐户使用常规服务帐户

    以域管理员身份启动 PowerShell，然后键入以下内容以创建新的 AD 域用户：

    ```PowerShell
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    
    New-ADUser –SamAccountName svcMIMMA –name svcMIMMA
    Set-ADAccountPassword –identity svcMIMMA –NewPassword $sp
    Set-ADUser –identity svcMIMMA –Enabled 1 –PasswordNeverExpires 1
    ```

    请勿为 MIM MA 帐户启用“拒绝从网络登录”权限，因为该帐户需要“允许网络登录”权限。

## <a name="create-mim-service-accounts-groups-and-service-principal"></a>创建 MIM 服务帐户、组和服务主体

继续以域管理员身份使用 PowerShell。
   
12. 创建“MIMService_Server”组并将所有 MIM 服务服务器添加到此组  。  键入以下 PowerShell 命令，为 MIM 服务服务器创建新的 AD 组，并将 MIM 服务服务器 Active Directory 计算机帐户（例如 contoso\MIMPortal $）添加到此组  。

    ```PowerShell
    New-ADGroup –name MIMService_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMService_Servers
    Add-ADGroupMember -identity MIMService_Servers -Members MIMPortal$
    ```

1.  创建 MIM 服务 gMSA。

    ```PowerShell
    New-ADServiceAccount -Name MIMSrvGMSAsvc -DNSHostName MIMSrvGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMService_Servers" -OtherAttributes @{'msDS-AllowedToDelegateTo'='FIMService/mimportal.contoso.com'} 
    ```

1.  执行以下 PowerShell 命令，注册服务主体名称并启用 Kerberos 委托：

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSrvGMSAsvc -TrustedForDelegation $true -ServicePrincipalNames @{Add="FIMService/mimportal.contoso.com"}
    ```

1.  对于 SSPR 方案，需要使用 MIM 服务帐户才能与 MIM 同步服务进行通信，因此 MIM 服务帐户必须是 MIMSyncAdministrators 或 MIM 同步密码重置和浏览组的成员：

    ```PowerShell
    Add-ADGroupmember -identity MIMSyncPasswordSet -Members MIMSrvGMSAsvc$ 
    Add-ADGroupmember -identity MIMSyncBrowse -Members MIMSrvGMSAsvc$ 
    ```

16. 重新启动 MIM 服务服务器以刷新与服务器关联的 Kerberos 令牌，因为“MIMService_Server”组成员身份已更改。

## <a name="create-other-mim-accounts-and-groups-if-needed"></a>根据需要创建其他 MIM 帐户和组

如果你正在配置 MIM SSPR，请遵循上述适用于 MIM 同步服务和 MIM 服务的相同准则，你可以为以下各项创建其他 gMSA：

- MIM 密码重置网站应用程序池
- MIM 密码注册网站应用程序池

如果你正在配置 MIM PAM，请遵循上述适用于 MIM 同步服务和 MIM 服务的相同准则，你可以为以下各项创建其他 gMSA：

- MIM PAM REST API 网站应用程序池
- MIM PAM 组件服务
- MIM PAM 组件服务

## <a name="specifying-a-gmsa-when-installing-mim"></a>安装 MIM 时请指定 gMSA

通常，在大多数情况下，使用 MIM 安装程序时，若要指示你希望使用 gMSA 而不是常规帐户，请将美元符号字符追加到 gMSA 名称（例如 contoso\MIMSyncGMSAsvc $），并将密码字段留空  。 但有一种情况例外，即 miisactivate 工具接受不带美元符号的 gMSA 名称  。
<br/>

> [!div class="step-by-step"]
> [Windows Server »](prepare-server-ws2016.md)
