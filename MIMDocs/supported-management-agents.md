---
title: 支持的连接器 | Microsoft Docs
description: 使用连接器来管理 MIM 与连接的数据源之间的数据传输。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 1/24/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 1f7d4150ce7012cd4726126ba50b1ab0f94f474c
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690677"
---
# <a name="connect-to-your-directories"></a>连接到目录

连接器将特定已连接数据源链接到 Microsoft Identity Manager SP1 (MIM)。 连接器将数据从已连接数据源移动到 MIM。 对 MIM 中的数据进行了修改时，连接器还可以将数据导出到已连接数据源，以使其与 MIM 保持同步。 通常，每个已连接目录至少有一个连接器。

在 Forefront Identity Manager 中，连接器被称为管理代理。 某些文章或部分产品中仍使用了该术语，但这两个术语是指同一概念。

本文介绍的是 MIM 中包含和支持的连接器，但用于 Extensible Connectivity 2.0 的连接器可实现与甚至更多数据源的连接。 一些合作伙伴已按照这种方式创建了自己的连接器，wiki [FIM 2010：合作伙伴的管理代理](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx)中提供了相关完整列表。

## <a name="supported-connectors-in-mim-2016-sp1"></a>MIM 2016 SP1 中支持的连接器

| 名称 | 支持的已连接数据源版本和技术链接 |
| ---- | ----------------------------------------------- |
| Active Directory 域服务 | Windows Server 2012 和 Windows Server 2016 中的 Active Directory |
| Active Directory 轻型目录服务 (ADLDS) | Active Directory 轻型目录服务 (ADLDS) |
| Active Directory 全局地址列表 (GAL) | Active Directory 全局地址列表 (GAL) - Exchange 2013、Exchange 2016 |
| Extensible Connectivity 2.0 | 任何基于呼叫或基于文件的数据源 |
| FIM 服务 | MIM 服务。 请注意，MIM 同步服务和 MIM 服务必须是同一版本。 |
| IBM DB2 Universal Database | IBM DB2 版本 9.5 或 9.7；IBM DB2 OLEDB v9.5 FP5 或 v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory 版本 8.7.3、8.8.5 和 8.8.6 |
| Oracle 数据库 | Oracle 数据库 10 g 或 11g；64 位客户端 |
| Microsoft SQL Server | SQL Server 2012、2014、2016 |
| Oracle（以前的 Sun 和 Netscape）Directory Server | Sun Directory Server 6.x、7.x 和 Oracle 11 |
| [Windows PowerShell 连接器](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 或更高版本 |
| [Microsoft Azure Active Directory 连接器](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory（不建议用于新部署） |
| [通用 LDAP 连接器](https://msdn.microsoft.com/library/dn510997.aspx) | [LDAP v3 服务器（与 RFC 4510 兼容）](reference/microsoft-identity-manager-2016-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| [通用 SQL 连接器](reference/microsoft-identity-manager-2016-connector-genericsql.md) | [所有 64 位 ODBC 驱动程序均支持该连接器](reference/microsoft-identity-manager-2016-connector-genericsql.md#overview-of-the-generic-sql-connector) |
| [适用于 Lotus Domino 的连接器](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes v8.5.x 版 |
| [SharePoint Services 连接器 UPA](https://msdn.microsoft.com/library/dn511003.aspx) | 含 User Profile Service Application (UPA) 的 SharePoint Server 2013 或 2016 |
| [适用于 Web 服务的连接器](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 或 6.0；Oracle PeopleSoft 9.1；Oracle eBusiness 12.1 以及其他 SOAP 和 REST API](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [属性值对文本文件](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | 属性值对文本文件 |
| [带分隔符的文本文件](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | 带分隔符的文本文件 |
| [目录服务标记语言 (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | 目录服务标记语言 (DSML) 2.0 |
| [定宽文本文件](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | 定宽文本文件 |
| [LDAP 数据交换格式 (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP 数据交换格式 (LDIF) |
| [Microsoft Graph 连接器](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>相关主题

[FIM 2010 R2 中的管理代理](https://technet.microsoft.com/library/jj133885.aspx)
