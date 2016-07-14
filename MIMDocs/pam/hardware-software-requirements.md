---
title: "硬件和软件要求 | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: a6bdf1b947ee3ebc4c9e89e74b2912697ebf1f60
ms.openlocfilehash: 77e7174e94ea8032c4e57155db489f493ce18177


---

除基础软件平台、足够内存或磁盘空间以及网络连接外，对硬件不做任何要求。 本文提供了基本部署的最低要求。 此文并非用于演示性能、可伸缩性或高可用性，并且不表示针对大型企业或生产环境的推荐的部署拓扑。

## 从软件包安装

下面的软件可从 TechNet 评估中心或 MSDN 下载：  
- Microsoft Identity Manager 2016
  - 服务和门户：包含适用于 MIM 服务和 MIM 门户以及 PAM 方案的安装程序
  - 加载项和扩展：包含适用于请求程序 PowerShell cmdlet 的安装程序

下面的软件可从 GitHub 下载：  
- PAMSamplePortal：包含适用于 REST API 的示例 Web 应用程序

## 所需软件

- Windows Server 2012 R2  
- Windows 8 1 企业版或 Windows 10 企业版  
- SQL Server 2012 Service Pack 1 或 SOL Server 2014  

## 评估软件

如果没有用于 Windows、SQL Server 或 Windows Server 的许可证，则可以下载评估版。

### TechNet 评估中心

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 企业版](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 企业版](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Microsoft 下载中心

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 及其必备组件](https://www.microsoft.com/download/details.aspx?id=42039)

## 硬件要求

对于 PAM 的每个组件，请参阅软件产品的系统要求。

对于 CORPDC：  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) 或更低版本

对于 CORPWKSTN：  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

对于 PRIVDC：  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

对于 PAMSRV：
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) 或 [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)



<!--HONumber=Jul16_HO2-->


