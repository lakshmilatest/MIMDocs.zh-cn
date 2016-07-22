---
title: "为 Privileged Access Management 配置 MIM 环境 | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: 9cf126d898c93faf89d7119136cce4e4963bb63d
ms.openlocfilehash: c9f2cf2ba1f42ea1513ae38d8089839d85ae5553


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
[[!div class="step-by-step"] [开始 »](step-1-prepare-corp-domain.md)](step-1-prepare-corp-domain.md)



<!--HONumber=Jul16_HO2-->


