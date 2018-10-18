---
title: BHOLD 证明安装 | Microsoft Docs
description: 使用 BHOLD 证明模块可以指定审阅者和执行评审
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e4c3a6248585d55fddbbca3153f33734d7c5c429
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358102"
---
# <a name="bhold-attestation-installation"></a>BHOLD 证明安装

使用 BHOLD 证明模块可以指定审阅者并定期评审用户与每个应用程序权限和帐户之间的关系。

## <a name="bhold-attestation-installation-requirements"></a>BHOLD 证明安装要求

安装 BHOLD 证明模块前，必须在计划安装 BHOLD 证明模块的服务器上安装 BHOLD 核心模块。 有关安装 BHOLD 核心模块的信息，请参阅 [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)（BHOLD 核心安装）。 因为 BHOLD 证明模块联系人会以电子邮件的形式向用户发送消息，所以环境必须具有简单邮件传输协议 (SMTP) 电子邮件服务器，如 Microsoft Exchange Server。

> [!IMPORTANT]
> 安装 BHOLD 报表和 BHOLD 证明时，必须先安装 BHOLD 报表，然后才能安装 BHOLD 证明。

## <a name="before-you-begin"></a>在开始之前

开始安装 BHOLD 证明模块前，需要准备提供 BHOLD 证明安装向导完成安装所需的信息。 以下工作表可以帮助记录这些信息，以便在需要时可以随时提供。

| **项目**                                    | **描述**                                                                                                                                                                                                           | **值**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 对域/计算机使用安全提供程序 | 选中后，会指定 Active Directory 域服务安全控制对 BHOLD 核心的访问。                                                                                                                | 选定复选框。 重要说明：如果未选中此复选框，则安装将失败。                                                                                                                                                                                                                   |
| **域**                                  | 指定包含在安装 BHOLD 核心时创建的服务帐户的域。 有关详细信息，请参阅 [BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)（BHOLD 核心安装）。 | 域名由向导自动提供。 仅当不正确时，更改名称。 重要说明：指定域名时，请使用 NetBIOS（短）名称，不要使用完全限定的域名 (FQDN)。 例如，如果域的 FQDN 是 fabrikam.com，将域名指定为 FABRIKAM。 |
| **User**                                    | 指定 BHOLD 核心服务用户帐户的登录名。                                                                                                                                                          | 在此处填写用户帐户名称：                                                                                                                                                                                                                                                                                    |
| **密码**                                | 指定服务用户帐户的密码。                                                                                                                                                                       | 在此处填写密码：重要说明：确保将此密码保存在一个隐藏的安全位置。                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>BHOLD 证明安装

要安装 BHOLD 证明模块，请以域管理员组成员的身份登录，下载以下文件，并在要安装 BHOLD 证明模块的服务器上以管理员身份运行该文件：

- BholdAttestation<em>\<版本\></em>\_Release.msi

将 \<版本\> 替换为要安装的 BHOLD 证明版本的版本号。

若要以管理员身份运行程序文件，右键单击该文件，然后单击“以管理员身份运行”。

## <a name="next-steps"></a>后续步骤

- [BHOLD 安装指南](bhold-installation-guide.md)
- [BHOLD 开发人员参考](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD 版本历史记录](../reference/version-bhold-history.md)
