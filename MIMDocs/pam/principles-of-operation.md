---
title: "了解 PAM 组件 | Microsoft Docs"
description: "Privileged Access Management 与 MIM 共享某些组件，且拥有少数自身的组件。 了解它们如何协同工作。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 53fe79f251c3b18426f16b4007cda49e67d7b028
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="understand-the-components-of-pam"></a>了解 PAM 的组件

Privileged Access Management 将管理访问权限与日常的用户帐户分开。 该解决方案依赖于并行林：

- *CORP*：包含一个或多个域的通用公司林。 尽管你可能有多个 CORP 林，但为了简单起见，这些文章中的示例均假定使用包含一个域的单林。  
- *PRIV*：专门为此 PAM 方案创建的专用林。 该林包含一个可容纳在一个或多个 CORP 域中隐藏的特权组和帐户的域。

针对 PAM 配置的 MIM 解决方案包含以下组件：  

- **MIM 服务**：实现用于执行标识和访问管理操作的业务逻辑，包括特权帐户管理和提升请求处理。   
- **MIM 门户**：基于 SharePoint 的门户，由 SharePoint 2013 托管，可提供管理员管理和配置 UI。
- **MIM 服务数据库**：存储在 SQL Server 2012 或 2014 中，保留 MIM 服务所需的标识数据和元数据。
- **PAM 监视服务**和 **PAM 组件服务**：管理特权帐户的生命周期和在组成员身份生命周期中协助 PRIV AD 的两个服务。
- **PowerShell cmdlet**：供 PAM 管理员填充 MIM 服务和 PRIV AD（其用户和组对应于 CORP 林中的用户和组），以及用于最终用户在管理帐户中请求实时 (JIT) 使用权限。
- **PAM REST API 和示例门户**：供开发人员将 PAM 方案中的 MIM 与自定义客户端集成以进行提升，而无需使用 PowerShell 或 SOAP。 通过示例 Web 应用程序演示了 REST API 的使用。

完成安装和配置后，由 PRIV 林中迁移过程创建的每个组都是基于 SIDHistory 的影子安全组（在带有 Windows Server vNext 的将来更新中是外部主体组），该安全组会镜像原始 CORP 林中的 SID 组。 此外，当 MIM 服务向 PRIV 林中的这些组添加成员时，这些成员身份将有时效性。

因此，当用户使用 PowerShell cmdlet 请求提升并且其请求获得批准时，MIM 服务会将他们在 PRIV 林中的帐户添加到 PRIV 林中的组。 当用户使用其特权帐户登录时，他们的 Kerberos 令牌将包含一个与 CORP 林中的组的安全标识符 (SID) 相同的 SID。 由于 CORP 林配置为信任 PRIV 林，用于访问 CORP 林中的资源的提升帐户，因此对于检查 Kerberos 组成员身份的资源来说，看起来是该资源的安全组的成员。 这可通过 Kerberos 跨林身份验证提供。

此外，这些成员身份是有时效性的，因此在预配置的时间间隔后，用户的管理帐户将不再属于 PRIV 林中的组。 因此，该帐户不可再用于访问其他资源。
