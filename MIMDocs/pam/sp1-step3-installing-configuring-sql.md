---
title: 步骤 3 配置 SQL
description: 本文是该系列文章的的第 3 步，介绍如何使用脚本配置Privileged Identity Manager，同时讨论了 SQL Server 的配置步骤。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 69f86c5366f7b662f3a11fa4ac8d44159421f909
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518152"
---
# <a name="step-3-configuring-sql"></a>步骤 3 配置 SQL

> [!div class="step-by-step"]
> [« 步骤 2](sp1-step2-configuring-corp-domain.md)
> [步骤 4 »](sp1-step4-configuring-sharepoint.md)

在使用以下步骤继续前，确保使用的是 SQL Server 2012 SP1 或更高版本或 SQL server 2014。 对于加入域的服务器，使用 MIMAdmin 帐户登录，否则以本地管理员身份登录
1. 以管理员身份运行 PowerShell
2. cd $env: SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. 选择菜单选项 3（SQL Server 安装程序）

   如果该服务器尚未加入到 PRIV 域，它将提示你输入凭据，并将服务器加入到该域。
   在加入域后，计算机将重新启动。 在成功重新启动时，使用 MIMAdmin 帐户登录服务器。

5. 以管理员身份运行 PowerShell
6. cd $env: SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. 选择菜单选项 3（SQL Server 安装程序）

出现提示时，为 MIMAdmin 服务帐户提供密码，然后继续安装。 完成后，转到步骤 4。

> [!div class="step-by-step"]
> [« 步骤 2](sp1-step2-configuring-corp-domain.md)
> [步骤 4 »](sp1-step4-configuring-sharepoint.md)
