---
title: "步骤 8 PAM 部署验证"
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
ms.openlocfilehash: 743ba586374ccc04e9ddafff759a00574e13f6ac


---

# 步骤 8 PAM 部署验证

部署包附带可执行 PAM 方案的验证脚本，以验证 PAM 部署是否按预期运行。
若要使用部署验证，请修改名为 <PamValidation/> 的PAMDeploymentConfig.xml 部分。

>[!Note] 验证要求客户端计算机加入到 CORP 域，并安装 PAM 客户端组件。 有关如何安装客户端的脚本，请参阅附录。

必须在 PAMDeploymentConfig.xml <PAMValidationClient/> 标记中更新客户端计算机的名称，<PAMValidation/> 节点中的其余数据只有在与现有用户/组冲突的情况下才需要编辑，因为此验证将尝试创建用户和组。
使用以下步骤来执行验证：

步骤 1：

1. 以 CORP 域管理员身份登录 CORPDC
2. 以管理员身份运行 PowerShell
3. cd $env: SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

这将创建验证所需的必要组和用户

步骤 2：

1. 以 MIMAdmin 身份登录 PAM 服务器
2. 以管理员身份运行 PowerShell
3. cd $env: SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

此步骤将用户和组迁移到 PAM 环境

步骤 3：

1. 以本地管理员身份登录 CORP 客户端
2. 以管理员身份运行 PowerShell
3. cd $env: SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


此步骤将提示你输入 CORPAdmin 凭据。 提供凭据后，它将添加所需的用户到“远程桌面用户”和“远程管理用户”组。
在 CORP 客户端上，使用以下命令以你正在验证的 PRIV 用户身份打开 PowerShell。 </br></br>
**Runas /u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
从 PowerShell 窗口，键入：

1. cd $env: SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  这将显示请求的状态。
  初始情况下，用户不具备资源访问权限。 用户实时添加到角色后，将授予用户访问权限。 请求持续时间到期后，用户将无权再次访问。
  对于请求到期时间，该脚本使用默认值（11 分钟）。



<!--HONumber=Sep16_HO4-->


