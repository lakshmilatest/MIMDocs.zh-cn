---
title: "PAM 灾难恢复 | Microsoft Docs"
description: "了解如何配置 Privileged Access Management，以便获得高可用性和灾难恢复。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 2fab9af837ed11b1f2f7f32c9ced6d79c8cc9d00
ms.contentlocale: zh-cn
ms.lasthandoff: 07/10/2017


---

<a id="high-availability-and-disaster-recovery-considerations-for-the-bastion-environment" class="xliff"></a>
# 堡垒环境的高可用性和灾难恢复注意事项
本文介绍了为 Privileged Access Management (PAM) 部署 Active Directory 域服务 (AD DS) 和 Microsoft Identity Manager 2016 (MIM) 时的高可用性和灾难恢复的注意事项。

企业会比较关注 Windows Server、SQL Server 和 Active Directory 中的工作负荷的高可用性和灾难恢复问题。 但是，Privileged Access Management 的堡垒环境的可靠的可用性也很重要。 当用户与组织的 IT 基础结构的组件进行交互以承担管理角色时，堡垒环境是 IT 基础结构中的关键部分。 有关整体的高可用性的更多信息，请下载白皮书 [Microsoft High Availability Overview（Microsoft 高可用性概述）](http://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc)。

<a id="high-availability-and-disaster-recovery-scenarios" class="xliff"></a>
## 高可用性和灾难恢复场景

在制定高可用性和灾难恢复的计划时，请考虑以下问题：

- 哪些功能可能会受到服务中断的影响？
- 哪些功能是业务关键型和/或对 IT 运营很重要？
- 哪些风险可能导致这些系统的服务中断？

这些问题涉及的范围将影响部署和运营的总成本，因此，组织可能将某些功能的优先级设置为高于其他功能，同时也接受较低优先级的功能的临时服务中断的风险。 下表概括了组织可能使用的一种可能的优先级排序：

| **堡垒林功能** | **恢复过程中的相对优先级** | **功能不可用时的缓解措施** |
| --------------------------- | --------------------- | -------------- |
| 建立信任关系         | 低 | 等待直到还原堡垒环境 |
| 用户和组的迁移   | 低 | 等待直到还原堡垒环境 |
| MIM 管理          | 低 | 等待直到还原堡垒环境 |
| 特权角色激活  | 中型 | 使用专用的智能卡支持的帐户，手动将用户添加到管理组 |
| 资源管理         | 高 | 使用专用的智能卡支持的帐户，手动将用户添加到管理组 |
| 监视现有林中的用户和组 | 低 | 等待直到还原堡垒环境 |

现在让我们依次详细介绍这些堡垒林功能。

<a id="trust-establishment" class="xliff"></a>
### 建立信任关系

现有林和堡垒环境的林的域之间需要建立林信任关系。 这样，在堡垒环境中经过身份验证的用户可以管理现有林中的资源。 可能需要其他配置，例如，在早期版本的 Windows Server 上允许迁移现有域中的用户。

信任关系的建立要求现有林域的控制器，以及堡垒环境的 MIM 和 AD 组件处于联机状态。  如果在建立信任关系过程中上述任何一个组件出现故障，则在解决该故障之后管理员可以重试建立信任关系。  如果现有林域控制器或堡垒环境已从故障中恢复，那么 MIM 还包括 PowerShell cmdlet `Test-PAMTrust` 和 `Test-PAMDomainConfiguration`，可以使用它们确认信任关系依旧存在。

<a id="user-and-group-migration" class="xliff"></a>
### 用户和组的迁移

建立信任关系之后，可以在堡垒环境中创建影子组，以及为这些组的成员及审批者创建用户帐户。 这样可以使这些用户激活特权角色，并重新获得有效的组成员身份。

用户和组的迁移要求现有林域的控制器，以及堡垒环境的 MIM 和 AD 组件处于联机状态。   如果无法访问现有林域控制器，那么将无法向堡垒环境中添加其他用户和组，但现有用户和组不会受到影响。 如果迁移过程中上述任何一个组件出现故障，则在解决该故障之后管理员可以重试迁移。

<a id="mim-administration" class="xliff"></a>
### MIM 管理
迁移用户和组之后，管理员可以在 MIM 中进一步配置角色分配，将用户作为候选人进行链接，以便将其激活为角色。  他们还可以为审批和 Azure MFA 配置 MIM 策略。  

MIM 管理要求堡垒环境的 MIM 和 AD 组件处于联机状态。

<a id="privileged-role-activation" class="xliff"></a>
### 特权角色激活
当用户想要激活特权角色时，他们需要在堡垒环境域中进行身份验证，并向 MIM 提交一个请求。  MIM 包括 SOAP 和 REST API，以及 PowerShell 和网页中的用户界面。

特权角色激活要求堡垒环境的 MIM 和 AD 组件处于联机状态。  此外，如果将 MIM 配置为对所选角色使用 [Azure MFA 激活](use-azure-mfa-for-activation.md)，那么需要访问 Internet 以联系 Azure MFA 服务。

<a id="resource-management" class="xliff"></a>
### 资源管理
将用户成功激活为角色后，域控制器将为用户生成现有域的域控制器使用的 Kerberos 票证，并且将识别用户的新临时组成员身份。

资源管理要求资源域的域控制器，以及堡垒环境中的域控制器处于联机状态。  用户被激活之后，发出其 Kerberos 票证并不要求堡垒环境中的 MIM 或 SQL 处于联机状态。  （请注意，如果将 Windows Server 2012 R2 作为堡垒环境的功能级别，则 MIM 需要处于联机状态，以便终止临时组成员身份。）

<a id="monitoring-of-users-and-groups-in-the-existing-forest" class="xliff"></a>
### 监视现有林中的用户和组
MIM 还包括 PAM 监视服务，该服务会定期检查现有域中的用户和组，并相应地更新 MIM 数据库和 AD。  角色激活或资源管理过程中并不要求该服务处于联机状态。

监视要求现有林域的控制器，以及堡垒环境的 MIM 和 AD 组件处于联机状态。  

<a id="deployment-options" class="xliff"></a>
## 部署选项
[Environment overview（环境概述）](environment-overview.md)介绍了一种基本拓扑，该拓扑适用于不针对高可用性的技术。 本节介绍如何在该拓扑的基础上进行扩展，以便为具有单个站点的组织和具有多个现有站点的组织提供高可用性。

<a id="networking" class="xliff"></a>
### 网络

堡垒环境中的计算机之间的网络通信应与现有的网络隔离，例如，可以使用不同的物理或虚拟网络。  根据堡垒环境存在的风险，计算机之间可能还需要有独立的物理互连。  特定的故障转移群集技术对网络接口有额外的要求。

堡垒环境中托管 Active Directory 域服务和托管 MIM 服务的计算机需要与现有林中的资源建立双向连接，以便：
- 由 PRIV 林域控制器对用户进行身份验证
- 用户请求激活
- 用户获取现有林中的资源都可使用的 Kerberos 票证
- MIM 监视现有林域
- MIM 使用现有林中的邮件服务器发送电子邮件。

<a id="minimal-high-availability-topologies" class="xliff"></a>
### 最小高可用性拓扑
组织可以选择其堡垒环境中的哪些功能需要高可用性，高可用性具有以下约束：

- 堡垒环境提供的任何功能的高可用性需要至少两个域控制器。  
- 激活请求的高可用性需要至少两台托管 MIM 服务的计算机，并且还要求具有 SQL Server 的高可用性。
- SQL Server 具有故障转移群集的高可用性需要至少两个提供 SQL Server 的服务器，并且这些服务器不能与域控制器相同。
- 不应在域控制器上安装 MIM 服务，以尽可能减少每个服务器的受攻击面。

堡垒环境中的所有功能的最小高可用性拓扑包含至少四台服务器，以及共享存储。 其中两个服务器必须配置为域控制器，用于提供 Active Directory 域服务。 另外两个服务器可以配置为分别提供 SQL Server 和 MIM 服务的故障转移群集。

此外，典型的堡垒环境的部署还应包括用于管理这些服务器和监视组件的特权管理工作站

下面的关系图显示了一种可能的体系结构：

![堡垒拓扑 - 关系图](media/bastion1.png)

可以为每个功能配置更多服务器，以便在一定负荷条件下提供更高的性能，或者用于实现地理冗余，如下所述。

<a id="deployments-supporting-multiple-sites" class="xliff"></a>
### 支持多个站点的部署
为跨多个站点部署的资源选择适当的部署拓扑取决于三个因素：  
- 高可用性和灾难恢复的目标与风险  
- 用于托管堡垒环境的硬件功能  
- 每个站点的管理工作模型。

一种最简单的方法是在一个特定站点托管堡垒环境。  正常情况下，用户将连接到该站点的堡垒环境中的 MIM 部署并请求激活，激活操作将对每个站点的资源产生影响。  如果网络链接断开或者托管堡垒环境的站点不可用，则可以在另一个站点上访问脱机凭据，以便在网络重新连接之前执行临时管理。  该方法可能适用于本地管理较少并且受限于要将站点重新连接到组织的网络的其余部分的特定站点，例如分支机构。

![多站点单堡垒拓扑 - 关系图](media/bastion2.png)

对于跨站点的高可用性和灾难恢复，还可以在各个站点中部署堡垒环境的组件，共享公共的 PRIV 目录和公共的 SQL 数据库。  在该拓扑中，如果网络链接中断，每个站点上的用户可以继续不受影响地操作。

![多站点多堡垒拓扑 - 关系图](media/bastion3.png)

该部署方法的一个限制是 SQL Server 需要有跨这两个站点的群集，这可能比较难以部署。 在此情况下，可以考虑将仅复制堡垒环境的 Active Directory（PRIV 林）视为一种替代方法。  如果站点之间出现网络中断，那么站点 B 中以前已经激活其特权角色的用户能够继续操作，以管理站点 B 中的资源。

![多站点重复堡垒拓扑 - 关系图](media/bastion4.png)

如果每个站点代表单独的管理边界，那么也可以部署多个独立的堡垒环境。  虽然每个堡垒环境具有相同的软件和不同的域名，每个堡垒环境的目录之间和数据库之间仍不存在通用性。 想要管理特定站点中的资源的用户将激活该站点中的堡垒环境中的用户帐户。

![多站点独立堡垒拓扑 - 关系图](media/bastion5.png)

最后，可以有更复杂的部署，因为可能会单独配置多个堡垒环境来管理特定域中的资源。

![多站点复杂堡垒拓扑 - 关系图](media/bastion6.png)

<a id="hosted-bastion-environment" class="xliff"></a>
### 托管的堡垒环境
一些组织也考虑建立独立于他们的任何现有站点的堡垒环境。 堡垒环境软件可以托管在组织的网络内部或外部的托管提供商的虚拟化平台上。  当评估这种方法时，请记住：

- 为了防止来自现有域的攻击，堡垒环境的管理必须独立于现有域的管理帐户。
- 堡垒环境需要与现有域中的域控制器建立 TCP/IP 连接。  可以在[《如何为域和信任关系配置防火墙》](https://support.microsoft.com/kb/179442)文章中找到端口列表。
- Active Directory 域服务的虚拟化部署需要虚拟化平台的特定功能，如[《虚拟化域控制器部署和配置》](https://technet.microsoft.com/library/jj574223.aspx)中所述。
- MIM 服务的 SQL Server 的高可用性部署需要专门的存储配置，如下面的 [SQL Server 数据库存储](#sql-server-database-storage)一节所述。  目前，并非所有的托管提供程序会提供可以托管适用于 SQL Server 故障转移群集的磁盘配置的 Windows Server。

<a id="deployment-preparation-and-recovery-procedures" class="xliff"></a>
## 部署准备和恢复过程
堡垒环境的高可用性或灾难恢复部署准备需要考虑如何在共享存储上安装 Windows Server Active Directory、SQL Server 及其数据库，以及如何安装 MIM 服务和 PAM 组件。

<a id="windows-server" class="xliff"></a>
### Windows Server
Windows Server 包含用于高可用性的内置功能，使多台计算机能够作为一个故障转移群集在一起工作。 群集服务器通过物理电缆和软件进行连接。 如果一个或多个群集节点出现故障，其他节点就会开始提供服务（该过程称为故障转移）。   可从[《故障转移群集概述》](https://technet.microsoft.com/library/hh831579.aspx)中获取更多详细信息。

请确保堡垒环境中的操作系统和应用程序可以接收有关安全问题的更新。 某些更新可能需要重启服务器，因此在各个服务器之间协调应用更新的时间可避免长时间的停机。 一种方法是对 Windows Server 故障转移群集中的服务器使用[群集感知更新](https://technet.microsoft.com/library/hh831694.aspx)。

堡垒环境中的服务器将加入到域中，并依赖于域服务。 请确保不会无意间将它们配置为依赖于服务（如 DNS）的特定域控制器。

<a id="bastion-environment-active-directory" class="xliff"></a>
### 堡垒环境 Active Directory
Windows Server Active Directory 域服务原生支持高可用性和灾难恢复。

<a id="preparation" class="xliff"></a>
#### 准备
在堡垒环境中特权访问管理的典型的生产部署包含至少两个域控制器。 有关在堡垒环境中设置第一个域控制器的说明包括在部署文章的第 2 步：[Prepare the PRIV domain controller（准备 PRIV 域控制器）](step-2-prepare-priv-domain-controller.md)。

有关添加其他域控制器的过程，请参阅[在现有域中安装副本 Windows Server 2012 域控制器（级别 200）](https://technet.microsoft.com/library/jj574134.aspx)。  

>[!NOTE]
> 如果域控制器要在虚拟化平台上托管，例如 Hyper-V，请查阅[虚拟化域控制器部署和配置](https://technet.microsoft.com/library/jj574223.aspx)中的注意事项。

<a id="recovery" class="xliff"></a>
#### 恢复
在发生故障后，确保堡垒环境中至少一个域控制器可用，然后再重启其他服务器。

在一个域中，Active Directory 在各个域控制器中分配弹性单一主机操作 (FSMO) 角色，如 [How Operations Masters Work](https://technet.microsoft.com/library/cc780487.aspx)（操作主机的工作原理）中所述。  如果一个域控制器出现故障，可能需要传输分配给该域控制器的一个或多个[域控制器角色](https://technet.microsoft.com/library/cc786438.aspx)。

确定某个域控制器将不再返回到生产环境后，请务必检查是否有任何角色分配给此域控制器，并根据需要重新分配这些角色。 有关说明请参阅 [View the Current Operations Master Role Holders](https://technet.microsoft.com/library/cc816893.aspx)（查看当前操作主机角色持有者）及其相关的文章。

此外建议检查加入到堡垒环境中的计算机的 DNS 设置，以及与该域控制器具有信任关系的 CORP 域中的域控制器，以确保没有对它们硬编码为依赖于该域控制器计算机的 IP 地址。

<a id="sql-server-database-storage" class="xliff"></a>
### SQL Server 数据库存储
高可用性部署要求具有 SQL Server 故障转移群集，而 SQL Server 故障转移群集实例则根据数据库和日志存储的所有节点之间的共享存储进行回复。 共享存储可以是 Windows Server 故障转移群集群集磁盘的形式，也可以是存储区域网络 (SAN) 上的磁盘或在 SMB 服务器上的文件共享的形式。  请注意，这些形式只能用于堡垒环境中；建议不要与堡垒环境之外的其他工作负载共享存储，否则可能会损坏堡垒环境的完整性。

<a id="sql-server" class="xliff"></a>
### SQL Server
在堡垒环境中 MIM 服务要求具有 SQL Server 部署。   为实现高可用性，可以使用故障转移群集实例 (FCI) 部署 SQL。 与单独的实例不同，在 FCI 中 SQL Server 的高可用性受到 FCI 中的冗余节点的保护。 当出现故障或进行计划的升级时，资源组所有权将转移到另一个 Windows Server 故障转移群集节点。

如果你只需要支持灾难恢复，而不需要高可用性，那么可以使用日志传送、事务复制、快照复制或数据库镜像，而无需使用故障转移群集。   

<a id="preparation" class="xliff"></a>
#### 准备
在堡垒环境中安装 SQL Server 时，它必须独立于 CORP 林中已存在的任何 SQL Server。  而且，建议将 SQL Server 部署在与域控制器的服务器不同的专用服务器上。
详细信息记录在[《AlwaysOn 故障转移群集实例》](https://msdn.microsoft.com/library/ms189134.aspx)的 SQL Server 指南中。

<a id="recovery" class="xliff"></a>
#### 恢复
如果将 SQL Server 配置为使用日志传送进行灾难恢复，那么必须采取措施在恢复过程中更新 SQL Server。  此外，还需要重启每个 MIM 服务实例。

如果 SQL Server 出现故障，或者 SQL Server 与 MIM 服务之间的连接已中断，那么在还原 SQL Server 后，建议重启每个 MIM 服务。  这样可以确保 MIM 服务重新建立与 SQL Server 的连接。

<a id="mim-service" class="xliff"></a>
### MIM 服务
MIM 服务需要处理激活请求。  为了在托管 MIM 服务的计算机停止运行以进行维护时仍可以接收激活请求，需要部署多台 MIM 服务计算机。  请注意，将用户添加到组后 Kerberos 操作中不涉及 MIM 服务。  

<a id="preparation" class="xliff"></a>
#### 准备
建议在加入到 PRIV 域的多个服务器上部署 MIM 服务。
要实现高可用性，请参阅 Windows Server 文档[《故障转移群集硬件要求和存储选项》](https://technet.microsoft.com/library/jj612869.aspx)和[《Creating a Windows Server 2012 Failover Cluster》](http://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx)（创建 Windows Server 2012 故障转移群集）。

对于跨多个服务器的生产部署，可以使用网络负载平衡 (NLB) 来分配处理负载。  你还需要一个别名（例如，A 或 CNAME 记录），以便向用户公开一个公用名。

>[!IMPORTANT]
> 如果使用除 Windows Server 2012 R2 中的 NLB 功能以外的负载平衡技术，请确保你的解决方案会将会话重定向到同一台服务器，而不是随机的服务器。

在多服务器 MIM 部署中，每个 MIM 服务都具有外部主机名、服务名称和服务分区名称。  服务名称的默认值是计算机的名称，外部主机名和服务分区名称的默认值在 MIM 服务安装期间的询问 MIM 服务的服务器地址的屏幕上配置。 这三个名称分别作为配置节点 `resourceManagementService` 的属性 `externalHostName`、`serviceName` 和 `servicePartitionName` 存储在 %ProgramFiles%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config 文件中。  

当 MIM 服务收到请求时，服务分区名称存储为该请求的一个属性。   随后，只允许具有相同的服务分区名称的其他 MIM 服务安装程序与该请求进行交互。  因此，如果 PAM 方案包括手动批准或其他长时间的请求处理，请确保每个 MIM 服务在该配置文件中具有相同的 `servicePartitionName` 属性。

<a id="recovery" class="xliff"></a>
#### 恢复
在发生故障后，确保在重启 MIM 服务之前堡垒环境中至少有一个 Active Directory 域控制器和 SQL Server 可用。  

工作流实例只能由与启动它的 MIM 服务服务器具有相同的服务分区名称和服务名称的 MIM 服务服务器完成。  如果托管正在处理请求的 MIM 服务的特定计算机出现故障，并且该计算机将不再返回服务中，那么需要在新的计算机上安装 MIM 服务。 安装完成后在新的 MIM 服务上，编辑 *resourcemanagementservice.exe.config* 文件，将新的 MIM 部署的属性 `serviceName` 和 `servicePartitionName` 设为和出现故障的计算机相同的主机名和服务分区名称。

<a id="mim-pam-components" class="xliff"></a>
### MIM PAM 组件
MIM 服务和门户安装程序还包含附加的 PAM 组件，包括 PowerShell 模块和两个服务。

<a id="preparation" class="xliff"></a>
#### 准备
在堡垒环境中的每个正在安装 MIM 服务的计算机上安装 Privileged Access Management 组件。  你无法随后添加这些组件。

<a id="recovery" class="xliff"></a>
#### 恢复
从故障中恢复之后，确保 MIM 服务至少在一台服务器上运行。  然后使用 `net start "PAM Monitoring service"` 命令确保在该服务器上同时运行 MIM PAM 监视服务。

如果堡垒环境林功能级别为 Windows Server 2012 R2，请使用命令 `net start "PAM Component service"` 确保在该服务器上还运行 MIM PAM 组件服务。

