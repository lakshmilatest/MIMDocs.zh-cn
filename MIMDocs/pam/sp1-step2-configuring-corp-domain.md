---
title: "步骤 2 配置 CORP 域"
description: "准备 CORP 域，其具有将由 Privileged Identity Manager 使用脚本进行管理的现有标识或新标识"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 7919fa3fdbb6613fbfa636ddb34762faa85925b9


---

# 步骤 2 配置 CORP 域

SIDs.txt 复制到 CORPDC 后，**对于 PRIVOnly 部署不需要**

1. 以管理员身份登录 CORPDC
2. 以管理员身份运行 PowerShell
3. cd $env: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 选择菜单选项 2（CORP 林配置）



<!--HONumber=Sep16_HO4-->


