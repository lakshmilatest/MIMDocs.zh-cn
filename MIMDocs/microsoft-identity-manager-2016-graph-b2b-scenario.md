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
ms.openlocfilehash: ac11a4dfb23944d50dbbcf0b0d70c915f186c159
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479156"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>通过 Azure 应用程序代理（公共预览版）实现 Microsoft Identity Manager (MIM) 2016 SP1 与 Azure AD 企业对企业 (B2B) 之间的协作
============================================================================================================================

预览版中的初始方案是外部用户 AD 帐户生命周期管理。   在这种情况下，组织已邀请来宾访问其 Azure AD 目录，并希望通过 [Azure AD 应用程序](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)代理或其他网关机制为这些来宾提供对本地 Windows 集成身份验证或基于 Kerberos 的应用程序的访问权限。 Azure AD 应用程序代理要求每个用户都拥有自己的 AD DS 帐户，以便进行标识和委派

## <a name="scenario-specific-supported-guidance"></a>特定于方案的受支持指南

在这种情况下，组织已邀请来宾访问其 Azure AD 目录，并希望向这些来宾授予对本地 Windows 的访问权限。 通过 [Azure AD 应用程序](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)代理或其他网关机制的集成身份验证或基于 Kerberos 的应用程序。 Azure AD 应用程序代理要求每个用户都拥有自己的 AD DS 帐户，以便进行标识和委派

对使用 MIM 和 Azure 应用程序代理配置 B2B 做出的一些假设

-   你已安装 [Graph 管理代理](microsoft-identity-manager-2016-connector-graph.md)。

-   你已设置本地 AD 和 Azure AD Connect，用于将用户和组同步到 Azure AD。

    -   使用 [Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory) 控制应用程序访问的 Office 组

-   你已设置应用程序代理连接器和连接器组，如果没有，则可以访问[此处](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector)进行安装和配置

-   发布一个或多个应用程序，这些应用程序依赖于 Windows 集成身份验证或通过 Azure AD 应用代理的各 AD 帐户

-   你已邀请或你邀请一位或多位在 Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>中创建的来宾

-   安装了 Microsoft Identity Manager，并且执行了服务和门户以及 Active Directory 管理代理的基本配置。
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>B2B 端到端部署

方案

Contoso Pharmaceuticals 作为其研发部门的一部分为 Trey Research Inc. 工作。 Trey Research 员工需要访问 Contoso Pharmaceuticals 提供的研究报告应用程序。

-   Contoso Pharmaceuticals 位于独立租户中，已配置自定义域。

-   邀请了外部用户：访问 Contoso Pharmaceuticals 租户。
    此用户已接受邀请并可以访问共享的资源。

-   通过应用代理发布了应用程序，在此方案中，作为示例对来宾用户使用 MIM 服务和门户以参与 MIM 进程，即示例技术支持方案。

## <a name="create-the-graph-management-agent"></a>创建 Graph 管理代理

注意：在创建连接器之前，请确保已查看 [Graph 管理代理](microsoft-identity-manager-2016-connector-graph.md)。

在 Synchronization Service Manager UI 中，选择“连接器”和“创建”。
选择“Graph (Microsoft)”并为其指定一个描述性名称

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>连接性

在“连接”页面上，必须指定“图形 API 版本生产就绪”是 V 1.0，“非生产”是 Beta

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>功能

在“全局参数”页面上，将 DN 配置为增量更改日志和其他 LDAP 功能。 该页面预填充 LDAP 服务器提供的信息。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>全局参数

在“全局参数”页面上，将 DN 配置为增量更改日志和其他 LDAP 功能。 该页面预填充 LDAP 服务器提供的信息。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>配置预配层次结构

此页面用于将 DN 组件（例如 OU）映射到应预配的对象类型，例如 organizationalUnit。 要保留此默认值，请单击“下一步”

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>配置分区和层次结构

在“分区和层次结构”页面上，选择包含你计划导入和导出的对象的所有命名空间。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>选择对象类型

在“分区和层次结构”页面上，选择包含你计划导入和导出的对象的所有命名空间。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>选择属性

在“选择属性”屏幕上，选择要管理 B2B 用户的所需属性。 需要“id”属性

-   id

-   displayName

-   mail

-   givenName

-   surname

-   userPrincipalName

-   userType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>配置定位点

对于“配置定位点”，配置定位点是必需的步骤。 默认情况下使用 id 属性进行映射。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>配置连接器筛选器

在“配置连接器筛选器”页面上，可以根据属性筛选器筛选出对象。 在这种 B2B 方案中，目标只是引入 userType 等于来宾而不是成员的用户。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>配置联接和投影规则

配置联接和投影规则由同步规则处理，不需要在连接器本身上标识联接和投影。 保留默认设置并单击“确定”。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>配置属性流

同联接和投影一样，不需要定义属性流，因为它是由稍后创建的同步规则处理的。 保留默认设置并单击“确定”。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>配置取消设置

如果 metaverse 对象已删除，可以使用“配置取消设置”删除该对象。 在该测试中，我们让它们成为断开器，因为目标是将它们留在 Azure 中，我们也不会将任何内容导出到 Azure，因为它只能导入。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>配置扩展

在此管理代理上配置扩展是一个选项，这不是必需项，因为我们正在使用同步规则。 如果我们之前决定在属性流中使用高级规则，那么这将是一个可定义的选项。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>创建 MIM 服务同步规则

在下面的步骤中，我们开始映射 B2B 来宾帐户和属性流。 在此假设你已经配置 Active Directory 管理代理，并且配置了 FIM MA 以与 MIM 服务和门户进行通信。

在创建同步规则之前，我们需要使用 MV 设计器创建一个绑定到 person 对象的名为 userPrincipalName 的属性。

在同步客户端上，选择 Metaverse 设计器

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

然后选择 Person 对象类型

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

然后在“操作”下，单击“添加属性”

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

最后填写以下详细信息

属性名称：userPrincipalName

属性类型：String (Indexable)

Indexed = True

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

接下来的步骤将需要配置 FIM 服务管理代理和 Active Directory 域服务管理代理的最低配置。

有关配置的更多详细信息，请参阅此处<https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> - 如何将用户设置为 AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>同步规则：将来宾用户导入 MV 以便从 Azure Active Directory 同步服务 Metaverse<br>

导航到 MIM 服务和门户，选择“同步规则”并单击“新建”。
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)选择“在 FIM 中创建资源”。![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

流规则：

| **仅限初始流** | **用作存在性测试** | **流（FIM 值 ⇒ 目标属性）**                          |
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

此同步规则在 Active Directory 中创建用户

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
| **Y**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=scontoso,DC=com"⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>同步规则：导入 B2B 来宾用户对象 SID 以便登录到 MIM 

此同步规则在 Active Directory 中创建用户

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

最后，我们邀请用户，然后按以下顺序运行管理：

-   MIMMA 管理代理上的完全导入和同步

-   ADMA_SCONTOSO_B2B 管理代理上的完全导入和同步

-   B2B Graph 管理代理上的完全导入和同步

-   ADMA_SCONTOSO_B2B 管理代理上的导出、增量导入和同步

-   MIMMA 管理代理上的导出、增量导入和同步

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>最后：B2B 来宾的应用程序代理和登录到 MIM

现在我们已经在 MIM 中创建了同步规则。 在应用代理配置中，定义使用云原则以允许在应用代理中使用 KCD。
此外，接下来将用户手动添加到管理用户和组。 在 MIM 中创建之前不向用户显示的选项可以在设置后将来宾添加到 Office 组，需要本文中未包含的一些配置。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **仅限初始流** | **用作存在性测试** | **流（FIM 值 ⇒ 目标属性）**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |

全部配置后

最后使 B2B 用户登录并看到应用程序

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>后续步骤
----------

[How Do I Provision Users to AD DS](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)（如何将用户预配到 AD DS）

[Functions Reference for FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)（FIM 2010 的函数参考）

[如何提供对本地应用程序的安全远程访问](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[下载用于 Microsoft Graph 的 Microsoft Identity Manager 管理代理（预览版）](http://go.microsoft.com/fwlink/?LinkId=717495)
