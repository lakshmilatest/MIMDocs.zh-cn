---
title: BHOLD 分析安装 | Microsoft Docs
description: BHOLD 分析模块可以基于规则测试数据访问权限
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 24e43d8ad886fcd7bfa7a9e694754b956f1659cd
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290265"
---
# <a name="bhold-analytics-installation"></a>BHOLD 分析安装

BHOLD 分析模块可以基于规则测试数据访问权限，确保组织可以有效地控制对其数据的访问权限，同时符合内部和外部访问要求。 BHOLD 分析模块生成的自动影响分析可以让你大致了解受强制实施建议规则影响的用户数，包括遵守规则的用户和违反规则的用户。 BHOLD 分析模块还可以提供有关遵守规则和违反规则的用户的详细列表。

## <a name="bhold-analytics-installation-requirements"></a>BHOLD 分析安装要求

安装 BHOLD 分析模块前，必须在计划安装 BHOLD 分析模块的服务器上安装 BHOLD 核心模块。 有关安装 BHOLD 核心模块的信息，请参阅 [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)（BHOLD 核心安装）。

## <a name="before-you-begin"></a>在开始之前

开始安装 BHOLD 分析模块前，需要准备提供 BHOLD 分析安装向导完成安装所需的信息。 以下工作表可以帮助记录这些信息，以便在需要时可以随时提供。

| **项目**                                    | **描述**                                                                                                                                                                                                           | **值**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 对域/计算机使用安全提供程序 | 选中后，会指定 Active Directory 域服务安全控制对 BHOLD 核心的访问。                                                                                                                | 选定复选框。 重要说明：如果未选中此复选框，则安装将失败。                                                                                                                                                                                                                   |
| **域**                                  | 指定包含在安装 BHOLD 核心时创建的服务帐户的域。 有关详细信息，请参阅 [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)（BHOLD 核心安装）。 | 域名由向导自动提供。 仅当不正确时，更改名称。 重要说明：指定域名时，请使用 NetBIOS（短）名称，不要使用完全限定的域名 (FQDN)。 例如，如果域的 FQDN 是 fabrikam.com，将域名指定为 FABRIKAM。 |
| **User**                                    | 指定 BHOLD 核心服务用户帐户的登录名。                                                                                                                                                          | 在此处填写用户帐户名称：                                                                                                                                                                                                                                                                                    |
| **密码**                                | 指定服务用户帐户的密码。                                                                                                                                                                       | 在此处填写密码：重要说明：确保将此密码保存在一个隐藏的安全位置。                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>BHOLD 分析安装

要安装 BHOLD 分析模块，请以域管理员组成员的身份登录，下载以下文件，并在要安装 BHOLD 分析模块的服务器上以管理员身份运行该文件：

- BholdAnalytics<em>\<版本\></em>\_Release.msi

将 \<版本\> 替换为要安装的 BHOLD 分析版本的版本号。

若要以管理员身份运行程序文件，右键单击该文件，然后单击“以管理员身份运行”。

# <a name="next-steps"></a>后续步骤

- [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)（BHOLD 核心安装）
- [BHOLD 安装指南](bhold-installation-guide.md)
- [BHOLD 版本历史记录](../reference/version-bhold-history.md)