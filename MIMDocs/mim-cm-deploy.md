---
title: 部署 Microsoft Identity Manager 证书管理器 | Microsoft Docs
description: 安装 Microsoft Identity Manager 2016 证书管理器
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/19/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 3c2246ec21ad73cf025daec5c56295ec57838bb2
ms.sourcegitcommit: 3502d636687e442f7d436ee56218b9b95f5056cf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2018
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>部署 Microsoft Identity Manager 证书管理器 2016 (MIM CM)

Microsoft Identity Manager 证书管理器 2016 (MIM CM) 的安装包含多个步骤。 我们将细分事项，这是简化安装过程的一种方法。 在执行任何实际的 MIM CM 步骤之前必须执行预备步骤。 不执行预备步骤，安装则可能失败。 

1. 部署概述
2. 预先部署步骤
3. 还有什么？

下图显示的示例为可能使用的环境类型。 标有数字的系统在图下的列表中列出，成功完成本文中的步骤需要这些系统。 最后，使用 Windows 2016 数据中心服务器：

![环境图](media/mim-cm-deploy/image001.png)

1. CORPDC - 域控制器
2. CORPCM - MIM CM 服务器
3. CORPCA - 证书颁发机构
4. CORPCMR - MIM CM Rest API Web - 用于 Rest API 的 CM 门户 - 供将来使用
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – 已加入 Windows 10 域

## <a name="deployment-overview"></a>部署概述

- 基本操作系统安装
  - 实验室包含 Windows 2016 数据中心服务器。
       >[!NOTE]
有关 MIM 2016 支持的平台的详细信息，请阅读标题为[MIM 2016 支持的平台](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md)的文章。
- 预先部署步骤
  - [扩展架构](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)
  - 创建服务帐户
  - [创建证书模板](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)
  - IIS
  - 配置 Kerberos
  - 与数据库相关的步骤
    - SQL 配置要求
    - 数据库权限
- 部署

## <a name="pre-deployment-steps"></a>预先部署步骤

MIM CM 配置向导要求在此过程中提供信息，以便成功完成各步骤。 
![](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>扩展架构

扩展架构的进程是非常简单的，但是由于其不可逆性，必须谨慎处理。

>[!NOTE]
此步骤要求使用的帐户具有架构管理员权限。

- 浏览到 MIM 媒体的位置，并导航到 \\证书管理\\x64 文件夹。

- 将架构文件夹复制到 CORPDC，然后导航到它。

    ![](media/mim-cm-deploy/image005.png)

- 运行脚本 resourceForestModifySchema.vbs 单一林方案

- 为资源林方案运行脚本：
  - DomainA – 用户位置 (userForestModifySchema.vbs)
  - ResourceForestB – CM 安装位置 (resourceForestModifySchema.vbs)

>[!NOTE]
架构更改是单向操作，并需要林恢复回滚，以确保有必要的备份。 有关通过执行此操作对架构所做的更改的详细信息，请查看文章 [Forefront Identity Manager 2010 Certificate Management Schema Changes](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)（Forefront Identity Manager 2010 证书管理架构更改）

![](media/mim-cm-deploy/image007.png)

运行脚本，脚本完成运行后，将收到一条成功消息。

![成功消息](media/mim-cm-deploy/image009.png)

现在，AD 中的架构已扩展为支持 MIM CM。

### <a name="creating-service-accounts-and-groups"></a>创建服务帐户和组

下表总结了 MIM CM 所需的帐户和权限。
可以允许 MIM CM 自动创建以下帐户，或者可以在安装前创建它们。 可以更改实际帐户名。 如果是自己创建帐户，请考虑采用能够将用户帐户名和其功能轻松匹配的方式对用户帐户命名。

Users：

![](media/mim-cm-deploy/image010.png)

![](media/mim-cm-deploy/image012.png)

| **角色**                   | **用户登录名** |
|----------------------------|---------------------|
| MIM CM 代理               | MIMCMAgent          |
| MIM CM 密钥恢复代理  | MIMCMKRAgent        |
| MIM CM 授权代理 | MIMCMAuthAgent      |
| MIM CM CA 管理员代理    | MIMCMManagerAgent   |
| MIM CM Web 池代理      | MIMCMWebAgent       |
| MIM CM 注册代理    | MIMCMEnrollAgent    |
| MIM CM 更新服务      | MIMCMService        |
| MIM 安装帐户        | MIMINSTALL          |
| 支持人员代理            | CMHelpdesk1-2       |
| CM 管理员                 | CMManager1-2        |
| 订阅方用户            | CMUser1-2           |

组：

| **角色**               | **组**         |
|------------------------|-------------------|
| CM 支持人员成员    | MIMCM-支持人员    |
| CM 管理员成员     | MIMCM-管理员    |
| CM 订阅方成员 | MIMCM-订阅方 |

Powershell：代理帐户

```
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>更新代理帐户的 CORPCM 服务器本地策略 

| **用户登录名** | **说明和权限**   |
|------|---------------------|
| MIMCMAgent          | 提供下列服务： </br>- 检索来自 CA 的已加密私钥。 </br>- 保护 FIM CM 数据库中的智能卡 PIN 信息。 </br>- 保护 FIM CM 和 CA 之间的通信。 </br></br> 此用户帐户需要以下访问控制设置：</br>-   “允许本地登录”用户权限。</br>-   “颁发和管理证书”用户权限。 </br>- 对位于以下位置的系统 Temp 文件夹的读写权限：%WINDIR%\\Temp。</br>- 数字签名和在用户存储中颁发和安装的加密证书。
|MIMCMKRAgent        | 从 CA 恢复已存档私钥。 此用户帐户需要以下访问控制设置：</br> -   “允许本地登录”用户权限。</br>- 本地“管理员”组中的成员身份。 </br>- KeyRecoveryAgent 证书模板上的注册权限。 </br>- 在用户存储中颁发并安装“密钥恢复代理”证书。 必须将证书添加到 CA 上的密钥恢复代理列表。 </br>- 对位于以下位置的系统 Temp 文件夹的读写权限：```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | 确定用户和组的用户权限。 此用户帐户需要以下访问控制设置： </br>- Pre-Windows 2000 Compatible Access 域组中的成员身份。 </br> - 已被授予“生成安全审核”用户权限。             |
| MIMCMManagerAgent   | 执行 CA 管理活动。 </br> 必须给此用户分配管理 CA 权限。        |
| MIMCMWebAgent       | 为 IIS 应用程序池提供标识。 FIM CM 在使用此用户凭据的 Microsoft Win32® 应用程序编程接口进程内运行。 </br> 此用户帐户需要以下访问控制设置：</br> - 本地 IIS_WPG、windows 2016 = IIS_IUSRS 组中的成员身份。 </br>- 本地“管理员”组中的成员身份。</br>- 已被授予“生成安全审核”用户权限。 </br>- 已被授予“作为操作系统的一部分”用户权限。 </br>- 已被授予“替换进程级别令牌”用户权限。</br>- 已作为 IIS 应用程序池的标识“CLMAppPool”分配。 </br>- 已被授予 HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\CLM\\v1.0\\Server\\WebUser 注册表项上的读取权限。 </br>- 此帐户的委派也必须受到信任。|
| MIMCMEnrollAgent    | 代表用户执行注册。 此用户帐户需要以下访问控制设置：</br>- 在用户存储中颁发和安装的注册代理证书。</br>-   “允许本地登录”用户权限。 </br>- 对“注册代理”证书模板（如果使用了自定义模板，也可为自定义模板）的注册权限。                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>为 MIM CM 服务帐户创建证书模板

在 MIM CM 使用的服务帐户中，其中三个需要证书，并且配置向导要求用户提供用于请求证书的证书模板名。

需要证书的服务帐户是：

- MIMCMAgent：此帐户需要用户证书

- MIMCMEnrollAgent：此帐户需要注册代理证书

- MIMCMKRAgent：此帐户需要“密钥恢复代理”证书

AD 中已有模板，但我们需要创建自己的版本，与 MIM CM 一起使用。 因为我们需要对原始基线模板做出修改。

以上三个帐户都将在组织内具有提升的权限，应谨慎处理。

#### <a name="create-the-mim-cm-signing-certificate-template"></a>创建 MIM CM 签名证书模板

1. 从“管理工具”中打开“证书颁发机构”。
2. 在“证书颁发机构”控制台的控制台树中，展开“Contoso-CorpCA”，然后单击“证书模板”。
3. 右键单击 **“证书模板”**，然后单击 **“管理”**。
4. 在“证书模板控制台”的“细节”**窗格**中，选择并右键单击“用户”，然后单击“复制模板”。
5. 在“复制模板”对话框中，选择“Windows Server 2003 Enterprise”，然后单击“确定”。

![显示产生的变化](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    MIM CM does not work with certificates based on version 3 certificate templates. You must create a Windows Server® 2003 Enterprise (version 2)certificate template. See the following link for V3 details https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade

6. 在“新模板的属性”对话框“常规”选项卡上的“模板显示名称”框中，键入“MIM CM 签名”。 将“有效期”更改为“2 年”然后清除“在 Active Directory 中发布证书”复选框。

7. 在“请求处理”选项卡上，确保选中“允许导出私钥”复选框，然后单击“加密选项卡”。

8. 在“加密选择”对话框中，禁用“Microsoft Enhanced Cryptographic Provider v1.0”，启用“Microsoft 增强 RSA 和 AES 加密提供程序”，然后单击“确定” 。

在“使用者名称”**Subject Name**选项卡上，清除“在使用者名称中包括电子邮件名” 和“电子邮件名”复选框。

在“扩展”**Extensions**选项卡的“这个模板中包括的扩展”列表中，确保选中“应用程序策略”，然后单击“编辑”。

在“编辑应用程序策略扩展”对话框，同时选中“加密文件系统”和“安全电子邮件”应用程序策略。 单击 **“删除”**，然后单击 **“确定”**。

在“安全性”选项卡上，执行以下步骤：

- 删除“管理员”。

- 删除“域管理员”。

- 删除“域用户”。

- 仅将“读取”和“写入”权限分配到“Enterprise Admins”。

- 添加“MIMCMAgent”。

- 将“读取”和“注册”权限分配到“MIMCMAgent”。

在“新模板的属性”对话框中，单击“确定”。

使“证书模板控制台”处于打开状态。

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>创建 MIM CM 注册代理证书模板

-   在“证书模板控制台”的“细节”窗格中，选择并右键单击“注册代理”，然后单击“复制模板” 。

在“复制模板”对话框中，选择“Windows Server 2003 Enterprise”，然后单击“确定”。

在“新模板的属性”对话框“常规”选项卡的“模板显示名称”框中，键入“MIM CM 注册代理”。 请确保“有效期”为“2 年”。

在“请求处理”选项卡上，启用“允许导出私钥”，然后单击 CSP 或加密选项卡。

在“CSP 选择”对话框中，禁用“Microsoft Base Cryptographic Provider v1.0”和“Microsoft Enhanced Cryptographic Provider v1.0”，启用“Microsoft 增强 RSA 和 AES 加密提供程序”，然后单击“确定”。

在“安全性”选项卡上，执行以下步骤：

- 删除“管理员”。

- 删除“域管理员”。

- 仅将“读取”和“写入”权限分配到“Enterprise Admins”。

- 添加“MIMCMEnrollAgent”。

- 将“读取”和“注册”权限分配到“MIMCMEnrollAgent”。

在“新模板的属性”对话框中，单击“确定”。

使“证书模板控制台”处于打开状态。

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>创建 MIM CM 密钥恢复代理证书模板

1. 在“证书模板”控制台的“细节”窗格中，选择并右键单击“密钥恢复代理”，然后单击“复制模板” 。

2. 在“复制模板”对话框中，选择“Windows Server 2003 Enterprise”，然后单击“确定”。

3. 在“新模板的属性”对话框“常规”选项卡的“模板显示名称”框中，键入“MIM CM 密钥恢复代理”。 确保“加密选项卡”上的“有效期”为“2 年”。

4. 在“提供程序选择”对话框中，禁用“Microsoft Enhanced Cryptographic Provider v1.0”，启用“Microsoft 增强 RSA 和 AES 加密提供程序”，然后单击“确定”。

5. 在“颁发要求”选项卡上，确保“CA 证书管理程序批准”为“禁用”状态。

6. 在“安全性”选项卡上，执行以下步骤：

    - 删除“管理员”。

    - 删除“域管理员”。

    - 仅将“读取”和“写入”权限分配到“Enterprise Admins”。

    - 添加“MIMCMKRAgent”。

    - 将“读取”和“注册”权限分配到“KRAgent”。

7. 在“新模板的属性”对话框中，单击“确定”。

8. 关闭“证书模板控制台” 。

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>发布所需的位于证书颁发机构处的证书模板

1. 还原“证书颁发机构”控制台。

2. 在“证书颁发机构”控制台的控制台树中，右键单击“证书模板”，指向“新建”，然后单击“要颁发的证书模板”。
3. 在“启用证书模板”对话框中，选择“MIM CM 注册代理”、“MIM CM 密钥恢复代理”和“MIM CM 签名”。 单击" **确定**"。
4. 在控制台树中，单击“证书模板”。
5. 验证三个新模板是否显示在“细节”窗格中，然后关闭“证书颁发机构”。
    ![MIM CM 签名](media/mim-cm-deploy/image016.png)
6. 关闭所有打开的窗口并注销。

### <a name="iis-configuration"></a>IIS 配置 

为了托管 CM 的网站，请安装和配置 IIS

#### <a name="install-and-configure-iis"></a>安装和配置 IIS

1. 以 MIMINSTALL 帐户，登录到 COR

>[!IMPORTANT]
MIM 安装帐户应为本地管理员

2. 打开 PowerShell，运行以下命令

   - ```Install-WindowsFeature –ConfigurationFilePath```

>[!NOTE]
 在默认情况下，使用 IIS 7 安装名为“默认网站”的站点。 如果该站点已重命名或删除，在可以安装 MIM CM 之前，名为“默认网站”的站点必须可用。

#### <a name="configuring-kerberos"></a>配置 Kerberos

MIMCMWebAgent 帐户将运行 MIM CM 门户。 在 IIS 和正常运行的内核模式中，默认在 IIS 中使用身份验证。 将禁用 Kerberos 内核模式身份验证，改为在 MIMCMWebAgent 帐户上配置 SPN。 某些命令在活动目录和 CORPCM 服务器中需要提升的权限。

![](media/mim-cm-deploy/image020.png)

```
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}

```

**更新 CORPCM 上的 IIS


![](media/mim-cm-deploy/image022.png)

```
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true

```


>[!NOTE]
将需要为“cm.contoso.com”添加 DNS A 记录，并指向 CORPCM IP

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>在 MIM CM 门户上要求 SSL

强烈建议用户在 MIM CM 门户上要求 SSL。 若不要求，向导甚至将对此进行警告。

1. 为分配到默认站点的 cm.contoso.com 注册 Web 证书

2. 打开“IIS 管理器” 并导航到“证书管理”

3. 在“功能视图”中，双击“SSL 设置”。

4. 在 SSL 设置页，选择“要求 SSL”。

5. 在“操作”窗格中，单击“应用”。

### <a name="database-configuration-corpsql-for-mim-cm"></a>MIM CM 的数据库配置 CORPSQL

1. 确保连接到 CORPSQL01 服务器。

2. 确保以 SQL DBA 身份登录

3. 运行以下 T-SQL 脚本，使 CONTOSO\\MIMINSTALL 帐户在转到配置步骤时能够创建数据库

>[!NOTE]
准备好退出和策略模块时，我们将需要回到 SQL

```
create login [CONTOSO\\MIMINSTALL] from windows;
exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
```

![MIM CM 配置向导错误消息](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Microsoft Identity Manager 2016 证书管理的部署

1. 确保已连接到 CORPCM 服务器，并且 MIMINSTALL 帐户是本地管理员组的成员。

2. 确保以 Contoso\\MIMINSTALL 身份登录。

3. 装载 Microsoft Identity Manager SP1 ISO。

4. 打开“证书管理\\x64”目录。

5. 在“x64”窗口中，右键单击“安装”，然后单击“以管理员身份运行”。

6. 在“欢迎使用 Microsoft Identity Manager 证书管理安装向导”页上，单击“下一步”。

7. 在“最终用户许可协议”页上，阅读协议，选中“我接受许可协议中的条款”复选框，然后单击下一步。

8. 在“自定义安装”页上，确保“MIM CM 门户”和“MIM CM 更新服务组件”设置为待安装，然后单击“下一步”。

9. 在“虚拟 Web 文件夹”页上，确保虚拟文件夹名是“CertificateManagement”，然后单击“下一步”。

10. 在“安装 Microsoft Identity Manager 证书管理”页上，单击“安装”。

11. 在已完成的Microsoft Identity Manager 证书管理安装向导”页上，单击“完成”。

![已完成 MIM CM 向导](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Microsoft Identity Manager 2016 证书管理的配置向导

在登录到 CORPCM 之前，请将 MIMINSTALL 添加到配置向导的“域管理员”、“架构管理员”和“本地管理员”组。 可在稍后完成配置之后将它删除。      
    
![错误消息](media/mim-cm-deploy/image028.png)

1. 从“开始”菜单上，单击“证书管理配置向导”。 以“管理员”身份运行
2. 在“欢迎使用配置向导”页上，单击“下一步”。
3. 在“CA 配置” 页上，确保所选的 CA 是 Contoso-CORPCA-CA所选的服务器是 CORPCA.CONTOSO.COM，然后单击“下一步”。
4. 在“安装 Microsoft® SQL Server® 数据库”页上的“SQL Server 名称”框中，键入“CORPSQL1”，选中“使用我的凭据创建数据库”复选框，然后单击“下一步”。
5. 在“数据库设置”页上，接受 FIMCertificateManagement 的默认数据库名称，确保选中“SQL 集成身份验证”，然后单击“下一步”。

6. 在“设置 Active Directory”页上，接受为服务连接点提供的默认名称，然后单击“下一步”。

7. 在“身份验证方法”页上，确认已选中“Windows 集成身份验证”，然后单击“下一步”。

8. 在“代理 - FIM CM”页上，清除“使用 FIM CM 默认设置”复选框，然后单击“自定义帐户”。

9. 在“代理 - FIM CM”多选项卡式对话框中，在每个选项卡上键入以下信息：
   - 用户名：Update 
   - 密码：Pass\@word1
   - 确认密码：Pass\@word1
   - 使用现有用户：已启用
>[!NOTE]
我们之前已创建了这些帐户。 请确保为全部六个代理帐户选项卡重复步骤 8 中的过程。

![MIM CM 帐户](media/mim-cm-deploy/image030.png)

10. 完成所有代理帐户信息后，单击“确定”。

11. 在“代理 - MIM CM”页上，单击“下一步”。

12. 在“设置服务器证书”页上，启用以下证书模板：
    - 将用于恢复代理“密钥恢复代理”证书的证书模板：MIMCMKeyRecoveryAgent。
    - 将用于 FIM CM 代理证书的证书模板：MIMCMSigning。
    - 将用于注册代理证书的证书模板：FIMCMEnrollmentAgent。
13. 在“设置服务器证书”页上，单击“下一步”。
14. 在“安装电子邮件服务器、文档打印”页的“指定要使用的 SMTP 服务器名称，以便通过电子邮件发送注册通知”框中，单击“下一步”。
15. 在“已准备好进行配置”页上，单击“配置”。
16. 在“配置向导 - Microsoft Forefront Identity Manager 2010 R2”警告对话框中，单击“确定” 确认未在 IIS 虚拟目录上启用 SSL。

    ![media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    在配置向导完成执行之前，切勿单击“完成”按钮。 可在以下位置找到向导日志：%programfiles%\\Microsoft Forefront Identity Management\\2010年\\证书管理\\config.log
17. 单击 **“完成”**。

![已完成 MIM CM 向导](media/mim-cm-deploy/image033.png)

18. 关闭所有打开的窗口。

19. 在浏览器中将 https://cm.contoso.com/certificatemanagement 添加到本地 Intranet 区域。

20. 从服务器 CORPCM 访问网站 https://cm.contoso.com/certificatemanagement  

    ![](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>验证 CNG 密钥隔离服务

1. 从“管理工具”中打开“服务”。

2. 在“细节”窗格中，双击“CNG 密钥隔离”。

3. 在“常规”选项卡上，将“启动类型”更改为“自动”。

4. 在“常规”选项卡中，若服务不处于启动状态则启动服务。

5. 在“常规”**General**选项卡上，单击“确定”。

### <a name="installing-and-configuring-the-ca-modules-"></a>安装和配置 CA 模块：

在此步骤中，我们将在证书颁发机构上安装和配置 FIM CM CA 模块。

1. 将 FIM CM 配置为仅检查管理操作的用户权限

2. 在“C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\证书管理\\web”窗口中，复制一个 web.config 副本，将它命名为 web.1.config。

3. 在“Web”窗口，右键单击“Web.config”，然后单击“打开”。

    >[!Note]
    在记事本中打开 Web.config 文件

4. 在打开文件后，按 CTRL+F。

5. 在“查找和替换”对话框的“查找内容”框中，键入“UseUser”，然后单击“查找下一个”三次。

6. 关闭“查找和替换”对话框。

7. 将位于 \<add key=”Clm.RequestSecurity.Flags” value=”UseUser,UseGroups” /\> 行上。 将该行更改为 \<add key=”Clm.RequestSecurity.Flags” value=”UseUser” /\>。

8. 关闭该文件，保存所有更改。

9. 在 SQL 服务器上为 CA 计算机创建帐户\<无 脚本\>

10. 确保连接到 CORPSQL01 服务器。

11. 确保以 DBA 身份登录

12. 从“开始”菜单，启动“SQL Server Management Studio”。

13. 在“连接到服务器”对话框的“服务器名称”框中，键入“CORPSQL01”，然后单击“连接”。

14. 在控制台树中，展开“安全性”，然后单击“登录”。

15. 右键单击“登录” ，然后单击“新建登录名” 。

16. 在“常规”页的“登录名”框中，键入“contoso\\CORPCA\$”。 选择“Windows 身份验证”。 默认数据库是 FIMCertificateManagement。

17. 在左窗格中，选择“用户映射”。 在右窗格中，单击 FIMCertificateManagement 旁“映射”列中的复选框。 在“FIMCertificateManagement 的数据库角色成员身份”列表中，启用 clmApp 角色。

18. 单击" **确定**"。

19. 关闭“Microsoft SQL Server Management Studio” 。

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>在证书颁发机构上安装 FIM CM CA 模块

1. 确保连接到 CORPCA 服务器。

2. 在“x64”窗口中，右键单击“Setup.exe”，然后单击“以管理员身份运行”。

3. 在“欢迎使用 Microsoft Identity Manager 证书管理安装向导”页上，单击“下一步”。

4. 在“最终用户许可协议”页上，阅读协议。 选择“我接受许可协议中的条款”复选框，然后单击“下一步”。

5. 在“自定义安装”页，选择“MIM CM 门户”，然后单击“将无法使用这项功能”。

6. 在“自定义安装”页，选择“MIM CM 更新服务”，然后单击“将无法使用这项功能”。

    >[!Note]
    这将使“MIM CM CA 文件”成为对安装启用的唯一功能。

7. 在“自定义安装”页上，单击“下一步”。

8. 在“安装 Microsoft Identity Manager 证书管理”页上，单击“安装”。

9. 在“已完成 Microsoft Identity Manager 证书管理安装向导”页上，单击“完成”。

10. 关闭所有打开的窗口。

### <a name="configure-the-mim-cm-exit-module"></a>配置 MIM CM 退出模块

1. 从“管理工具”中打开“证书颁发机构”。

2. 在控制台树中，右键单击“contoso-CORPCA-CA”，然后单击“属性”。

3. 在“退出模块”选项卡上，选择“FIM CM 退出模块”，然后单击“属性”。

4. 在“指定 CM 数据库连接字符串”框中，键入“Connect Timeout=15;Persist Security Info=True; Integrated Security=sspi;Initial Catalog=FIMCertificateManagement;Data Source=CORPSQL01”。 使“加密连接字符串”复选框处于选中状态，然后单击“确定”。
5. 在“Microsoft FIM 证书管理”消息框中，单击“确定”。

6. 在“contoso-CORPCA-CA 属性”对话框中，单击“确定”。

7. 右键单击 contoso-CORPCA-CA****，指向“所有任务”，然后单击“停止服务”。 等待 Active Directory 证书服务停止。

8. 右键单击 contoso-CORPCA-CA****，指向“所有任务”，然后单击“启动服务”。

9. 最小化证书颁发机构控制台。

10. 从“管理工具”中打开“事件查看器”。

11. 在控制台树中，展开“应用程序和服务日志”，然后单击“FIM 证书管理” 。

12. 在事件列表中，验证自证书服务上次重启后，最新事件不包括任何“警告”或“错误”事件。

    >[!NOTE] 
    最新事件应该声明使用 ```SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit``` 中的设置加载的退出模块

13. 最小化事件查看器。

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>将 MIMCMAgent 证书的指纹复制到 Windows® 剪贴板

1. 还原“证书颁发机构”控制台。

2. 在控制台树中，展开“contoso CORPCA CA”，然后单击“颁发的证书”。

3. 在“细节”窗格中，双击“申请人姓名”栏中有“CONTOSO\\MIMCMAgent”且“证书模板”栏中有“FIM CM 签名”的证书。

4. 在“详细信息”  选项卡上，选择“指纹”  字段。

5. 选择指纹，然后按 CTRL+C。

    >[!NOTE]
    不要在指纹字符列表中包含先导空格。

6. 在“证书”对话框中，单击“确定”。

7. 从“开始”菜单的“搜索程序和文件”框中，键入“记事本”，然后按 ENTER。

8. 在“记事本”中，从“编辑”菜单，单击“粘贴”。

9. 从“编辑”菜单中，单击“替换”。

10. 在“查找内容”框中，键入空格字符，然后单击“全部替换” 。

    >[!Note]
    此操作将删除指纹中字符间的所有空格。
    
11. 在“替换”对话框中，单击“取消”。

12. 选择已转换的“thumbprintstring”，然后按 CTRL+C。

13. 关闭“记事本”，不保存更改。

### <a name="configure-the-fim-cm-policy-module"></a>配置 FIM CM 策略模块

1. 还原“证书颁发机构”控制台。

2. 右键单击“contoso-CORPCA-CA”，然后单击“属性”。

3.  在“contoso CORPCA CA 属性”对话框的“策略模块”选项卡上，单击“属性”。

- 在“常规”选项卡上，确保选中“将非 FIM CM 请求传递到默认策略模块进行处理”。
- 在“签名证书”选项卡上，单击“添加”。
- 在“证书”对话框中，右键单击“请指定十六进制编码的证书哈希”框，然后单击“粘贴”。
- 在“证书”对话框中，单击“确定”。
    >[!Note]
    如果“确定”按钮未启用，则从 clmAgent 证书复制指纹时，会意外地将隐藏字符包含在指纹字符串中。 在此练习中，重复自“任务 4：将 MIMCMAgent 证书的指纹复制到 Windows 剪贴板”开始的所有步骤。

- 在“配置属性”对话框中，确保指纹显示在“有效的签名证书”列表中，然后单击“确定”。

- 在“FIM 证书管理”消息框中，单击“确定”。

- 在“contoso-CORPCA-CA 属性”对话框中，单击“确定”。

- 右键单击 contoso-CORPCA-CA****，指向“所有任务”，然后单击“停止服务”。

- 等待 Active Directory 证书服务停止。

- 右键单击 contoso-CORPCA-CA****，指向“所有任务”，然后单击“启动服务”。

- 关闭“证书颁发机构”控制台。

- 关闭所有打开的窗口，然后注销。

- 部署的最后一个步骤是确保 CONTOSO\\MIMCM 管理员可以部署和创建模板，并配置系统，而无需是构架管理员和域管理员。 下一个脚本将使用 dsacls 通过 ACL 列出证书模板的权限。 请运行具有完全权限的帐户，以更改林中每个现有证书模板的安全性读取和写入权限。

- 第一个步骤：配置服务连接点和目标组权限以及委派配置文件模板管理
  - 配置服务连接点 (SCP) 上的权限。

  - 配置委派的配置文件模板管理。

  - 配置服务连接点 (SCP) 上的权限。 \<无脚本\>

     -   确保连接到 CORPDC 虚拟服务器。

     -   以 contoso\\corpadmin 身份登录

     -   从“管理工具”中打开“Active Directory 用户和计算机”。

     -   在“Active Directory 用户和计算机”的“查看”菜单上，确保“高级功能”已启用。

     -   在控制台树中，展开“Contoso.com \| System \| Microsoft \| Certificate Lifecycle Manager”，然后单击“CORPCM”。

     -   右键单击“CORPCM”，然后单击“属性”。

     -   在“CORPCM 属性”对话框中的“安全性”选项卡上，添加具有相应权限的以下组：

    | Group          | 权限                                                                                                                                                         |
    |----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | mimcm-管理员 | 读取 </br> FIM CM 审核</br> FIM CM 注册代理</br> FIM CM 请求注册</br> FIM CM 请求恢复</br> FIM CM 请求续订</br> FIM CM 请求撤销 </br> FIM CM 请求取消阻止智能卡 |
    | mimcm-支持人员 | 读取</br> FIM CM 注册代理</br> FIM CM 请求撤销</br> FIM CM 请求取消阻止智能卡                                                                                |
- 在“CORPDC 属性”对话框中，单击“确定”。

- 使“Active Directory 用户和计算机”处于打开状态。

- 在下级用户对象上配置权限
    - 确保仍位于“Active Directory 用户和计算机”控制台中。
    - 在控制台树中，右键单击“Contoso.com”，然后单击“属性”。
    - 在“**安全**”选项卡上，单击“**高级**”。
    - 在“Contoso 的高级安全设置”对话框中，单击“添加”。
    - 在“选择用户、计算机、服务帐户或组”对话框的“输入要选择的对象名称”框中，键入“mimcm-管理员”，然后单击“确定”。
    - 在“Contoso 的权限条目”对话框的“应用于”列表中，选择“下级用户对象”，然后为以下权限选中“允许”复选框：
        - 读取所有属性
        - 读取权限
        - FIM CM 审核
        - FIM CM 注册代理
        - FIM CM 请求注册
        - FIM CM 请求恢复
        - FIM CM 请求更新
        - FIM CM 请求撤销
        - FIM CM 请求取消阻止智能卡
    - 在“Contoso 的权限条目”对话框中，单击“确定”。
    - 在“Contoso 的高级安全设置”对话框中，单击“添加”。
    - 在“选择用户、计算机、服务帐户或组”对话框的“输入要选择的对象名称”框中，键入“mimcm-支持人员”，然后单击“确定”。
    - 在“Contoso 的权限条目”对话框的“应用于”列表中，选择“下级用户对象”，然后为以下权限选中“允许”复选框：- 读取所有属性 - 读取权限 - FIM CM 注册代理 - FIM CM 请求撤销 - FIM CM 请求取消阻止智能卡
    - 在“Contoso 的权限条目”对话框中，单击“确定”。
    - 在“Contoso 的高级安全设置”对话框中，单击“确定” 。
    - 在“contoso.com 属性”对话框中，单击“确定”。
    - 使“Active Directory 用户和计算机”处于打开状态。

    - 配置下级用户对象上的权限\<无脚本\>
        - 确保仍位于“Active Directory 用户和计算机”控制台中。
        - 在控制台树中，右键单击“Contoso.com”，然后单击“属性”。
        - 在“**安全**”选项卡上，单击“**高级**”。
        - 在“Contoso 的高级安全设置”对话框中，单击“添加”。
        - 在“选择用户、计算机、服务帐户或组”对话框的“输入要选择的对象名称”框中，键入“mimcm-管理员”，然后单击“确定”。
        - 在“CONTOSO 的权限条目”对话框的“应用于”列表中，选择“下级用户对象”，然后为以下权限选中“允许”复选框：
            - 读取所有属性
            - 读取权限
            - FIM CM 审核
            - FIM CM 注册代理
            - FIM CM 请求注册
            - FIM CM 请求恢复
            - FIM CM 请求更新
            - FIM CM 请求撤销
            - FIM CM 请求取消阻止智能卡
    - 在“CONTOSO 的权限条目”对话框中，单击“确定”。
    - 在“CONTOSO 的高级安全设置”对话框中，单击“添加”。
    - 在“选择用户、计算机、服务帐户或组”对话框的“输入要选择的对象名称”框中，键入“mimcm-支持人员”，然后单击“确定”。
    - 在“CONTOSO 的权限条目”对话框的“应用于”列表中，选择“下级用户对象”，然后为以下权限选中“允许”复选框：- 读取所有属性 - 读取权限 - FIM CM 注册代理 - FIM CM 请求撤销 - FIM CM 请求取消阻止智能卡
    - 在“contoso 的权限条目”对话框中，单击“确定”。
    - -在“Contoso 的高级安全设置”对话框中，单击“确定”。
    - 在“contoso.com 属性”对话框中，单击“确定”。
    - 使“Active Directory 用户和计算机”处于打开状态。
- 第二个步骤：委派证书模板管理权限\<脚本\>
    - 委派证书模板容器上的权限。
    - 委派 OID 容器上的权限。
    - 委派现有证书模板上的权限。
- 定义证书模板容器上的权限。
     1. 还原“Active Directory 站点和服务”控制台。
     2. 在控制台树中，依次展开“服务”、“公钥服务”，然后单击“证书模板”。
     3. 在控制台树中，右键单击“证书模板”，然后单击“委派控制”。
     4. 在“控制委派”向导中，单击“下一步”。
     5. 在“用户或组”页上，单击“添加”。
     6. 在“选择用户、计算机或组”对话框的“输入要选择的对象名称”框中，键入“mimcm-管理员”，然后单击“确定”。
     7. 在“用户或组”页上，单击“下一步”。
     8. 在“要委派的任务”页上，依次单击“创建要委托的自定义任务”、“下一步”。
     9.  在“Active Directory 对象类型”页上，确保选中“此文件夹、此文件夹中的现有对象和此文件夹中新对象的创建”，然后单击“下一步”。
     10. 在“权限”页的“权限”列表中，选中“完全控制”复选框，然后单击“下一步”。
     11. 在“完成控制委派向导”页上，单击“完成”。

- 定义 OID 容器上的权限。
     1. 在控制台树中，右键单击“OID”，然后单击“属性”。
     2. 在“OID 属性”对话框的“安全性”选项卡上，单击“高级”。
     3. 在“OID 的高级安全设置”对话框中，单击“添加”。
     4. 在“选择用户、计算机、服务帐户或组”对话框的“输入要选择的对象名称”框中，键入“mimcm-管理员”，然后单击“确定”。
     5. 在“OID 的权限条目”对话框中，确保权限应用于“这个对象及全部后代”，单击“完全控制”，然后单击“确定”。
     6. 在“OID 的高级安全设置”对话框中，单击“确定”。
     7. 在“OID 属性”对话框中，单击“确定”。
     8. 关闭“Active Directory 站点和服务”。

脚本：OID、配置文件模板和证书模板容器上的权限

![](media/mim-cm-deploy/image021.png)

```
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/en-us/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

脚本：委派现有证书模板上的权限。  

![](media/mim-cm-deploy/image039.png)  

dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
