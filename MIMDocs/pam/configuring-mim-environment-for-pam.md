---
title: "部署和配置 PAM | Microsoft Identity Manager"
description: "安装 MIM 并为 Privileged Access Management 对其进行配置的路线图。"
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 4b4953089cb676baae97988f380debbfefcd1083


---

# 为 Privileged Access Management 配置 MIM 环境
在设置跨林访问环境、安装和配置 Active Directory 和 Microsoft Identity Manager 以及演示实时访问请求时，需要完成 7 个步骤。

下面列出了这些步骤，以便你从头开始生成测试环境。 如果要将 PAM 应用到现有的环境中，则可以使用自己的域控制器或者用户帐户，而不是创建一个新的帐户以匹配实例。

1.  将 *CORPDC* 服务器准备作为域控制器，并将 *CORPWKSTN* 准备作为成员工作站。

2.  将 *PRIVDC* 服务器准备作为域控制器。

3.  在 *PRIV* 林中准备 *PAMSRV* 服务器。

4.  在 *PAMSRV* 上安装 MIM 组件、在 *CONTOSO* 林成员工作站上安装 cmdlet，并且使它们为特许访问权限管理做好准备。

5.  建立 *PRIV* 和 *CONTOSO* 林二者之间的信任关系。

6.  使可访问受保护资源的特权安全组和成员帐户为实时特许访问权限管理做好准备。

7.  演示请求、接收和使用对受保护资源的提升的特权访问权限。

>[!div class="step-by-step"]
[启动 »](step-1-prepare-corp-domain.md)



<!--HONumber=Jul16_HO3-->


