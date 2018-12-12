---
title: 将 MIM 特定服务转换为 gMSA | Microsoft Docs
description: 描述配置 gMSA 的基本步骤的主题。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 96d375d82a71a21f0be444d628f387c4e1ffdd09
ms.sourcegitcommit: 9e420840815adb133ac014a8694de9af4d307815
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "52825767"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>将 MIM 特定服务转换为 gMSA

本指南将逐步调试为支持的服务配置 gMSA 的基本步骤。 一旦预配置了环境，转换为 gMSA 的过程将变得很简单。

所需修补程序：\<链接到最新的知识库 (KB)\>

支持：

-   MIM 同步服务 (FIMSynchronizationService)
-   MIM 服务 (FIMService)
-   MIM 密码注册
-   MIM 密码重置
-   PAM 监视服务 (PamMonitoringService)
-   PAM 组件服务 (PrivilegeManagementComponentService)

不支持：

-   MIM 门户不受支持，它是 SharePoint 环境的一部分，你需要在场模式下对其进行部署，还需要[在 SharePointServer 中配置自动密码更改](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   所有管理代理
-   Microsoft 证书管理
-   BHOLD

<a name="general-information"></a>常规信息 
--------------------

需要阅读来完成设置并理解

-   [组托管服务帐户概述](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Windows 域控制器上的第一个步骤

1.  如果需要，创建密钥分发服务 (KDS) 根密钥（每个域仅一次）。 DC 上的 KDS 服务使用根密钥（以及其他信息）来生成密码。

    -   Add-KDSRootKey –EffectiveImmediately

    -   “–EffectiveImmediately”指最多等待 \~10 小时/需要复制到所有 DC。 对于两个域控制器来说，这大约花费 1 小时。

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>同步服务
-----------------------

1.  创建名为“MIMSync_Servers”的组并将所有同步服务器添加到此组。

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  然后从 Windows PowerShell 执行以下命令，因为具有计算机帐户的域管理员已经加入了域

    -   New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   获取 GSMA 的详情以进行同步：

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   如果运行的是 PCNS 服务，则需要更新代理

    -   Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. 接下来在同步服务上，请务必备份加密密钥，因为在更改模式安装时将需要此密钥

    -   在安装了同步服务的服务器上，查找同步服务密钥管理工具

    -   默认情况下，已选择“导出密钥集” 

    -   单击“下一步”

    -   现在将提示你输入现有同步帐户信息

    -   输入并验证 FIM 同步帐户信息

        -   帐户名称 - 初始安装期间使用的同步服务帐户的帐户名称

        -   密码 - 同步服务帐户的密码

        -   域 - 同步服务帐户所属的域

    -   单击“下一步”

    -   如果输入的信息不正确，你将收到以下错误

    -   现在你已成功输入帐户信息，因此可以选择更改备份加密密钥的目标位置（导出文件位置）

        -   默认情况下，导出文件位置为 C:\\Windows\\system32\\miiskeys-1.bin ****。

4. 安装 Microsoft Identity Manager SP1 同步服务内部版本 4.4.1302.0。 可在批量许可证下载中心或 MSDN 下载网站上找到。 安装完成后，保存键集 miiskeys.bin。

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. 安装最新的[修补程序 4.5.x.x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) 或更高版本。

- 修补后，停止 FIM 同步服务。
- 控制面板程序和功能 Microsoft Identity Manager
- 同步服务更改 -\> 下一步 -\> 配置 -\> 下一步

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  清除帐户名称
-  键入服务帐户名称“MIMSyncGMSA”（带屏幕截图上的 \$ 符号）
- 。 密码留空。

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- 下一步 -\> 下一步 -\> 安装
- 从保存的 .bin 文件还原键集。

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> 添加的 SQL 权限是为登录创建名的帐户，因此必须允许应用更改模式权限的用户在同步服务数据库上添加帐户和 dbo。

## <a name="mim-service"></a>MIM 服务
-----------

>[!IMPORTANT]
>首次将 MIM 服务相关帐户转换为 gMSA 帐户时，必须使用以下过程。 附录中提到的 PowerShell cmdlet 仅可用于在完成初始配置后更改帐户信息。*

1.  为 MIM 服务、PAM Rest API、PAM 监视服务、PAM 组件服务、SSPR 注册门户、SSPR 重置门户创建组托管帐户。

    -   确保更新 gMSA 委派和 SPN
        -   Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames \@{Add="\<SPN\>"}
        -   委托
            -   Set-ADServiceAccount -Identity \<gsmaaccount\> -TrustedForDelegation \$true
        -   约束的委派
            -   \$delspns = 'http/mim', 'http/mim.contoso.com'
            -   New-ADServiceAccount -Name \<gsmaaccount\> -DNSHostName \<gsmaaccount\>.contoso.com -PrincipalsAllowedToRetrieveManagedPassword \<group\> -ServicePrincipalNames \$spns -OtherAttributes \@{'msDS-AllowedToDelegateTo'=\$delspns }

2.  在同步组中添加 MIM 服务的帐户。 这对于 SSPR 来说是必要的。

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **注意**：  由于重启 Windows 后，Microsoft 密钥分发服务未启动，使用托管帐户的服务在重启服务器后挂起的已知问题。 无法启动此服务，也无法重启 Windows。 至少在 Windows Server 2012 R2 上，这个问题是可复现的。 此问题的解决方法为运行命令 

-   **sc triggerinfo kdssvc start/networkon**

    在连接网络时（通常在启动循环早期），启动 Microsoft 密钥分发服务。

    请参阅关于类似问题的讨论：<https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  运行 MIM 服务的提升 MSI 并选择更改。

5.  在“配置主服务器连接页面”上，勾选“为 Exchange (托管帐户)使用不同的帐户”复选框。 此处，你可以选择使用具有邮箱的旧帐户或使用云邮箱。

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  在“配置 MIM 服务帐户”页面，键入末尾为 \$ 符号的帐户。 同时键入服务电子邮件帐户密码。 应禁用服务帐户密码。

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  因为 LogonUser 函数不适用于托管帐户，“下一步”页面将显示“请检查服务帐户在当前配置下是否安全”警告。

![cid:image007.png\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  在“配置 Privileged Access Management REST API”页面，键入末尾为 \$ 符号的应用程序池帐户名称并将“密码”字段保留为空。

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  在“配置 PAM 组件服务”页面，键入末尾为 \$ 符号的服务帐户并将“密码”字段保留为空。

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![cid:image010.png\@01D36EB8.A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  在“配置 Privileged Access Management 监视服务”页面，键入末尾为 \$ 符号的服务帐户并将“密码”字段保留为空。

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  在“配置 MIM 密码注册门户”页面，键入末尾为 \$ 符号的帐户名称并将“密码”字段保留为空。

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  在“配置 MIM 密码重置门户”页面，键入末尾为 \$ 符号的帐户名称并将“密码”字段保留为空。

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  完成安装。

注意：

-  安装后，在注册表中创建两个新密钥，路径如下：
    - “HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Forefront Identity
    - Manager\\2010\\Service”，用于存储加密的 Exchange 密码。 一个用于
    - Exchange Online，另一个用于 Exchange 本地（其中一个应为
    - 空）。

![cid:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- 为更新密码，我们提供了脚本（[单击此处下载](microsoft-identity-manager-2016-gmsascript.md)）以便客户不必运行更改模式

- 要加密 Exchange 密码，安装程序会创建其他服务并
    - 在托管帐户下运行。 安装期间，以下消息将被添加到
    - 应用程序事件日志。

![cid:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
