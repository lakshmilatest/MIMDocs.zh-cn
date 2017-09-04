---
title: "PAM 环境概述 | Microsoft Docs"
description: "查找要成功部署 Privileged Access Management 所需的虚拟机数量和配置"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3be2e19673a863098739e830d9c83ce264abf412
ms.sourcegitcommit: 210195369d2ecd610569d57d0f519d683ea6a13b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/01/2017
---
# <a name="environment-overview"></a>环境概述

Privileged Access Management 与具有各自分开的驱动器的虚拟机 (VM) 协同工作，这些驱动器在共享网络上相互连接。 可以通过 Windows 8.1、Windows Server 2012 R2 或其他操作系统平台托管这些虚拟机。

![PAM 服务器：关系和支持的平台 - 关系图](media/pam-test-lab-architecture.png)

至少需要三个虚拟机。  如果尚无可供 PAM 管理的 AD 域，还额外需要一个用作 CORP 域控制器的 VM。  若要配置 PRIV 软件以实现高可用性，还额外需要两个 VM。

存储 VM 磁盘映像的驱动器至少需要 120GB 的可用磁盘空间。  如果打算部署以实现高可用性，请确保磁盘子系统满足 SQL 共享存储的要求。  共享存储可以是 Windows Server 故障转移群集群集磁盘的形式，也可以是存储区域网络 (SAN) 上的磁盘或在 SMB 服务器上的文件共享的形式。

>[!IMPORTANT]
存储空间只能用于堡垒环境。 不建议与堡垒环境之外的其他工作负载共享存储空间，因为这样可能会破坏堡垒环境的完整性。

## <a name="next-steps"></a>后续步骤

- [Active Directory 域服务的 Privileged Access Management](privileged-identity-management-for-active-directory-domain-services.md) 概述了 PAM 及其工作原理。
- [了解 PAM 的组件](principles-of-operation.md)概述了 PAM 的各种组件。