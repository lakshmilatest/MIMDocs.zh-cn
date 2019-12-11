---
title: 为 B2B 配置用于 Microsoft Graph 的 Microsoft Identity Manager 连接器 | Microsoft Docs
author: billmath
description: Microsoft Graph 连接器是外部用户 AD 帐户生命周期管理。 在这种情况下，组织已邀请来宾访问其 Azure AD 目录，并希望为这些来宾授予对本地 Windows 集成身份验证或基于 Kerberos 的应用程序的访问权限
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 139c58510117ad422529a4ff0facd23040023713
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "64520961"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>通过 Azure 应用程序代理实现 Microsoft Identity Manager (MIM) 2016 SP1 与 Azure AD 企业对企业 (B2B) 之间的协作
============================================================================================================================

初始方案是外部用户 AD 帐户生命周期管理。   在这种情况下，组织已邀请来宾访问其 Azure AD 目录，并希望通过 [Azure AD 应用程序代理](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)或其他网关机制为这些来宾提供对本地 Windows 集成身份验证或基于 Kerberos 的应用程序的访问权限。 Azure AD 应用程序代理要求每个用户都拥有自己的 AD DS 帐户，以便进行标识和委派。

## <a name="scenario-specific-guidance"></a>特定于方案的指南

对使用 MIM 和 Azure AD 应用程序代理配置 B2B 做出的一些假设：

-   你已经部署了本地部署 AD，并且已安装 Microsoft Identity Manager 以及为 MIM 服务、MIM 门户、Active Directory 管理代理 (AD MA) 和 FIM 管理代理 (FIM MA).进行了基本配置。
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   你已按照文章中有关如何下载和安装 [Graph 连接器](microsoft-identity-manager-2016-connector-graph.md)的说明进行操作。

-   你已配置 Azure AD Connect，用于将用户和组同步到 Azure AD。

-   你已配置 Azure AD Connect，用于同步 Office 组，以便控制应用程序[返回本地 AD DS](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   你已设置应用程序代理连接器和连接器组，如果没有，则可以访问[此处](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector)进行安装和配置

-   你已发布一个或多个应用程序，这些应用程序依赖于 Windows 集成身份验证或通过 Azure AD 应用代理的各 AD 帐户

-   你已邀请或你邀请一位或多位来宾，会导致在 Azure AD 中创建一个或多个用户<https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>B2B 端到端部署示例方案

本指南基于以下方案：

Contoso Pharmaceuticals 与 Trey Research Inc. 合作，属于后者的研发部门。 Trey Research 员工需要访问 Contoso Pharmaceuticals 提供的研究报告应用程序。

-   Contoso Pharmaceuticals 位于他们自己的租户中，已配置自定义域。

-   有人邀请了外部用户访问 Contoso Pharmaceuticals 租户。
    此用户已接受邀请并可以访问共享的资源。

-   Contoso Pharmaceuticals 已通过应用代理发布应用程序。 在本方案中，示例应用程序是 MIM 门户。 这将使来宾用户参与 MIM 进程，例如位于帮助台方案中或请求访问 MIM 中的组。


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>配置 AD 和 Azure AD Connect 以排除从 Azure AD 添加用户

默认情况下，Azure AD Connect 将假定 Active Directory 中的非管理员用户需要同步到 Azure AD。  如果 Azure AD Connect 在 Azure AD 中发现某个现有用户与本地部署 AD 的用户匹配，则 Azure AD Connect 将匹配这两个帐户并假设这是用户的早期同步，然后将本地部署 AD 设为可信。  但是，此默认行为不适用于用户帐户在 Azure AD 中产生的 B2B 流。 

因此，从 Azure AD 通过 MIM 引入到 AD DS 的用户需要在存储后，Azure AD 不会尝试将这些用户同步回 Azure AD。
实现这一点的一种方法是在 AD DS 中创建新的组织单位，并将 Azure AD Connect 配置为排除该组织单位。  

详细信息请参阅 [Azure AD Connect sync:Configure filtering](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering)（Azure AD Connect 同步：配置筛选）。 
 

## <a name="create-the-azure-ad-application"></a>创建 Azure AD 应用程序 


注意：在 MIM 同步中创建图形连接器的管理代理之前，请确保已查看了部署 [Graph 连接器](microsoft-identity-manager-2016-connector-graph.md)的指南，并创建了具有客户端 ID 和机密的应用程序。
确保应用程序至少已授予以下权限之一：`User.Read.All`、`User.ReadWrite.All`、`Directory.Read.All` 或 `Directory.ReadWrite.All`。 

## <a name="create-the-new-management-agent"></a>创建新的管理代理


在 Synchronization Service Manager UI 中，选择“连接器”和“创建”。 ****   ****
选择“Graph (Microsoft)”并为其指定一个描述性名称。 ****  

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>连接性

在连接页面上，必须指定 Graph API 版本。 生产就绪 PAI 为 V 1.0，非生产为 Beta 版本   。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>全局参数

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>配置预配层次结构

此页面用于将 DN 组件（例如 OU）映射到应预配的对象类型，例如 organizationalUnit。 本方案无需这样做，因此将其保留为默认值并单击“下一步”。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>配置分区和层次结构

在“分区和层次结构”页面上，选择包含你计划导入和导出的对象的所有命名空间。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>选择对象类型

在“对象类型”页面上，选择计划导入的对象类型。 必须至少选择“用户”。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>选择属性

在“选择属性”屏幕上，从 Azure AD 中选择管理 AD 中 B2B 用户所需的属性。 属性“ID”是必选项。  稍后将在此配置中使用属性 `userPrincipalName` 和 `userType`。  其他属性是可选项，包括

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>配置定位点

在“配置定位点”屏幕上，配置定位点属性是必需的步骤。 默认情况下，使用 ID 属性进行用户映射。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>配置连接器筛选器

在“配置连接器筛选器”页面上，MIM 可以根据属性筛选器筛选出对象。 在 B2B 的这种方案中，目的是仅引入 `userType` 属性的值等同于 `Guest` 的用户，而不是引入 userType 等同于 `member` 的用户。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>配置联接和投影规则

本指南假设你将创建同步规则。  由于配置联接和投影规则由同步规则处理，因此不需要在连接器本身上标识联接和投影。 保留默认设置并单击“确定”。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>配置属性流

本指南假设你将创建同步规则。  在 MIM 同步中定义属性流不需要投影，因为它是由稍后创建的同步规则处理的。 保留默认设置并单击“确定”。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>配置取消设置

如果删除了 metaverse 对象，则用于配置取消设置的设置允许将 MIM 同步配置为删除对象。 在此方案中，我们将它们设置为断开器，为的是将其保留在 Azure AD 中。 在此方案中，我们不将任何内容导出到 Azure AD，且连接器配置为仅限导入。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>配置扩展

在此管理代理上配置扩展是一个选项，这不是必需项，因为我们正在使用同步规则。 如果我们之前决定在属性流中使用高级规则，则会有一个定义规则扩展的选项。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>扩展 metaverse 架构


在创建同步规则之前，我们需要使用 MV 设计器创建一个绑定到 person 对象的名为 userPrincipalName 的属性。

在同步客户端上，选择 Metaverse 设计器

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

然后选择 Person 对象类型

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

然后在“操作”下，单击“添加属性”

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

然后填写以下详细信息

属性名称：userPrincipalName 

属性类型:字符串（可编制索引） 

Indexed = True 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>创建 MIM 服务同步规则

在下面的步骤中，我们开始映射 B2B 来宾帐户和属性流。 在此假设：你已配置 Active Directory MA，并且配置了 FIM MA 以将用户转到 MIM 服务和门户。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

下一步骤将需要为 FIM MA 和 AD MA 添加最小配置。

有关配置的更多详细信息，请参阅此处<https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> - 如何将用户设置为 AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>同步规则：将来宾用户导入 MV，以从 Azure Active Directory 同步服务 Metaverse<br>

导航到 MIM 门户，选择“同步规则”并单击“新建”。  通过图形连接器创建 B2B 流的入站同步规则。
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

在关系条件步骤中，请务必选择“在 FIM 中创建资源”。
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

配置以下入站属性流规则。  请确保填充 `accountName`、`userPrincipalName` 和 `uid` 属性，因为稍后将在本方案中使用它们：

| **仅限初始流** | **用作存在性测试** | **流（源值 ⇒ FIM 属性）**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName](javascript:void(0);)                        |
|                       |                           | [Left(id,20)⇒accountName](javascript:void(0);)                        |
|                       |                           | [id⇒uid](javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType](javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [surname⇒sn](javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
|                       |                           | [id⇒cn](javascript:void(0);)                                          |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone](javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>同步规则：创建访问 Active Directory 的来宾用户帐户 

此同步规则在 Active Directory 中创建用户。  确保 `dn` 流必须将用户置于从 Azure AD Connect 中排除的组织单位中。  此外，更新 `unicodePwd` 的流以符合 AD 密码策略 - 用户无需知道密码。  注意，`userAccountControl` 的 `262656` 值会对标记 `SMARTCARD_REQUIRED` 和 `NORMAL_ACCOUNT` 进行编码。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

流规则：

| **仅限初始流** | **用作存在性测试** | **流（FIM 值 ⇒ 目标属性）**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=contoso,DC=com"⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>可选同步规则：导入 B2B 来宾用户对象 SID 以登录到 MIM 

此入站同步规则将 Active Directory 中用户的 SID 属性带回 MIM，因此用户可以访问 MIM 门户。  MIM 门户要求用户在 MIM 服务数据库中填充属性 `samAccountName`、`domain` 和 `objectSid`。

将源外部系统配置为 `ADMA`，因为当 MIM 创建用户时，AD 将自动设置 `objectSid` 属性。
 
请注意，如果将用户配置为在 MIM 服务中创建，请确保它们不在用于员工 SSPR 管理策略规则的任何集合的范围内。  你可能需要更改集合定义以排除 B2B 流创建的用户。 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **仅限初始流** | **用作存在性测试** | **流（源值 ⇒ FIM 属性）**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>运行同步规则

接着，我们邀请用户，然后按以下顺序运行管理代理同步规则：

-   `MIMMA` 管理代理上的完全导入和同步。  这可确保 MIM 同步已配置最新的同步规则。

-   `ADMA` 管理代理上的完全导入和同步。  这可确保 MIM 和 Active Directory 保持一致。  此时，来宾还没有任何挂起的导出。

-   B2B Graph 管理代理上的完全导入和同步。  它将来宾用户引入到 metaverse。  此时，一个或多个帐户将为 `ADMA` 挂起的导出。  如果没有挂起的导出，则检查来宾用户是否已导入到连接器空间中，并且是否已配置规则使这些来宾用户成为给定 AD 帐户。

-   `ADMA` 管理代理上的导出、增量导入和同步。  如果导出失败，请检查规则配置并确定是否存在任何缺少的架构要求。 

-   `MIMMA` 管理代理上的导出、增量导入和同步。  完成该操作后，应不再有任何挂起的导出。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>可选：B2B 来宾应用程序代理登录到 MIM 门户

现在我们已经在 MIM 中创建了同步规则。 在应用代理配置中，定义使用云主体以允许在应用代理中使用 KCD。
此外，接下来将用户手动添加到管理用户和组。 在 MIM 中创建之前不向用户显示的选项可以在设置后将来宾添加到 Office 组，需要本文中未包含的一些配置。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


一旦全部配置完毕，让 B2B 用户登录并查看应用程序。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>后续步骤
----------

[How Do I Provision Users to AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)（如何将用户预配到 AD DS）

[Functions Reference for FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)（FIM 2010 的函数参考）

[如何提供对本地应用程序的安全远程访问](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[下载用于 Microsoft Graph 的 Microsoft Identity Manager 连接器](http://go.microsoft.com/fwlink/?LinkId=717495)
