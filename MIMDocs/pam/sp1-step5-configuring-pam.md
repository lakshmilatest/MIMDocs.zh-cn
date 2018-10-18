---
title: 步骤 5 安装/配置 PAM
description: 这是使用脚本配置 Privileged Identity Manager 的第 5 步，其中还介绍了在 PAM 服务器上进行部署的步骤。
keywords: ''
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: e3e0d45f01f651400842b52e275ca4b6939f78c4
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333830"
---
# <a name="step-5-installingconfiguring-pam"></a>步骤 5 安装/配置 PAM

> [!div class="step-by-step"]
> [« 步骤 4](sp1-step4-configuring-sharepoint.md)
> [步骤 6 »](sp1-step6-setup-pam-trust.md)

对于加入域的 PAMServer，以 MIMAdmin 身份登录，否则以本地管理员身份登录。
1. 以管理员身份运行 PowerShell
2. cd $env: SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. 选择菜单选项 5（MIM PAM 安装程序）

>[!NOTE]
>如果计算机尚未加入到 PRIV 域，它将要求提供凭据。 在加入域后，计算机将重新启动。

PAMServer 重新启动后，使用 MIMAdmin 帐户重新登录到计算机。

1. 以管理员身份运行 PowerShell
2. cd $env: SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. 选择菜单选项 5（MIM PAM 安装程序）

   出现提示时，输入 MIM 监视账户、MIM 组件帐户、MIM MA 帐户、MIM 服务帐户、MIM 管理帐户和 SharePoint 帐户的密码。
   当安装完成后时，将重新启动计算机。

> [!div class="step-by-step"]
> [« 步骤 4](sp1-step4-configuring-sharepoint.md)
> [步骤 6 »](sp1-step6-setup-pam-trust.md)
