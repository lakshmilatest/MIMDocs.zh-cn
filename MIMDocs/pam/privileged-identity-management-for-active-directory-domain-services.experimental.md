---
title: "什么是 ADDS 的 PAM？ |Microsoft Docs"
description: "Privileged Access Management (PAM) 可帮助组织限制现有 Active Directory 环境内的特权访问权限。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c227d848506a41fdd3569954961b021ca4e92d40
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# Active Directory 域服务的 Privileged Access Management
<a id="privileged-access-management-for-active-directory-domain-services" class="xliff"></a>
Privileged Access Management (PAM) 可帮助组织限制现有 Active Directory 环境内的特权访问权限。

![PAM 步骤：准备、保护、操作、监视 - 关系图](media/MIM_PIM_SetupProcess.png)

通过关注准备、保护和监视环境的周期，Privileged Access Management 可实现以下两个目标：

- 通过维护已知不受恶意攻击影响的单独堡垒环境，重新建立对存在隐患的 Active Directory 环境的控制。  
- 隔离特权帐户的使用，以减少这些凭据被盗的风险。

> [!NOTE]
> PAM 是采用 Microsoft Identity Manager (MIM) 实现的 [Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM) 实例。

## PAM 可帮助解决哪些问题？
<a id="what-problems-does-pam-help-solve" class="xliff"></a>
企业现在关心的一个实际问题有关 Active Directory 环境内的资源访问问题。 特别麻烦的问题是有关漏洞、未经授权的特权提升和其他类型的未经授权的访问的消息（包括哈希传递、票证传递、鱼叉式网络钓鱼和 Kerberos 泄露）。

现在，攻击者可以非常轻松地获取域管理员帐户凭据，但用户却很难在发生这些攻击后立即发现。 PAM 的目标是在提高你对环境的控制和感知的同时，减少恶意用户获得访问权的机会。

PAM 使攻击者更难入侵网络和获取特权帐户访问。 PAM 添加了对特权组的保护，以便控制对一系列加入域的计算机和这些计算机上的应用程序的访问权限。 它还添加了监视能力更强、可见性更高且更精细的控制，以便组织可以看到谁是其特权管理员以及他们正在执行的操作。 PAM 可使组织更深入地了解如何在该环境中使用管理帐户。

## 如何安装 PAM？
<a id="how-is-pam-set-up" class="xliff"></a>
PAM 基于实时管理准则构建，该管理与[充分管理 (JEA)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362) 相关。 JEA 是一个 Windows PowerShell 工具包，可定义一组用于执行特权活动的命令，以及一个管理员在其中获取运行这些命令授权的终结点。 在 JEA 中，管理员决定具有特定特权的用户可以执行特定任务。 每当符合条件的用户需要执行该任务时，启用他们相应的权限。 权限在指定时间段后过期，以防恶意用户盗用访问权限。

PAM 安装和操作具有四个步骤。


1.  **准备域**：标识现有林中的哪些组具有重要权限。 在堡垒林中，重新创建这些不含成员的组。

2.  **保护**：当用户请求实时管理时，设置生命周期和身份验证保护，例如 Multi-Factor Authentication (MFA)。 MFA 有助于防止以编程方式实现的恶意软件攻击或以下凭据被盗。

3.  **操作**：在满足身份验证要求并批准某个请求后，用户帐户即可临时添加到堡垒林中的某个特权组。 在预设时间段内，管理员具有分配给该组的所有特权和访问权限。 此时间段过后，该帐户将从该组中删除。

4.  **监视**：PAM 将添加特权访问请求的审核、警报和报告。 你可以查看特权访问的历史记录，并查看执行了某个活动的人员。 你可以决定该活动是否有效并轻松标识未经授权的活动，例如尝试将用户直接添加到原始林中的某个特权组。 无论是在标识恶意软件，还是在跟踪“内部”攻击者方面，此步骤都非常重要。

## PAM 的工作原理是怎样的？
<a id="how-does-pam-work" class="xliff"></a>
PAM 基于 AD DS 中的新功能（特别是域帐户身份验证和授权）和 Microsoft Identity Manager 中的新功能。 PAM 将特权帐户从现有 Active Directory 环境中分隔出来。 当需要使用特权帐户时，需要先请求并随后批准该帐户。 批准后，将通过新堡垒林（而不是用户或应用程序的当前林）中的外主体组向该特权帐户提供相关权限。 使用堡垒林可使组织获得更大的控制权，例如用户何时可成为特权组成员以及用户需要进行身份验证的方式。

还可以在高可用性配置中部署此解决方案的 Active Directory、MIM 服务和其他部分。

以下示例介绍了 PIM 工作原理的详细信息。

![PIM 流程与参与者 - 关系图](media/MIM_PIM_howitworks.png)

堡垒林发布限时组成员身份，这进而生成限时票证授予票证 (TGT)。 如果应用和服务存在于信任堡垒林的林中，则基于 Kerberos 的应用程序或服务可以接受并强制执行这些 TGT。

日常用户帐户无需移动到新的林。 这同样适用于计算机、应用程序及其组。 它们保留在现有林中此时所在的位置。 例如，关注当下这些网络安全问题，但没有计划立即将服务器基础结构升级到下一版本的 Windows Server 的组织。 该组织通过使用 MIM 和新的堡垒林，仍然可以利用此合并的解决方案，并且可以更好地控制对现有资源的访问权限。

PAM 具有以下优势：

-   **隔离/划分特权范围**：对于还用于非特权任务（例如查看电子邮件或浏览 Internet）的帐户，用户不持有特权。 用户需要请求权限。 所有请求都是基于 PAM 管理员定义的 MIM 策略进行批准或拒绝。 在批准请求之前，特权访问不可用。

-   **升级和证明**：提供新的身份验证和授权质询，有助于管理单独的管理帐户的生命周期。 用户可以请求提升管理帐户，并使该请求通过 MIM 工作流。

-   **附加日志记录**：除了内置 MIM 工作流，还有其他 PAM 日志记录，用于标识该请求、确定授权方式，以及批准后所发生的任何事件。

-   **可自定义的工作流**：基于请求用户或已请求角色的参数，可以针对不同方案配置 MIM 工作流，并且可以使用多个工作流。

## 用户如何请求特权访问？
<a id="how-do-users-request-privileged-access" class="xliff"></a>
用户可通过多种方式提交请求，包括：  
- MIM 服务 Web 服务 API  
- REST 终结点  
- Windows PowerShell (`New-PAMRequest`)

## 提供了哪些工作流和监视选项？
<a id="what-workflows-and-monitoring-options-are-available" class="xliff"></a>
例如，假设在安装 PIM 之前，用户已是管理组的某个成员。 作为 PIM 安装的一部分，将从管理组中删除该用户，并且将在 MIM 中创建策略。 该策略指明如果该用户请求管理权限并且通过 MFA 进行身份验证，将批准该请求，并且用户的单独帐户将添加到堡垒林中的特权组。

假定该请求获得批准，操作工作流将直接与堡垒林 Active Directory 通信，以将用户放入某个组中。 例如，当 Jen 请求管理 HR 数据库时，Jen 的管理帐户将在几秒内添加到堡垒林中的特权组。 此组中她的管理帐户成员身份将在时间限制后过期。 在 Windows Server Technical Preview 中，该成员身份在 Active Directory 中与时间限制相关联；在 Windows Server 2012 R2 中，该时间限制在堡垒林中由 MIM 强制执行。

> [!NOTE]
> 当将新成员添加到某一组时，需要将该更改复制到堡垒林中的其他域控制器 (DC)。 复制延迟可能会影响用户访问资源的能力。 有关复制延迟的详细信息，请参阅[How Active Directory Replication Topology Works](https://technet.microsoft.com/library/cc755994.aspx)（Active Directory 复制拓扑的工作原理）。
>
> 与此相反，已到期的链接由安全帐户管理器 (SAM) 实时评估。 即使组成员的添加需要由接收访问请求的 DC 重复执行，也可在任意 DC 上对组成员的重复删除直接进行评估。

此工作流专门适用于这些管理帐户。 仅需要对特权组的偶尔访问权限的管理员（甚至是脚本）可以精确请求该访问权限。 MIM 将记录请求和 Active Directory 中的更改，你可在事件查看器中查看它们或将该数据发送到企业监视解决方案中，如 System Center 2012 - Operations Manager 审核收集服务 (ACS) 或其他第三方工具。
