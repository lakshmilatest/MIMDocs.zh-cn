---
title: 支持的软件平台 | Microsoft Docs
description: 查找与各 MIM 2016 组件兼容的产品和版本
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: c6da739349f8c4ba4016635e326ed30d5f5954c6
ms.sourcegitcommit: 67e2de99f86e762125979233f6ee80afcd78dc4d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/25/2019
ms.locfileid: "56795411"
---
# <a name="supported-platforms-for-mim-2016"></a>MIM 2016 支持的平台

下表介绍了 Microsoft Identity Manager 2016 每个组件支持的平台和版本。 标有 * 的版本仅在包含最新修补程序的 MIM 2016 Service Pack 1 中受支持。  不建议使用标有“NR”的版本，虽然此版本受支持，但如果开始全新部署 MIM 平台，就不建议使用。


| **MIM 组件** | **平台** | **版本** |
|-------------------|--------------|--------------|
| **MIM 同步** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | 适用于用户预配、PCNS 和 GAL 同步的 Active Directory 功能级别 | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | MIM 同步数据库 | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | 适用于用户设置、PCNS 和 GAL 同步的 Active Directory（可选）|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | 适用于邮箱设置和 GAL 同步的 Exchange（可选）|Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | 开发环境（可选） | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | 其他已连接的系统（可选） | Active Directory 域服务<br/>Active Directory<br/>轻型目录服务<br/>SQL Server 2008 或更高版本<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> 其他第三方产品 |
| **MIM 服务和门户** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |PAM 方案：Windows Server | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |PAM 方案：适用于堡垒环境 PAM 林的 Active Directory | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |PAM 方案：适用于 PAM 方案现有 (CORP) 林的 Active Directory | Windows Server 2008 <br/> Windows Server 2008 R2* <br/> Windows Server 2012* <br/> Windows Server 2012 R2* <br/> Windows Server 2016 * |
| | MIM 服务数据库 | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 |
| | SharePoint | SharePoint Foundation 2010 (NR)<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | 适用于 MIM 服务审批和组管理电子邮件的邮件服务器（可选） | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016* <br/> Exchange Online *（仅在版本 [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490) 前通知） |
| | 浏览器 | 所有受支持的主流浏览器*（移动设备受限）|
| **MIM 服务报告** | Windows Server |  Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | 数据仓库 | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager*（带 4.4.1459）<br/> [System Center 2016 的 SQL Server 版本兼容性](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016) |
| **MIM 密码重置和注册门户** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Web 浏览器 | 所有受支持的主流浏览器 |
| **MIM 加载项和扩展** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Outlook 集成（可选） | Outlook 2010（在 Windows 上，即点即用除外）<br/>Outlook 2013（在 Windows 上，即点即用除外） <br/> Outlook 2016（在 Windows 10 上，即点即用除外）* |
| | PAM PowerShell 请求程序 cmdlet（可选） | Windows 8.1<br/>Windows 10 |
| **MIM 证书管理**（服务器和 CA 集成） | Windows server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | 证书颁发机构 | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | MIM CM 数据库 | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **MIM 证书管理**（应用程序） | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM 证书管理**（批量客户端） | Windows | Windows 7 |
| **MIM 证书管理**（基于客户端 ActiveX 的智能卡） | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> Windows 10 |
| **MIM BHOLD 套件** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | BHOLD 数据库 | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | 邮件服务器（可选） | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016* |
| | Web 浏览器 | Internet Explorer 支持使用 Silverlight 的浏览器 |
