---
title: "部署 PAM 步骤 1 - CORP 域 | Microsoft Docs"
description: "准备 CORP 域，其具有将由 Privileged Identity Manager 管理的现有标识或新标识"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 1164e7efb70d911497b08248b68f8d929bc6d3fb
ms.lasthandoff: 05/02/2017


---

# <a name="step-1---prepare-the-host-and-the-corp-domain"></a>步骤 1 - 准备主机和 CORP 域

>[!div class="step-by-step"]
[步骤 2 »](step-2-prepare-priv-domain-controller.md)


在该步骤中你将准备托管堡垒环境。 如果有必要，还将在具有由堡垒环境管理的身份的新域和林（*CORP* 林）中创建域控制器和成员工作站。 此 CORP 林将模拟具有要管理的资源的现有林。 本文档包含要保护的示例资源：一个文件共享。

如果你有现有的 Active Directory (AD) 域，并且其域控制器运行 Windows Server 2012 R2 或更高版本，而你是域管理员，那么你可以改用该域。  

## <a name="prepare-the-corp-domain-controller"></a>准备 CORP 域控制器

本节介绍如何设置 CORP 域的域控制器。 在 CORP 域中，管理用户由堡垒环境管理。 本示例中使用的 CORP 域的域名系统 (DNS) 名称为 *contoso.local*。

### <a name="install-windows-server"></a>安装 Windows Server

在一台虚拟机上安装 Windows Server 2012 R2 或 Windows Server 2016 Technical Preview 4 或更高版本以创建名为 *CORPDC* 的计算机。

1. 选择 **Windows Server 2012 R2 Standard（带有 GUI 的服务器）x64** 或 **Windows Server 2016 Technical Preview（带有桌面体验的服务器）**。

2. 查看并接受许可条款。

3. 由于磁盘将为空，请选择“自定义: 仅安装 Windows”并使用未初始化的磁盘空间。

4. 以其管理员身份登录到此新计算机。 导航到“控制面板”。 将计算机名称设为“CORPDC”，并在虚拟网络上为其提供一个静态 IP 地址。 重新启动服务器。

5. 在服务器重启后，以管理员身份登录。 导航到“控制面板”。 配置计算机以检查更新，并安装所需的任何更新。 重新启动服务器。

### <a name="add-roles-to-establish-a-domain-controller"></a>添加角色以建立域控制器

在本节中你将添加 Active Directory 域服务 (AD DS)、DNS 服务器和文件服务器（文件和存储服务部分的一部分）角色，并将此服务器提升到新林“contoso.local”的域控制器。

> [!NOTE]  
> 如果你已有一个域用作你的 CORP 域，并且该域使用 Windows Server 2012 R2 或更高版本作为其域控制器，则可以跳到[创建用于演示的其他用户和组](#create-additional-users-and-groups-for-demonstration-purposes)。

1. 以管理员身份登录，并启动 PowerShell。

2. 键入下列命令。

  ```
  import-module ServerManager

  Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

  Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
  ```

  这将提示使用安全模式管理员密码。 请注意，将出现 DNS 委派和加密设置的警告消息。 这些都是正常的。

3. 在林创建操作完成后，请注销。 服务器将自动重启。

4. 在服务器重启后，以该域的管理员身份登录到 CORPDC。 通常为用户 CONTOSO\\管理员，该用户具有在 CORPDC 上安装 Windows 时创建的密码。

### <a name="create-a-group"></a>创建组

使用 Active Directory 创建用于审核的组，如果该组不存在。 组的名称必须是后跟三个美元符号的 NetBIOS 域名，例如“CONTOSO$$$”。

对于每个域，以域管理员的身份登录到域控制器，并执行以下步骤：

1. 启动 PowerShell。

2. 键入以下命令，但是将“CONTOSO”替换为 NetBIOS 域名。

  ```
  import-module activedirectory

  New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
  ```

在某些情况下该组可能已经存在 - 如果 AD 迁移场景中也使用该域，则这是正常情况。

### <a name="create-additional-users-and-groups-for-demonstration-purposes"></a>创建用于演示的其他用户和组

如果创建了新的 CORP 域，那么应创建用于演示 PAM 场景的其他用户和组。 用于演示的用户和组不应为域管理员或由 AD 中的 adminSDHolder 设置控制。

> [!NOTE]
> 如果有将用作 CORP 域的域，并且它具有可用于演示的用户和组，则可以跳到[配置审核](#configure-auditing)部分。

我们将创建一个名为“CorpAdmins”的安全组和一个名为“Jen”的用户。 如果你想要，可以使用不同的名称。

1. 启动 PowerShell。

2. 键入下列命令。 将密码 'Pass@word1' 替换为其他密码字符串。

  ```
  import-module activedirectory

  New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

  New-ADUser –SamAccountName Jen –name Jen

  Add-ADGroupMember –identity CorpAdmins –Members Jen

  $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  Set-ADAccountPassword –identity Jen –NewPassword $jp

  Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
  ```

### <a name="configure-auditing"></a>配置审核

你需要在现有林中启用审核，才能在这些林中建立 PAM 配置。  

对于每个域，以域管理员的身份登录到域控制器，并执行以下步骤：

1. 转到“开始” > “管理工具”（或者在 Windows Server 2016 上为“Windows 管理工具”），并启动“组策略管理”。

2. 导航到此域的域控制器策略。  如果为 contoso.local 创建了新域，则导航到“林：contoso.local” > “域” > “contoso.local” > “域控制器” > “默认域控制器策略”。 将显示一条信息性消息。

3. 右键单击“默认域控制器策略”，然后选择“编辑”。 此时将显示新窗口。

4. 在“组策略管理编辑器”窗口中的“默认域控制器策略”树下，导航到“计算机配置” > “策略” > “Windows 设置” > “安全设置” > “本地策略” > “审核策略”。

5. 在详细信息窗格中，右键单击“审核帐户管理”，然后选择“属性”。 选择“定义这些策略设置”，在“成功”和“失败”上分别放置一个复选框，然后依次单击“应用”和“确定”。

6. 在详细信息窗格中右键单击“审核目录服务访问”，然后选择“属性”。 选择“定义这些策略设置”，在“成功”和“失败”上分别放置一个复选框，然后依次单击“应用”和“确定”。

7. 关闭“组策略管理编辑器”窗口和“组策略管理”窗口。

8. 通过启动 PowerShell 窗口并键入以下内容应用审核设置：

  ```
  gpupdate /force /target:computer
  ```

几分钟后将显示消息“计算机策略更新已成功完成”。

### <a name="configure-registry-settings"></a>配置注册表设置

在本节中你将配置 SID 历史记录迁移所需的注册表设置，该设置将用于 Privileged Access Management 组创建。

1. 启动 PowerShell。

2. 键入以下命令，这些命令将源域配置为允许远程过程调用 (RPC) 访问安全帐户管理器 (SAM) 数据库。

  ```
  New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

  Restart-Computer
  ```

这将重启域控制器，CORPDC。 有关此注册表设置的详细信息，请参阅[《如何解决在使用 ADMTv2 进行林间 SIDHistory 迁移时出现的问题》](http://support.microsoft.com/kb/322970)。

## <a name="prepare-a-corp-workstation-and-resource"></a>准备 CORP 工作站和资源

如果还没有加入到域中的工作站计算机，请按照以下说明准备该计算机。  

> [!NOTE]
> 如果已经有加入到域中的工作站，请跳到[创建用于演示的资源](#create-a-resource-for-demonstration-purposes)。

### <a name="install-windows-81-or-windows-10-enterprise-as-a-vm"></a>安装 Windows 8.1 或者 Windows 10 企业版作为 VM

在未安装任何软件的另一台新虚拟机上，安装 Windows 8.1 企业版或 Windows 10 企业版以创建计算机“CORPWKSTN”。

1. 在安装过程中使用快速设置。

2. 请注意，安装可能无法连接到 Internet。 选择“创建本地帐户”。 指定不同的用户名；不要使用“Administrator”或“Jen”。

3. 使用“控制面板”，为这台计算机提供虚拟网络上的一个静态 IP 地址，并将接口的首选 DNS 服务器设置为 CORPDC 服务器中的服务器。

4. 使用“控制面板”，将 CORPWKSTN 计算机加入到 contoso.local 域。 你需要提供 Contoso 域管理员凭据。 完成此步骤后，重新启动计算机 CORPWKSTN。

### <a name="create-a-resource-for-demonstration-purposes"></a>创建用于演示的资源

你需要资源来演示使用 PAM 的基于安全组的访问控制。  如果还没有资源，可以使用文件文件夹进行演示。  这将使用在 contoso.local 域中创建的“Jen”和“CorpAdmins”AD 对象。

1. 连接到工作站 CORPWKSTN。 单击“切换用户”图标，然后单击“其他用户”。 请确保用户 CONTOSO\\Jen 可以登录到 CORPWKSTN。

2. 创建名为“CorpFS”的新文件夹，并将其与“CorpAdmins”组共享。

3. 以管理员身份打开 PowerShell。

4. 键入下列命令。

  ```
  mkdir c:\corpfs

  New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

  $acl = Get-Acl c:\corpfs

  $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

  $acl.SetAccessRule($car)

  Set-Acl c:\corpfs $acl
  ```

在下一步骤中你将要准备 PRIV 域控制器。

>[!div class="step-by-step"]
[步骤 2 »](step-2-prepare-priv-domain-controller.md)

