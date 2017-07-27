---
title: "支持的连接器 | Microsoft Docs"
description: "使用连接器来管理 MIM 与目录之间的数据传输。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b26fe7bc56ab8229054afb1409c3652e81464a3d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="connect-to-your-directories"></a>连接到目录

连接器将特定已连接数据源链接到 Microsoft Identity Manager (MIM)。 连接器将数据从已连接数据源移动到 MIM。 对 MIM 中的数据进行了修改时，连接器还可以将数据导出到已连接数据源，以使其与 MIM 保持同步。 通常，每个已连接目录至少有一个连接器。

在 Forefront Identity Manager 中，连接器被称为管理代理。 某些文章或部分产品中仍使用了该术语，但这两个术语是指同一概念。

本文介绍的是 MIM 中包含的连接器，但用于 Extensible Connectivity 2.0 的连接器可实现与甚至更多数据源的连接。 某些合作伙伴按照这种方式创建了其自己的连接器，wiki [FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx)（FIM 2010：合作伙伴的管理代理）中提供了相关完整列表。

## <a name="supported-connectors-in-mim-2016"></a>MIM 2016 中支持的连接器

| Name | 支持的已连接数据源版本 |
| ---- | ----------------------------------------------- |
| Active Directory 域服务 | Active Directory 2000、2003、2003 R2、2008、2008 R2、2012 |
| Active Directory 轻型目录服务 (ADLDS) | Active Directory 轻型目录服务 (ADLDS) |
| Active Directory 全局地址列表 (GAL) | Active Directory 全局地址列表 (GAL) – Exchange 2000、2003、2007、2010、2013 |
| Extensible Connectivity 2.0 | 任何基于呼叫或基于文件的数据源 |
| MIM 服务 | Microsoft Docs 2016 |
| IBM DB2 Universal Database | IBM DB2 版本 9.1、9.5 或 9.7；IBM DB2 OLEDB v9.5 FP5 或 v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory 版本 8.7.3、8.8.5 和 8.8.6 |
| Oracle 数据库 | Oracle 数据库 10 g 或 11g；64 位客户端 |
| Microsoft SQL Server | SQL Server 2000、2005、2008、2008 R2、2012 |
| Oracle（以前的 Sun 和 Netscape）Directory Server | Sun Directory Server 6.x、7.x 和 Oracle 11 |
| [适用于 FIM 2010 R2 的 Windows PowerShell 连接器](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 或更高版本 |
| [适用于 FIM 2010 R2 的 Microsoft Azure Active Directory 连接器](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [适用于 FIM 2010 R2 的通用 LDAP 连接器](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | LDAP v3 服务器（与 RFC 4510 兼容） |
| [适用于 Lotus Domino 的连接器](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes v8.0.x 或 v8.5.x 版 |
| [适用于 FIM 2010 R2 的 SharePoint 服务连接器](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | 含 User Profile Service Application (UPA) 的 SharePoint Server 2013 或 2016 |
| [适用于 Web 服务的连接器](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 或 6.0；Oracle PeopleSoft 9.1；Oracle 电子商务 12.1 |
| 属性值对文本文件 | 属性值对文本文件 |
| 带分隔符的文本文件 | 带分隔符的文本文件 |
| 目录服务标记语言 (DSML) | 目录服务标记语言 (DSML) 2.0 |
| 定宽文本文件 | 定宽文本文件 |
| LDAP 数据交换格式 (LDIF) | LDAP 数据交换格式 (LDIF) |

## <a name="related-topics"></a>相关主题

[FIM 2010 R2 中的管理代理](https://technet.microsoft.com/library/jj133885.aspx)
