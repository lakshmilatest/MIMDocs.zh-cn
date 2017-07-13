---
title: "部署 PAM 步骤 2 – PRIV DC | Microsoft Docs"
description: "准备 PRIV 域控制器，它将提供堡垒环境，Privileged Access Management 在此环境中是独立的。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: edc15b41d4248887f4a93217f68d8125f6500585
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# 步骤 2 - 准备第一个 PRIV 域控制器
<a id="step-2---prepare-the-first-priv-domain-controller" class="xliff"></a>

>[!div class="step-by-step"]
[« 步骤 1](step-1-prepare-corp-domain.md)
[步骤 3 »](step-3-prepare-pam-server.md)

在此步骤中，你将创建一个新域，用于为管理员进行身份验证提供堡垒环境。  此林将需要至少一个域控制器和至少一个成员服务器。 成员服务器将在下一步骤中进行配置。

## 创建一个新的 Privileged Access Management 域控制器
<a id="create-a-new-privileged-access-management-domain-controller" class="xliff"></a>

本节将安装一个虚拟机，充当新林的域控制器

### 安装 Windows Server 2012 R2
<a id="install-windows-server-2012-r2" class="xliff"></a>
在未安装任何软件的另一台新虚拟机上，请安装 Windows Server 2012 R2 以使计算机为“PRIVDC”。

1. 选择执行 Windows Server 的自定义（非升级）安装。 在安装时，指定 **Windows Server 2012 R2 Standard（带有 GUI 的服务器）x64**；_不要选择_“数据中心或服务器核心”。

2. 查看并接受许可条款。

3. 由于磁盘将为空，请选择“自定义: 仅安装 Windows”并使用未初始化的磁盘空间。

4. 安装操作系统版本后，以新管理员的身份登录此新计算机。 通过“控制面板”将计算机名称设置为“PRIVDC”，为其提供一个虚拟网络上的静态 IP 地址，并将 DNS 服务器配置为上一步骤中安装的域控制器。 这将需要重新启动服务器。

5. 重启服务器后，以管理员身份登录。 使用“控制面板”，配置计算机以检查更新，并安装所需的任何更新。 这可能需要重新启动服务器。

### 添加角色
<a id="add-roles" class="xliff"></a>
添加 Active Directory 域服务 (AD DS) 和 DNS 服务器角色。

1. 以管理员身份启动 PowerShell。

2. 键入以下命令以准备安装 Windows Server Active Directory。

  ```
  import-module ServerManager

  Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
  ```

### 配置 SID 历史记录迁移所需的注册表设置
<a id="configure-registry-settings-for-sid-history-migration" class="xliff"></a>

启动 PowerShell 并键入以下命令，以便将源域配置为允许远程过程调用 (RPC) 访问安全帐户管理器 (SAM) 数据库。

```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## 创建一个新的 Privileged Access Management 林
<a id="create-a-new-privileged-access-management-forest" class="xliff"></a>

然后，将此服务器提升为新林中的域控制器。

在本文中，名称 priv.contoso.local 用作新林的域名。  林的名称并不重要，并且它不需要从属于组织中现有的林名称。 但是，新林的域名和 NetBIOS 名称必须具有唯一性且不同于组织中任何其他域的名称。  

### 创建一个域和林
<a id="create-a-domain-and-forest" class="xliff"></a>

1. 在“PowerShell”窗口中，键入以下命令以创建新域。  此操作还将在上一步骤所创建的高级域 (contoso.local) 中创建一个 DNS 委派。  如果想要在以后配置 DNS，则忽略 `CreateDNSDelegation -DNSDelegationCredential $ca` 参数。

  ```
  $ca= get-credential

  Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
  ```

2. 当出现弹出窗口时，为 CORP 林管理员提供凭据（如：用户名 CONTOSO\\Administrator 和步骤 1 中对应的密码）。

3. “PowerShell”窗口将提示你使用安全模式管理员密码。 输入两次新密码。 将出现 DNS 委派和加密设置的警告消息；这些是正常的。

完成林创建后，服务器将自动重新启动。

### 创建用户和服务帐户
<a id="create-user-and-service-accounts" class="xliff"></a>
创建用于 MIM 服务和门户设置的用户和服务帐户。 这些帐户都将置于 priv.contoso.local 域的用户容器中。

1. 重启服务器后，以域管理员身份 (PRIV\\Administrator) 登录到 PRIVDC。

2. 启动 PowerShell，键入以下命令。 密码 'Pass@word1' 只是一个示例，应为帐户使用不同的密码。

  ```
  import-module activedirectory

  $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  New-ADUser –SamAccountName MIMMA –name MIMMA

  Set-ADAccountPassword –identity MIMMA –NewPassword $sp

  Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

  Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

  Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

  Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

  Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMSync –name MIMSync

  Set-ADAccountPassword –identity MIMSync –NewPassword $sp

  Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMService –name MIMService

  Set-ADAccountPassword –identity MIMService –NewPassword $sp

  Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SharePoint –name SharePoint

  Set-ADAccountPassword –identity SharePoint –NewPassword $sp

  Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SqlServer –name SqlServer

  Set-ADAccountPassword –identity SqlServer –NewPassword $sp

  Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

  Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

  Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

  New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

  Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

  Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

  Add-ADGroupMember "Domain Admins" SharePoint

  Add-ADGroupMember "Domain Admins" MIMService
  ```

### 配置审核和登录权限
<a id="configure-auditing-and-logon-rights" class="xliff"></a>

你需要设置审核权限，以便跨林实现 PAM 配置。  

1. 确保以域管理员身份登录 (PRIV\\Administrator)。

2. 转到“开始” > “管理工具” > “组策略管理”。

3. 导航到“林: priv.contoso.local” > “域” > “priv.contoso.local” > “域控制器” > “默认域控制器策略”。 将显示一条警告消息。

4. 右键单击“默认域控制器策略”，然后选择“编辑”。

5. 在“组策略管理编辑器”控制台树中，导航到“计算机配置” > “策略” > “Windows 设置” > “安全设置” > “本地策略” > “审核策略”。

6. 在“详细信息”窗格中，右键单击“审核帐户管理”并选择“属性”。 单击“定义这些策略设置” ，依次选中“成功”  、“失败”  复选框，然后依次单击“应用”  和“确定” 。

7. 在“详细信息”窗格中，右键单击“审核目录服务访问”并选择“属性”。 单击“定义这些策略设置” ，依次选中“成功”  、“失败”  复选框，然后依次单击“应用”  和“确定” 。

8. 导航到“计算机配置” > “策略” > “Windows 设置” > “安全设置” > “帐户策略” > “Kerberos 策略”。

9. 在“详细信息”窗格中，右键单击“用户票证最长寿命”并选择“属性”。 单击“定义这些策略设置” ，然后将小时数设置为 *1*，再依次单击“应用”  和“确定” 。 请注意，窗口中的其他设置也会更改。

10. 在“组策略管理”窗口中，选择“默认域策略”，右键单击并选择“编辑”。

11. 展开“计算机配置” > “策略” > “Windows 设置” > “安全设置” > “本地策略”并选择“用户权限分配”。

12. 在“详细信息”窗格上，右键单击“拒绝作为批处理作业登录”，然后选择“属性”。

13. 选中“定义这些策略设置”复选框，单击“添加用户或组”，然后在“用户和组”名称字段中，键入 *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* 并单击“确定”。

14. 单击“确定”以关闭该窗口。

15. 在“详细信息”窗格上，右键单击“拒绝通过远程桌面服务登录”，然后选择“属性”。

16. 选中“定义这些策略设置”复选框，单击“添加用户或组”，然后在“用户和组”名称字段中，键入 *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* 并单击“确定”。

17. 单击“确定”以关闭该窗口。

18. 关闭“组策略管理编辑器”窗口和“组策略管理”窗口。

19. 以管理员身份启动 PowerShell 窗口，并键入以下命令以从组策略设置中更新 DC。

  ```
  gpupdate /force /target:computer
  ```

  一分钟后，它将完成并显示消息“计算机策略更新已成功完成”。


### 在 PRIVDC 上配置 DNS 名称转发
<a id="configure-dns-name-forwarding-on-privdc" class="xliff"></a>

使用 PRIVDC 中的 PowerShell，配置 DNS 名称转发以使 PRIV 域能够识别其他现有的林。

1. 启动 PowerShell。

2. 对于每个现有林顶部的每个域，键入以下命令，指定现有的 DNS 域（例如：contoso.local）以及该域的主服务器的 IP 地址。  

  如果在上一步创建了一个 contoso.local 域，则指定  *10.1.1.31* 作为 CORPDC 计算机的虚拟网络 IP 地址。

  ```
  Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
  ```

> [!NOTE]
> 其他林也必须能够按指定路线将 PRIV 林的 DNS 查询发送到此域控制器。  如果具有多个现有的 Active Directory 林，则还必须为这些林的每一个添加一个 DNS 条件转发器。

### 配置 Kerberos
<a id="configure-kerberos" class="xliff"></a>

1. 使用 PowerShell 添加 SPN，以便 SharePoint、PAM REST API 和 MIM 服务可以使用 Kerberos 身份验证。

  ```
  setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
  setspn -S http/pamsrv PRIV\SharePoint
  setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
  setspn -S FIMService/pamsrv PRIV\MIMService
  ```

> [!NOTE]
> 本文的后续步骤将介绍如何在单台计算机上安装 MIM 2016 服务器组件。 如果打算添加另一台服务器以实现高可用性，则将需要如 [FIM 2010: Kerberos Authentication Setup](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx)（FIM 2010：Kerberos 身份验证设置）中所述的其他 Kerberos 配置。

### 配置委派以提供 MIM 服务帐户访问权限
<a id="configure-delegation-to-give-mim-service-accounts-access" class="xliff"></a>

以域管理员身份在 PRIVDC 上执行以下步骤。

1. 启动“Active Directory 用户和计算机” 。  
2. 右键单击域“priv.contoso.local”并选择“委派控制”。  
3. 在“选定的用户和组”选项卡上，单击“添加”。  
4. 在“选择用户、计算机或组”窗口中，键入“mimcomponent; mimmonitor; mimservice” 并单击“检查名称”。 在名称带有下划线后，单击“确定”，然后单击“下一步”。  
5. 在常见任务列表中，选择“创建、删除和管理用户帐户”和“修改组成员身份”，然后单击“下一步”和“完成”。

6. 再次右键单击域“priv.contoso.local”并选择“委派控制”。  
7. 在“选定的用户和组”选项卡上，单击“添加”。  
8. 在“选择用户、计算机或组”窗口中，输入“MIMAdmin”，然后单击“检查名称”。 在名称带有下划线后，单击“确定”，然后单击“下一步”。  
9. 选择“自定义任务” ，使用“常规权限” 应用到“此文件夹” 。    
10. 在权限列表中，选择以下项：  
  - **读取**  
  - **写入**  
  - **创建所有子对象**  
  - **删除所有子对象**  
  - **读取所有属性**  
  - **写入所有属性**  
  - **迁移 SID 历史记录**  
  依次单击“下一步”  、“完成” 。

11. 再次右键单击域“priv.contoso.local”并选择“委派控制”。  
12. 在“选定的用户和组”选项卡上，单击“添加”。  
13. 在“选择用户、计算机或组”窗口中，输入“MIMAdmin”然后单击“检查名称”。 在名称带有下划线后，单击“确定” ，然后单击“下一步” 。  
14. 选择“自定义任务” ，应用到“此文件夹” ，然后单击“仅用户对象” 。    
15. 在权限列表中，选择“更改密码”  和“重置密码” 。 然后依次单击“下一步”  和“完成” 。  
16. 关闭“Active Directory 用户和计算机”。

17. 打开命令提示符。  
18. 查看 PRIV 域中 Admin SD Holder 对象上的访问控制列表。 例如，如果你的域为“priv.contoso.local”，则键入以下命令  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"
  ```
19. 根据需要更新访问控制列表，以确保 MIM 服务和 MIM 组件服务可以更新由此 ACL 保护的组成员。  键入命令：  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"
  ```
20. 重新启动 PRIVDC 服务器以使这些更改起效。

## 准备 PRIV 工作站
<a id="prepare-a-priv-workstation" class="xliff"></a>

如果还没有将加入到 PRIV 域以执行 PRIV 资源（例如 MIM）维护的工作站计算机，请按照这些说明来准备工作站。  

### 安装 Windows 8.1 或者 Windows 10 企业版
<a id="install-windows-81-or-windows-10-enterprise" class="xliff"></a>

在未安装任何软件的另一台新虚拟机上，安装 Windows 8.1 或 Windows 10 企业版，以使计算机为“PRIVWKSTN”。

1. 在安装过程中使用快速设置。

2. 请注意，安装可能无法连接到 Internet。 单击“创建本地帐户”。 指定不同的用户名；不要使用“Administrator”或“Jen”。

3. 使用“控制面板”，为这台计算机提供虚拟网络上的一个静态 IP 地址，并将接口的首选 DNS 服务器设置为 PRIVDC 服务器。

4. 使用“控制面板”，将 PRIVWKSTN 计算机加入到 priv.contoso.local 域。 此操作将需要提供 PRIV 域管理员凭据。 完成此步骤后，重启计算机 PRIVWKSTN。

若要了解更多详细信息，请参阅[保护访问特权工作站](https://technet.microsoft.com/en-us/library/mt634654.aspx)。

下一步将介绍准备 PAM 服务器。

>[!div class="step-by-step"]
[« 步骤 1](step-1-prepare-corp-domain.md)
[步骤 3 »](step-3-prepare-pam-server.md)
