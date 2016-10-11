---
title: "MIM2016 SP1 PAM 部署脚本"
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
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 43a176ed2f1375eb98851064c460515ec09de132


---

# MIM2016 SP1 PAM 部署脚本

在此 Service Pack 中，我们引入了一套部署脚本，更方便部署 PAM。 这些脚本可在下载中心获取。 在尝试使用脚本之前，确保下列假设适用于你的环境，这一点很重要。

重要假设：
1. 所有计算机上的操作系统至少是 Windows Server 2012 R2。 如果试用 Windows Server 2016 Technical Preview 5，则 PRIV 域控制器必须安装有 TP5内部版本。
2. 必须配置 DNS，以便域控制器和组件服务器之间的名称解析为自动。
3. 安装二进制文件必须在指定的服务器上本地提供，以安装 SQL、SharePoint 和 MIM。
4. 该环境有三个专用（物理或虚拟）计算机，独立运行 CORPDC、 PRIVDC 和 PAMSERVER。
5. 对于验证选项，假定专用客户端计算机存在以执行此步骤。

>[!NOTE] 如果遇到脚本执行的任何问题，可能需要看一下这些日志。 所有脚本日志都保存在 %AppData%\MIMPAMInstall 中。 请压缩文件夹到 Zip 文件，然后通过电子邮件发送到 mim2016@microsoft.com，写明操作和错误的详细信息。



<!--HONumber=Sep16_HO4-->


