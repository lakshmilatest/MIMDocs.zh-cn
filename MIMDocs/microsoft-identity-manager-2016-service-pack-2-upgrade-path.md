---
title: 从 FIM 2010 R2 和 MIM 2016 SP2 升级到 Microsoft Identity Manager 2016 Service Pack 2 | Microsoft Docs
description: 了解如何升级你的 FIM 2010 R2 或 MIM 2016 SP2 组件，然后安装 MIM 2016 中的新增组件。
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 09/16/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: bdf34be4841b1a911fdb61673e5a3855e66e7320
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "73568056"
---
# <a name="mim-2016-sp2-upgrade--from-forefront-identity--or-microsoft-identity-manager"></a>从 Forefront Identity 或 Microsoft Identity Manager 升级到 MIM 2016 SP2

组织可以从 Microsoft Identity Manager 或 Forefront Identity Manager 的早期版本升级到 Microsoft Identity Manager 2016 SP2。  本文的各个部分介绍了支持的升级路径。

有多个升级选项可用。 若已在运行 MIM 2016 且无需刷新基础平台（Windows Server、SQL、SharePoint、SCSM DW），或者使用组托管的服务帐户运行 MIM 服务且不使用 MIM 语言包，则就地升级/修补程序 (.msp) 安装将是最简单的选项。 反之，建议使用完全安装。

> [!IMPORTANT]
> 请先查看更新的[软件必备项](prepare-server-ws2016.md#software-prerequisites)部分，然后再安装 MIM 2016 SP2

## <a name="upgrade-from-fim-2010-r2-sp1-or-later-fim-builds"></a>从 FIM 2010 R2 SP1 或更高版本的 FIM 版本升级

> [!NOTE]
> FIM 2010 R2 SP1（内部版本 4.1.3419.0）是支持直接升级到 MIM 2016 SP2 的最低 Forefront Identity Manager 版本。 不支持从 FIM 早期版本直接升级到 MIM 2016 SP2。 若运行的是 4.1.3419.0 之前的 FIM 版本，必须先升级到 FIM 2010 R2 SP1，然后再升级到 MIM 2016 SP2。

1. **选项 1：使用现有数据库进行完全安装**
    1. 创建 FIMSynchronizationService 和 FIMService 数据库的备份副本。
    1. 导出已更改的所有 FIM 服务 RCDC 对象及 RCDC 资源字符串。
    1. 导出同步服务加密密钥。
    1. 备份 miisserver.exe.config、同步服务器“扩展”文件夹、Microsoft.ResourceManagement.Service.exe.config，因为 MIM 安装程序可能会重写对配置文件的自定义更改。
    1. 卸载所有 FIM 组件，包括语言包（首先卸载它）。 
    1. *可选：* 将 FIM 数据库移到其他 SQL 服务器。 建议在 MIM 服务器上创建 SQL 别名，使用这些别名代替真实的 SQL Server 名称，以便于将来进行 MIM 数据库迁移。
    1. 按照 [MIM 部署指南](microsoft-identity-manager-deploy.md)在同一台或其他服务器上从 .iso 安装媒介安装 MIM 2016 SP2，出现提示时选择使用现有数据库，提供先前导出的同步服务加密密钥。
    1. 检查 miisserver.exe.config 和 Microsoft.ResourceManagement.Service.exe.config 文件是否缺少 .net 重定向或已添加的自定义部分。
    1. 安装 MIM 2016 SP2 语言包（如有必要）。
    1. 将自定义项重新提交到 RCDC 对象和 RCDC 资源字符串，必要时重新提交本地化。
    1. 升级 FIM 外接程序和密码重置客户端，若 MIM 服务服务器名称更改则提供新的 MIM 服务服务器名称。
    
## <a name="upgrade-from-previous-mim-2016-builds"></a>从 MIM 2016 早期版本升级
1. 创建 FIMSynchronizationService 和 FIMService 数据库的备份副本。
1. 导出对 RCDC 对象和 RCDC 资源字符串执行的所有自定义 FIM 服务本地化。
1. 导出同步服务加密密钥。
1. 备份 miisserver.exe.config、同步服务器“扩展”文件夹、Microsoft.ResourceManagement.Service.exe.config，因为 MIM 安装程序可能会重写对配置文件的自定义更改。
1. 卸载已使用的 MIM 语言包。
1. 停止 MIM 服务。
1. **选项 1：就地升级 - 修补程序安装**
    1. 应用 MIM 2016 SP2 同步服务[修补程序](https://www.microsoft.com/download/details.aspx?id=100412)
    1. 应用 MIM 2016 SP2 服务[修补程序](https://www.microsoft.com/download/details.aspx?id=100412)
    1. 检查 miisserver.exe.config 和 Microsoft.ResourceManagement.Service.exe.config 文件是否缺少 .net 重定向或任何必须添加的自定义部分。
    1. 安装 MIM 2016 SP2 语言包（如有必要）。
    1. 将自定义项重新提交到 RCDC 对象和 RCDC 资源字符串，必要时重新提交本地化。
    1. 升级 MIM 2016 外接程序和密码重置客户端。
1. **选项 2：使用现有数据库进行完全安装**
    1. 卸载所有 MIM 组件  。
    1. *可选：* 将 FIM 数据库移到其他 SQL 服务器。 建议在 MIM 服务器上创建 SQL 别名，使用这些别名代替真实的 SQL Server 名称，以便于将来进行 MIM 数据库迁移。
    1. 按照 [MIM 部署指南](microsoft-identity-manager-deploy.md)在同一台或其他服务器上从 .iso 安装媒介安装 MIM 2016 SP2，出现提示时选择使用现有数据库，提供先前导出的同步服务加密密钥。
    1. 检查 miisserver.exe.config 和 Microsoft.ResourceManagement.Service.exe.config 文件是否缺少 .net 重定向或任何必须添加的自定义部分。
    1. 安装 MIM 2016 SP2 语言包（如有必要）。
    1. 将自定义项重新提交到 RCDC 对象和 RCDC 资源字符串，必要时重新提交本地化。
    1. 升级 MIM 2016 外接程序和密码重置客户端，若 MIM 服务服务器名称更改则提供新的 MIM 服务服务器名称。

> [!NOTE]
> MIM 2016 SP2 之后的语言包更新将分发为修补程序（.msp 文件），因此无需卸载/重新安装语言包。

有关升级及数据库备份过程的详细信息，请参阅文章[升级到 FIM 2010 R2](https://docs.microsoft.com/previous-versions/mim/jj134291%28v%3dws.10%29)，此文适用于任何 FIM 或 MIM 升级过程。
