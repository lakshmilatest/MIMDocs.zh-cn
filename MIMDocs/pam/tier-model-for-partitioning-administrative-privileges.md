---
title: PAM 环境层模型 | Microsoft Docs
description: 了解基于对风险的承受程度分离系统的层模型。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/30/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8e7b7217714f0ef74c1d959eb51dac07018d6e77
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379717"
---
# <a name="tier-model-for-partitioning-administrative-privileges"></a>用于对管理权限进行划分的层模型

本文介绍了一种安全模型，旨在通过从高风险区隔离高特权活动以防止特权提升。 该模型在提供一个良好的用户体验的同时仍坚持了最佳实践和安全原则。

## <a name="elevation-of-privilege-in-active-directory-forests"></a>Active Directory 林中的特权提升

获得对 Windows Server Active Directory (AD) 林永久管理权限的用户、服务或应用程序帐户会为组织的任务和业务带来大量风险。 这些帐户经常成为攻击者的目标，因为一旦入侵，攻击者便有权连接到域中的其他服务器或应用程序。

层模型基于管理员所管理的资源在他们之间创建分区。 控制用户工作站的管理员与那些控制应用程序或管理企业标识的管理员相分离。 有关此模型的详细信息，请参阅 [Securing privileged access reference material](http://aka.ms/tiermodel)（保护特权访问的参考资料）。

## <a name="restricting-credential-exposure-with-logon-restrictions"></a>通过登录限制来限制凭据公开

减少管理帐户的凭据被盗风险通常需要重新调整管理实践来限制向攻击者公开。 第一步，建议组织应当执行以下操作：

- 限制在其中公开管理凭据的主机数量。
- 将角色权限限制为最低要求。
- 确保管理任务不会在用于标准用户活动（例如，电子邮件和 Web 浏览）的主机上执行。

下一步是实现登录限制，并使过程和实践遵循层模型要求。 理想情况下，凭据公开还应减小为每个层中的角色所需的最小特权。

应强制执行登录限制，以确保高特权的帐户没有对安全级别较低的资源的访问权限。 例如：

- 域管理员（第 0 层）无法登录到企业服务器（第 1 层）和标准用户工作站（第 2 层）。
- 服务器管理员（第 1 层）无法登录到标准用户工作站（第 2 层）。

>[!NOTE]
> 服务器管理员不应在域管理员组中。 应为负责管理域控制器和企业服务器的人员提供独立的帐户。

可以通过以下方式强制执行登录限制：

- 组策略登录权限限制，包括：
    - 拒绝从网络访问该计算机
    - 拒绝作为批处理作业登录
    - 拒绝作为服务登录
    - 拒绝本地登录
    - 拒绝通过远程桌面设置登录  
- 如果使用 Windows Server 2012 或更高版本，则使用身份验证策略和接收器
- 如果帐户在专用管理员林中，则使用选择性身份验证

## <a name="next-steps"></a>后续步骤

- 下一篇文章[规划堡垒环境](planning-bastion-environment.md)将介绍如何添加 Microsoft Identity Manager 的专用管理林以建立管理帐户。
- [特权访问工作站](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)提供专用于免受 Internet 攻击和威胁矢量的敏感任务的操作系统。
