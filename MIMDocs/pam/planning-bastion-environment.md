---
title: "规划堡垒环境 | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: b8af77d2354428da19d91d5f02b490012835f544
ms.openlocfilehash: 0ed48d43825e1a876c4d96cafcb6c17cac26610f


---

# 规划堡垒环境

向 Active Directory 添加具有专用管理林的堡垒环境使组织可以在安全控制强于自己现有生产环境的环境中轻松地管理管理帐户、工作站和组。

此体系结构会启用许多在单个林体系结构中不可能或不容易配置的控件。 其中包括将生产环境中特权级别非常高的帐户设置为管理林中的标准非特权用户，从而进行更出色的技术强制管理。 此体系结构还可以利用信任关系的选择性身份验证功能，以便仅限于登录到经过授权的主机（或向其公开凭据）。 在生产林需要更高级别的保证且进行完整的重新生成时避免开销和复杂性的情况下，管理林可以提供一个提升生产环境保证级别的环境。

除了专用管理林之外，可能还用到其他技术。 其中包括限制公开管理凭据的位置、限制该林中用户的角色权限，并确保在用于标准用户活动（例如，电子邮件和 Web 浏览）的主机上不执行管理任务。

## 最佳做法注意事项

专用管理林是标准的单一域 Active Directory 林，用于 Active Directory 管理。 使用管理林和域的优势是，比生产林具有更多的安全措施，因为生产林的用例设有限制。 而且，由于此林处于隔离状态，并且不信任组织的现有林，因此另一林的安全损害将不会扩展到此专用林。

管理林设计具有以下注意事项：

### 作用域受限

管理林的价值在于提供高级别的安全保障，从而减少了攻击面。 此林可容纳其他管理功能和应用程序，但每增大一次作用域，都将增大林及其资源的攻击面。 目标是限制林的功能，以最小化攻击面。

根据分区管理权限的[层模型](tier-model-for-partitioning-administrative-privileges.md)，专用管理林中的帐户应该在单层中，通常是第 0 层或第 1 层。 如果一个林在第 1 层中，请考虑将其限定于应用程序（例如财务应用）或用户社区（例如外包 IT 供应商）的特定范围。

### 信任受限

生产 CORP 林应信任管理 PRIV 林，反之则不行。 这可以是域信任，也可以是林信任。 管理林域不需要信任托管的域和林来管理 Active Directory，尽管其他应用程序可能需要双向信任关系、安全验证以及测试。

应使用选择性身份验证，以确保管理林中的帐户仅使用适当的生产主机。 至于在 Active Directory 中维护域控制器和委派权限，这通常需要将域控制器的“允许登录”权限授予管理林中指定的第 0 层管理帐户。 有关详细信息，请参阅 [Configuring Selective Authentication Settings](http://technet.microsoft.com/library/cc755844.aspx)（配置选择性身份验证设置）。

## 保持逻辑分隔

为了确保堡垒环境不受到组织 Active Directory 中的现有或未来安全事件影响，为堡垒环境准备系统时，应使用以下准则：

- Windows Server 不应加入域或利用现有环境中的软件或设置分配。

- 堡垒环境必须包含其自身的 Active Directory 域服务，并向堡垒环境提供 Kerberos 以及 LDAP、DNS、时间和时间服务。

- MIM 不应在现有环境中使用 SQL 数据库场。 在堡垒环境中，应在专用服务器上部署 SQL Server。

- 堡垒环境需要 Microsoft Identity Manager 2016，具体来说就是必须部署 MIM 服务和 PAM 组件。

- 在现有林中，堡垒环境的备份软件和媒体必须与系统的备份软件和媒体分开，以避免现有林中的管理员破坏堡垒环境的备份。

- 管理堡垒环境服务器的用户必须从现有环境中的管理员无法访问的工作站进行登录，以避免泄露堡垒环境的凭据。

## 确保管理服务的可用性

由于应用程序管理将过渡到堡垒环境，因此需要考虑如何提供足够的可用性以满足这些应用程序的要求。 包括以下技术：

- 将 Active Directory 域服务部署在堡垒环境中的多台计算机上。 至少需要两台服务器以确保持续的身份验证，即使其中一台因定期维护而临时重新启动。 如果负载较高或者需管理多个地理区域中的资源和管理员，可能需要更多的计算机。

- 在现有林和专用管理林中，准备“break glass”帐户以备不时之需。

- 将 SQL Server 和 MIM 服务部署在堡垒环境中的多台计算机上。

- 每次对专用管理林中的用户或角色定义进行更改后，都要保留 AD 和 SQL 的备份副本。

## 配置适当的 Active Directory 权限

应根据 Active Directory 管理的要求，对管理林配置最小特权。

- 管理林中用于管理生产环境的帐户不应被授予对管理林、其中的域或工作站的管理权限。

- 高于管理林自身的管理权限应由离线进程对其进行严格的控制，以降低攻击者或恶意内部人员清除审核日志的可能性。 这还有助于确保具有生产管理员帐户的人员无法放宽对其帐户的限制，从而避免增大组织的风险。

- 管理林应遵循域的 Microsoft Security Compliance Manager (SCM) 配置以及身份验证协议的强配置。

在安装 Microsoft 标识管理器之前创建堡垒环境时，请标识并创建将用于此环境中管理的帐户。 这将包括：

- **Break glass 帐户**应仅能够登录到堡垒环境中的域控制器。

- **“Red Card”管理员**可预配其他帐户并执行计划外的维护。 不会向这些帐户提供对堡垒环境外的现有林或系统的任何访问权限。 凭据（例如，智能卡）应该以物理方式保护，并且应记录这些帐户的使用情况。

- **服务帐户**：Microsoft Identity Manager、SQL Server 以及其他软件都需要这样的帐户。

## 加强主机

加入到管理林的所有主机（包括域控制器、服务器和工作站）都应安装了最新的操作系统及 Service Pack 并保持最新。

- 执行管理所需的应用程序应预安装在工作站上以便使用它们的帐户不需要在本地管理员组中就可以安装它们。 域控制器维护通常使用 RDP 和远程服务器管理工具执行。

- 管理林主机应使用安全更新自动更新。 虽然这可能产生中断域控制器维护操作的风险，但它仍极大地缓解了未安装修补程序的漏洞带来的安全风险。

### 标识管理主机

系统或工作站的风险应通过在系统中执行最高风险的活动进行衡量，如 Internet 浏览、发送和接收电子邮件，或者使用其他应用程序处理未知或不受信任的内容。

管理主机包括以下计算机：

- 在其中以物理方式键入或输入管理员凭据的台式电脑。

- 在其中运行管理会话和工具的管理“跳转服务器”。

- 在其中执行管理操作的所有主机，包括那些使用标准用户桌面运行 RDP 客户端进行远程管理服务器和应用程序的主机。

- 承载需要管理的应用程序的服务器，并且无法通过使用 RDP 与受限的管理模式或 Windows PowerShell 远程进行访问。

### 部署专用管理工作站

虽然不太方便，但必须为具有高效管理凭据的用户专门配备单独的加强型工作站。 请使用等于或大于权限级别（委托给凭据）的安全级别提供主机，这一点至关重要。 为实现其他保护，请考虑采用以下方法：

- **验证版本中的所有媒体是否干净**可防御在下载或存储过程中安装在主映像或注入安装文件中的恶意软件。

- **安全基线** 应在启动配置时使用。 Microsoft Security Compliance Manager (SCM) 可帮助配置管理主机上的基线。

- **安全引导** 可防止攻击者或恶意软件试图将未经签名的代码加载到启动过程中。

- **软件限制** 可确保在管理主机上仅执行经过授权的管理软件。 借助经过授权的应用程序允许列表，客户可对此任务使用 AppLocker 以帮助阻止执行恶意软件和不受支持的应用程序。

- **全卷加密** 可防止计算机实体丢失（如远程使用的管理笔记本电脑）。

- **USB 限制**可针对物理感染进行保护。

- **网络隔离** 可防止网络攻击和管理员无意的操作。 主机防火墙应阻止除明确需要的所有传入连接，并阻止所有不必要的出站 Internet 访问。

- **反恶意软件** 可抵御已知的威胁和恶意软件。

- **攻击缓解** 可防范未知的威胁和攻击，包括 Enhanced Mitigation Experience Toolkit (EMET)。

- **攻击面分析** 可在安装新软件过程中防止将新的攻击媒介引入 Windows。 Attack Surface Analyzer (ASA) 等工具有助于评估主机上的配置设置，并标识由软件或配置更改引入的攻击途径。

- **管理权限**不应授予给其本地计算机上的用户。

- **RestrictedAdmin 模式**下可传出 RDP 会话，除非角色需要。 有关详细信息，请参阅 [Windows Server 中远程桌面服务的新增功能](https://technet.microsoft.com/library/dn283323.aspx)。

其中的某些衡量方式看起来很极端，但最近几年的公开启示都显示出技术精湛的攻击者具有破坏目标的强大能力。

## 准备在堡垒环境中管理的现有域

MIM 使用 PowerShell cmdlet 在现有 AD 域和堡垒环境中的专用管理林之间建立信任关系。 在部署堡垒环境之后并且在任何用户或组转换为 JIT 之前，`New-PAMTrust` 和 `New-PAMDomainConfiguration` cmdlet 将更新域信任关系并创建 AD 和 MIM 所需的项目。

当现有 Active Directory 拓扑发生改变时，可使用 `Test-PAMTrust`、 `Test-PAMDomainConfiguration`、 `Remove-PAMTrust` 和 `Remove-PAMDomainConfiguration` cmdlet 来更新信任关系。

### 为每个林建立信任关系

必须为每个现有林都运行一次 `New-PAMTrust` cmdlet。 该命令在管理域中的 MIM 服务计算机上调用。 此命令的参数是现有林顶级域的域名以及该域管理员的凭据。

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

建立信任关系后，要对每个域都进行配置，以便从堡垒环境中进行管理，如下一部分中所述。

### 启用每个域的管理

启用现有域管理具有七项要求。

#### 1.本地域上的安全组

现有域中必须有一个组，其名称为后跟三个美元符号的 NetBIOS 域名（例如，CONTOSO$$$）。 组作用域必须为“本地域”，并且组类型必须为“安全”。 在专用管理林中创建与此域中的组具有相同“安全”标识符的组时需要满足此要求。 通过以下 PowerShell 命令创建此组，由现有域的管理员执行并在已加入到现有域的工作站上运行：

```
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

#### 2.成功和失败的审核

对于审核，域控制器上的组策略设置必须包括“审核”帐户管理和“审核”目录服务访问的成功和失败审核。 可以通过组策略管理控制台实现此目的，由现有域的管理员执行并在已加入到现有域的工作站上运行：

3. 转到“开始” > “管理工具” > “组策略管理”。

4. 导航到“林: contoso.local” > “域” > “contoso.local” > “域控制器” > “默认域控制器策略”。 将出现通知性消息。

    ![默认域控制器策略 - 屏幕截图](media/pam-group-policy-management.jpg)

5. 右键单击“默认域控制器策略”，然后选择“编辑”。 将出现一个新窗口。

6. 在“组策略管理编辑器”窗口中的“默认域控制器策略”树下，导航到“计算机配置” > “策略” > “Windows 设置” > “安全设置” > “本地策略” > “审核策略”。

    ![组策略管理编辑器 - 屏幕截图](media/pam-group-policy-management-editor.jpg)

5. 在详细信息窗格中，右键单击“审核帐户管理”，然后选择“属性”。 选择“定义这些策略设置”，在“成功”和“失败”上分别放置一个复选框，然后依次单击“应用”和“确定”。

6. 在详细信息窗格中右键单击“审核目录服务访问”，然后选择“属性”。 选择“定义这些策略设置”，在“成功”和“失败”上分别放置一个复选框，然后依次单击“应用”和“确定”。

    ![成功和失败的策略设置 - 屏幕截图](media/pam-group-policy-management-editor2.jpg)

7. 关闭“组策略管理编辑器”窗口和“组策略管理”窗口。 然后，通过启动 PowerShell 窗口并键入以下内容应用审核设置：

    ```
    gpupdate /force /target:computere
    ```

消息“计算机策略更新成功完成。” 应在几分钟之后出现。

#### 3.允许连接到本地安全机构

域控制器必须允许本地安全机构 (LSA) 的 TCP/IP 上的 RPC 连接（来自堡垒环境）。 在较旧版本的 Windows Server 上，必须在注册表中启用 LSA 中的 TCP/IP 支持：

```
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

#### 4.创建 PAM 域配置

`New-PAMDomainConfiguration` cmdle 必须在管理域中的 MIM 服务计算机上运行。 此命令的参数是现有域的域名以及该域管理员的凭据。

```
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

#### 5.为帐户授予读取权限

堡垒林中用于建立角色（使用 `New-PAMUser` 和 `New-PAMGroup` cmdlet 的管理员）的帐户，以及 MIM 监视器服务使用的帐户需要在该域中拥有读取权限。

执行以下步骤将为用户 PRIV\Administrator在 CORPDC  域控制器上开启域 Contoso 的读取访问：

1. 确保你以 Contoso 域管理员（如 Contoso\Administrator）的身份登录到 CORPDC。

2. 启动 Active Directory 用户和计算机。

3. 右键单击域“contoso.local”并选择“委托控件”。

4. 在“选定的用户和组”选项卡上单击“添加”。

5. 在“选择用户、计算机或组”弹出窗口上，单击“位置”，并将位置更改为 priv.contoso.local。 在“对象名称”上，键入“Domain Admins”并单击“检查名称”。 当显示弹出窗口时，为用户名键入 priv\administrator 和密码。

6. 在域管理员后键入“; MIMMonitor”。 为名称 Domain Admins 和 MIMMonitor 加了下划线后，单击“确定”，然后单击“下一步”。

7. 在常见任务列表中，选择“读取所有用户信息”，然后依次单击“下一步”和“完成”。

18. 关闭“Active Directory 用户和计算机”。

#### 6.Break glass 帐户

如果特权访问管理项目的目标是减少向域永久分配的具有域管理特权的帐户数量，则域中必须具有 *break glass* 帐户，以防止信任关系以后出现问题。 每个域中都应存在用于紧急访问生产林的帐户，并且应只能登录到预控制器。 对于具有多个站点的组织，可能还需要其他帐户以备不时之需。

#### 7.在堡垒环境中更新权限

查看该域的系统容器中 AdminSDHolder 对象上的特权。 *AdminSDHolder* 对象具有唯一的访问控制列表 (ACL)，用于控制安全主体成员（为内置特权 Active Directory 组的成员）的权限。 请注意，如果对默认权限进行了任何更改，都会影响到域中具有管理权限的用户，因为这些权限将不适用于帐户在堡垒环境中的用户。

## 选择要包含的用户和组

下一步是定义 PAM 角色，并关联它们应当访问的用户和组。 对于标识为在堡垒环境中处于托管状态的层而言，这通常为用户和组的一个子集。 有关详细信息，请参阅[定义 Privileged Access Management 角色](defining-roles-for-pam.md)。



<!--HONumber=Jul16_HO2-->


