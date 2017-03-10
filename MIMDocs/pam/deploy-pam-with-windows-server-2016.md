---
title: "通过 Windows Server 2016 部署 MIM Privileged Access Management | Microsoft Docs"
description: "了解如何使用 Server 2016 部署 Privileged Access Management"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 02/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
translationtype: Human Translation
ms.sourcegitcommit: 18accbf24fc7af1a27e2e88059a9a8371dfd2c4d
ms.openlocfilehash: 49be7f3bd364e9202b177ead1fbe2607be91a323


---



# <a name="deploy-mim-pam-with-windows-server-2016"></a>通过 Windows Server 2016 部署 MIM PAM


通过此方案，MIM 2016 SP1 可将 Windows Server 2016 的功能用作“PRIV”林的域控制器。  配置此方案时，用户的 Kerberos 票证角色激活的剩余时间将受到限制。 

>[!Note]
Technical Preview 5 之前的 Windows Server 2016 早期技术预览版不可与此 MIM 版本一起使用。

## <a name="preparation"></a>准备

实验室环境至少需要两个 VM：

-   一个 VM 托管运行 Windows Server 2016 的 PRIV 域控制器

-   一个 VM 托管运行 Windows Server 2016（推荐）或 Windows Server 2012 R2 的 MIM 服务

>[!NOTE]
如果实验室环境中尚无“CORP”域，则需要为该域额外添加一个域控制器。 “CORP”域控制器可运行 Windows Server 2016 或 Windows Server 2012 R2。


执行[入门指南](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services.md)中所述的安装，**下述所示除外**：

-   如果正在新建 CORP 域，则在按照[步骤 1 - 准备 CORP 域控制器](/microsoft-identity-manager/pam/step-1-prepare-corp-domain.md)中的说明操作时，可选择根据需要将 CORP 域配置为位于 Windows Server 2016 功能级别上。 **若选择此选项，请进行以下调整**：

    -   如果使用 Windows Server 2016 介质，则安装选项将称为 Windows Server 2016（带桌面版体验的服务器）。

    -   可在 Install-ADDSForest 命令的参数中提供 7 作为域和林版本号，为 CORP 林和域指定 Windows Server 2016 功能级别，如下所示：
     ```
        Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
        ```
    -   在“新建用户和组”中，当 CORP 和 PRIV 域控制器是 Windows Server 2016 域功能级别时，**无需**最后一个命令 (New-ADGroup -name 'CONTOSO\$\$\$' …)。

    -   当 CORP 和 PRIV 域控制器是 Windows Server 2016 域功能级别时，“配置审核”（项目 #8）和“配置注册表设置”（项目 #10）中所述的更改是**推荐选项，但不是必需选项**。

-   如果选择使用 Windows Server 2012 R2 作为 CORPDC 的操作系统，则必须在 CORPDC 上安装修补程序 2919442、2919355[和更新 3155495](http://support.microsoft.com/kb/3156418)。

-   按照[步骤 2 - 准备 PRIV 域控制器](/microsoft-identity-manager/pam/step-2-prepare-priv-domain-controller.md)中的说明操作，下述调整除外：

    -   使用 Windows Server 2016 介质进行安装。 安装选项将称为 Windows Server 2016（带桌面版体验的服务器）。

    -   在“添加角色”说明（项目 #4）中，**必须在 PowerShell 命令的第 4 行中将域和林版本号指定为 7**，以便稍后所述的 Windows Server AD 功能可启用。

        ```
        Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
        ```  

    -   配置审核和登录权限时，请注意组策略管理项目将位于 Windows 管理工具文件夹中。

    -   **当 PRIV 域是 Windows Server 2016 域功能级别时，无需**配置 SID 历史记录迁移（项目 #8）所需的注册表设置。

    -   配置委派后且在重启服务器之前，请以管理员身份启动 PowerShell 窗口并键入以下命令，进而在 Windows Server 2016 Active Directory 中启用 Privileged Access Management 功能。

    ```
    $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
    Enable-ADOptionalFeature \$of -scope ForestOrConfigurationSet -target "priv.contoso.local"
    ```

  -   配置委派后且在重启服务器之前，请向 MIM 管理员和 MIM 服务帐户授予创建和更新卷影主体的权限。

     a. 启动 PowerShell 窗口并键入 ADSIEdit。

     b。 打开“操作”菜单并单击“连接到”。 在连接点设置上，将命名上下文从“默认命名上下文”更改为“配置”，然后单击“确定”。

     c. 连接后，在 ADSI 编辑器下方的窗口左侧，展开“配置”节点以查看“CN=Configuration,DC=priv,....”。 展开 CN=Configuration，然后展开 CN=Services。

     d. 右键单击“CN=Shadow Principal Configuration”，然后单击“属性”。 属性对话框出现时，更改为“安全性”选项卡。

     e. 单击“添加”。 指定帐户“MIMService”，并指定稍后将执行 New-PAMGroup 以创建其他 PAM 组的所有其他 MIM 管理员。 对于每位用户，请在允许的权限列表中添加“写入”、“创建所有子对象”和“删除所有子对象”。 添加权限。

     f. 更改为高级安全设置。 在允许 MIMService 访问的行上，单击“编辑”。 将“适用于”设置更改为“对此对象及所有后代”。 更新此权限设置并关闭“安全性”对话框。

     g. 关闭 ADSI 编辑器。

 -   配置委派后且在重启服务器之前，请向 MIM 管理员授予创建和更新身份验证策略的权限。

     a.  启动 Powershell 窗口并键入以下命令，将每一行（共&4; 行）中 MIM 管理员帐户的名称替换为“mimadmin”：
    ```
       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicy

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


-   按照[步骤 3 - 准备 PAM 服务器](/microsoft-identity-manager/pam/step-3-prepare-pam-server.md)中的说明操作并进行下述调整。

    -   若在 Windows Server 2016 上进行安装，请注意“ApplicationServer”角色不可用。

    -   若在 Windows Server 2016 上安装 MIM，**则不可安装 SharePoint 2013**。

-   按照[步骤 4 – 在 PAM 服务器和工作站上安装 MIM 组件](/microsoft-identity-manager/pam/step-4-install-mim-components-on-pam-server.md)中的说明操作并进行下述调整。

    -   安装 MIM 服务和 PAM 组件的用户**必须在 AD 中具有到 PRIV 域的写权限**，因为 MIM 安装将创建新的 AD OU“PAM 对象”。

    -   如果未安装 SharePoint，请不要安装 MIM 门户。

-   按照[步骤 5 - 建立信任](/microsoft-identity-manager/pam/step-5-establish-trust-between-priv-corp-forests.md)中的说明操作并进行下述调整：

    -   建立单向信任时，仅指定前两个 PowerShell 命令（get-credential 和 New-PAMTrust），请**不要执行 New-PAMDomainConfiguration 命令**。

    -   建立信任后，以 PRIV\\Administrator 身份登录到 PRIVDC，然后启动 PowerShell 并键入以下命令：
  ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1

     netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
     /usero:contoso\\administrator /passwordo:Pass\@word1  

     netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1
  ```

-   **当 CORP 和 PRIV 域均位于 Windows Server 2016 域功能级别上时，无需执行**项目 #5（验证信任）。

## <a name="more-information"></a>更多信息

- [Active Directory 域服务的 Privileged Access Management](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services.md)
- [为 Privileged Access Management 配置 MIM 环境](/microsoft-identity-manager/pam/configuring-mim-environment-for-pam.md)
- [使用脚本配置 PAM](/microsoft-identity-manager/pam/sp1-pam-configure-using-scripts.md)



<!--HONumber=Feb17_HO3-->


