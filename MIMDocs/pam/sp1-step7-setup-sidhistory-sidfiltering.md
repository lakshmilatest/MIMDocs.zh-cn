---
title: 步骤 7 安装 SID 历史记录/SID 筛选
description: 这是使用脚本配置 Privileged Identity Manager 的第 7 步。 该步骤包含设置 SID 历史记录/SID 筛选。
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
ms.openlocfilehash: f85dd4eff32d5207948ec332bf2e9850b14a86fe
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379322"
---
# <a name="step-7-set-up-sid-historysid-filtering"></a>步骤 7 设置 SID 历史记录/SID 筛选

> [!div class="step-by-step"]
> [« 步骤 6](sp1-step6-setup-pam-trust.md)
> [步骤 8 »](sp1-step8-pam-deployment-verification.md)

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

> [!div class="step-by-step"]
> [« 步骤 6](sp1-step6-setup-pam-trust.md)
> [步骤 8 »](sp1-step8-pam-deployment-verification.md)
