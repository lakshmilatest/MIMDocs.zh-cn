---
title: "步骤 7 安装 SID 历史记录/SID 筛选"
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
ms.openlocfilehash: a98d83a22c61ef534fcc02725e4cd500be10cc8a


---

# <a name="step-7-set-up-sid-historysid-filtering"></a>步骤 7 设置 SID 历史记录/SID 筛选

>[!div class="step-by-step"]
[« 步骤 6](sp1-step6-setup-pam-trust.md)
[步骤 8 »](sp1-step8-pam-deployment-verification.md)

**这对于仅 PRIV 环境而言不需要** 使用 MIMAdmin 帐户登录 PAMServer。

1. 以管理员身份登录 CORP DC
2. 以管理员身份运行 PowerShell
3. cd $env: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 选择菜单选项 8（安装 SID 历史记录/SID 筛选）

执行成功后，你将看到以下消息：<br/></br>
对于 SID 筛选： <br/></br>
“将信任设置为不筛选 SID”或者“不为此信任启用 SID 筛选功能”。 </br></br>
对于 SID 历史记录： </br></br>
“为此信任启用 SID 历史记录”或“已经为此信任启用 SID 历史记录”。

>[!div class="step-by-step"]
[« 步骤 6](sp1-step6-setup-pam-trust.md)
[步骤 8 »](sp1-step8-pam-deployment-verification.md)



<!--HONumber=Nov16_HO2-->


