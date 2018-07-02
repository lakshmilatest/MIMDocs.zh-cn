---
title: 步骤 7 安装 SID 历史记录/SID 筛选
description: 这是使用脚本配置 Privileged Identity Manager 的第 7 步。 该步骤包含设置 SID 历史记录/SID 筛选。
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: d2512690ce648767157a7417e5b41095c970b8eb
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289102"
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
