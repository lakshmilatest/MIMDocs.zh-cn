---
title: "步骤 2 配置 CORP 域"
description: "本文介绍配置 corp 域所需的第 2 个步骤，其中涉及将 sids.txt 复制到 CORPDC 后运行一个脚本"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: 8480350f85b3543a32d4db3dbc6a388afcb16352
ms.lasthandoff: 01/10/2017


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

