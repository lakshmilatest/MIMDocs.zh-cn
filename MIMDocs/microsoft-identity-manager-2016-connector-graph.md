---
title: 用于 Microsoft Graph 的 Microsoft Identity Manager 连接器 | Microsoft Docs
author: fimguy
description: 用于 Microsoft Graph 的 Microsoft Identity Manager 连接器支持外部用户 AD 帐户生命周期管理。 在这种情况下，组织已邀请来宾访问其 Azure AD 目录，并希望为这些来宾授予对本地 Windows 集成身份验证或基于 Kerberos 的应用程序的访问权限
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 2e376bcc88518b911f93ce9cd4ab920eb428815b
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358646"
---
<a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>用于 Microsoft Graph 的 Microsoft Identity Manager 连接器
=======================================================================================

<a name="summary"></a>“摘要” 
=======

[用于 Microsoft Graph 的 Microsoft Identity Manager 连接器](http://go.microsoft.com/fwlink/?LinkId=717495)为 Azure AD Premium 客户启用其他集成方案。  它在 MIM 同步 Metaverse 中显示从 [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 和 beta 版中获取的附加对象。

<a name="scenarios-covered"></a>涵盖的方案
=================

<a name="b2b-account-lifecycle-management"></a>B2B 帐户生命周期管理
--------------------------------

用于 Microsoft Graph 的 Microsoft Identity Manager 连接器的初始方案是作为连接器帮助自动执行外部用户的 AD DS 帐户生命周期管理。 在此方案中，组织使用 Azure AD Connect 将员工从 AD DS 同步到 Azure AD，并且还邀请来宾访问其 Azure AD 目录。 邀请来宾将导致外部用户对象位于该组织的 Azure AD 目录中，而该目录不在该组织的 AD DS 中。 那么组织希望通过 [Azure AD 应用程序代理](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)或其他网关机制为这些来宾提供对本地 Windows 集成身份验证或基于 Kerberos 的应用程序的访问权限。 Azure AD 应用程序代理要求每个用户都拥有自己的 AD DS 帐户，以便进行标识和委派。  

要了解如何将 MIM 同步配置为自动为来宾创建和维护 AD DS 帐户，请在阅读本文说明后，继续阅读文章[通过 Azure 应用程序代理实现 MIM 2016 SP1 与 Azure AD 企业对企业 (B2B) 之间的协作](~/microsoft-identity-manager-2016-graph-b2b-scenario.md)。  本文说明了连接器所需的同步规则。

<a name="other-identity-management-scenarios"></a>其他标识管理方案
---------------

连接器可用于其他特定标识管理方案，包括在 Azure AD 中创建、读取、更新和删除用户、组和联系人对象，而不仅仅是将用户和组同步到 Azure AD。 在评估可能的方案时，请记住：此连接器无法在某种方案中运行，这会导致数据流重叠，与 Azure AD Connect 部署发生实际或潜在的同步冲突。  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) 是一种建议方法，它通过将用户和组从本地目录同步到 Azure AD 来集成本地目录和 Azure AD。  Azure AD Connect 具有更多同步功能，并支持密码和设备写回等方案，这些方案对于 MIM 创建的对象是不可能实现的。 例如，如果数据被引入到 AD DS，请确保将其从 Azure AD Connect 中排除，尝试将这些对象匹配回 Azure AD 目录。  此连接器也不能用于更改 Azure AD Connect 创建的 Azure AD 对象。



<a name="preparing-to-use-the-connector-for-microsoft-graph"></a>准备使用 Microsoft Graph 的连接器
=============================================================

<a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>对连接器进行授权以检索或管理 Azure AD 目录中的对象
----------------------------------------------------

1.  连接器需要在 Azure AD 中创建 Web 应用/API 用程序，这样就可以授予它适当的权限以通过 Microsoft Graph 对 Azure AD 对象进行操作。

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

图 1. 新应用程序注册

2.  在 Azure 门户中，打开创建的应用程序，并将应用程序 ID 保存为客户端 ID，以便稍后在 MA 的连接页面上使用：

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

图 2. 应用程序 ID

3.  通过打开“所有设置 - \> 秘钥”生成新的客户端密码。 设置一些秘钥说明并选择必要的持续时间。 保存更改。 离开页面后密码值将不可用。

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

图 3. 新建客户端密码

4.  通过打开“所需权限”将“Microsoft Graph API”添加到应用程序中。

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

图 4. 添加新的 API

以下权限应添加到应用程序，使其可以使用"Microsoft Graph API"，具体权限视方案而定：

| 对象操作 | 所需权限                                                                  | 权限类型 |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| 导入组          | `Group.Read.All` 或 `Group.ReadWrite.All`                                                | 应用程序     |
| 导入用户           | `User.Read.All`、`User.ReadWrite.All`、`Directory.Read.All` 或 `Directory.ReadWrite.All` | 应用程序     |

有关所需权限的更多详细信息，可访问[此处](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)。

5. 授予应用程序所需的权限。


<a name="installing-the-connector"></a>安装连接器
========================

6.  在安装连接器之前，请确保在同步服务器上具有以下内容： 

 - Microsoft .NET 4.5.2 Framework 或更高版本
 - Microsoft Identity Manager 2016 SP1，并且必须使用修补程序 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) 或更高版本。

7. 除了 Microsoft Identity Manager 2016 SP1 的其他连接器之外，Microsoft Graph 的连接器还可从 [Microsoft 下载中心](https://www.microsoft.com/en-us/download/details.aspx?id=51495)下载。

8.  重启 MIM 同步服务。
 
<a name="connector-configuration"></a>连接器配置
=======================


9.  在 Synchronization Service Manager UI 中，选择“连接器”和“创建”。
选择“Graph (Microsoft)”，创建连接器并为其指定一个描述性名称。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. 在 MIM 同步服务 UI 中，指定应用程序 ID 和生成的客户端密码。 MIM 同步中配置的每个管理代理都应在 Azure AD 中具有自己的应用程序，以避免为同一应用程序并行运行导入。


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

图 5. 连接页面

连接页面（图 5）包含使用的 Graph API 版本和租户名称。 客户端 ID 和客户端密码表示必须在 Azure AD 中创建的 WebAPI 应用程序的应用程序 ID 和密钥值。

11. 在“全局参数”页面上进行任何必要的更改：

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

图 6. 全局参数页面

全局参数页面包含以下设置：

- DateTime 格式：用于 Edm.DateTimeOffset 类型的任何属性的格式。 在导入过程中，所有日期都通过使用该格式转换为字符串。 设置格式适用于保存日期的任何属性。

 - HTTP 超时（秒）：将在每次对 WebAPI 应用程序发起 HTTP 调用期间使用的超时（以秒为单位）。

 - 在下次登录时强制更改已创建用户的密码：此选项用于将在导出过程中创建的新用户。 如果选项已启用，则 [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) 属性将设置为 true，否则它将为 false。

<a name="configuring-the-connector-schema-and-operations"></a>配置连接器架构和操作
=========================

12.   配置架构。  连接器支持以下对象类型列表：

-   用户

    -   完整/增量导入

    -   导出（添加、更新、删除）

-   Group

    -   完整/增量导入

    -   导出（添加、更新、删除）


支持的属性类型的列表：

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset`（连接器空间中的字符串）

-   `microsoft.graph.directoryObject`（连接器空间中对任何受支持的对象的引用）

-   `microsoft.graph.contact`

以上列表中任一类型都支持多值属性（集合）。

连接器对所有对象的定位点和 DN 使用“`id`”属性。  因此，不需要重命名，因为 Graph API 不允许对象更改其“ID”属性。


<a name="access-token-lifetime"></a>访问令牌生存期
=====================

Graph 应用程序需要访问令牌才能访问 Graph API。 连接器将为每个导入迭代请求一个新的访问令牌（导入迭代取决于页面大小）。 例如：

-   Azure AD 包含 10000 个对象

-   在连接器中配置的页面大小为 5000

在这种情况下，导入期间将有两次迭代，每次迭代都会返回 5000 个对象进行同步。因此，一个新的访问令牌将被请求两次。

在导出期间，将为每个必须添加/更新/删除的对象请求一个新的访问令牌。

<a name="troubleshooting"></a>疑难解答
===============

**启用日志**

如果 Graph 中有任何问题，可以使用日志定位问题。 因此，可以[与使用通用连接器一样](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx)启用跟踪。 或只需将以下内容添加到 `miiserver.exe.config`（在 `system.diagnostics/sources` 部分中）：


\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

>   \<add initializeData="ConnectorsLog"   type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,   Culture=neutral, PublicKeyToken=b77a5c561934e089"   name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,   DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>

>[!NOTE]
>如果“在单独的进程中运行此管理代理”已启用，则应使用 `dllhost.exe.config` 而不使用 `miiserver.exe.config`。

**访问令牌过期错误**

连接器可能会返回 HTTP 错误 401 未授权，消息“访问令牌已过期。”：

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

图 7. “访问令牌已过期。” 错误

此问题的原因可能是从 Azure 端配置访问令牌生存期。 默认情况下，访问令牌在 1 小时后过期。 要延长过期时间，请参阅[本文](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes)。

使用 [Azure AD PowerShell 模块公共预览版本](https://www.powershellgallery.com/packages/AzureADPreview)的示例

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@('{"TokenLifetimePolicy":{"Version":1, **"AccessTokenLifetime":"5:00:00"**}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$true -Type "TokenLifetimePolicy"

<a name="next-steps"></a>后续步骤
----------
- [Graph 浏览器，非常适用于排查 HTTP 调用问题]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Microsoft Graph 的版本控制、支持和重大更改策略](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [下载用于 Microsoft Graph 的 Microsoft Identity Manager 连接器](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-guides"></a>特定于方案的指南
----------------------------------
[MIM B2B 端到端部署]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
