---
title: "步骤 4 配置 SharePoint"
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
ms.openlocfilehash: aed3370e8dbc458c97c60686957f337cfd451d33


---

# 步骤 4 配置 SharePoint

SharePoint 必须是附带 SP1 的 SharePoint Foundation 2013。

对于加入域的服务器，以 MIMAdmin 身份登录

1. 以管理员身份运行 PowerShell
2.  .\PAMDeployment.ps1
3.  选择菜单选项 4（SharePoint 安装程序）


对于工作组服务器

1. 以管理员身份运行 PowerShell
2.  cd $env: SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. 选择菜单选项 4（SharePoint 安装程序）

安装 SharePoint 时，计算机将多次重新启动。 每次重新启动，SharePoint 安装程序必须重新运行，确保使用 MIMAdmin 帐户登录。
如果安装有 SharePoint 的计算机没有 Internet 连接来下载系统必备组件，可以单独下载并置于本地文件夹中。 **此本地文件夹路径需要在 <PrerequisitesBinaryLocation/> 中的 PAMConfiguration.xml 文件中更新。** 请参阅附录 5 获取下载这些文件的链接。
安装完成后，SharePoint 配置 GUI 将打开，并逐步执行各个步骤以完成 SharePoint 安装。 请选择完整的服务器，并浏览其余 UI。 安装完成后，将提示运行配置向导。 完成如下所示的步骤。

1. 在“**连接到服务器场**”选项卡上，将其更改为“**创建新服务器场**”。
2. 将 **SQLServer** 指定为用于配置数据库的数据库服务器，并将 **SharePoint ServiceAccount** 指定为 SharePoint 要使用的数据库访问帐户。
3. 指定一个密码作为场安全密码**（后面将不会再使用它）**
4. 接受其余的 SharePoint 配置向导默认设置以创建单服务器场

可在[步骤 3 - 准备 PAM 服务器](/microsoft-identity-manager/pam/step-3-prepare-pam-server)中的**配置 SharePoint** 部分找到详细信息，操作完成时，再次运行“.\PAMDeployment.ps1”脚本，选择选项 4（SharePoint 安装程序）来完成此步骤。



<!--HONumber=Sep16_HO4-->


