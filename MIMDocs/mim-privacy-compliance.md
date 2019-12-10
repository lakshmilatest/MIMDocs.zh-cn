---
title: Microsoft Identity Manager 数据处理 | Microsoft Docs
description: 了解 Microsoft Identity Manager 数据处理以标识和报告环境内的数据，并基于操作功能和要求在给定的系统中采取措施。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 12/02/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: 6f861c5b1984de70a91edcac89276402f289e355
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "68701485"
---
# <a name="microsoft-identity-manager-data-handling"></a>Microsoft Identity Manager 数据处理 

本文为组织如何作出可应用于多个连接的数据源的决定提供了指导。  这可以通过搜索、删除、更新和报告操作来实现。  在决定删除或更新方法前，了解身份管理器系统 (MIM) 的当前设计和配置至关重要。 

下面介绍一些方案，客户将需要考虑并回答以下问题： 

- 需要什么数据来进行身份管理以帮助完成业务流程？
- 当前数据将存储在 MIM 中的哪个位置？
- 将如何在系统中使用此数据？
- 是否要与任何外部合作伙伴数据源共享此数据（导出）
- 数据及其处理的权威来源是什么？
- 数据保留和数据删除计划将会就绪吗？
- 是否已找到处理和管理数据所需的所有技术？

为了帮助了解当前的 MIM 环境，你可以利用以下工具来记录 MIM 环境，或遵从实施设计文档。
- [MIM 文档管理程序 - 允许导出当前配置](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>搜索并标识个人数据
在 MIM 中搜索数据将取决于配置和设置。 大多数环境是互连的，但会为清楚起见，我们按照高级别组件对其进行分类。

### <a name="synchronization-service"></a>同步服务

MIM 中与用户相关的所有数据均源自 Active Directory (AD) 和 HR 数据源。 在搜索个人数据时，应首先考虑的搜索位置是 AD 或已连接数据源。 

如果不确定权威来源，则可以从 MIM Synchronization Service Manager 控制台跟踪此用户，单击“Metaverse 搜索栏”来查看存储在数据库中的个人身份数据。 用户可搜索特定的用户或属性。

- 执行审阅或搜索用户对象数据
    - 打开“同步服务客户端”
        - 使用 metaverse 设计器可查看属性流导入和优先级。
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - 使用 metaverse 搜索可搜索数据库中的任何对象和属性 ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
找到对象后，单击该对象将打开用户配置文件页。 对象详细信息会提供以下内容的全面详细信息：对象、其属性、最后修改时间和权威来源及从以下管理代理配置示例派生的相关已连接数据源。

![mim-privacy-compliance.PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>服务和门户 / PAM
如果安装了服务和门户或 PAM 的实例，那么能够搜索用户非常重要。 

如果安装了门户，则可以使用 UI 来搜索任何属性或查询特定用户。

如果只安装了服务服务器（不带门户 UI），则可基于 [FIMAutomation PSSnapin] 运行搜索语法，可在[此处](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx)找到示例。

PAM 可以使用上述相同的语法，或者可以使用 [MIMPAM 模块](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1)，特别是 get-pamuser cmdlet 来搜索 PAM 环境中的用户。

搜索可用数据的其他报告选项位于服务和门户中。
- [混合报告](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [通过 SCSM 进行报告](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
Bhold Core 服务有一个可用来搜索用户或属性的 UI。 

![bhold 搜索](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

如果正在将 BHOLD 与[访问管理连接器](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install)同步以获取同步服务，用户将能够看到已连接的用户对象以及发送到 BHOLD Core 的属性。

此外，还可以加载 BHOLD 报告模块。

- [BHOLD 报告](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>证书管理
证书管理服务搜索内置于该 UI。 管理员将启动并选择“查找用户并查看或管理其信息”  

![cm 搜索](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>导出个人数据
由于与 MIM 中的实体相关的数据派生自多个源，因此，大多数数据均存储在同步服务数据库中。 为此，应从 MIM 同步导出对象相关数据，或者你可以确定此数据的所有者。

### <a name="synchronization-service"></a>同步服务
用于导出数据的同步服务只需从搜索 UI 选择该数据并复制粘贴到 csv 或首选格式。 导出此数据的另一种方法是创建基于文件的 MA，以删除所需的关于感兴趣的标记用户的最新数据。 可在[此处](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/)找到使用基于文件的 MA 的示例。


### <a name="service-and-portal--pam"></a>服务和门户 / PAM
借助服务和门户以及 PAM，你可以导出此数据，基于 [FIMAutomation PSSnapin] 运行搜索语法（可在[此处](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx)找到示例），并将其发送到 [csv](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6)。

PAM 可以使用上述相同的语法，或者可以使用 [MIMPAM 模块](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1)，特别是 get-pamuser 来搜索 PAM 环境中的用户并将其发送到 csv。

- [示例：使用 PowerShell 查询 MIM 服务](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
可以使用 bhold 报告模块将 Bhold 数据导出为首选格式。

### <a name="certificate-management"></a>证书管理
将与个人数据相关的证书管理数据连接到 Active Directory。 管理员可以使用 Active Directory powershell 导出此数据。

## <a name="updating-personal-data"></a>更新个人数据

有关 MIM 解决方案中的用户或对象的个人数据通常派生自组织的已连接数据源中用户的对象。 因为对 HR 源中的用户配置文件或记录的另一个权威系统（例如 AD）进行的任何更改之后都将反映在 MIM 同步服务中。

### <a name="synchronization-service"></a>同步服务

为了执行管理操作，管理员必须是同步操作的一部分或[此处](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10))定义的管理员。

通过定义来自权威来源中的规则来完成数据更新。 管理控制台帮助识别权威来源以在源上对其进行更新。 另一种选择是创建同步规则或规则扩展来控制数据更新（如果 HR 数据等来源仍需要保留）。 这些是可用的支持选项。

有关更新属性的不同方式的详细信息，请参阅以下内容。 

- [使用规则扩展](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Understanding Data Synchronization with External Systems](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))（了解如何与外部系统进行数据同步）

### <a name="service-and-portal--pam"></a>服务和门户/PAM

可以使用 FIMAutomation 或 PAM cmdlet 来更新要包含 PAM 数据的服务和门户。 如果有门户，也可以通过搜索直接进行更新并修改该对象。 有一点需要注意，根据配置，只从门户进行更新并不意味着它会保持不变。 因为权威来源很大程度上取决于整体配置。

### <a name="bhold"></a>BHOLD

可以使用 BHOLD Core 用户界面或访问管理连接器直接更新用户。

### <a name="certificate-management"></a>证书管理

证书管理服务中的用户在 Active Directory 中均有反映。 若要更新，请使用 Active Directory 更改对象详细信息。

## <a name="deleting-personal-data"></a>删除个人数据

>[!Note] 
> 本文将提供有关如何从 Microsoft Identity Manager 删除个人数据的指南，并可用来支持你在 GDPR 下的义务。 如果正在查找有关 GDPR 的常规信息，请参阅[服务信任门户的 GDPR 部分](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)。

MIM 中的数据从其连接的数据源同步并始终从其进行更新。 在目标中删除一个对象时，出于安全调查的目的，可以保留该对象在 MIM 中的数据。 对象删除是根据连接的数据源规则或规则扩展（代码）和/或对象删除规则进行配置的。

### <a name="synchronization-service"></a>同步服务
同步服务如同许多其他方法一样依据业务流程来处理数据或删除数据。 为了便于理解，下面介绍一些文章来帮助了解有关删除和更新属性的方法： 

- [Understanding Deprovisioning](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)（了解取消设置）
- [使用规则扩展](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [MIM 最佳做法](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>服务和门户/PAM

建议采用服务和门户，这会保留默认 30 天的系统资源保留配置。 它会告诉服务何时将被删除，不仅是请求数据，而且包括需要从系统中清除的所有对象。 一旦发生此过程，将删除链接到此对象的所有数据，其中包括所有 SSPR 注册数据。 这将进入上面的对象删除配置。 我们确实有一个存储对象 guid 的表。 为了减少版本 4.4.1459 中表的总体大小，我们添加了称为 FIM_DeleteExpiredSystemObjectsJob 的过程。可在[此处](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden)找到有关此过程的详细信息。

![mim-privacy-compliance-srrc.PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold 等连接到同步服务的大多数系统可以配置为在删除 HR 等源对象后删除。 这是在管理代理上进行配置的， 并由同步服务功能下所述的对象删除规则控制。

另一种方法是直接从 BHOLD Core 用户界面删除用户对象。 根据设置，这可以正常工作，但请注意，如果未在源上删除预配逻辑，它可能会重新创建此用户。
![mim-privacy-compliance-bholdr.PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>证书管理
若要从 CM 删除用户，请在 Active Directory 中删除该用户。

证书管理，因为它将仅存储带有域 sAMAccountName 的证书服务的配置文件 uid。 从 AD 中删除用户后，用户缓存仅存在于他们已注册的证书中。 不建议删除数据库中的任何内容，因为这可能会在整体上对环境的操作造成负面影响。

## <a name="opt-out-of-telemetry"></a>选择退出遥测
早期版本的 FIM/MIM 用于收集有关每个部署的匿名遥测，并通过 HTTPS 将此数据传输到 Microsoft 服务器。 过去，Microsoft 利用该数据来帮助改进未来版本的 FIM/MIM。

>[!Note] 
> 在稍后的 4.5.x.x 版本或更高版本中，数据收集将被禁用。

若要在早期版本中禁用数据收集，请运行更改模式，并取消选择以下提示：

![mim-privacy-compliance-ceip.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

或编辑注册表，并将值设置为 0：(Component)CEIP HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010

![mim-privacy-compliance-ceip2.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>后续步骤 
- [有关 SQL 相关隐私指南](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [服务信任门户的 GDPR 部分](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 存档：提升 - 实现 Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
