---
title: "步骤 2 配置 CORP 域"
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
ms.openlocfilehash: b7acc475c505b29559c510fd3fa350fed1c3157b


---

# <a name="step-2-configuring-the-corp-domain"></a>步骤 2 配置 CORP 域

>[!div class="step-by-step"]
[« 步骤 1](sp1-step1-configuring-priv-domain.md)
[步骤 3 »](sp1-step3-installing-configuring-sql.md)

SIDs.txt 复制到 CORPDC 后，**对于 PRIVOnly 部署不需要**

1. 以管理员身份登录 CORPDC
2. 以管理员身份运行 PowerShell
3. cd $env: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 选择菜单选项 2（CORP 林配置）

>[!div class="step-by-step"]
[« 步骤 1](sp1-step1-configuring-priv-domain.md)
[步骤 3 »](sp1-step3-installing-configuring-sql.md)



<!--HONumber=Nov16_HO2-->


