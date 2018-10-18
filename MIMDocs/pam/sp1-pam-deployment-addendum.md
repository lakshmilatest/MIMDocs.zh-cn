---
title: 附录
description: 这是介绍 PAM 脚本化部署的文档的附录。 其中介绍了如何配置 PRIV 域和 CORP 域，以及如何设置客户端执行验证并提供如何请求协助的信息。
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
ms.openlocfilehash: fb478c7f5df50cc4f4a8f9fb9fff1b3a0b431a14
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333915"
---
# <a name="pam-deployment-scripts-addendum"></a>PAM 部署脚本附录：

## <a name="addendum-1-setting-up-the-priv-domain"></a>附录 1 设置 PRIV 域

压缩文件解压缩到 $env:SYSTEMDRIVE\PAM 文件夹中后，编辑 PAMDeploymentConfig.xml 以提供 PRIV 林的详细信息。 请更新 DNSName、NetbiosName、DC 名称、数据库/日志路径和 Sysvol 路径。 还更新域和 ForestMode。 如果要测试 Windows Server Technical Preview 5，请将 DomainMode 和 ForestMode设置为 WinThreshold。

1. 以管理员身份登录 PRIV 域
2. 以管理员身份运行 PowerShell
3. cd $env: SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. 选择菜单选项 9（Priv 林安装程序）


DC 将在完成后自动重新启动。 目录服务还原模式 (DSRM) 管理员密码必须符合以下条件：

  * 密码长度至少为 15 个字符
  * 密码中包含至少一个小写字符
  * 密码中包含至少一个大写字符
  * 密码中包含至少一个数字或特殊字符

## <a name="addendum-2-setting-up-the-corp-domain"></a>附录 2 设置 CORP 域

如果从 PAM 着手，并且想要设置测试环境，该脚本还允许配置 CORP 域。 压缩文件解压缩到 $env:SYSTEMDRIVE\PAM 文件夹后，编辑 PAMDeploymentConfig.xml 以添加 CORP 林的详细信息。 更新 DNSName、NetbiosName、DC 名称、数据库/日志路径和 Sysvol 路径。 林功能级别必须至少为 Windows Server 2012 R2。

1. 以管理员身份登录 CPRP 域 DC
2. 以管理员身份运行 PowerShell
3. cd $env: SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. 选择菜单选项 10（CORP 林安装程序）

域控制器将在完成时自动重新启动

## <a name="addendum-3-setting-up-a-corp-client-to-do-the-validation"></a>附录 3 设置 CORP 客户端以进行验证

配置文件中的 ClientBinaryLocation 必须指向 setup.exe 所在的位置。
以本地管理员身份登录客户端，并在提升的 PowerShell 窗口中运行以下命令：

1. cd $env: SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. 选择菜单选项 7（MIM PAM 客户端安装程序）


如果计算机未加入域，它将提示输入 CORP 管理员凭据以执行域加入。 在加入域后，必须重新启动计算机。 再次以本地管理员身份登录客户端，并在提升的 PowerShell 窗口中运行以下命令：

1. cd $env: SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. 选择菜单选项 7（MIM PAM 客户端安装程序）

继续执行上面提供的步骤 8。

## <a name="addendum-4-if-something-goes-wrong"></a>如果出现问题，请参阅附录 4

所有脚本日志都保存在 %AppData%\MIMPAMInstall 中。 请将文件夹压缩到 Zip 文件，然后通过电子邮件发送到 [mim2016@microsoft.com](mailto:mim2016@microsoft.com)，写明操作和错误的详细信息。
