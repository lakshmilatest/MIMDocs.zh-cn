---
title: Microsoft Identity Manager 授权和下载 | Microsoft Docs
description: 本文概述了 Microsoft Identity Manager (MIM) 2016 的授权方法，并提供了软件下载指针。
keywords: ''
author: markwahl-msft
ms.author: mwahl
manager: femila
ms.date: 10/18/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: e0bfd868345b8e7dcc6a02e745d3ccbf632a6c58
ms.sourcegitcommit: b09a8c93983d9d92ca4871054650b994e9996ecf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2019
ms.locfileid: "73329285"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Microsoft Identity Manager 2016 授权和下载

本文概述了 Microsoft Identity Manager (MIM) 2016 的授权方法，并提供了软件下载指针。

## <a name="licensing-mim-for-your-organization"></a>为组织授权 MIM

Microsoft Identity Manager 2016 采用每用户授权模式。  “产品条款”和相关文档中提供了授权的详细信息，可以从[“授权条款”](https://www.microsoft.com/licensing/product-licensing/products.aspx)页下载它们。

### <a name="licensing-for-azure-ad-premium-customers"></a>为 Azure AD Premium 客户授权

作为企业移动性 + 安全性的一部分，Azure Active Directory Premium（P1 和 P2）随附 Microsoft Identity Manager 2016。

可通过 [Microsoft 企业协议](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx)、[Open Volume License Program](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx) 和[云解决方案提供商](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409)计划获取 Azure AD Premium。 Azure 和 Office 365 订阅者还可以联机购买 Azure Active Directory Premium P1 和 P2。  请参阅 [Azure Active Directory 定价](https://azure.microsoft.com/pricing/details/active-directory/)，了解详细信息。

### <a name="mim-cals"></a>MIM CAL

如果你没有为用户提供 Azure Active Directory Premium 订阅，且正在使用除同步之外的更多 MIM 功能，那么身份在 MIM 中进行管理的每个用户都需要[客户端访问许可证 (CAL)](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx)。 如果希望外部用户（如业务合作伙伴、外部承包商或客户）能够访问 MIM，可以为每个外部用户都获取 CAL，也可以获取外部连接器 (EC) 许可。 对于身份仅在 Microsoft Identity Manager 同步服务中管理，且不在其他任何 MIM 组件中管理的用户，不需要 Microsoft Identity Manager 2016 CAL。

### <a name="licenses-for-platform-components"></a>平台组件许可证

必须有 Windows Server 许可证，才能将 Microsoft Identity Manager 2016 的服务器软件用作 Windows Server 附加产品。 此外，还必须安装 SQL Server，才能部署 MIM。  MIM 不随附 Windows Server 和 SQL Server 许可证。

## <a name="obtaining-mim-software"></a>获取 MIM 软件

开始 MIM 全新安装或从旧版本升级前，请确保拥有的是最新版本。

若要开始全新安装，必须下载与方案相关的每个 MIM 组件的安装文件。 然后，下载这些文件的所有更新，并从“下载中心”分别下载其他所有组件。


| 方案 | 组件 | 是否为方案所必需？ | DVD ISO 文件夹名称 | 注释 |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|同步| 同步服务（包括连接到 AD 的连接器） | 是 | `Synchronization Service` | |
| 同步 | PCNS | 否 | `Password Change Notification Service` |  要安装在域控制器上 |
| 同步 | 适用于 LDAP、SQL、Web Services、PowerShell、Lotus Domino、Graph 的连接器 | 否 | 不适用 | 通过“下载中心”分发 |
| 保护 Windows 和 Microsoft Azure Active Directory | MIM 服务 | 是 | `Service and Portal` | |
| 自助服务 | MIM 服务、MIM 门户 | 是 | `Service and Portal` | |
| 自助服务 | 加载项和扩展 | 否 | `Add-ins and extensions` | 要安装在最终用户电脑上 |
| 自助服务 | SCSM 报告 | 否 | `Data Warehouse Support Scripts` | |
| 自助服务 | 混合报告代理 | 否 | 不适用 | 通过“下载中心”分发 |
| 自助服务 | 语言包 | 否 | `LANGUAGE Packs` | |
| 证书管理 | CM | 是 | `Certificate Management` | |
| 证书管理 | CM Bulk 客户端 | 否 | `CM Bulk Client` | |
| 证书管理 | CM 客户端 | 否 | `CM Client`  | |
| 证书管理 | 适用于 Windows 的 CM 应用 | 否 | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>获取 Windows Installer 包

对于全新安装，大多数组织都是从[批量许可服务中心](https://www.microsoft.com/licensing/servicecenter/default.aspx)下载 MIM 安装包。 


DVD ISO 文件为每个 MIM 组件都包含一个文件夹：同步服务、服务和门户等。若要在与下载软件不同的计算机上安装软件，请务必复制整个 ISO 文件或组件文件夹：不要只复制文件夹中的 MSI 文件，而忽略其余的文件和子文件夹。

如果无权访问批量许可服务中心，拥有相应开发人员订阅的客户也可以从 [Visual Studio 我的权益下载](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202&pgroup=)中下载 MIM 2016 SP2 作为 ISO 文件。  搜索“Microsoft Identity Manager 2016 Service Pack 2”。  

如果无权访问批量许可服务中心，且只想在有限的时间内试用 MIM 软件，可以下载 [MIM 2016 评估版](https://www.microsoft.com/en-us/download/details.aspx?id=48244)。 此版软件不适用于生产用途，在首次安装 180 天后停止运行，且无法升级。 必须使用 Windows Server 2008 R2、Windows Server 2012 或 Windows Server 2012 R2，才能安装评估版。  如果刚开始接触 MIM 且正在学习这项技术，请注意，所有 MIM 方案都需要 Active Directory 域、Windows Server 和 SQL Server。 如果还没有 Windows Server 或 SQL Server，不妨尝试[为 VM 预配 SQL Server 2016 和 Windows Server 2016](https://azure.microsoft.com/blog/azure-images-sql-server-2016-on-windows-server-2016/)。

### <a name="obtaining-updates"></a>获取更新

从 MSI 安装 MIM 后，接下来应安装必要的修补程序。

在 [Identity Manager 版本发行历史记录](./reference/version-history.md)中查找最新的更新版本，其中包含安装程序补丁文件的下载网站链接。

为了确定哪些更新文件是必要的，下表列出了组件和更新中相应补丁 (MSP) 文件的名称。

| 方案 | 组件 | DVD ISO 文件夹名称 | 相应的更新补丁文件名 |
|----------|-----------|-   |-------------------|----------|--------------|
|同步| 同步服务 | `Synchronization Service` | `MIMSyncService_x64*.msp` |
| 自助服务 | MIM 服务、MIM 门户 | `Service and Portal` | `MIMService_x64*msp` |
| 自助服务 | 加载项和扩展 | `Add-ins and extensions` | `MIMAddinsExtensions*msp` |
| 自助服务 | 语言包 | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| 访问管理 (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`、`BHOLD*.msi` |
| 证书管理 | CM |  `Certificate Management` | `MIMCM*.msp` |
| 证书管理 | CM Bulk 客户端 |  `CM Bulk Client` |`MIMCMBulkClient*msp` |
| 证书管理 | CM 客户端 | `CM Client` |`MIMCMClient*msp` |

安装 MSP 文件前，请务必先阅读与更新相关的所有发行说明。

[BHOLD](https://www.microsoft.com/download/details.aspx?id=55950) 更新不作为 MSP 文件分发，仅作为 MSI 安装程序分发。

### <a name="additional-downloads"></a>其他下载

以下是可能相关的下载链接：

- [MIM 混合报告代理](https://www.microsoft.com/download/details.aspx?id=55112)

- [通用 LDAP 连接器、通用 SQL 连接器、Graph 连接器、Lotus Domino 连接器、PowerShell 连接器、Web Services 连接器](http://go.microsoft.com/fwlink/?LinkId=717495)

- [SharePoint 用户配置文件存储连接器](https://www.microsoft.com/download/details.aspx?id=41164)

- 如果还没有 Active Directory 域，且正在设置 PAM 方案以供试验，请参阅 [MIM 2016 SP1 PAM 部署脚本](sp1-deployment-scripts.md)。

## <a name="next-steps"></a>后续步骤

- 详细了解通过 [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md) 实现的方案。
- 阅读[容量计划指南](capacity-planning-guide.md)。
- 部署 MIM 以用于[同步方案](microsoft-identity-manager-deploy.md)或[权限访问管理方案](./pam/privileged-identity-management-for-active-directory-domain-services.md)。

