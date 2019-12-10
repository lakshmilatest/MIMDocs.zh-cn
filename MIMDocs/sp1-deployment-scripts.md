---
title: MIM2016 SP1 PAM 部署脚本
description: 此页包含在关于使用脚本配置 Privileged Identity Manager 的系列文章中。 其中包括一系列有关环境的假设。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 139ff94ecc38de37ac8eb6536d1b4d2a42909536
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "64517465"
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>MIM2016 SP1 PAM 部署脚本

在此 Service Pack 中，我们引入了一套部署脚本，更方便部署 PAM。 这些脚本可在下载中心获取。 在尝试使用脚本之前，请务必确保满足以下需求：

1. 所有服务器上的操作系统至少是 Windows Server 2012 R2。
2. 必须配置 DNS，才能在域控制器和组件服务器之间启用名称解析。
3. 安装二进制文件必须在指定的服务器上本地提供，以安装 SQL、SharePoint 和 MIM。
4. 该环境有三个专用（物理或虚拟）计算机，独立运行 CORPDC、 PRIVDC 和 PAMSERVER。
5. 对于验证选项，你需要具有专用的工作站。

>[!NOTE]
>如果遇到脚本执行的任何问题，可能需要看一下这些日志。 所有脚本日志都保存在 %AppData%\MIMPAMInstall 中。 请将文件夹压缩为 zip 文件，并在支持案例中添加此信息以及操作和错误详细信息。

准备好开始使用 PAM 部署脚本了吗？ 首先来[使用脚本配置 PAM](./pam/sp1-pam-configure-using-scripts.md)。
