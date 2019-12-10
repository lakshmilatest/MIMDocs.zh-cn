---
title: 规划 TLS 1.2 环境中的 Microsoft Identity Manager 2016 | Microsoft Docs
description: 规划 TLS 1.2 环境中的 Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/26/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b574a9c8536a460cdddf57131a821775672c082a
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "73383912"
---
# <a name="planning-mim-2016-sp2-in-tls-12-or-fips-mode-environments"></a>规划 TLS 1.2 或 FIPS 模式环境中的 MIM 2016 SP2


> [!IMPORTANT]
> 本文仅适用于 MIM 2016 SP2

在已禁用除 TLS 1.2 之外的所有加密协议的锁定环境中安装 MIM 2016 SP2 时，要满足以下要求：
- MIM 组件需要 Windows Server 和 .NET Framework 的最新更新，以安装 .NET 3.5 Framework 中的 TLS 1.2 支持，建立安全的 TLS 1.2 连接。 可能需要  [启用 .NET Framework 的 SystemDefaultTlsVersions](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the-net-framework)，和/或在客户端和服务器子项的注册表中[禁用除 TLS 1.2 之外的所有 SCHANNEL 协议](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings)，具体取决于服务器配置。  

## <a name="mim-synchronization-service-sql-ma"></a>MIM 同步服务，SQL MA

- MIM 同步服务和内置的 SQL 管理代理需要 [SQL Native Client 11.0.7001.0](https://www.microsoft.com/download/details.aspx?id=50402) 或更高版本，以便 SQL Server 与 TLS 1.2 建立安全连接。

## <a name="mim-service"></a>MIM 服务
- 在仅 TLS 1.2 环境中，MIM 服务无法使用自签名证书。 安装 MIM 服务时，选择受信任的证书颁发机构颁发的强加密兼容证书。
- 此外，MIM 服务安装程序还需要 [OLE DB Driver for SQL Server 版本 18.2](https://www.microsoft.com/download/details.aspx?id=56730) 或更高版本。

## <a name="fips-mode-considerations"></a>FIPS 模式注意事项

若在启用了 FIPS 模式的服务器上安装 MIM 服务，必须禁用 FIPS 策略验证，以允许执行 MIM 服务工作流。 若要执行此操作，请将 enforceFIPSPolicy enabled=false 元素添加到 Microsoft.ResourceManagement.Service.exe.config 文件的 runtime 部分，将其置于 runtime 和 assemblyBinding 部分之间，如下所示：     

```XML
<runtime>
<enforceFIPSPolicy enabled="false"/>
<assemblyBinding ...>
```    