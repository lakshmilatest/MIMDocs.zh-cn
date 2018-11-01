---
title: 为 PAM 定义特权角色 | Microsoft Docs
description: 决定哪些特权角色应该进行管理，并为每个角色定义管理策略。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 38a9fc174c037e5d7c3ea17b46dcf9f6ea924822
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "50380011"
---
# <a name="define-roles-for-privileged-access-management"></a>为 Privileged Access Management 定义角色

使用 Privileged Access Management，可以将用户分配到特权角色，以便根据需要激活这些角色以进行即时访问。 这些角色是在堡垒环境中手动定义和建立的。 本文将指导你完成确定哪些角色要通过 PAM 进行管理的过程，以及介绍如何使用适当的权限和限制定义这些角色。

定义特权访问管理的角色一种简单直接的方法是对电子表格中的所有信息进行编译。 在角色中列出角色，并使用列来标识管理要求和权限。

治理要求因现有的标识和访问策略或符合性要求而异。 用于标识每个角色的参数可能包括：

- 角色所有者。
- 可以担任相应角色的候选用户
- 应与角色用法相关联的身份验证、审批或通知控件。

角色权限将取决于要托管的应用程序。 本文使用 Active Directory 作为示例应用程序，将权限分为两类：

- 管理 Active Directory 服务本身（例如，配置复制拓扑）所需的权限

- 管理 Active Directory 中保存的数据（例如，创建用户和组）所需的权限

## <a name="identify-roles"></a>确定角色

首先确定你要使用 PAM 管理的所有角色。 在电子表格中，每个潜在的角色将具有自己的行。

若要查找适当的角色，请在管理范围内考虑每个应用程序：

- 应用程序位于[第 0 层、第 1 层还是第 2 层](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)？
- 影响应用程序的机密性、完整性或可用性的权限有哪些？
- 应用程序是否依赖系统的其他组件？ 例如，应用程序是否依赖数据库、网络、安全基础结构、虚拟化或托管平台？

确定如何对这些应用程序的注意事项进行分类。 你希望角色有明确的界限，并且仅为角色提供可完成应用程序内的常见管理任务的足够权限。

你总是希望为了最少权限分配来设计角色。 这可能基于用户的当前（或计划）组织责任，并且将包含用户职责所需的特权。 它可能还包含简化操作的特权，而不会带来风险。

审视权限是否包含角色的其他注意事项如下：

- 有多少人要使用特定角色？ 如果少于两个人，那么可能会因为定义的范围太窄而用处不大，或者你已经定义了特定一个人的职责。

- 一个人拥有多少角色？ 用户是否可以根据其任务选择适当的角色？

- 用户群体以及他们与应用程序的交互方式是否与特权访问管理兼容？

- 是否可以将管理与审核分离，以便处于管理角色的用户无法清除其操作的审核记录？

## <a name="establish-role-governance-requirements"></a>建立角色管理要求

在确定候选角色后，开始填写电子表格。 针对与组织相关的要求创建列。 要考虑的一些要求包括：

- 谁是将负责进一步定义角色、选择权限并维护角色的管理设置的角色所有者。

- 谁是将负责执行角色职责或任务的角色拥有者（用户）？

- 哪种访问方法（在下一节中讨论）将适用于角色拥有者？

- 当用户激活其角色时，是否需要角色所有者手动批准？

- 当用户激活其角色时，是否需要通知？

- 使用此角色时，是否应在 SIEM 系统中发出警报或通知，以便进行跟踪？

- 是否需要将要激活角色的用户限制为仅能够登录到计算机，从而需要有访问权限才能执行角色职责并充分地验证主机是否能够保护特权/凭据不被滥用。

- 是否需要向角色拥有者提供专用管理员工作站？

- 哪些应用程序权限（参见下面的 AD 的示例列表）与此角色相关联？

## <a name="select-an-access-method"></a>选择访问方法

Privileged Access Management 系统中可以有多个分配了相同权限的角色。 如果访问治理要求因用户群而异，可能会出现这种情况。 例如，对于全职员工与来自其他组织的外包 IT 员工，组织可以对他们应用不同的策略。

在某些情况下，可以将用户永久分配到某角色。 在这种情况下，用户无需请求或激活角色分配。 永久分配情况的示例如下：

- 现有林中的托管服务帐户

- 现有林中的用户帐户，凭据是在 PAM 外部托管。 这可以是“Break Glass”帐户。 Break Glass 帐户可能需要诸如“域/DC 维护”之类的角色，以便修复信任和 DC 运行状况等问题。 （Break Glass 帐户永久分配到角色，密码受物理保护）

- 管理林中的用户帐户，使用密码进行身份验证。 这可以是需要永久全天候管理权限的用户，他们从无法支持强身份验证的设备进行登录。

- 管理林中使用智能卡或虚拟智能卡的用户帐户（例如，执行罕见维护任务所需的使用离线智能卡的帐户）

对于担心凭据可能遭到盗窃或滥用的组织，[《Using Azure MFA for activation》](use-azure-mfa-for-activation.md)（使用 Azure MFA 进行激活）指南提供了有关如何将 MIM 配置为在进行角色激活时需要额外带外检查的说明。

## <a name="delegate-active-directory-permissions"></a>委派 Active Directory 权限

创建新域时，Windows Server 将自动创建默认组，例如“域管理员”。 这些组可以简化入门过程，并可能适合较小的组织。 对于大型组织或需要更多地隔离管理权限的组织，应清空这些组，并将它们替换为提供精确权限的组。

域管理员组的一个局限性是它不能包含来自外部域的成员。 另一个局限性是它对以下三种单独的功能授予权限：

- 自行管理 Active Directory 服务
- 管理 Active Directory 中保存的数据
- 启用远程登录到加入域的计算机。

新建仅提供必要权限的安全组，而不是“域管理员”等默认组。 然后，应使用 MIM 为这些管理员帐户动态提供组成员身份。

### <a name="service-management-permissions"></a>服务管理权限

下表提供了与用于管理 AD 的所含角色相关的权限示例。

| Role | 描述 |
| ---- | ---- |
| 域/DC 维护 | 通过“域\管理员”组中的成员身份，可以进行故障排除和更改域控制器操作系统。 可以执行的操作包括，将新的域控制器提升到林中的现有域和 AD 角色委派。
|管理虚拟 DC | 使用虚拟化管理软件管理域控制器 (DC) 虚拟机 (VM)。 可以通过完全控制管理工具中所有虚拟机或使用基于角色的访问控制 (RBAC) 功能授予此特权。 |
| 扩展架构 | 管理架构，包括添加新的对象定义、更改架构对象的权限以及更改对象类型的架构默认权限。 |
| 备份 Active Directory 数据库 | 对 Active Directory 数据库进行完整的备份，包括委托给 DC 和域的所有密码。 |
| 管理信任和功能级别 | 创建和删除与外部域和林的信任。 |
| 管理站点、子网和复制 | 管理 Active Directory 复制拓扑对象，包括修改站点、子网以及站点链接对象和启动复制操作 |
| 管理 GPO | 在整个域中创建、删除和修改组策略对象 |
| 管理区域 | 在 Active Directory 中创建、删除和修改 DNS 区域和对象 |
| 修改第 0 层 OU | 在 Active Directory 中修改第 0 层 OU 和包含的对象。 |

### <a name="data-management-permissions"></a>数据管理权限

下表中的权限示例与添加角色来管理或使用 AD 中的数据相关。

| Role | 描述 |
| ---- | ---- |
| 修改第 1 层管理员 OU                 | 在 Active Directory 中修改包含第 1 层管理员对象的 OU |
| 修改第 2 层管理员 OU                 | 在 Active Directory 中修改包含第 2 层管理员对象的 OU |
| 帐户管理: 创建/删除/移动 | 修改标准用户帐户                                      |
| 帐户管理: 重置解锁       | 重设密码并解锁帐户                                  |
| 安全组: 创建修改          | 在 Active Directory 中创建和修改安全组              |
| 安全组: 删除                 | 在 Active Directory 中删除安全组                         |
| GPO 管理                         | 管理域/林中不影响第 0 层服务器的所有 GPO             |
| 加入电脑/本地管理员                    | 所有工作站的本地管理权限                               |
| 加入 Srv/本地管理员                   | 所有服务器的本地管理权限                                    |

## <a name="example-role-definitions"></a>角色定义示例

角色定义的选择视托管服务器层而定。 这还取决于选择的托管应用程序。 应用程序（如 Exchange）或第三方企业产品（如 SAP）通常额外提供自己的角色定义，以用于委派管理。

以下部分提供了典型企业方案的示例。

### <a name="tier-0---administrative-forest"></a>第 0 层 - 管理林

适用于堡垒环境中帐户的角色可能包括：

- 对管理林的紧急访问权限
- “红牌”管理员：管理林的管理员用户
- 生产林的管理员用户
- 生产林中得到应用程序有限管理权限委派的用户

### <a name="tier-0---enterprise-production-forest"></a>第 0 层 - 企业生产林

适用于管理第 0 层生产林帐户和资源的角色可能包括：

- 对生产林的紧急访问权限
- 组策略管理员
- DNS 管理员
- PKI 管理员
- AD 拓扑和复制管理员
- 第 0 层服务器的虚拟化管理员
- 存储管理员
- 第 0 层服务器的反恶意软件管理员
- 第 0 层 SCCM的 SCCM 管理员
- 第 0 层 SCOM的 SCOM 管理员
- 第 0 层的备份管理员
- 连接到第 0 层主机的带外和基板管理控制器（用于 KVM 或无人值守管理）的用户

### <a name="tier-1"></a>第 1 层

适用于管理和备份第 1 层中服务器的角色可能包括：

- 服务器维护
- 第 1 层服务器的虚拟化管理员
- 安全扫描程序帐户
- 第 1 层服务器的反恶意软件管理员
- 第 1 层 SCCM的 SCCM 管理员
- 第 1 层 SCOM的 SCOM 管理员
- 第 1 层服务器的备份管理员
- 第 1 层主机的带外和基板管理控制器（用于 KVM 或无人值守管理）的用户

此外，适用于管理第 1 层中企业应用程序的角色可能包括：

- DHCP 管理员
- Exchange 管理员
- Skype for Business 管理员
- SharePoint 场管理员
- 云服务管理员，例如公司网站或公共 DNS 的管理员
- HCM、财务或法律系统的管理员

### <a name="tier-2"></a>第 2 层

适用于非管理员用户和计算机管理的角色可能包括：

- 帐户管理员
- 支持人员
- 安全组管理员
- 工作站桌边支持

## <a name="next-steps"></a>后续步骤

- [保护特权访问的参考资料](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)
- [将 Azure MFA 用于激活](use-azure-mfa-for-activation.md)
