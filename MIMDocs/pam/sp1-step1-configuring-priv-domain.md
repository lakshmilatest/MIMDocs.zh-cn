---
title: 步骤 1 配置 Priv 域
description: 准备 CORP 域，其具有将由 Privileged Identity Manager 使用脚本进行管理的现有标识或新标识
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
ms.openlocfilehash: c1092df1c1fe43551dfde8bbe4f0e77cf46ee981
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518204"
---
# <a name="step-1-configuring-the-priv-domain"></a>步骤 1 配置 Priv 域

> [!div class="step-by-step"]
> [步骤 2 »](sp1-step2-configuring-corp-domain.md)

1. 以管理员身份登录 PRIVDC
   * 如果这是仅限 PRIV 环境，请登录 CORPDC
2. 以管理员身份运行 PowerShell
3. cd $env: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. 选择菜单选项 1（PRIV 林配置）


如果域中尚不存在管理 SQL/SharePoint 和 MIM 所需的服务帐户，将会自动创建。 系统将提示输入用于在脚本执行过程中创建这些服务帐户的密码。
如果 PRIV 域是 Windows Server 2016，功能级别设置为 Windows Server 2016 Technical Preview 5，则该脚本将提示用户启用 PAM 所需的可选 Active Directory“特权访问管理”功能。 确认“是”以继续。
对于低于 Windows Server 2016 的功能级别，解除将不执行其他配置的警告。 一旦管理员将功能级别提升至 Windows Server 2016，将需要重新运行 PAMDeployment.ps1 和 PAM 林配置。

>[!NOTE]
>PRIVOnly 配置不需要执行以下步骤

将在 $env: SYSTEMDRIVE\PAM 中生成的 SIDs.txt 复制到 CORPDC 上类似的文件夹。 这是 CORPDC 为 PRIV 用户设置权限以读取 CORP 用户属性所需的操作。
该脚本完成后，系统将提示你重新启动计算机以使更改生效。

> [!div class="step-by-step"]
> [步骤 2 »](sp1-step2-configuring-corp-domain.md)
