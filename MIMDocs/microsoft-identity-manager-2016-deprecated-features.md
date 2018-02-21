---
title: "MIM 弃用功能和未来规划 | Microsoft Docs"
description: "本文记录 MIM Identity Manager 2016 SP1 已弃用的功能。"
keywords: 
author: barclayn
ms.author: davidste
manager: mbaldwin
ms.date: 1/31/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: d1e016c45261be5fa9c4dba67ead7f88aa14890b
ms.sourcegitcommit: 24746cf23b4688b3f8290519527259fc469e0373
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/09/2018
---
# <a name="deprecated-features"></a>已弃用的功能

本文记录 Microsoft Identity Manager 2016 SP1 已弃用的功能。 如果 Microsoft Identity Manager 中仍有某个功能，则仍然支持该功能。 不建议为新部署使用这些功能，因为可能会在某个未来版本中删除它们。  我们不建议开发人员在任何新的应用程序或解决方案中使用已弃用的功能。

>[!NOTE]
使用 ** 对 Microsoft Identity Manager SP1 中已删除的特性和功能进行了标识。 <br>
有关详细信息，请参阅 [Microsoft Identity Manager 的支持周期](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

Microsoft 不建议客户开始进行 Microsoft BHOLD 套件组件的新部署。 BHOLD 的现有部署将继续受到支持。 Azure AD 现在提供[访问评审](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview)，替换某些 BHOLD 证明活动功能。

## <a name="certificate-management"></a>证书管理 
| “类别”                | **已弃用的功能**              | **替换和注释**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| 管理代理 | **FIM 证书管理 | MIM 2016 中已删除了 FIM 证书管理代理。                                                             |

## <a name="service-and-portal"></a>服务和门户

| “类别”                | **已弃用的功能**              | **替换和注释**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| 编程配置 | Web 服务配置界面（ma-data 和 mv-data） | 将在下一个版本中删除通过 FIM 服务 Web 服务配置 FIM 同步服务的功能。
|

## <a name="synchronization-service"></a>同步服务 

| “类别”                | **已弃用的功能**              | **替换和注释**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| 编程配置 | Web 服务配置界面 | 将在下一个版本中删除通过 FIM 服务配置 FIM 同步服务的功能。                                                          |
| 管理代理           | 内置 MA                        | MIM 2016 中已删除以下 MA： </br> 1.  **适用于 FIM 证书管理的 MA </br>2.  **适用于 Lotus Notes 的 MA</br> 3.  * *适用于 SAP R/3 的 MA </br> Lotus Notes 和 SAP R/3 的 MA 已替换为新版本。 有关详细信息，请参阅[最新连接器版本发行历史记录和下载](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| 管理代理           | ECMA1                               | 已将 ECMA1/XMA 扩展性框架替换为 ECMA 2.0。 需要使用 ECMA2.0 连接器更新现有的 ECMA1 管理代理。                                                                                                                                          |
| 管理代理           | 在进程外运行连接器      | 不会替换此功能。 同步服务将始终调用同一进程中的连接器。 连接器负责启动和管理另一个进程。 |
| 管理代理           | 配置分区显示名称    | 不会替换此功能。 此选项仅用于提供 WMI 接口中某个分区的备用名称。                                                                                                                                                                       |
| 运行配置文件                | 组合的配置文件                   | 将删除组合配置文件的增量导入/同步、完全导入/增量同步和完全导入/同步。 应改为通过两个步骤运行配置文件。 

>[!NOTE]
应仅在性能将受到大量现有断电器影响的环境中保留组合的运行配置文件。


| “类别”                | **已弃用的功能**              | **替换和注释**           |
|--------|-------|---|    
| 属性优先顺序 | 多控制/等优先级                       | 将删除“等优先级”这一功能。 此功能没有替代项。 应改为手动配置优先级。 如果你的环境部署了 FIM 服务管理代理，则你可以继续使用此功能。 此管理代理不提供手动配置优先级，以免声明性预配出现导出非优先现象。 |
| 联接规则           | 联接“任何”对象类型                             | 不会替换此功能。 所有联接规则均应显式定义他们尝试联接的 Metaverse 对象类型。       |
| 属性流      | 为导出的值取消选择“允许 NULL 值”            | 不会替换此功能。 会始终选择“允许 NULL 值”。 应确保在当前环境中选中“允许 NULL 值”。  |
| 属性流      | “不撤回属性”                            | 不会替换此功能。 将始终撤回属性，这是最佳做法。  |
| 规则扩展      | 在进程外运行 metaverse 和 ma 规则扩展 | 不会替换此功能。 Metaverse 和属性流规则将在与同步引擎相同的进程中运行。       |
| 规则扩展      | 事务属性                                | 不会替换此功能。 应避免使用此实用工具类在入站同步、预配同步和出战同步间传递数据。  |
| 规则扩展      | ExchangeUtils: Create55\* 方法                     | 将删除为 Exchange 5.5 服务器创建对象的方法。        |
| 介面            | Mms_Metaverse                                        | 在下一个版本中，将删除所有 ClmUtils 类成员。   |

## <a name="next-steps"></a>后续步骤
了解详细信息：

- Microsoft Identity Manager 与其前身 Forefront Identity Manager 仍然紧密相关。 如果仍然使用 FIM，或者想要引用其他文档，请参阅 [FIM 2010 R2 文档路线图](https://technet.microsoft.com/library/jj133885.aspx)。
- [部署 MIM 的拓扑注意事项](topology-considerations.md) 本文介绍了多种可以考虑实施的部署拓扑。
- [容量计划指南](capacity-planning-guide.md) 可以利用本指南以及测试环境，了解针对你的部署的适当范围。
