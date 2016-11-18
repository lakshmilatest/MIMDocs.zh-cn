---
title: "使用 Azure MFA 激活 PAM | Microsoft Docs"
description: "当你的用户激活 Privileged Access Management 中的角色时，将 Azure MFA 设置为安全性的第二层。"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: fa6d69038e5b2f0b933773381661929159198242


---

# <a name="using-azure-mfa-for-activation"></a>将 Azure MFA 用于激活
在配置 PAM 角色时，你可以选择对请求激活角色的用户进行授权的方式。 PAM 授权活动实现的选项有：

- 角色所有者批准
- Azure 多重身份验证 (MFA)

如果不启用检查，将自动为候选用户角色激活候选用户。

Microsoft Azure 多因素身份验证 (MFA) 是要求用户通过使用移动应用、电话呼叫或短信验证其登录尝试的身份验证服务。 它可与 Microsoft Azure Active Directory 搭配使用，并且作为一项适用于云和本地企业应用程序的服务。 对于 PAM 方案，不管候选用户先前如何对 Windows PRIV 域进行身份验证，Azure MFA 都提供可以在授权下使用的其他身份验证机制。

## <a name="prerequisites"></a>先决条件

若要将 Azure MFA 与 MIM 一起使用，你将需要：

- 每项提供 PAM 的 MIM 服务的 Internet 访问，用于联系 Azure MFA 服务
- Azure 订阅
- 适用于候选用户的 Azure Active Directory Premium 许可证或授权 Azure MFA 的替换方法
- 适用于所有候选用户的电话号码

## <a name="creating-an-azure-mfa-provider"></a>创建 Azure MFA 提供程序

在此部分中，将在 Microsoft Azure Active Directory 中设置 Azure MFA 提供程序。  如果你已在使用 Azure MFA，无论它们是单机配置还是通过 Azure Active Directory Premium 配置，请跳至下一节。

1.  打开 Web 浏览器，以 Azure 订阅管理员的身份连接到 [Azure 经典门户](https://manage.windowsazure.com)。

2.  在左下角单击“新建” 。

3.  依次单击“应用服务”>“Active Directory”>“多因素身份验证提供程序”>“快速创建”。

4.  在“名称”  字段中，输入 **PAM**，并在“使用模型”字段中，选择“每个已启用用户”。 如果你已具有 Azure AD 目录，则选择该目录。 最后，单击“创建” 。

## <a name="downloading-the-azure-mfa-service-credentials"></a>下载 Azure MFA 服务凭据

接下来，将生成一个包含身份验证材料的文件，PAM 需要该材料才能联系 Azure MFA。

1. 打开 Web 浏览器，以 Azure 订阅管理员的身份连接到 [Azure 经典门户](https://manage.windowsazure.com)。

2.  在 Azure 门户菜单中单击“Active Directory”  ，然后单击“多重身份验证提供程序”  选项卡。

3.  单击你将用于 PAM 的“Azure MFA 提供程序”，然后单击“管理” 。

4.  在新窗口中，单击左侧面板上“配置” 下的“设置” 。

5.  在“Azure 多因素身份验证”  窗口中，单击“下载”  下的“SDK” 。

6.  单击 ZIP 列中语言为 **ASP.net 2.0 C\#** 的 SDK 文件的“下载”链接。

![下载 Multi-Factor Authentication SDK - 屏幕截图](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  将生成的 ZIP 文件复制到安装了 MIM 服务的每个系统。 

>[!NOTE]
> 该 ZIP 文件包含用于对 Azure MFA 服务进行身份验证的密钥材料。

## <a name="configuring-the-mim-service-for-azure-mfa"></a>配置适用于 Azure MFA 的 MIM 服务

1.  以管理员或者安装了 MIM 的用户的身份登录到安装了 MIM 服务的计算机。

2.  在安装了 MIM 服务的目录下面创建新的目录文件夹，例如 `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service\\MfaCerts`。

3.  使用 Windows 资源管理器，导航到在上一部分中下载的 ZIP 文件的 **pf\\certs** 文件夹，并将文件 **cert\_key.p12** 复制到新目录。

4.  使用 Windows 资源管理器，导航到 ZIP 的 **pf** 文件夹，并在文本编辑器（如 Wordpad）中打开文件 **pf\_auth.cs**。

5.  找到以下三个参数：**LICENSE\_KEY**、**GROUP\_KEY**、**CERT\_PASSWORD**。

![复制 pf\_auth.cs 文件中的值 - 屏幕截图](media/PAM-Azure-MFA-Activation-Image-2.png)

6.  使用记事本打开位于 `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service` 中的 **MfaSettings.xml** 文件。

7.  将 pf\_auth.cs 文件中的 LICENSE\_KEY、GROUP\_KEY 和 CERT\_PASSWORD 参数的值复制到它们在 MfaSettings.xml 文件中的各自 xml 元素中。

8.  在 **<CertFilePath>** XML 元素中，指定之前提取的 cert\_key.p12 文件的完整路径名称。

9.  在 **<username>** 元素中输入任意用户名。

10.  在 **<DefaultCountryCode>** 元素中，输入你要拨打的用户的国家/地区代码，如 1 代表美国和加拿大。 在使用无国家/地区代码的电话号码注册用户的情况下使用此值。 如果用户的电话号码具有区别于为组织配置的国际国家/地区代码，则该代码必须包含在将注册的电话号码中。

11.  保存并覆盖 MIM 服务文件夹 `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service` 中的 **MfaSettings.xml** 文件。 

> [!NOTE]
> 在该过程结束时，请确保文件 **MfaSettings.xml** 及其任何副本或 ZIP 文件均不可公开读取。

## <a name="configure-pam-users-for-azure-mfa"></a>为 Azure MFA 配置 PAM 用户

对于要激活需要 Azure MFA 的角色的用户，其电话号码必须存储在 MIM 中。 有两种方式设置此属性。

首先，`New-PAMUser` 命令将 CORP 域中用户的目录条目的电话号码属性复制到 MIM 服务数据库。 请注意这是一次性操作。

其次，`Set-PAMUser` 命令将更新 MIM 服务数据库中的电话号码属性。 例如，下列内容取代 MIM 服务中的现有 PAM 用户的电话号码。 其目录条目保持不变。

```
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```


## <a name="configure-pam-roles-for-azure-mfa"></a>为 Azure MFA 配置 PAM 角色

将 PAM 角色的所有候选用户的电话号码存储在 MIM 服务数据库中后，可以为 Azure MFA 配置该角色。 将使用 `New-PAMRole` 或 `Set-PAMRole` 命令来完成该操作。 例如，

```
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

可以为某个角色禁用 Azure MFA，方法是指定 `Set-PAMRole` 命令中的参数“-MFAEnabled 0”。

## <a name="troubleshooting"></a>疑难解答

可以在“特权访问管理”事件日志中找到以下事件：

| ID  | 严重性 | 生成者 | 描述 |
|-----|----------|--------------|-------------|
| 101 | 错误       | MIM 服务            | 用户未完成 Azure MFA（例如，没有应答电话） |
| 103 | 信息 | MIM 服务            | 用户在激活过程中完成了 Azure MFA                       |
| 825 | 警告     | PAM 监视服务 | 电话号码已更改                                |

若要了解有关失败电话呼叫（事件 101）的详细信息，你还可以从 Azure MFA 中查看或下载报告。

1.  打开 Web 浏览器，以 Azure AD 全局管理员的身份连接到 [Azure 经典门户](https://manage.windowsazure.com)。

2.  在 Azure 门户菜单中选择“Active Directory”，然后选择“多重身份验证提供程序”选项卡。

3.  单击你将用于 PAM 的 Azure MFA 提供程序，然后单击“管理”。

4.  在新窗口中，单击“用法” ，然后单击“用户详细信息” 。

5.  选择时间范围，然后选中其他报表列中“名称”旁边的复选框。 单击“导出至 CSV” 。

6.  当报表已生成时，你可以在门户中查看，或者如果 MFA 报表非常多，可将其下载为 CSV 文件。 “身份验证类型”列中的“SDK”值表示与 PAM 激活请求相关的行：这些是源自 MIM 或其他本地软件的事件。 “用户名”  字段是 MIM 服务数据库中用户对象的 GUID。 如果调用失败，“身份验证”列中的值将为“否”，并且“调用结果”列中的值将包含失败原因的详细信息。



<!--HONumber=Nov16_HO2-->


