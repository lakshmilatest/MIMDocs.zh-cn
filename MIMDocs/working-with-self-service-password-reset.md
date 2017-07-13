---
title: "使用自助服务密码重置门户 | Microsoft Docs"
description: "请参阅 MIM 2016 中有关自助服务密码重置的新增内容，包括 SSPR 如何使用多重身份验证的相关内容。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: bbcfee70c71bc2f1a637327721d015434600c5e0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# 使用自助服务密码重置
<a id="working-with-self-service-password-reset" class="xliff"></a>
Microsoft 标识管理器 2016 将附加功能提供给自助服务密码重置功能。 此功能的几个重要功能已得到增强：

-   自助服务密码重置门户和 Windows 登录屏幕现在允许用户解锁其帐户，而无需更改密码或呼叫支持管理员。 用户会因各种合理原因导致他们的帐户遭锁定，例如，输入旧密码，使用双语计算机并将键盘设置为错误的语言，或试图登录到已向他人的帐户开放的共享工作站。

-   添加了新的身份验证入口 - 电话入口。 这允许通过电话呼叫进行用户身份验证。

-   已为 Microsoft Azure 多重身份验证 (MFA) 服务添加了此支持。 这可以用于现有的 SMS 一次性密码入口或新的电话入口。

## Azure 多重身份验证
<a id="azure-for-multi-factor-authentication" class="xliff"></a>
Windows Azure 多重身份验证是一种身份验证服务，该服务要求用户使用移动应用、电话呼叫或短信来验证其登录尝试。 它可与 Microsoft Azure Active Directory 搭配使用，并且作为一项适用于云和本地企业应用程序的服务。

Azure MFA 提供了额外的身份验证机制，可加强现有的身份验证过程，例如由 MIM 执行用于自助服务登录帮助的过程。

使用 Azure MFA 时，用户通过系统进行身份验证，以便在尝试重新获取对其帐户和资源的访问权限时验证他们的身份。 可以通过短信或电话呼叫进行身份验证。   身份验证越强，尝试获得访问权限的人员确实是拥有该身份的真实用户的可信性就越高。 通过身份验证后，用户可以选择要替换旧密码的新密码。

## 使用 MFA 设置自助服务帐户解锁和密码重置的先决条件
<a id="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa" class="xliff"></a>
本部分假定你已下载并完成部署 Microsoft 标识管理器 2016，包括以下组件和服务：

-   Windows Server 2008 R2 或更高版本已设置为 Active Directory 服务器，包括 AD 域服务和附带指定域（“corporate”域）的域控制器

-   定义帐户锁定的组策略

-   MIM 2016 同步服务 (Sync) 安装在已加入 AD 域的服务器上并在其上运行

-   已在服务器上安装并运行 MIM 2016 服务&amp;门户，包括 SSPR 注册门户和 SSPR 重置门户（可以和 Sync 一起位于同一服务器上）

-   为 AD-MIM 标识同步配置 MIM Sync，包括：

    -   配置 Active Directory 管理代理 (ADMA) 以便连接到 AD DS 和能够导入标识数据并将其导出到 Active Directory。

    -   配置 MIM 管理代理 (MIM MA) 以便连接到 FIM 服务数据库和能够导入标识数据并将其导出到 FIM 数据库。

    -   在 MIM 门户中配置同步规则，以允许用户数据同步并促进 MIM 服务中基于同步的活动。

-   在服务器或单独的客户端计算机上部署了 MIM 2016 加载项&amp;扩展，包括集成了 SSPR Windows 登录的客户端。

## 准备 MIM 以使用多重身份验证
<a id="prepare-mim-to-work-with-multi-factor-authentication" class="xliff"></a>
配置 MIM 同步以支持密码重置和帐户解锁功能。 有关详细信息，请参阅[安装 FIM 外接程序和扩展](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx)、[安装 FIM SSPR](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx)、[SSPR 身份验证入口](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx)和 [SSPR 测试实验指南](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

在下一部分中，将在 Microsoft Azure Active Directory 中设置 Azure MFA 提供程序。 作为该过程的一部分，将生成一个包含的身份验证材料的文件，MFA 需要该材料才能联系 Azure MFA。  若要继续，将需要 Azure 订阅。

### 在 Azure 中注册多重身份验证提供程序
<a id="register-your-multi-factor-authentication-provider-in-azure" class="xliff"></a>

1.  转到 [Azure 经典门户](http://manage.windowsazure.com)并以 Azure 订阅管理员的身份登录。

2.  在左下角单击“新建” 。

3.  依次单击“应用服务”&gt;“Active Directory”&gt;“多重身份验证提供程序”&gt;“快速创建”。

![Azure 门户快速创建 MFA 图像](media/MIM-SSPR-Azureportal.png)

4.  在“名称”字段中输入“SSPRMFA”，然后单击“创建”。

![Azure 门户 MFA 图像](media/MIM-SSPR-Azureportal-2.png)

5.  在 Azure 经典门户菜单中单击“Active Directory”，然后单击“多重身份验证提供程序”选项卡。

6.  在屏幕底部单击“SSPRMFA”  ，然后单击“管理”  。

    ![Azure 门户管理图标](media/MIM-SSPR-ManageButton.png)

7.  在新窗口中，单击左面板上“配置”下的“设置”。

8.  在“欺诈警报” 下，取消选中“报告欺诈时阻止用户”  。 这样做是为了防止阻塞整个服务。

9. 在打开的“Azure 多重身份验证”  窗口中，单击左侧菜单的“下载”  下的“SDK”  。

10. 单击 ZIP 列中语言为**适用于 ASP.net 2.0 C# 的 SDK**的文件的“下载”链接。

    ![Azure MFA 下载 zip 文件图像](media/MIM-SSPR-Azure-MFA.png)

11. 将生成的 ZIP 文件复制到安装了 MIM 服务的每个系统。  请注意，该 ZIP 文件包含用于对 Azure MFA 服务进行身份验证的密钥材料。

### 更新配置文件
<a id="update-the-configuration-file" class="xliff"></a>

1. 作为安装 MIM 的用户登录到已安装 MIM 服务的计算机。

2. 在安装 MIM 服务的目录下创建新目录文件夹，如 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**。

3. 使用 Windows 资源管理器，导航到在上一部分中下载的 ZIP 文件的 **\pf\certs** 文件夹并将文件 **cert_key.p12** 复制到新目录。

4.  在 SDK zip 文件的 **\pf** 文件夹中，打开文件 **pf_auth.cs**。

5.  找到以下三个参数： `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`。

    ![pf_auth.cs 代码图像](media/MIM-SSPR-pFile.png)

6.  在 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**中打开该文件： **MfaSettings**.xml。

7.  将 pf_aut.cs 文件中 `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` 参数的值复制到它们各自在 MfaSettings.xml 文件中的 xml 元素中。

8.  在 SDK zip 文件的 \pf\certs 下，提取文件 **cert_key.p12** ，并将其在 MfaSettings.xml 文件中的完整路径输入到 `<CertFilePath>` xml 元素中。

9. 在 `<username>` 元素中输入任意用户名。

10. 在 `<DefaultCountryCode>` 元素中，输入默认国家/地区代码。 如果用户注册的电话号码未附带国家/地区代码，这是他们将得到的国家/地区代码。 如果用户有一个国际国家/地区代码，它必须包含在已注册的电话号码中。

11. 使用同一位置中的相同名称保存 MfaSettings.xml 文件。

#### 配置电话入口或一次性密码 SMS 入口
<a id="configure-the-phone-gate-or-the-one-time-password-sms-gate" class="xliff"></a>

1.  启动 Internet Explorer 并导航到 MIM 门户，作为 MIM 管理员进行身份验证，然后单击左侧导航栏中的  **工作流** 。

    ![MIM 门户导航图像](media/MIM-SSPR-workflow.jpg)

2.  选中“密码重置身份验证工作流” 。

    ![MIM 门户工作流图像](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  单击“活动”  选项卡，然后向下滚动到“添加活动” 。

4.  选择“电话入口” 或“一次性密码 SMS 入口”，单击“选择”，然后单击“确定”。

你的组织中的用户现在可以注册以进行密码重置。  在此过程中，他们将输入其电话号码或手机号码，这样系统就知道如何呼叫他们（或向他们发送 SMS 消息）。

#### 注册用户以进行密码重置
<a id="register-users-for-password-reset" class="xliff"></a>

1.  用户将启动 Web 浏览器，导航到 MIM 密码重置注册门户。  （通常此门户将使用 Windows 身份验证配置）。  在该门户中，他们将再次提供其用户名和密码以确认其身份。

    他们需要进入密码注册门户，并使用其用户名和密码进行身份验证。

2.  在“电话号码”  或“移动电话”   字段中，他们必须输入国家/地区代码、一个空格和电话号码，然后单击“下一步” 。

    ![MIM 电话验证图像](media/MIM-SSPR-PhoneVerification.JPG)

    ![MIM 移动电话验证图像](media/MIM-SSPR-mobilephoneverification.JPG)

## 它是如何为用户提供服务的？
<a id="how-does-it-work-for-your-users" class="xliff"></a>
现在，所有内容均已配置并且正在运行，你可能想要知道用户在度假前重置了密码，但在回来后却意识到已经完全忘记了密码将经历哪些过程。

用户可以通过两种方法使用密码重置和帐户解锁功能：Windows 登录屏幕或自助服务门户。

通过在通过你组织的网络连接到 MIM 服务的加入域的计算机上安装 MIM 外接和扩展插件，用户可以在桌面登录时恢复忘记的密码。  以下步骤将引导你完成此过程。

#### 集成了 Windows 桌面登录的密码重置
<a id="windows-desktop-login-integrated-password-reset" class="xliff"></a>

1.  如果用户在登录屏幕中数次输入了错误的密码，他们将可以选择单击“登录时遇到问题？” 。

    ![登录屏幕图像](media/MIM-SSPR-problemsloggingin.JPG)

    单击此链接会将他们带入 MIM 密码重置屏幕，在该处他们可以更改其密码或解锁其帐户。

    ![MIM 密码重置图像](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  将引导用户进行身份验证。 如果已配置 MFA，用户将接到一个电话。

3.  然后 Azure MFA 将在后台拨打用户在注册该服务时提供的电话号码。

4.  当用户接听电话时，他们将需要按下手机上的井号键 #。 然后，用户在门户中单击“下一步”  。

    如果你还设置了其他入口，用户将需要在后续屏幕中提供更多信息。

    > [!NOTE]
    > 如果用户缺乏耐心，并且未按井号键 # 就单击“下一步”，身份验证将失败。

5.  身份验证成功后，将提供给用户两个选项：解锁帐户并保持当前密码，或设置一个新密码。

6.  然后，用户需要输入新密码两次才重置密码。

#### 从自助服务门户访问
<a id="access-from-the-self-service-portal" class="xliff"></a>

1.  用户可以打开 Web 浏览器，导航到“密码重置门户”  并输入其用户名，然后单击“下一步” 。

    如果已配置 MFA，用户将接到一个电话。 然后 Azure MFA 将在后台拨打用户在注册该服务时提供的电话号码。

    当用户接听电话时，他将需要按下手机上的井号键 #。 然后，用户在门户中单击“下一步”  。

2.  如果你还设置了其他入口，用户将需要在后续屏幕中提供更多信息。

    > [!NOTE]
    > 如果用户缺乏耐心，并且未按井号键 # 就单击“下一步”，身份验证将失败。

3.  用户必须对以下两个选项做出选择：重置密码或解锁帐户。 如果用户选择解锁帐户，该帐户将解除锁定。

    ![MIM 登录助手帐户解锁图像](media/MIM-SSPR-accountUnlock.JPG)

4.  身份验证成功后，将提供给用户两个选项：保持其当前密码或设置一个新密码。

5.  ![MIM 账户解锁成功图像](media/MIM-SSPR-account-unlock.JPG)

6.  如果用户选择重置其密码，他们将需要键入新密码两次，并单击“下一步”  以更改密码。

    ![MIM 登录助手密码重置图像](media/MIM-SSPR-PR1.JPG)
