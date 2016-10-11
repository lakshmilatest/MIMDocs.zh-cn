---
title: "步骤 5 安装/配置 PAM"
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
ms.openlocfilehash: a5d86991f1579f292d7d303148422cef746d008a


---
# 步骤 5 安装/配置 PAM

对于加入域的 PAMServer，以 MIMAdmin 身份登录，否则以本地管理员身份登录。
1. 以管理员身份运行 PowerShell
2. cd $env: SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. 选择菜单选项 5（MIM PAM 安装程序）

>[!NOTE] 如果计算机尚未加入到 PRIV 域，它将要求提供凭据。 在加入域后，计算机将重新启动。

PAMServer 重新启动后，使用 MIMAdmin 帐户重新登录到计算机。

1. 以管理员身份运行 PowerShell
2. cd $env: SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. 选择菜单选项 5（MIM PAM 安装程序）

  出现提示时，输入 MIM 监视账户、MIM 组件帐户、MIM MA 帐户、MIM 服务帐户、MIM 管理帐户和 SharePoint 帐户的密码。
  当安装完成后时，将重新启动计算机。



<!--HONumber=Sep16_HO4-->


