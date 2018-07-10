---
title: 通过 Identity Manager 2016 使用 Azure 中的混合报告 | Microsoft 文档
description: 了解如何将本地和云中的数据结合到 Azure 中的混合报告内，以及如何管理和查看这些报告。
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 2/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: 3c9e8c0fa0a0de3cf9710003d4d7f4ed9c0b03bd
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289639"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>使用 Identity Manager 中的混合报告

本文讨论如何将本地和云中的数据结合到 Azure 中的混合报告内，以及如何管理和查看这些报告。

## <a name="available-hybrid-reports"></a>可用混合报告
Azure Active Directory (Azure AD) 中提供了前三种 Microsoft Identity Manager 报告，如下所示：

- 密码重置活动：当用户使用自助服务密码重置 (SSPR) 执行密码重置时显示每个实例，并提供用于身份验证的入口或方法。

- 密码重置注册：显示用户每次注册 SSPR 的时间以及进行身份验证所用的方法。 例如，所用的方法可能是手机号码或问答。
   > [!NOTE]
   > 对于“密码重置注册”报告，不区分 SMS 入口和 MFA 入口。 两者均视为移动电话方法。

- 自助服务组活动：显示某人为在组和组创建中添加或删除自己而所作的每次尝试。

    ![Azure 混合报告 - 密码重置活动图像](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * 报告当前显示最多一个月的活动数据。
> * 必须卸载旧版混合报告代理。
> * 若要卸载混合报告，请卸载 MIMreportingAgent.msi 代理。

## <a name="prerequisites"></a>必备条件

* Identity Manager 2016 SP1 Identity Manager 服务，建议版本 [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager)。

* 目录中具有许可管理员的 Azure AD Premium 租户。

* 从 Identity Manager 服务器到 Azure 的传出 Internet 连接。

## <a name="requirements"></a>要求
下表列出了使用 Identity Manager 混合报告的要求：


|                                         要求                                         |                                                                                                                                                                                                                                                                                    描述                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD 高级版                                       |                                                                                                        混合报告是一项 Azure AD Premium 功能，必须安装 Azure AD Premium。 </br>有关详细信息，请参阅 [Azure AD Premium 入门](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium)。 </br>获取 [Azure AD Premium 免费 30 天试用版](https://azure.microsoft.com/trial/get-started-active-directory/)。                                                                                                         |
|                     你必须是 Azure AD 的全局管理员。                     |                                                                   默认情况下，只有全局管理员才能安装和配置代理，从而开始使用、访问门户并在 Azure 中执行任意操作。 </br>重要提示：安装代理时必须使用工作帐户或学校帐户。 不能使用 Microsoft 帐户。 有关详细信息，请参阅[以组织身份注册 Azure](https://docs.microsoft.com/azure/active-directory/sign-up-organization)。                                                                   |
| 在每个目标 Identity Manager Service 服务器上安装 Identity Manager 混合代理 |                                                                                                                                                                                                       为了接收数据并提供监视和分析功能，混合报告需要在目标服务器上安装和配置代理。  </br>                                                                                                                                                                                                       |
|                    到 Azure 服务终结点的出站连接                     | 安装和运行时期间，代理需要与 Azure 服务终结点相连。 如果出站连接被防火墙屏蔽，请确保已将下列终结点添加到允许列表中：<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net - 端口：5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         基于 IP 地址的出站连接                         |                                                                                                                                                                                                                      有关防火墙上基于 IP 地址的筛选，请参阅 [Azure IP 范围](https://www.microsoft.com/download/details.aspx?id=41653)。                                                                                                                                                                                                                      |
|                 已为出站流量筛选或禁用 SSL 检查                 |                                                                                                                                                                                                               如果网络层有针对出站流量的 SSL 检查或终止，则代理注册步骤或数据上传操作可能会失败。                                                                                                                                                                                                                |
|                      运行代理的服务器上的防火墙端口                       |                                                                                                                                                                                                          为了与 Azure 服务终结点进行通信，代理需要打开以下防火墙端口：<ul><li>TCP 端口 443</li><li>TCP 端口 5671</li></ul>                                                                                                                                                                                                          |
|          如果启用了 Internet Explorer 增强的安全性，则允许访问某些网站           |                                                                                如果启用了 Internet Explorer 增强的安全性，则必须在已安装代理的服务器上允许访问以下网站：<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>你的组织受 Azure Active Directory 信任的联合服务器（例如 <https://sts.contoso.com>）。</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>在 Azure AD 中安装 Identity Manager 报告代理
安装报告代理后，Identity Manager 活动中的数据从 Identity Manager 导出到 Windows 事件日志。 Identity Manager 报告代理处理这些事件，然后将其上传到 Azure。 在 Azure 中，针对所需的报告解析、解密并筛选这些事件。

1.  安装 Identity Manager 2016。

2.  下载 Identity Manager 报告代理，然后执行以下操作：

    a. 登录到 Azure AD 管理门户，然后选择 Active Directory。

    b. 双击你是全局管理员且拥有 Azure AD Premium 订阅的目录。

    c. 选择“配置”，然后下载报告代理。

3.  通过执行以下操作安装报告代理：

    a.  为 Identity Manager Service 服务器下载 [MIMHReportingAgentSetup.exe 文件](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe)。

    b.  运行 `MIMHReportingAgentSetup.exe`。 

    c.  运行代理安装程序。

    d.  确保 Identity Manager 报告代理服务正在运行。

    e.  重启 Identity Manager 服务。

4.  验证 Identity Manager 报告代理是否正在 Azure 中工作。

    可以通过使用 Identity Manager 自助服务密码重置门户来重置用户的密码，从而创建报告数据。 请确保成功完成密码重置后进行检查，以确保数据显示在 Azure AD 管理门户中。

## <a name="view-hybrid-reports-in-the-azure-portal"></a>在 Azure 门户中查看混合报告

1.  使用租户的全局管理员帐户登录到 [Azure 门户](https://portal.azure.com/)。

2.  选择 **Azure Active Directory**。

3.  在订阅的可用目录列表中，选择租户目录。

4.  选择“审核日志”。

5.  在“类别”下拉列表中，确保选择“MIM 服务”。

> [!IMPORTANT]
> Identity Manager 审核数据需要经过一段时间才能显示在 Azure 门户中。

## <a name="stop-creating-hybrid-reports"></a>停止创建混合报告
如果要停止将报告审核数据从 Identity Manager 上传到 Azure AD，请卸载混合报告代理。 使用 Windows 的“添加或删除程序”工具来卸载 Identity Manager 混合报告。

## <a name="windows-events-used-for-hybrid-reporting"></a>用于混合报告的 Windows 事件
通过 Identity Manager 生成的事件存储在 Windows 事件日志中。 你可以通过选择“应用程序和服务日志” > “Identity Manager 请求日志”在“事件查看器”中查看事件。 每个 Identity Manager 请求将以 JSON 结构导出为 Windows 事件日志中的一个事件。 你可以将结果导出到你的安全信息和事件管理 (SIEM) 系统。

|事件类型|ID|事件详细信息|
|--------------|------|-----------------|
|信息|4121|包括所有请求数据的 Identity Manager 事件数据。|
|信息|4137|Identity Manager 事件 4121 扩展，如果单个事件具有过多数据。 本事件中的标头采用以下格式显示：`"Request: <GUID> , message <xxx> out of <xxx>`。|
