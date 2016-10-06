---
title: "步骤 7 安装 SID 历史记录/SID 筛选"
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
ms.openlocfilehash: 4c5cfa92f3111a6d298f586ba547a1eca2502853


---

# 安装 SID 历史记录/SID 筛选

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



<!--HONumber=Sep16_HO4-->


