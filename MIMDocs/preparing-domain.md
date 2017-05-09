---
title: "设置 Microsoft Identity Manager 2016 的域 | Microsoft Docs"
description: "安装 MIM 2016 之前，创建 Active Directory 域控制器"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: e16bcc36fe4bccb621ba4d649aa0b015f2adbcdd
ms.lasthandoff: 01/24/2017


---

# <a name="set-up-a-domain"></a>设置域

>[!div class="step-by-step"]
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)

Microsoft Identity Manger (MIM) 适用于你的 Active Directory (AD) 域。 应已安装了 AD，并确保你的环境中有一个你能够管理的域的域控制器。

本文将指导你完成一些步骤来准备要与 MIM 协同使用的域。

## <a name="create-user-accounts-and-groups"></a>创建用户帐户和组

MIM 部署的所有组件都需要在域中具有自己的标识。 这包括服务和同步等 MIM 组件，以及 SharePoint 和 SQL。

> [!NOTE]
> 本演练使用名为 Contoso 的公司中的示例名和值。 将其替换为你自己的。 例如：
> - 域控制器名称 - **mimservername**
> - 域名 - **contoso**
> - 密码 - **Pass@word1**

1. 以域管理员的身份登录到域控制器（*例如：Contoso\Administrator*）。

2. 为 MIM 服务创建以下用户账户。 启动 PowerShell 并键入以下 PowerShell 脚本来更新域。

    ```
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMMA –name MIMMA
    Set-ADAccountPassword –identity MIMMA –NewPassword $sp
    Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSync –name MIMSync
    Set-ADAccountPassword –identity MIMSync –NewPassword $sp
    Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMService –name MIMService
    Set-ADAccountPassword –identity MIMService –NewPassword $sp
    Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSSPR –name MIMSSPR
    Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
    Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SharePoint –name SharePoint
    Set-ADAccountPassword –identity SharePoint –NewPassword $sp
    Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SqlServer –name SqlServer
    Set-ADAccountPassword –identity SqlServer –NewPassword $sp
    Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
    Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
    Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
    ```

3.  为所有组创建安全组。

    ```
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    ```

4.  添加 SPN，以便为服务账户启用 Kerberos 身份验证

    ```
    setspn -S http/mimservername.contoso.local Contoso\SharePoint
    setspn -S http/mimservername Contoso\SharePoint
    setspn -S FIMService/mimservername.contoso.local Contoso\MIMService
    setspn -S FIMSynchronizationService/mimservername.contoso.local Contoso\MIMSync
    ```

>[!div class="step-by-step"]
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)

