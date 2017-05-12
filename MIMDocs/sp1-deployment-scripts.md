---
title: "MIM2016 SP1 PAM 部署脚本"
description: "此页包含在关于使用脚本配置 Privileged Identity Manager 的系列文章中。 其中包括一系列有关环境的假设。"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 12c60e12dc5662ff0313e21bb9180b3709969af6
ms.contentlocale: zh-cn
ms.lasthandoff: 05/09/2017


---

# <a name="mim2016-sp1-pam-deployment-scripts"></a>MIM2016 SP1 PAM 部署脚本

在此 Service Pack 中，我们引入了一套部署脚本，更方便部署 PAM。 这些脚本可在下载中心获取。 在尝试使用脚本之前，确保下列假设适用于你的环境，这一点很重要。

重要假设：
1. 所有计算机上的操作系统至少是 Windows Server 2012 R2。 如果试用 Windows Server 2016 Technical Preview 5，则 PRIV 域控制器必须安装有 TP5内部版本。
2. 必须配置 DNS，以便域控制器和组件服务器之间的名称解析为自动。
3. 安装二进制文件必须在指定的服务器上本地提供，以安装 SQL、SharePoint 和 MIM。
4. 该环境有三个专用（物理或虚拟）计算机，独立运行 CORPDC、 PRIVDC 和 PAMSERVER。
5. 对于验证选项，假定专用客户端计算机存在以执行此步骤。

>[!NOTE]
>如果遇到脚本执行的任何问题，可能需要看一下这些日志。 所有脚本日志都保存在 %AppData%\MIMPAMInstall 中。 请将文件夹压缩到 Zip 文件，然后通过电子邮件发送到 mim2016@microsoft.com，写明操作和错误的详细信息。

准备好开始使用 PAM 部署脚本了吗？ 首先来[使用脚本配置 PAM](./pam/sp1-pam-configure-using-scripts.md)。

