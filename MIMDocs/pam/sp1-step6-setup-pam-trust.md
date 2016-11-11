---
title: "步骤 6 安装 PAM 信任"
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
ms.openlocfilehash: 5bcf4f4ef201236746ec1bf75c1c8900841a6c79


---

# <a name="step-6-set-up-the-pam-trust"></a>步骤 6 安装 PAM 信任

>[!div class="step-by-step"]
[« 步骤 5](sp1-step5-configuring-pam.md)
[步骤 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

**这对于仅 PRIV 环境而言不需要** 使用 MIMAdmin 帐户登录 PAMServer。

1. 使用 MIMAdmin 帐户登录 PAMServer
2. 以管理员身份运行 PowerShell
3. cd $env: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 选择菜单选项 6（PAM 信任安装程序）

  出现提示时，请输入 CORP 管理员帐户的凭据。 提供凭据后，将建立信任关系并完成配置。

>[!div class="step-by-step"]
[« 步骤 5](sp1-step5-configuring-pam.md)
[步骤 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)



<!--HONumber=Nov16_HO2-->


