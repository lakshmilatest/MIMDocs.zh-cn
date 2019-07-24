---
title: BHOLD SP1 安装 | Microsoft Docs
description: BHOLD SP1 安装文档
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 05eb2afc0ddbf6104e27a5c24e121a55bd805292
ms.sourcegitcommit: 4c4bc7aa42cd5984c838abdd302490355ddcb4ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2019
ms.locfileid: "68238900"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Microsoft BHOLD 套件 SP1 (6.0) 安装指南

Microsoft® BHOLD 套件服务包 1 (SP1) 是应用程序的集合，与 Microsoft Identity Manager 2016 SP1 (MIM) 配合使用时，可以将有效的角色管理、分析和证明添加到 MIM。 Microsoft BHOLD 套件 SP1 包括以下模块：

- BHOLD 核心
- 访问管理连接器
- BHOLD FIM/MIM 集成
- BHOLD 模型生成器
- BHOLD 分析
- BHOLD 报表
- BHOLD 证明


> [!NOTE]
> 适用对象  ：Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>本文档包括

本文档介绍如何规划 BHOLD 部署以满足业务需求，及如何安装每个 BHOLD 模块。 对于每个模块，详细介绍了相关的硬件、基础结构、软件要求、预安装的网络配置、安装过程中所需的信息和安装后的步骤（如有上述内容）。

## <a name="pre-requisite-knowledge"></a>必备知识

本文档假定你已基本了解如何在服务器计算机上安装软件。 还假定你具有 Active Directory® 域服务、Microsoft Identity Manager SP1 (FIM) 和 Microsoft SQL Server 2012 数据库软件的基本知识。 有关如何设置和配置 AD DS 和 FIM 等相关技术未在本文档中说明。 有关 Microsoft BHOLD 模块执行的函数的信息，请参阅 [the Microsoft BHOLD suite concepts guide](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)（Microsoft BHOLD 套件概念指南）。

## <a name="audience"></a>受众

本文档适用于打算部署 Microsoft BHOLD 套件的 IT 规划人员、系统架构师、技术决策者、顾问、基础结构规划人员和 IT 人员。

## <a name="bhold-infrastructure-considerations"></a>BHOLD 基础结构注意事项

大多数情况下，BHOLD 和 FIM 用于大型基础结构环境中。 可以定制 BHOLD 和 FIM 体系结构，满足特定业务需求。 以下部分提供了一些可行的体系结构解决方案。 本概述中未完整列出所有可能选项，但建议了在网络中部署 BHOLD 可以采用的方式。
 
此部分说明了以下主题：

- 单服务器体系结构
- 双服务器体系结构
- 双层体系结构
- SQL Server 建议

### <a name="single-server-architecture"></a>单服务器体系结构

用于小型组织中的部署或者用于开发时，可以在与 SQL Server 和 AD DS 相同的服务器上安装 BHOLD 和 FIM，如下图所示。
 
![单服务器体系结构](media/bhold-installation-guide/single.png)

BHOLD 套件 SP1 和 FIM 门户一起安装在一台服务器上时，必须为 BHOLD 和 FIM 在 DNS 中创建其他主机别名（CNAME 或 A 记录）。 这样可以为 BHOLD 和 FIM 服务创建单独的服务主体名称 (SPN)。 有关详细信息，请参阅 [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)（BHOLD 核心安装）。
有关采用单服务器配置安装 FIM 的指南，请参阅 Microsoft TechNet 库中的 [Common Configuration for Getting Started Guides](https://technet.microsoft.com/library/ff575965.aspx)（入门常用配置指南）。

### <a name="dual-server-architecture"></a>双服务器体系结构

如果在单独的服务器上安装 BHOLD 核心和 FIM，可以为不需要较复杂部署（例如多层体系结构提供的部署）的中等规模的组织提供更强的性能和更优的灵活性。 下图显示在自己的服务器上安装的 BHOLD 和 FIM；FIM 服务器还会运行 SQL Server，向 BHOLD 和 FIM 提供数据库服务。 在 FIM 服务器上运行的 FIM 同步服务会同步 FIM 数据库和 BHOLD 数据库之间的更改。 请注意，如果最终用户自助服务是必需的，则 BHOLD FIM 集成模块必须安装在与 FIM 服务和 FIM 门户相同的服务器上。 BHOLD FIM 集成模块需要 FIM 服务和 BHOLD FIM 集成模块安装在同一台服务器上。

![双服务器体系结构](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> BHOLD FIM 集成模块的报表功能要求 BHOLD 数据库和 FIM 数据库安装在同一个 SQL Server 实例上，并且 BHOLD 服务帐户必须拥有对 FIM 服务数据库的访问权限。

### <a name="two-tier-architecture"></a>双层体系结构

在大多数环境中，尤其是在性能很重要的环境中，应该在单独的服务器上运行 BHOLD 套件 SP1、FIM 和 SQL Server（双层体系结构）。 使用双层体系结构时，每层的内存和 CPU 资源都是专用的。 下图展示了一种用于配置双层体系结构的可能方式。 在 FIM 服务器上运行的 FIM 同步服务会同步 FIM 数据库和 BHOLD 数据库之间的更改。 请注意，如果最终用户自助服务是必需的，则 BHOLD FIM 集成模块必须安装在与 FIM 服务和门户相同的服务器上。

![双层体系结构](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>SQL Server 建议

在大型组织中部署 BHOLD 时，强烈建议遵守以下设置 Microsoft SQL Server 数据库的准则：

- 在独立于任何 FIM 或 BHOLD 服务的服务器上部署 SQL Server。
- 在物理磁盘级别上，将日志文件与数据文件隔离开。
- 如果使用 RAID 提供存储冗余，请使用 RAID 级别 10 (1+0)。 请勿使用 RAID 级别 5。
- 对运行 SQL Server 的服务器使用超过 2 GB 的物理内存时，请确保配置正确的设置。

有关 SQL Server 最佳做法的详细信息，请参阅 Microsoft TechNet 库中的 [Storage Top 10 Best Practices](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx)（10 大最佳存储做法）。

### <a name="trusted-certificates-list-update"></a>受信任证书列表更新

Windows 可以配置为在启动服务前验证证书链。 在此类系统上，如果使用未在服务器受信任的证书列表 (TCL) 中的证书对服务的可执行代码进行签名，则无法启动该服务。 Microsoft BHOLD 套件 SP1 软件是使用源自 Microsoft 根证书颁发机构 2010 证书的代码签名证书链进行代码签名的。
Windows 可以配置为通过 Internet 连接从 Microsoft 检索根证书。 但是在断开连接的系统上，Windows Server 仅包含在 Windows 发布前存在于根程序中的证书。 在 Windows Server 2010 之前的 Windows Server 版本中，这些证书不包含验证 BHOLD 套件 SP1 代码签名证书链所需的根证书。 如果打算在可能不具有最新 TCL 的系统上安装一个或多个 Microsoft BHOLD 套件 SP1 模块，则必须下载并安装根更新包，或使用组策略安装根更新包，然后才能安装 BHOLD 套件 SP1模块。 有关详细信息，请参阅 [Windows 根证书计划成员](http://support.microsoft.com/kb/931125)。

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>在 Windows Server 2012/2016 上安装 BHOLD 套件 SP1 所需的步骤 

![IIS 安装 BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

如果在 Windows Server 2012 或 2016 中安装 BHOLD 套件 SP1，则 BHOLD 网页不可用，除非修改 ```C:\Windows\System32\inetsrv\config``` 中的 applicationHost.config 文件。 在 ```<globalModules>``` 部分中，将 ```preCondition="bitness64``` 添加到以 ```<add name="SPNativeRequestModule"``` 开头的项，之后该项如下所示：

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

编辑并保存文件后，运行 iisreset 命令，重置 IIS 服务器。


## <a name="upgrading-bhold-suite"></a>升级 BHOLD 套件

不能升级现有 BHOLD 套件安装。 而是必须先卸载现有的 BHOLD 套件安装，然后才能更新 BHOLD 模块。 如果具有现有的 BHOLD 角色模型，则安装更新的 BHOLD 核心模块时，可以升级 BHOLD 数据库并使用。 有关详细信息，请参阅 [Replacing BHOLD Suite with BHOLD Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx)（将 BHOLD 套件替换为 BHOLD 套件 SP1）。


## <a name="next-steps"></a>后续步骤

- [BHOLD 开发人员参考](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD 版本历史记录](../reference/version-bhold-history.md)
