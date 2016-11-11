---
title: "步骤 3 配置 SQL"
description: "准备 CORP 域，其具有将由 Privileged Identity Manager 使用脚本进行管理的现有标识或新标识"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: 375a34e5255c90559fc0ffb3a80fc7c92ebd27a2


---
# <a name="step-3-configuring-sql"></a>步骤 3 配置 SQL

>[!div class="step-by-step"]
[« 步骤 2](sp1-step2-configuring-corp-domain.md)
[步骤 4 »](sp1-step4-configuring-sharepoint.md)

在使用以下步骤继续前，确保使用的是 SQL Server 2012 SP1 或更高版本或 SQL server 2014。 对于加入域的服务器，使用 MIMAdmin 帐户登录，否则以本地管理员身份登录
1. 以管理员身份运行 PowerShell
2. cd $env: SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. 选择菜单选项 3（SQL Server 安装程序）

  如果该服务器尚未加入到 PRIV 域，它将提示你输入凭据，并将服务器加入到该域。
  在加入域后，计算机将重新启动。 在成功重新启动时，使用 MIMAdmin 帐户登录服务器。

1. 以管理员身份运行 PowerShell
2. cd $env: SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. 选择菜单选项 3（SQL Server 安装程序）

出现提示时，为 MIMAdmin 服务帐户提供密码，然后继续安装。 完成后，转到步骤 4。

>[!div class="step-by-step"]
[« 步骤 2](sp1-step2-configuring-corp-domain.md)
[步骤 4 »](sp1-step4-configuring-sharepoint.md)



<!--HONumber=Nov16_HO2-->

