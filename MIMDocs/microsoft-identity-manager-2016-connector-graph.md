---
title: 用于 Microsoft Graph 的 Microsoft Identity Manager 管理代理 | Microsoft Docs
author: fimguy
description: Microsoft Graph（预览版）是外部用户 AD 帐户生命周期管理。 在这种情况下，组织已邀请来宾访问其 Azure AD 目录，并希望为这些来宾授予对本地 Windows 集成身份验证或基于 Kerberos 的应用程序的访问权限
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: a0e2e280c3678867efc2ae8afa46c04ed38a1e11
ms.sourcegitcommit: 637988684768c994398b5725eb142e16e4b03bb3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
---
<a name="the-microsoft-identity-manager-management-agent-for-microsoft-graph-public-preview"></a>用于 Microsoft Graph的 Microsoft Identity Manager 管理代理（公共预览版）
=======================================================================================

<a name="summary"></a>“摘要” 
=======

[用于 Microsoft Graph（预览版）的 Microsoft Identity Manager 管理代理](http://go.microsoft.com/fwlink/?LinkId=717495) 为 Azure AD Premium 客户启用其他集成方案。

[Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) 将本地目录与 Azure AD 集成在一起，确保用户在与 Azure AD 集成的 AD DS、Office 365、Azure 和 SaaS 应用程序中具有通用身份和一致的身份验证，方法是将用户和组从本地目录同步到 Azure AD。   除了到 Azure AD 的用户和组同步之外，此管理代理可部署用于 专用身份和访问管理操作。  此管理代理在 MIM 同步 Metaverse 中显示从 [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 和 beta 版中获取的附加对象。 

<a name="scenarios-covered"></a>涵盖的方案
=================

<a name="b2b-account-lifecycle-management"></a>B2B 帐户生命周期管理
--------------------------------

用于 Microsoft Graph（预览版）的 Microsoft Identity Manager 管理代理预览版中的初始方案是外部用户 AD 帐户生命周期管理。 在这种情况下，组织已邀请来宾访问其 Azure AD 目录，并希望通过 [Azure AD 应用程序](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish)代理或其他网关机制为这些来宾提供对本地 Windows 集成身份验证或基于 Kerberos 的应用程序的访问权限。 Azure AD 应用程序代理要求每个用户都拥有自己的 AD DS 帐户，以便进行标识和委派

未来可能会添加其他方案，并且[在此处记录](./microsoft-identity-manager-2016-graph-b2b-scenario.md)

<a name="determining-your-deployment-topology"></a>确定部署拓扑
====================================


<a name="preparing-to-use-the-management-agentma-for-microsoft-graph"></a>准备使用 Microsoft Graph 的管理代理 (MA)
=============================================================

<a name="authorizing-the-ma-to-manage-your-azure-ad-directory"></a>授权 MA 管理 Azure AD 目录
----------------------------------------------------

1.  图形管理代理需要在 AzureAD 中创建 Web 应用/API 应用程序。

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

图 1. 新应用程序注册

2.  打开创建的应用程序并在 MA 的连接页面上使用应用程序 ID，如客户端 ID：

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

图 2. 应用程序 ID

2.  通过打开“所有设置 - \> 秘钥”生成新的客户端密码。 设置一些秘钥说明并选择必要的持续时间。 保存更改。 离开页面后密码值将不可用。

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

图 3. 新建客户端密码

3.  通过打开“所需权限”将“Microsoft Graph API”添加到应用程序中。

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

图 4. 添加新的 API

以下权限应添加到“Microsoft Graph API”中：

| 对象操作 | 所需权限                                                                  | 权限类型 |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| 导入组          | Group.Read.All or Group.ReadWrite.All                                                | 应用程序     |
| 导入用户           | User.Read.All 或 User.ReadWrite.All 或 Directory.Read.All 或 Directory.ReadWrite.All | 应用程序     |

有关所需权限的更多详细信息，可访问[此处](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)

1.  使用应用程序 ID 和生成的客户端密码创建连接器。每个管理代理应在 AzureAD 中拥有自己的应用程序，以避免为同一应用程序并行运行导入。 图形连接器支持以下对象类型列表：

-   用户

    -   完整/增量导入

    -   导出（添加、更新、删除）

-   Group

    -   完整/增量导入

    -   导出（添加、更新、删除）


支持的属性类型的列表：

-   Edm.Boolean

-   Edm.String

-   Edm.DateTimeOffset（连接器空间中的字符串）

-   microsoft.graph.directoryObject（连接器空间中对任何受支持的对象的引用）

-   microsoft.graph.contact

前面列表中任何一种类型的表单也支持多值属性（集合）。

图形连接器对所有对象的定位点和 DN 使用“id”属性。

此时不支持重命名，因为 GraphAPI 不允许将对象更改为已存在对象的“id”属性。

<a name="access-token-lifetime"></a>访问令牌生存期
=====================

Graph 应用程序需要访问令牌才能访问 GraphAPI。 连接器将为每个导入迭代请求一个新的访问令牌（导入迭代取决于页面大小）。 例如：

-   AzureAD 包含 10000 个对象

-   在连接器中配置的页面大小为 5000

在这种情况下，导入期间将有两次迭代，每次迭代都会返回 5000 个对象进行同步。因此，一个新的访问令牌将被请求两次。

请注意，在导出期间，将为每个必须添加/更新/删除的对象请求一个新的访问令牌。

<a name="installing-the-connector"></a>安装连接器
========================

在使用连接器之前，请确保在同步服务器上具有以下内容：Microsoft .NET 4.5.2 Framework 或更高版本的 Microsoft Identity Manager 2016 SP1 必须使用修补程序 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) 或更高版本。

Microsoft Identity Manager 2016 SP1 的连接器，即 Graph 连接器可从 [Microsoft下载中心](https://www.microsoft.com/en-us/download/details.aspx?id=51495)下载。

<a name="connector-configuration"></a>连接器配置
=======================

连接页面：

![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

图 5. 连接页面

连接页面（图 1）包含使用的 Graph API 版本和租户名称。 客户端 ID 和客户端密码表示必须在 AzureAD 中创建的 WebAPI 应用程序的应用程序 ID 和密钥值。

全局参数页面：

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

图 6. 全局参数页面

全局参数页面包含以下设置：

DateTime 格式：用于 Edm.DateTimeOffset 类型的任何属性的格式。 在导入过程中，所有日期都通过使用该格式转换为字符串。 设置格式适用于保存日期的任何属性。

HTTP 超时（秒）：将在每次对 WebAPI 应用程序发起 HTTP 调用期间使用的超时（以秒为单位）。

在下次登录时强制更改已创建用户的密码：此选项用于将在导出过程中创建的新用户。 如果选项已启用，则 [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) 属性将设置为 true，否则它将为 false。

<a name="troubleshooting"></a>疑难解答
===============

**启用日志**

如果 Graph 中有任何问题，可以使用日志定位问题。 Graph 连接器使用与所有通用连接器中相同的源。 因此，可以[与使用通用连接器一样启用跟踪，请参阅 wiki](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx)。 或者只需将以下内容添加到 miiserver.exe.config（在 system.diagnostics/sources 部分内）：

\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

>   \<add initializeData="ConnectorsLog"   type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,   Culture=neutral, PublicKeyToken=b77a5c561934e089"   name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,   DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>

请注意：如果启用了“在单独的进程中运行此管理代理”，则应使用 dllhost.exe.config 而不是 miiserver.exe.config。

**访问令牌过期错误**

连接器可能会返回 HTTP 错误 401 未授权，消息“访问令牌已过期。”：

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

图 7. “访问令牌已过期。” 错误

此问题的原因可能是从 Azure 端配置访问令牌生存期。 默认情况下，访问令牌在 1 小时后过期。 要延长过期时间，请参阅[本文](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)。

使用 [Azure AD PowerShell 模块公共预览版本](https://www.powershellgallery.com/packages/AzureADPreview)的示例

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@('{"TokenLifetimePolicy":{"Version":1, **"AccessTokenLifetime":"5:00:00"**}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$true -Type "TokenLifetimePolicy"

<a name="next-steps"></a>后续步骤
----------
- [Graph 浏览器（非常适用于排查 HTTP 调用问题）]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Microsoft Graph 的版本控制、支持和重大更改策略](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [下载用于 Microsoft Graph 的 Microsoft Identity Manager 管理代理（预览版）](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-supported-guides"></a>特定于方案的受支持指南
----------------------------------
[MIM B2B 端到端部署]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
