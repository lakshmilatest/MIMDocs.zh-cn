---
title: 使用 Azure 多重身份验证服务器 SDK 激活 PAM 或 SSPR 方案 | Microsoft Docs
description: 设置 Azure 多重身份验证服务器 SDK，使其在用户激活 Privileged Access Management 和自助密码重置中的角色时作为第二安全层。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: b92a217dd86d9e4de177ebec9ecec7c76222d7b1
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358272"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>使用 Azure 多重身份验证服务器激活 PAM 或 SSPR
以下文档介绍如何设置 Azure MFA 服务器，使其在用户激活 Privileged Access Management 和自助密码重置中的角色时作为第二安全层。

> [!IMPORTANT]
> 由于 Azure 多重身份验证软件开发工具包的弃用通知。 在停用日期 2018 年 11 月 14 日前，现有客户仍可使用 Azure MFA SDK。 新客户和当前客户将无法再通过 Azure 经典门户下载 SDK。 若要下载，需要联系 Azure 客户支持，以便接收生成的 MFA 服务凭据包。 <br> Microsoft 开发团队正在通过与 Azure 多重身份验证服务器 SDK 集成来处理对 MFA 的更改。

下面这篇文章将概述发布后，从 Azure MFA SDK 到 Azure 多重身份验证服务器 SDK 的配置更新和针对简单切换进行启用的步骤，因为该内容将添加到即将推出的修补程序中，请参阅[版本历史记录](/reference/version-history.md)，了解相关公告。 

## <a name="prerequisites"></a>必备条件

要结合使用 MIM 和 Azure 多重身份验证服务器，需要：

- 每个提供 PAM 和 SSPR 的 MIM 服务或 MFA 服务器的 Internet 访问，用于联系 Azure MFA 服务
- Azure 订阅
- 安装已在使用 Azure MFA SDK
- 适用于候选用户的 Azure Active Directory Premium 许可证或授权 Azure MFA 的替换方法
- 适用于所有候选用户的电话号码
- MIM 修补程序 4.5. 或更高版本，请参阅[版本历史记录](/reference/version-history.md)，了解相关公告

## <a name="azure-multi-factor-authentication-server-configuration"></a>Azure 多重身份验证服务器配置 
> [!NOTE] 
> 在配置中，需要为 SDK 安装有效 SSL 证书。 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>步骤 1：从 Azure 门户下载 Azure 多重身份验证服务器 
登录 [Azure 门户](https://portal.azure.com/)并下载 Azure MFA 服务器。
![working-with-mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>步骤 2：生成激活凭据
使用“生成激活凭据以开始使用”链接，以生成激活凭据。 生成后，保存供稍后使用。

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>步骤 3：安装 Azure 多重身份验证服务器
下载服务器后，即可[安装](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server)它。  需要激活凭据。 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>步骤 4：创建将承载 SDK 的 IIS Web 应用程序
1. 打开 IIS 管理器 ![working-with-mfaserver-for-mim_iis.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  创建新的网站调用“MIM MFASDK”，将其链接到空白目录 ![working-with-mfaserver-for-mim_sdkweb.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. 打开多重身份验证控制台，然后单击 Web 服务 SDK ![working-with-mfaserver-for-mim_sdkinstall.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. 通过配置单击向导后，选择“MIM MFASDK”和应用池

> [!NOTE] 
> 向导将要求创建管理员组。 有关详细信息，请查看“Azure”>>“MFA Azure 多重身份验证服务器”文档。

5. 接下来，我们需要导入 MIM 服务帐户，打开多重身份验证控制台，选择“用户” a. 单击“从 Active Directory 导入” b. 导航到服务帐户 aka“contoso\mimservice” c. 单击“导入”和“关闭”![working-with-mfaserver-for-mim_importmimserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. 编辑 MIM 服务帐户以在多重身份验证控制台中启用 ![working-with-mfaserver-for-mim_enableserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. 在“MIM MFASDK”网站上更新 IIS 身份验证。 首先，我们将禁用“匿名身份验证”并启用“Windows 身份验证”![working-with-mfaserver-for-mim_iisconfig.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. 最后一步：将 MIM 服务帐户添加到“PhoneFactor 管理员”![working-with-mfaserver-for-mim_addservicetomfaadmin.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>为 Azure 多重身份验证服务器配置 MIM 服务 

### <a name="step-1-patch-server-to-452020"></a>步骤 1：将修补程序服务器升级到 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>步骤 2：备份并打开位于“C:\Program Files\Microsoft Forefront Identity Manager\2010\Service”中的 MfaSettings.xml

### <a name="step-3-update-the-following-lines"></a>步骤 3：更新以下行
1. 删除/清除以下配置条目行 <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD></CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. 更新或将以下各行添加到 MfaSettings.xml 中 <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. 重启 MIM 服务并测试 Azure 多重身份验证服务器的功能。

> [!NOTE] 
> 若要还原设置，请将 MfaSettings.xml 替换为步骤 2 中的备份文件


## <a name="next-steps"></a>后续步骤

-    [Azure 多重身份验证服务器入门](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [什么是 Azure 多重身份验证](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [使用自定义多重身份验证 API 激活 PAM 或 SSPR](Working-with-custommfaserver-for-mim.md)
- [MIM 版本发布历史记录](./reference/version-history.md)
