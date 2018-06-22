---
title: BHOLD 访问管理连接器安装 | Microsoft Docs
description: BHOLD 连接器模块支持数据的初始同步和正在进行的同步
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 6d7f19f470d0c0f82a68652115ab9265a13b3508
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2017
ms.locfileid: "24522322"
---
# <a name="access-management-connector-installation"></a>访问管理连接器安装

BHOLD 套件访问管理连接器模块支持向 BHOLD 中进行数据的初始同步和正在进行的同步。 访问管理连接器配合 Microsoft Identity Manager (MIM) 同步服务可以移动 BHOLD 核心数据库、FIM 2010 metaverse、目标应用程序和标识存储中的数据。 安装访问管理连接器模块后，即可创建 FIM 管理代理，用于控制 BHOLD 和 MIM 之间的数据流。

## <a name="access-management-connector-software-requirements"></a>访问管理连接器软件要求

安装访问管理连接器模块前，必须安装 Microsoft .NET Framework 4。 有关 .NET Framework 4 和安装说明的详细信息，请参阅 [Microsoft .NET 主页](http://www.microsoft.com/net)。
在运行 MIM 的 FIM 同步服务的计算机上，必须安装访问管理连接器。

## <a name="access-management-connector-setup"></a>访问管理连接器安装程序

若要安装访问管理控制模块，请以域管理员组成员的身份登录，下载以下文件，并在要安装 BHOLD FIM 集成模块的服务器上以管理员身份运行该文件：

- AccessManagementConnector.msi

若要以管理员身份运行程序文件，右键单击该文件，然后单击“以管理员身份运行”。

## <a name="next-steps"></a>后续步骤

- [BHOLD FIM integration installation](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx)（BHOLD FIM 集成安装）若要启用角色的最终用户自助服务，可以安装 BHOLD FIM 集成模块
- [BHOLD 安装指南](bhold-installation-guide.md)
- [BHOLD 开发人员参考](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD 版本历史记录](../reference/version-bhold-history.md)