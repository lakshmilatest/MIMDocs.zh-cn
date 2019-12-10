---
title: 使用自助密码重置 | Microsoft Docs
description: 请参阅 MIM 2016 中有关自助服务密码重置的新增内容，包括 SSPR 如何使用多重身份验证的相关内容。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/11/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 90452391170114270765e9a7fe08e98eea0747e4
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "67690693"
---
# <a name="self-service-password-reset-deployment-options"></a>自助密码重置部署选项

对于获得 [Azure Active Directory Premium 许可](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing)的新客户，建议使用 [Azure AD 自助密码重置](/azure/active-directory/authentication/concept-sspr-howitworks)，以提供最终用户体验。  Azure AD 自助密码重置可同时为用户提供基于 Web 和 Windows 集成的体验（用于重置其自己的密码），并支持多种与 MIM 相同的功能，包括备用电子邮件和问答入口。  部署 Azure AD 自助密码重置时，Azure AD Connect 支持[将新密码写回 AD DS](/azure/active-directory/authentication/concept-sspr-writeback)，并且 MIM [密码变更通知服务](deploying-mim-password-change-notification-service-on-domain-controller.md)还可用于向其他系统（例如另一供应商的目录服务器）转发该密码。  部署用于[密码管理](infrastructure/mim2016-password-management.md)的 MIM 不需要部署 MIM 服务或 MIM 自助密码重置或注册门户。  相反，可以执行以下步骤：

- 首先，如果需要向 Azure AD 和 AD DS 之外的目录发送密码，请将与连接器的 MIM 同步部署到 Active Directory 域服务和其他任何目标系统，配置用于[密码管理](infrastructure/mim2016-password-management.md)的 MIM，并部署[密码变更通知服务](deploying-mim-password-change-notification-service-on-domain-controller.md)。
- 其次，如果需要向 Azure AD 之外的目录发送密码，请配置用于[将新密码写回 AD DS](/azure/active-directory/authentication/concept-sspr-writeback)的 Azure AD Connect。
- （可选）[预注册用户](/azure/active-directory/authentication/howto-sspr-authenticationdata)。
- 最后，[向最终用户推广 Azure AD 自助密码重置](/azure/active-directory/authentication/howto-sspr-deployment)。

对于以前部署过用于自助密码重置的 Forefront Identity Manager (FIM) 并获得 Azure Active Directory Premium 授权的现有客户，建议计划过渡到 Azure AD 自助密码重置。  可以将最终用户过渡到 Azure AD 自助密码重置，而无需重新注册，方法是[通过 PowerShell 同步或设置用户的备用电子邮件地址或移动电话号码](/azure/active-directory/authentication/howto-sspr-authenticationdata)。 用户注册 Azure AD 自助密码重置后，FIM 密码重置门户即可停止使用。

对于尚未为其用户部署 Azure AD 自助密码重置的客户，MIM 还提供自助密码重置门户。  与 FIM 相比，MIM 2016 包含以下更改：

- MIM 自助密码重置门户和 Windows 登录屏幕允许用户解锁其帐户，而无需更改密码。
- 向 MIM 添加了新的身份验证入口 - 电话入口。 这能通过 Microsoft Azure 多重身份验证 (MFA) 服务使用电话呼叫实现用户身份验证。

从 MIM 2016 发布版到版本 4.5.26.0，需要依赖客户下载 Azure 多重身份验证软件开发工具包 (Azure MFA SDK)。  该 SDK 已停止使用，并且对现有客户的 Azure MFA SDK 支持将于 2018 年 11 月 14 日结束。 该日期之前，客户必须联系 Azure 客户支持，以便接收生成的 MFA 服务凭据包，因为他们将无法下载 Azure MFA SDK。 

#### <a name="new-update-current-azure-mfa-configuration-to-azure-multi-factor-authentication-server"></a>新内容！ 将当前的 Azure MFA 配置更新到 Azure 多重身份验证服务器

[本文](working-with-mfaserver-for-mim.md)介绍如何更新部署 MIM 自助密码重置门户和 PAM 配置，如何将 Azure 多重身份验证服务器用于多重身份验证。

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>使用用于多重身份验证的 Azure MFA 部署 MIM 自助密码重置门户

下一部分介绍如何部署 MIM 自助密码重置门户，将 Azure MFA 用于多重身份验证。  对于未针对其用户使用 Azure AD 自助密码重置的客户，这些步骤才是有必需的。

Windows Azure 多重身份验证是一种身份验证服务，该服务要求用户使用移动应用、电话呼叫或短信来验证其登录尝试。 它可与 Microsoft Azure Active Directory 搭配使用，并且作为一项适用于云和本地企业应用程序的服务。

Azure MFA 提供了额外的身份验证机制，可加强现有的身份验证过程，例如由 MIM 执行用于自助服务登录帮助的过程。

使用 Azure MFA 时，用户通过系统进行身份验证，以便在尝试重新获取对其帐户和资源的访问权限时验证他们的身份。 可以通过短信或电话呼叫进行身份验证。   身份验证越强，尝试获得访问权限的人员确实是拥有该身份的真实用户的可信性就越高。 通过身份验证后，用户可以选择要替换旧密码的新密码。

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>使用 MFA 设置自助服务帐户解锁和密码重置的先决条件

本部分假定你已下载并完成部署 Microsoft 标识管理器 2016 [MIM 同步、MIM 服务和 MIM 门户组件](microsoft-identity-manager-deploy.md)，包括以下组件和服务：

-   Windows Server 2008 R2 或更高版本已设置为 Active Directory 服务器，包括 AD 域服务和附带指定域（“corporate”域）的域控制器

-   定义帐户锁定的组策略

-   MIM 2016 同步服务 (Sync) 安装在已加入 AD 域的服务器上并在其上运行

-   已在服务器上安装并运行 MIM 2016 服务&amp;门户，包括 SSPR 注册门户和 SSPR 重置门户（可以和 Sync 一起位于同一服务器上）

-   为 AD-MIM 标识同步配置 MIM Sync，包括：

    -   配置 Active Directory 管理代理 (ADMA) 以便连接到 AD DS 和能够导入标识数据并将其导出到 Active Directory。

    -   配置 MIM 管理代理 (MIM MA) 以便连接到 FIM 服务数据库和能够导入标识数据并将其导出到 FIM 数据库。

    -   在 MIM 门户中配置同步规则，以允许用户数据同步并促进 MIM 服务中基于同步的活动。

-   在服务器或单独的客户端计算机上部署了 MIM 2016 加载项&amp;扩展，包括集成了 SSPR Windows 登录的客户端。

此方案要求你的用户具有 MIM CAL，以及 Azure MFA 订阅。

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>准备 MIM 以使用多重身份验证
配置 MIM 同步以支持密码重置和帐户解锁功能。 有关详细信息，请参阅[安装 FIM 外接程序和扩展](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx)、[安装 FIM SSPR](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx)、[SSPR 身份验证入口](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx)和 [SSPR 测试实验指南](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

在下一部分中，将在 Microsoft Azure Active Directory 中设置 Azure MFA 提供程序。 作为该过程的一部分，将生成一个包含的身份验证材料的文件，MFA 需要该材料才能联系 Azure MFA。  若要继续，将需要 Azure 订阅。

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>在 Azure 中注册多重身份验证提供程序

1.  创建 [MFA 提供程序](/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)。

2. 建立支持案例并请求适用于 ASP.net 2.0 C# 的 Direct SDK。 由于 Direct SDK 已遭弃用，因此，SDK 将仅提供给 MIM（具有 MFA）的当前用户。 新客户应采用将与 MFA 服务器集成的下一个 MIM 版本。

3. 将生成的 ZIP 文件复制到安装了 MIM 服务的每个系统。  请注意，该 ZIP 文件包含用于对 Azure MFA 服务进行身份验证的密钥材料。

### <a name="update-the-configuration-file"></a>更新配置文件

1. 作为安装 MIM 的用户登录到已安装 MIM 服务的计算机。

2. 在安装 MIM 服务的目录下创建新目录文件夹，如 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**。

3. 使用 Windows 资源管理器，导航到在上一部分中下载的 ZIP 文件的 **\pf\certs** 文件夹并将文件 **cert_key.p12** 复制到新目录。

4.  在 SDK zip 文件的 **\pf** 文件夹中，打开文件 **pf_auth.cs**。

5.  找到以下三个参数： `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`。

    ![pf_auth.cs 代码图像](media/MIM-SSPR-pFile.png)

6.  在“C:\Program Files\Microsoft Forefront Identity Manager\2010\Service”中，打开以下文件  ：MfaSettings.xml。 

7.  将 pf_aut.cs 文件中 `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` 参数的值复制到它们各自在 MfaSettings.xml 文件中的 xml 元素中。

8.  在 SDK zip 文件的 \pf\certs 下，提取文件 **cert_key.p12** ，并将其在 MfaSettings.xml 文件中的完整路径输入到 `<CertFilePath>` xml 元素中。

9. 在 `<username>` 元素中输入任意用户名。

10. 在 `<DefaultCountryCode>` 元素中，输入默认国家/地区代码。 如果用户注册的电话号码未附带国家/地区代码，这是他们将得到的国家/地区代码。 如果用户有一个国际国家/地区代码，它必须包含在已注册的电话号码中。

11. 使用同一位置中的相同名称保存 MfaSettings.xml 文件。

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>配置电话入口或一次性密码 SMS 入口

1.  启动 Internet Explorer 并导航到 MIM 门户，作为 MIM 管理员进行身份验证，然后单击左侧导航栏中的  **工作流** 。

    ![MIM 门户导航图像](media/MIM-SSPR-workflow.jpg)

2.  选中“密码重置身份验证工作流”  。

    ![MIM 门户工作流图像](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  单击“活动”  选项卡，然后向下滚动到“添加活动”  。

4.  选择“电话入口”  或“一次性密码 SMS 入口”  ，单击“选择”  ，然后单击“确定”  。

注意：如果使用 Azure MFA 服务器或另一个本身生成一次性密码的提供程序，请确保上面配置的长度字段与 MFA 提供程序生成的长度字段长度相同。  对于 Azure MFA 服务器，长度为 6。  Azure MFA 服务器还会生成自己的消息文本，因此将忽略短信。

你的组织中的用户现在可以注册以进行密码重置。  在此过程中，他们将输入其电话号码或手机号码，这样系统就知道如何呼叫他们（或向他们发送 SMS 消息）。

#### <a name="register-users-for-password-reset"></a>注册用户以进行密码重置

1.  用户将启动 Web 浏览器，导航到 MIM 密码重置注册门户。  （通常此门户将使用 Windows 身份验证配置）。  在该门户中，他们将再次提供其用户名和密码以确认其身份。

    他们需要进入密码注册门户，并使用其用户名和密码进行身份验证。

2.  在“电话号码”  或“移动电话”   字段中，他们必须输入国家/地区代码、一个空格和电话号码，然后单击“下一步”  。

    ![MIM 电话验证图像](media/MIM-SSPR-PhoneVerification.JPG)

    ![MIM 移动电话验证图像](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>它是如何为用户提供服务的？
现在，所有内容均已配置并且正在运行，你可能想要知道用户在度假前重置了密码，但在回来后却意识到已经完全忘记了密码将经历哪些过程。

用户可以通过两种方法使用密码重置和帐户解锁功能：Windows 登录屏幕或自助服务门户。

通过在通过你组织的网络连接到 MIM 服务的加入域的计算机上安装 MIM 外接和扩展插件，用户可以在桌面登录时恢复忘记的密码。  以下步骤将引导你完成此过程。

#### <a name="windows-desktop-login-integrated-password-reset"></a>集成了 Windows 桌面登录的密码重置

1.  如果用户在登录屏幕中数次输入了错误的密码，他们将可以选择单击“登录时遇到问题？”  。

    ![登录屏幕图像](media/MIM-SSPR-problemsloggingin.JPG)

    单击此链接会将他们带入 MIM 密码重置屏幕，在该处他们可以更改其密码或解锁其帐户。

    ![MIM 密码重置图像](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  将引导用户进行身份验证。 如果已配置 MFA，用户将接到一个电话。

3.  然后 Azure MFA 将在后台拨打用户在注册该服务时提供的电话号码。

4.  当用户接听电话时，他们将需要按下手机上的井号键 #。 然后，用户在门户中单击“下一步”  。

    如果你还设置了其他入口，用户将需要在后续屏幕中提供更多信息。

    > [!NOTE]
    > 如果用户缺乏耐心，并且未按井号键 # 就单击“下一步”  ，身份验证将失败。

5.  身份验证成功后，将提供给用户两个选项：解锁帐户并保持当前密码，或设置一个新密码。

6.  然后，用户需要输入新密码两次才重置密码。

#### <a name="access-from-the-self-service-portal"></a>从自助服务门户访问

1.  用户可以打开 Web 浏览器，导航到“密码重置门户”  并输入其用户名，然后单击“下一步”  。

    如果已配置 MFA，用户将接到一个电话。 然后 Azure MFA 将在后台拨打用户在注册该服务时提供的电话号码。

    当用户接听电话时，他将需要按下手机上的井号键 #。 然后，用户在门户中单击“下一步”  。

2.  如果你还设置了其他入口，用户将需要在后续屏幕中提供更多信息。

    > [!NOTE]
    > 如果用户缺乏耐心，并且未按井号键 # 就单击“下一步”  ，身份验证将失败。

3.  用户必须对以下两个选项做出选择：重置密码或解锁帐户。 如果用户选择解锁帐户，该帐户将解除锁定。

    ![MIM 登录助手帐户解锁图像](media/MIM-SSPR-accountUnlock.JPG)

4.  身份验证成功后，将提供给用户两个选项：保持其当前密码或设置一个新密码。

5.  ![MIM 帐
6.  户解锁成功图像](media/MIM-SSPR-account-unlock.JPG)

6.  如果用户选择重置其密码，他们将需要键入新密码两次，并单击“下一步”  以更改密码。
