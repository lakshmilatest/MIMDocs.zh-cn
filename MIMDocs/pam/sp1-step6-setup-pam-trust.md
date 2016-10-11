---
title: "步骤 6 安装 PAM 信任"
description: "准备 CORP 域，其具有将由 Privileged Identity Manager 使用脚本进行管理的现有标识或新标识"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 46afda513e849e457f5f3644a46f244161467e50


---

# 安装 PAM 信任

**这对于仅 PRIV 环境而言不需要** 使用 MIMAdmin 帐户登录 PAMServer。

1. 使用 MIMAdmin 帐户登录 PAMServer
2. 以管理员身份运行 PowerShell
3. cd $env: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 选择菜单选项 6（PAM 信任安装程序）

  出现提示时，请输入 CORP 管理员帐户的凭据。 提供凭据后，将建立信任关系并完成配置。



<!--HONumber=Sep16_HO4-->


