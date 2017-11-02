---
title: "使用 MIM 2016 处理 Azure 中的混合报告 | Microsoft Docs"
description: "了解如何将本地和云中的数据结合到 Azure 中的混合报告内，以及如何管理和查看这些报告。"
keywords: 
author: fimguy
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cf8395583dcfcc2a84237bad80b6a4ca40ce166c
ms.sourcegitcommit: f077508b5569e2a96084267879c5b6551e1e0905
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2017
---
# <a name="working-with-identity-manager-hybrid-reporting---public-preview-refresh"></a>使用 Identity Manager 混合报告 - 公开预览版（新版）

## <a name="available-hybrid-reports"></a>可用混合报告
Azure AD 中可用的前三个 Microsoft 标识管理器 (MIM) 报告是“密码重置活动”、“密码重置注册”和“自助服务组活动”。

-   当用户使用 SSPR 执行密码重置并提供用于身份验证的入口或 **方法** 时，密码重置活动将显示每个实例。

-   每当用户注册用于身份验证的 SPR 和 **方法** （例如移动电话号码或问题和回答）时，密码重置注册将显示。
    请注意，对于密码重置注册，不区分 SMS 入口和 MFA 入口，两者都视为“移动电话” 。

-   自助服务组活动显示某人为在组和组创建中添加或删除自己所作的每次尝试。

    ![Azure 混合报告 - 密码重置活动图像](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> 报告当前显示最多一个月前的数据。</br>
> 必须卸载旧版混合代理</br>
> 如果要卸载混合报告，请卸载 MIMreportingAgent.msi 代理。

## <a name="prerequisites"></a>先决条件

1.  安装 Microsoft 标识管理器 2016 RTM/或 SP1 MIM 服务。

2.  请确保目录中有一个带有许可管理员的 Azure AD 高级租户。

3.  请确保你具有从 Microsoft 标识管理器服务器到 Azure 的传出 Internet 连接。

## <a name="requirements"></a>要求
下表列出了使用 Microsoft Identity Manager 混合报告所要满足的要求。

| 要求 | 说明 |
| --- | --- |
| Azure AD 高级版 | 混合报告是一项 Azure AD Premium 功能，必须安装 Azure AD Premium。 </br></br>有关详细信息，请参阅 [Azure AD Premium 入门](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-get-started-premium)。 </br>若要开始 30 天免费试用，请参阅[开始试用](https://azure.microsoft.com/trial/get-started-active-directory/)。 |
| 必须是 Azure AD 全局管理员才能开始使用 |默认情况下，只有全局管理员才能安装和配置代理，从而入门、访问门户并在 Azure 中执行任意操作。 </br></br>**重要提示：**安装代理时必须使用工作或学校帐户。 不能使用 Microsoft 帐户。 有关详细信息，请参阅[以组织身份注册 Azure](https://docs.microsoft.com/en-us/azure/active-directory/sign-up-organization)。 |
| 在每个目标 MIM Service 服务器上安装 Microsoft Identity Manager 混合代理 | 混合报告要求在目标服务器上安装和配置代理，用于接收数据并提供监视和分析功能。 </br>|
| 到 Azure 服务终结点的出站连接 | 安装和运行时期间，代理需要与 Azure 服务终结点相连。 如果出站连接被防火墙屏蔽，请确保已将下列终结点添加到允许列表中： </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net - 端口：5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|基于 IP 地址的出站连接 | 有关防火墙上基于 IP 地址的筛选，请参阅 [Azure IP 范围](https://www.microsoft.com/en-us/download/details.aspx?id=41653)。|
| 已为出站流量筛选或禁用 SSL 检查 | 如果网络层有出站流量的 SSL 检查或终端，代理注册步骤或数据上载操作可能会失败。 |
| 运行代理的服务器上的防火墙端口。 |为了让代理能够与 Azure 服务终结点进行通信，代理要求打开以下防火墙端口。</br></br><li>TCP 端口 443</li><li>TCP 端口 5671</li> |
| 在启用“IE 增强的安全性”的情况下，允许访问以下网站 |如果启用了“IE 增强的安全配置”，必须在即将安装代理的服务器上允许访问以下网站。</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Azure Active Directory 信任的组织联合服务器。 例如，https://sts.contoso.com。</li> |
</BR>

## <a name="install-microsoft-identity-manager-reporting-agent-in-azure-ad"></a>在 Azure AD 中安装 Microsoft 标识管理器报告代理
安装报告代理后，Microsoft 标识管理器活动中的数据从 MIM 导出到 Windows 事件日志。 MIM 报告代理处理这些事件，并将其上传到 Azure。 在 Azure 中，针对所需的报告解析、解密并筛选这些事件。

1.  安装 Microsoft 标识管理器 2016。

2.  下载 Microsoft 标识管理器报告代理：

    1.  登录到 Azure AD 管理门户并单击 Active Directory 图标。

    2.  双击你担任全局管理员的目录，你将拥有一个 Azure AD 高级订阅。

    3.  单击“配置”  ，然后下载报告代理。

3.  安装 Microsoft 标识管理器报告代理：

    1.  将 [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) 下载到 Microsoft Identity Manager 服务服务器。
    2.  运行 `MIMHReportingAgentSetup.exe` 
    3.  运行代理安装程序。

    4.  请确保 MIM 报告代理服务正在运行

    5.  重新启动 MIM 服务。

4.  验证 Microsoft 标识管理器报告是否在 Azure 中工作。

    可以通过使用 Microsoft 标识管理器自助服务密码重置门户来重置用户的密码，从而创建报告数据。 请确保密码重置已成功完成，然后检查该数据是否在 Azure AD 管理门户中显示。

## <a name="view-hybrid-reports-in-the-azure-portal"></a>在 Azure 门户中查看混合报告

1.  为租户使用全局管理员帐户登录到 [Azure 门户](https://portal.azure.com/)。

2.  单击“Azure Active Directory” 图标。

3.  从订阅的可用目录列表中选择租户目录。

4.  单击“审核日志”。

5.  请确保在“类别”下拉菜单中选择“MIM 服务”。

> [!WARNING]
> Microsoft Identity Manager 审计数据需要经过一段时间才能显示在 Azure 门户中。

## <a name="stop-creating-hybrid-reports"></a>停止创建混合报告
如果要停止将报告审核数据从 Microsoft 标识管理器上传到 Azure Active Directory，请卸载混合报告代理。 使用 Windows“添加或删除程序”工具，卸载 Microsoft 标识管理器混合报告。

## <a name="windows-events-used-for-hybrid-reporting"></a>用于混合报告的 Windows 事件
由 Microsoft Identity Manager 生成的事件记录在 Windows 事件日志中，并且在事件查看器中可见，位于以下位置：“应用程序和服务”日志 -&gt;“Identity Manager 请求日志”。 每个 MIM 请求以 JSON 结构导出为 Windows 事件日志中的事件。 这可以导出到你的 SIEM。

|事件类型|ID|事件详细信息|
|--------------|------|-----------------|
|信息|4121|包括所有请求数据的 MIM 事件数据。|
|信息|4137|MIM 事件 4121 扩展名，用于单个事件具有过多数据的情况。 本事件中的头采用以下格式： `"Request: <GUID> , message <xxx> out of <xxx>`|
