---
title: Microsoft Identity Manager 2016 最佳做法 | Microsoft Docs
description: ''
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/05/2018
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 9ef96b88942fd33107d9021ddddb90d0d80dbed1
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290112"
---
# <a name="microsoft-identity-manager-2016-best-practices"></a>Microsoft Identity Manager 2016 最佳做法

本主题介绍部署和操作 Microsoft Identity Manager 2016 (MIM) 的最佳做法

## <a name="sql-setup"></a>SQL 设置
> [!NOTE]
> 有关设置运行 SQL 的服务器的以下建议假设一个 FIMService 专用的 SQL 实例和一个 FIMSynchronizationService 数据库专用的 SQL 实例。 如果在合并环境中运行 FIMService，则必须对配置进行适当调整。

对结构化查询语言 (SQL) 服务器的配置对于优化系统性能至关重要。 在大规模实施中达到最佳 MIM 性能取决于运行 SQL 的服务器的最佳做法的应用。 有关详细信息，请参阅有关 SQL 最佳做法的下列主题：

-   [Storage Top 10 Best Practices](http://go.microsoft.com/fwlink/?LinkID=183663)（10 大最佳存储做法）

-   [Optimizing tempdb Performance](http://go.microsoft.com/fwlink/?LinkID=188267)（优化 tempdb 性能）

-   [SQL Server 最佳做法文章](http://go.microsoft.com/fwlink/?LinkID=188268)

-   [Reorganizing and Rebuilding Indexes](http://go.microsoft.com/fwlink/?LinkID=188269)（重新组织和重新生成索引）

### <a name="presize-data-and-log-files"></a>预先设置数据和日志文件的大小

请勿依赖自动增长。 应手动管理这些文件的增长。 出于安全考虑，可开启自动增长，但应主动管理数据文件的增长。 有关 MIM 数据库的示例大小，请参阅 [FIM 容量规划指南](http://go.microsoft.com/fwlink/?LinkID=185246)。

### <a name="to-presize-sql-data-and-log-files"></a>预先设置 SQL 数据和日志文件的大小

1.  启动 SQL Server Management Studio。

2.  导航到数据库 FIMService，右键单击“FIMService”，然后单击“属性”。

3.  在“文件”页中，将数据库文件展开到所需大小。

### <a name="isolate-log-from-data-files"></a>将日志与数据文件隔离

按照 SQL Server 最佳做法将数据库的事务和数据日志文件隔离到单独的物理磁盘上。

创建其他 tempdb 文件

为了获得最佳性能，建议在 tempdb 文件中为每个 CPU 内核创建一个数据文件。

### <a name="to-create-additional-tempdb-files"></a>创建其他 tempdb 文件

1.  启动 SQL Server Management Studio。

2.  导航到系统数据库中的数据库 tempdb，右键单击“tempdb”，然后单击“属性”。

3.  在“文件”页中，为每个 CPU 内核创建一个数据文件。 请务必将 tempdb 数据和日志文件分离到不同的驱动器和主轴。

### <a name="ensure-adequate-space-for-log-files"></a>请确保有足够的空间保存日志文件

请务必了解恢复模式的磁盘要求。 在初始系统加载期间，简单的恢复模式对于限制磁盘空间的使用可能是适当的，但是最近备份后创建的数据具有数据丢失的风险。 使用完整恢复模式时，需要管理备份（包括事务日志的频繁备份）的磁盘使用率，防止磁盘空间使用率过高。 有关详细信息，请参阅[恢复模式概述](http://go.microsoft.com/fwlink/?LinkID=185370)。

### <a name="limit-sql-server-memory"></a>限制 SQL Server 内存

根据 SQL 服务器上的内存大小，以及是否与其他服务（即 MIM 2016 服务和 MIM 2016 同步服务）共享 SQL 服务器，可能需要限制 SQL 的内存使用量。 可通过以下步骤执行此操作。

1. 启动 SQL Server Enterprise Manager。

2. 选择“新建查询”。

3. 运行以下查询：

   ```SQL
   USE master

   EXEC sp_configure 'show advanced options', 1

   RECONFIGURE WITH OVERRIDE

   USE master

   EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
   WITH OVERRIDE
   ```

   此示例将 SQL Server 重新配置为使用不超过 12GB 的内存。

4. 使用下列查询验证设置：

   ```SQL
   USE master

   EXEC sp_configure 'max server memory (MB)'--- verify the setting

   USE master

   EXEC sp_configure 'show advanced options', 0

   RECONFIGURE WITH OVERRIDE
   ```

### <a name="backup-and-recovery-configuration"></a>备份和恢复配置

一般来说，应与数据库管理员合作，共同设计备份和恢复策略。 下面给出了一些建议：
- 根据组织的备份策略执行数据库备份。 
- 如果未计划增量日志备份，应将数据库设置为简单恢复模式。 
- 务必先了解不同恢复模型的含义，再实现备份策略。 了解这些模型的磁盘空间要求。 完整恢复模式需要频繁的日志备份，防止磁盘空间使用率过高。 

有关详细信息，请参阅[恢复模式概述](http://go.microsoft.com/fwlink/?LinkID=185370)和 [FIM 2010 Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864)（FIM 2010 备份和还原指南）。

## <a name="create-a-backup-administrator-account-for-the-fim-service-after-installation"></a>安装后创建 FIMService 的备份管理员帐户

FIMService 管理员组的成员拥有对部署 MIM 至关重要的独有权限。 如果无法以管理员组成员身份登录，唯一的解决方法是回退到系统的以前备份。 为了缓解此情况，建议在安装后配置过程中将其他用户添加到 FIM 管理组。

## <a name="fim-service"></a>FIM 服务


### <a name="configuring-fim-service-service-exchange-mailbox"></a>配置 FIM 服务的服务 Exchange 邮箱

以下是为 MIM 2016 服务的服务帐户配置 Microsoft Exchange Server 的最佳做法。

- 配置服务帐户，使其只能接受来自内部电子邮件地址的邮件。 具体而言，服务帐户邮箱应永远不能接收来自外部 SMTP 服务器的邮件。

#### <a name="to-configure-the-service-account"></a>配置服务帐户

1.  在 Exchange 管理控制台中，选择“FIM 服务的服务帐户”。

2.  选择“属性”，选择“邮件流设置”，然后选择“邮件传送限制”。

3.  选择“要求所有发件人都经过身份验证”复选框。

有关详细信息，请参阅[配置邮件传送限制](http://go.microsoft.com/fwlink/?LinkID=183625)。

-   将服务帐户配置为拒绝大于 1 MB 的邮件。 遵循最佳做法[为邮箱或启用邮件的公用文件夹配置邮件大小限制](http://go.microsoft.com/fwlink/?LinkID=183626)。

-   将服务帐户配置为具有 5 GB 的邮箱存储配额。 若要获得最佳结果，请按照 [Configure Storage Quotas for a Mailbox](http://go.microsoft.com/fwlink/?LinkID=156929)（配置邮箱的存储配额）中列出的最佳做法进行操作。

## <a name="mim-portal"></a>MIM 门户


### <a name="disable-sharepoint-indexing"></a>禁用 SharePoint 索引

建议禁用 Microsoft Office SharePoint® 索引。 无需为文档编制索引。 编制索引会导致 MIM 中出现很多错误日志条目，并可能发生性能问题。 若要禁用 SharePoint 索引，请按照以下步骤操作：

1.  在托管 MIM 2016 门户的服务器上，单击“开始”。

2.  单击“所有程序”。

3.  在“所有程序”列表中，单击“管理工具”。

4.  在“管理工具”下，单击“ SharePoint 管理中心”。

5.  在“管理中心”页面上，单击“操作”。

6.  在“操作”页的“全局配置”下，单击“计时器作业定义”。

7.  在“计时器作业定义”页上，单击“SharePoint Services 搜索刷新”。

8.  在“编辑计时器作业”页上，单击“禁用”。

## <a name="mim-2016-initial-data-load"></a>MIM 2016 初始数据加载

此部分列出了一系列步骤，可用于提升将数据从外部系统初始加载到 MIM 的性能。 请务必了解，其中许多步骤仅在系统初始填充阶段执行。 应在加载完成后重置它们。 这是一次性操作，不是连续性同步。

> [!NOTE]
> 若要详细了解如何在 MIM 和 Active Directory 域服务 (AD DS) 之间同步用户，请参阅 FIM 文档中的[如何将用户从 Active Directory 同步到 FIM](http://go.microsoft.com/fwlink/?LinkID=188277)。
> 
> [!IMPORTANT]
> 请确保已应用本指南 SQL 设置部分所述的最佳做法。 

### <a name="step-1-configure-the-sql-server-for-initial-data-load"></a>步骤 1：配置 SQL Server 以进行初始数据加载
初始数据加载可能是一个漫长的过程。 计划初始加载大量数据时，可临时禁用全文搜索，并在 MIM 2016 管理代理 (FIM MA) 上完成导出后重新启用全文搜索，从而缩短填充数据库所需的时间。

暂时关闭全文搜索：

1.  启动 SQL Server Management Studio。

2.  选择“新建查询”。

3.  运行以下 SQL 语句：

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

> [!IMPORTANT]
> 如果不执行这些过程，可能会导致磁盘空间使用率过高，从而导致磁盘空间不足。 可在[恢复模式概述](http://go.microsoft.com/fwlink/?LinkID=185370)中找到有关此主题的更多详细信息。 [FIM 备份和还原指南](http://go.microsoft.com/fwlink/?LinkID=165864)包含更多信息。

### <a name="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process"></a>步骤 2：在加载过程中应用所需的最低 MIM 配置

在初始加载过程中，应仅对管理策略规则 (MPR) 和设置定义应用 FIM 配置所需的最低配置。 完成数据加载后，创建部署所需的其他设置。 使用操作工作流上的连续策略更新设置，通过追溯方式将这些策略应用于已加载数据。

### <a name="step-3-configure-and-populate-the-fim-service-with-external-identity-data"></a>步骤 3：使用外部标识数据配置和填充 FIM 服务

此时，应按照“如何将用户从 Active Directory 域服务同步到 FIM”指南中的过程操作，在系统中配置并同步 Active Directory 中的用户。 如果需要同步组信息，请按照[如何将组从 Active Directory 域服务同步到 FIM](https://technet.microsoft.com/library/ff686936(v=ws.10).aspx) 指南中的过程操作。

#### <a name="synchronization-and-export-sequences"></a>同步和导出序列

若要优化性能，可在导致连接器空间中出现大量挂起导出操作的同步运行后运行导出。 然后，在与受影响的连接器空间相关联的管理代理上运行确认导入。 例如，需要在初始数据加载过程中，在多个管理代理上运行同步运行配置文件时，应在运行每个单独的同步之后运行导出，然后运行增量导入。
对于初始化周期中每个源管理代理，请执行以下步骤：

1.  源管理代理上的完全导入。

2.  源管理代理上的完全同步。

3.  所有受影响的目标管理代理（包含暂存导出操作）上的导出。

4.  所有受影响的目标管理代理（包含暂存导出操作）上的增量导入。

### <a name="step-4-apply-your-full-mim-configuration"></a>步骤 4：应用完整的 MIM 配置

完成初始数据加载后，应为部署应用完整的 MIM 配置。

根据你的方案，这可能包括创建附加集、MPR 和工作流。 对于需要通过追溯方式应用到系统中的所有现有对象的任何策略，使用操作工作流上的连续策略更新设置，通过追溯方式将这些策略应用于已加载数据。

### <a name="step-5-reconfigure-sql-to-previous-settings"></a>步骤 5：将 SQL 重新配置为以前的设置

请记住将 SQL 设置更改为其正常设置。 这包括：

-   启用全文搜索

-   根据你的组织的策略更新备份策略

完成初始数据加载后，需要重新启用全文搜索。 运行以下 SQL 语句可再次启用全文搜索：

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

如果必须切换到简单恢复模式，请确保根据组织的备份策略重新配置备份计划。 [FIM 备份和还原指南](http://go.microsoft.com/fwlink/?LinkID=165864)中提供了 FIM 备份计划的其他详细信息。

## <a name="configuration-migration"></a>配置迁移


### <a name="avoid-changing-display-names"></a>请勿更改显示名称

对于许多对象类型（如 MPR），syncproduction.ps1 脚本将显示名称用作两个系统之间的唯一定位属性。 因此，对现有 MPR 显示名称的更改会导致删除现有 MPR，然后创建新的 MPR。 出现此结果是因为迁移过程无法成功联接已更改联接条件的 MPR。 若要避免此问题，可将自定义属性绑定到所有配置对象类型，并将该属性用作联接条件。 通过此操作，可在不影响迁移过程的情况下修改显示名称。

### <a name="avoid-changing-the-content-of-intermediate-files"></a>请勿更改中间文件的内容

虽然低级对象的文件格式和应用程序编程接口 (API) 是公用的，且操作受开发人员支持，但不建议在迁移期间更改中间格式的内容。 但是，可能需要从 changes.xml 删除整个 ImportObject，或者对 pilot.xml 执行查找和替换操作，以替换生产 DNS 信息的版本号或试点域名系统 (DNS) 信息。

### <a name="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions"></a>在跨版本迁移时，请确保 pilot.xml 中的版本号正确

虽然不推荐或不支持跨版本号迁移，但通常可通过将试点版本号替换为 pilot.xml 中的生产版本号来执行此操作。 具体而言， WorkflowDefinition 和

ActivityInformationConfiguration 对象需要版本号来准确引用生产环境中的工作流活动。 如果未能替换版本号，Compare-FIMConfig cmdlet 会识别出 WorkflowDefinitions 上 Extensible Object Markup Language (XOML) 属性之间的差异，并迁移试点版本号。 生产 FIM 服务可能无法启动具有错误版本号的工作流活动。

### <a name="avoid-cyclic-references"></a>请勿使用循环引用

一般情况下，不建议在 MIM 配置中使用循环引用。 但是，当集 A 引用集 B 且集 B 也引用集 A 时，有时会出现循环。为避免循环引用的问题，应更改集 A 或集 B 的定义，使其不相互引用。 然后，重新开始迁移过程。 如果存在循环引用，且 Compare-FIMConfig cmdlet 导致错误，则需要手动中断循环。 由于 Compare-FIMConfig cmdlet 以优先级顺序输出更改列表，因此它要求配置对象的引用之间不存在循环。

## <a name="security"></a>安全

### <a name="mim-ma-account"></a>MIM MA 帐户

不应将 MIM MA 帐户视为服务帐户，而应将其视为普通用户帐户。 必须能够本地登录此帐户，以便 FIM 同步服务的服务帐户对其进行模拟。

启用 MIM MA 帐户以进行本地登录

1.  依次单击“开始”、“管理工具”和“本地安全策略”。

2.  打开“本地策略”节点，然后单击“用户权限分配”。

3.  在“允许本地登录”策略中，请确保明确指定 FIM MA 帐户，或将其添加到已授予访问权限的一个组中。

### <a name="fim-synchronization-service-and-fim-services-accounts"></a>FIM 同步服务和 FIM 服务帐户

若要通过安全方式配置运行 MIM 服务器组件的服务器，应限制服务帐户。 使用上述步骤打开 MIM MA 帐户，对 FIM 同步服务和 FIM 服务帐户设置以下限制：

-   拒绝作为批处理作业登录

-   拒绝本地登录

-   拒绝从网络访问该计算机

服务帐户不应该是本地管理员组的成员。

FIM 同步服务的服务帐户不应是用于控制 FIM 同步服务（以 FIMSync 开头的组，例如 FIMSyncAdmins 等）访问权限的安全组成员。

> [!IMPORTANT]
>  如果选择两个服务帐户使用同一帐户的选项，并将 FIM 服务和 FIM 同步服务分开，则无法在 mms 同步服务服务器上设置“拒绝从网络访问此计算机”。 如果访问被拒绝，将禁止 FIM 服务联系 FIM 同步服务以更改配置和管理密码。

### <a name="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile"></a>部署到网亭式计算机的密码重置应设置本地安全性以清除虚拟内存页面文件

在要作为网亭的工作站上部署 FIM 密码重置时，建议启用“关闭：清除虚拟内存页面文件本地安全性策略”设置，确保进程内存中的敏感信息对未经授权用户不可用。

### <a name="implementing-ssl-for-the-fim-portal"></a>实现 FIM 门户的 SSL

强烈建议在 FIM 门户服务器上使用安全套接字层 (SSL) 来保护客户端和服务器之间的通信。

实现 SSL：

1.  在 MIM 门户服务器上，打开 IIS Manager。

2.  单击本地计算机名称。

3.  单击“服务器证书”。

4.  单击“创建证书申请”。

5.  在“公用名”文本框中，输入服务器的名称。

6.  单击“下一步”，然后再次单击“下一步”。

7.  将文件保存到任意位置。 需要在后续步骤中访问此位置。

8.  浏览到 https://servername/certsrv。 将 servername 替换为颁发证书的服务器名称。

9.  单击“申请新证书”。

10. 单击“提交高级申请”。

11. 单击“使用 base 64 编码文件提交证书申请”。

12. 粘贴在上一步中保存的文件内容。

13. 在证书模板中，选择 Web 服务器。

14. 单击“提交”。

15. 将证书保存到桌面。

16. 在 IIS Manager 中，单击“完成证书申请”。

17. 将 IIS Manager 指向刚保存到桌面的证书。

18. 对于友好名称，请键入服务器的名称。

19. 单击“站点”，然后选择 SharePoint – 80。

20. 单击“绑定”，然后单击“添加”。

21. 选择“https”。

22. 对于证书，选择与服务器名称相同的证书（即刚导入的证书）。

23. 单击“确定”。

24. 删除 HTTP 绑定。

25. 单击“SSL 设置”，然后选中“需要 SSL”。

26. 保存设置。

27. 依次单击“开始”、“管理工具”、“SharePoint 3.0 管理中心”。

28. 单击“操作”，然后单击“备用访问映射”。

29. 单击“ http://servername ”。

30. 将 http://servername 更改为 https://servername ，然后单击“确定”。

31. 依次单击“开始”、“运行”，键入 iisreset，然后单击“确定”。

## <a name="performance"></a>性能

为了实现最佳性能配置：

-   应用本文档中 SQL 设置部分所述的 SQL 设置最佳做法。

-   在 MIM 门户网站上禁用 SharePoint 索引。 有关详细信息，请参阅本文档中的“禁用 SharePoint 索引”部分。

## <a name="feature-specific-best-practices"></a>功能特定的最佳做法 


### <a name="request-management"></a>申请管理

默认情况下，MIM 2016 会清除过期的系统对象，其中包括超过 30 天的已完成申请以及关联的审批、审批回复和工作流实例。 如果组织需要更长时间的申请历史记录，应从 MIM 导出申请并将其存储在辅助数据库中，使保留时间超过 30 天。 虽然可配置 30 天申请删除时间范围，但延长此时间范围可能会因系统中的额外对象导致性能下降。

### <a name="management-policy-rules"></a>管理策略规则

#### <a name="use-the-appropriate-mpr-type"></a>使用合适的 MPR 类型

MIM 提供了两种类型的 MPR、请求和集转换：

- 请求 MPR (RMPR)

  - 用于定义针对资源的创建、读取、更新或删除 (CRUD) 操作的访问控制策略（身份验证、授权和操作）。
  - 在 MIM 中对目标资源发出 CRUD 操作请求时应用。
  - 根据规则中定义的匹配条件设置范围，即规则应用于哪些 CRUD 请求。

- 集转换 MPR (TMPR)
  - 用于定义策略，而不管对象如何进入由转换集表示的当前状态。 使用 TMPR 建立权利策略模型。
  - 资源进入或离开关联集时应用。
  - 范围设置为集的成员。

>[备注]有关其他详细信息，请参阅 [Designing Business Policy Rules](http://go.microsoft.com/fwlink/?LinkID=183691)（设计业务策略规则）。

#### <a name="only-enable-mprs-as-necessary"></a>仅在必要时启用 MPR

应用配置时，请使用最小特权原则。 MPR 控制 MIM 部署的访问策略。 仅启用大多数用户使用的功能。 例如，并非所有用户都使用 MIM 进行组管理，因此应禁用关联的组管理 MPR。 默认情况下，MIM 禁用大多数非管理员权限。

#### <a name="duplicate-built-in-mprs-instead-of-directly-modifying"></a>应复制内置 MPR，而不是直接修改
需要修改内置 MPR 时，应使用所需配置创建一个新的 MPR，并关闭内置 MPR。 这可确保对通过升级过程引入的内置 MPR 的任何更改都不会对系统配置产生负面影响。

#### <a name="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs"></a>最终用户权限应使用范围设置为用户业务需求的显式属性列表
通过使用显式属性列表，有助于在向对象添加属性时，防止向无特权用户意外授予权限。 管理员应明确需要对新属性授予权限，而不是尝试删除权限。

对数据的访问应将范围设置为用户的业务需求。 例如，组成员不应对其所属组的筛选器属性具有访问权限。 筛选器可能会意外泄露用户通常无法访问的组织数据。

#### <a name="mprs-should-reflect-effective-permissions-in-the-system"></a>MPR 应该反映系统中的有效权限
请勿授予用户从不使用的属性的权限。 例如，不应授予修改核心资源属性（如 objectType）的权限。 尽管有 MPR，在创建资源类型后对其进行的任何修改尝试都会被系统拒绝。

#### <a name="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs"></a>在 MPR 中使用显式属性时，读取权限应与修改和创建权限分开

在 MPR 中显式列出属性时，创建和修改所需的属性通常与可用于读取的属性不同。 例如，可为系统属性（如 Creator 或 objectId）授予读取权限，不能为系统属性指定创建或修改权限。

#### <a name="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules"></a>在规则中使用显式属性时，创建权限应与修改权限分开

创建操作要求用户在操作中选择 objectType。 这是核心系统属性，在创建操作后无法修改。

#### <a name="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements"></a>对所有具有相同访问要求的属性使用同一个请求 MPR

对于具有不会改变的相同访问要求的属性，可将其合并到单个请求 MPR 以提高效率。

#### <a name="avoid-giving-unrestricted-access-even-to-selected-principal-groups"></a>请勿向选定主体组授予无限制访问权限

在 MIM 中，权限被定义为肯定断言。 由于 MIM 不支持“拒绝”权限，因此，如果向资源授予无限制访问权限，则会导致在权限中指定任何例外情况变得复杂起来。 最佳做法是仅授予必要的权限。

#### <a name="use-tmprs-to-define-custom-entitlements"></a>使用 TMPR 定义自定义权利

使用集转换 MPR (TMPR) 而不是 RMPR 来定义自定义权利。 TMPR 提供基于状态的模型，根据定义的转换集或角色的成员身份以及伴随的工作流活动来分配或删除权利。 应始终成对定义 TMPR，一个用于资源向内转换，一个用于资源向外转换。此外，每个转换 MPR 应包含用于预配和取消预配活动的单独工作流。

> [!NOTE]
> 任何取消预配的工作流都应确保“连续策略更新”属性设置为 true。

#### <a name="enable-the-set-transition-in-mpr-last"></a>最后启用“集向内转换 MPR”

创建 TMPR 对时，最后打开“集向内转换 MPR”。 此顺序可确保将权利添加到集然后从中删除时，在打开“向内 MPR”（未打开“向外 MPR”）的情况下，没有资源留下。

#### <a name="workflows-in-tmpr-should-check-target-resource-state-first"></a>TMPR 中的工作流应首先检查目标资源状态

预配工作流应首先进行检查，确定目标资源是否已根据权利进行预配。 如果是，则不执行任何操作。

取消预配工作流应首先进行检查，确定是否已预配目标资源。 如果是，应取消预配目标资源。 否则，不执行任何操作。

#### <a name="select-run-on-policy-update-for-tmprs"></a>为 TMPR 选择“持续策略更新”

这可确保实施策略更新时应用正确的预配行为，并确保此行为在与 TMPR 关联的操作工作流上使用持续策略更新标志。 这可确保策略定义中的更改将操作工作流应用于转换集的新成员。

#### <a name="avoid-associating-the-same-entitlement-with-two-different-transition-sets"></a>请勿将相同的权利与两个不同的转换集关联

如果资源从一个集移动到另一个集，则将同一权利与两个不同的转换集关联可能导致不必要的撤消和重新授予权利。 最佳做法是确保一个集包含需要关联权利的所有资源。 这可确保转换集与授予工作流的权利之间的一对一关系。

#### <a name="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system"></a>删除系统中的权利时，请使用恰当的操作顺序

删除系统中权利执行的步骤顺序可能会导致两个不同的操作结果。 请确保了解哪种顺序适用于所需效果。

删除系统中的某个权利（并从当前拥有此权利的所有成员中撤消它）：

1.  禁用 T-In MPR。 这可避免新的授权。

2.  删除 T 集筛选器或进行更改，使该集为空。 这将导致所有现有成员向外转换并应用向外转换策略，包括与权利相关联的已配置取消预配工作流。

3.  禁用 T-Out MPR。

撤消权利而不影响当前成员（例如，停止使用 MIM 管理权利）的具体步骤：

1.  禁用 T-In MPR。 这可避免新的授权。

2.  禁用 T-Out MPR。

3.  删除 T 集筛选器或进行更改，使该集为空。 因为集不再与 TMPR 关联，所以不应用取消预配工作流。

### <a name="sets"></a>设置数

为集应用最佳做法时，需要考虑可管理性优化的影响和未来管理的方便性。 在应用这些建议之前，应按预期生产规模执行适当测试，确定性能与可管理性之间的最佳平衡。

>[!NOTE]
> 以下所有指南适用于动态集和动态组。


#### <a name="minimize-the-use-of-dynamic-nesting"></a>最大程度减少动态嵌套的使用

这是指引用另一个集的 ComputedMember 属性的集的筛选器。 使用嵌套集的常见原因是避免在多个集之间复制成员条件。 虽然这种方法可能会提高集的可管理性，但会降低性能。 可通过复制嵌套集的成员条件（而不是复制嵌套集本身）来优化性能。

可能存在必须使用嵌套集来满足功能要求的情况。 以下是应使用嵌套集的主要情况。 例如，若要定义没有全职员工雇主的所有组的集，必须使用集的嵌套，如下所示：`/Group[not(Owner = /Set[ObjectID = ‘X’]/ComputedMember]`，其中“X”是“所有全职员工”集的 ObjectID。

#### <a name="minimize-the-use-of-negative-conditions"></a>尽量减少否定条件的使用

否定条件是使用以下运算符或函数的成员条件：`!=`、`not()`、`\<` 和 `\<=`。 为了优化性能，在可能的情况下，使用多个肯定条件而不是否定条件来表达所需条件。

#### <a name="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes"></a>尽量减少使用基于多值引用属性的成员条件

应尽量减少使用基于多值引用属性的条件，因为大量此类集可能会影响成员条件中使用的属性的操作性能。

### <a name="password-reset"></a>密码重置

#### <a name="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile"></a>用于密码重置的网亭式计算机应设置本地安全性，清除虚拟内存页面文件

在要作为网亭的工作站上部署 MIM 密码重置时，建议启用“关闭：清除虚拟内存页面文件”本地安全策略设置，以确保未经授权用户无法访问进程内存中的敏感信息。

#### <a name="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to"></a>用户应该始终在登录的计算机上注册密码重置

当用户尝试通过 Web 门户注册密码重置时，无论是谁登录网站，MIM 始终都会代表登录用户启动注册。 用户应该始终在登录的计算机上注册密码重置。

#### <a name="do-not-set-the-avoidpdconwan-registry-key-to-true"></a>请勿将 AvoidPdcOnWan 注册表项设置为 true

使用 MIM 2016 密码重置时，请勿将 AvoidPdcOnWan 注册表项设置为 true。

如果此注册表项设置为 true，用户很可能会通过密码入口，在主域控制器 (PDC) 上重置其密码，并尝试登录。 由于此注册表项，本地域控制器不会使用 PDC 执行辅助验证，因此拒绝登录请求。 如果用户被拒绝了足够多次，将被锁定在域外，并且需要致电支持人员。

#### <a name="do-not-turn-on-logging-of-clear-text-passwords"></a>请勿启用明文密码记录

在 Windows 中打开诊断服务级别跟踪时，可以记录明文密码

Communication Foundation (WCF)。 默认情况下，不启用此选项，并且不建议在生产环境中启用此选项。 用户注册密码重置时，这些密码将显示为加密的简单对象访问协议 (SOAP) 消息中的明文元素。 有关详细信息，请参阅[配置消息日志记录](http://go.microsoft.com/fwlink/?LinkID=168572)。

#### <a name="do-not-map-an-authorization-workflow-to-the-password-reset-process"></a>请勿将授权工作流映射到密码重置过程

不应将授权工作流附加到密码重置操作。 密码重置需要包含活动（如审批活动）的同步响应和授权工作流为异步。

#### <a name="do-not-map-multiple-action-activities-to-password-reset"></a>请勿将多个操作活动映射到密码重置

请勿将包含多个操作活动的工作流附加到密码重置操作。 其中一个示例方案是将第二个 AD DS 密码重置活动附加到密码重置 MPR。 不支持此方案。

#### <a name="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow"></a>在添加、删除或更改现有工作流中活动的顺序时，需要重新注册

在现有工作流中添加、删除或更改身份验证活动的顺序时，请始终选择需要重新注册的选项。 在工作流添加活动或从工作流中删除活动之后，但在重新注册之前，尝试对密码重置进行身份验证的用户可能会遇到不良影响。

### <a name="portal-configuration-and-resource-control-display-configuration"></a>门户配置和资源控制显示配置

#### <a name="consider-adding-a-privacy-disclaimer-to-the-user-profile-page"></a>请考虑向用户个人资料页面添加隐私免责声明

默认情况下，在 MIM 中，一些用户个人资料信息可向其他用户显示。 为了方便用户，管理员应考虑将符合公司策略的自定义文本添加到“用户个人资料”页面。 有关将自定义文本添加到 MIM 门户页面的详细信息，请参阅 [Introduction to Configuring and Customizing the FIM Portal](http://go.microsoft.com/fwlink/?LinkID=165848)（配置和自定义 FIM 门户简介）。

### <a name="schema"></a>Schema

#### <a name="do-not-delete-person-or-group-resource-types"></a>请勿删除人员或组资源类型

虽然人员和组资源类型未标记为核心资源类型，但不应删除资源本身或向其分配的属性。 MIM 门户中的用户界面 (UI) 要求存在人员和组资源类型及其属性。

#### <a name="do-not-modify-the-core-attributes"></a>请勿修改核心属性

有 13 个核心属性分配给所有资源类型。 不应以任何方式修改其与任何资源类型的关系。 这 13 个核心属性包括：

-   CreatedTime

-   Creator

-   DeletedTime

-   描述

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   区域设置

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

请勿删除依赖于审核要求的架构资源

在对架构资源仍有审核要求时，不应删除这些资源。

#### <a name="making-regular-expressions-case-insensitive"></a>将正则表达式设置为不区分大小写

在 MIM 中，将正则表达式设置为不区分大小写可能会有所帮助。 可使用 ?!: 忽略组中的大小写。 例如，对于“员工类型”，请使用

`\^(?!:contractor\|full time employee)%.`

#### <a name="calculation-of-the-member-attribute"></a>成员属性的计算

向同步引擎公开的成员属性实际上会映射到 ComputedMembers。 它是基于条件的成员和手动选择的成员的组合。 即使添加了所有三个属性（Filter、ExplicitMembers 和 ComputedMembers），除了组和集之外，不会针对资源类型进行成员属性的动态计算。

#### <a name="leading-and-trailing-spaces-in-strings-are-ignored"></a>将忽略字符串中的前导和尾部空格

在 MIM 中，可输入包含前导和尾部空格的字符串，但 MIM 系统会忽略这些空格。 如果提交带前导和尾部空格的字符串，同步引擎和 Web 服务将忽略这些空格。

#### <a name="empty-strings-do-not-equal-null"></a>空字符串不等于 null

在此版 MIM 中，空字符串不等于 null。 空字符串输入被视为有效值。 不存在被视为 null。

### <a name="workflow-and-request-processing"></a>工作流和请求处理

#### <a name="do-not-delete-default-workflows-that-are-shipped-with-mim-2016"></a>请勿删除 MIM 2016 随附的默认工作流

MIM 随附以下工作流，不得删除它们：

-   过期工作流

-   适用于管理员的筛选器验证工作流

-   适用于非管理员的筛选器验证工作流

-   组过期通知工作流

-   组验证工作流

-   所有者审批工作流

-   密码重置操作工作流

-   密码重置身份验证工作流

-   具有所有者授权的请求者验证

-   没有所有者授权的请求者验证

-   注册所需的系统工作流

#### <a name="do-not-run-two-or-more-approvalactivities-in-parallel"></a>请勿并行运行两个或多个 ApprovalActivities

不应并行运行两个或多个 ApprovalActivities。 这样做可能会导致请求滞留在授权阶段。 对于多个审批，在审批中包括大型审批者列表，或者将两个活动进行连续序列化。

#### <a name="authorization-activities-should-not-modify-mim-resources-data"></a>身份验证活动不应修改 MIM 资源数据

请勿将修改 MIM 资源的活动（例如函数评估器活动）用作授权工作流中工作流的一部分。 由于请求未在处理的授权点提交，因此可应用对标识信息执行的任何修改，尽管请求可能被拒绝。

### <a name="understanding-fim-service-partitions"></a>了解 FIM 服务分区

MIM 的目标是根据配置的业务策略，处理可由各种 MIM 客户端（如 FIM 同步服务和自助服务组件）发起的请求。 按照设计，每个 FIM 服务实例属于由一个或多个 FIM 服务实例组成的逻辑组，也称为 FIM 服务分区。 如果只部署了一个 FIM 服务实例来处理所有请求，可能遇到处理延迟。 某些操作甚至可能超出适用于自助服务操作的默认超时值。 FIM 服务分区可帮助你解决此问题。

有关详细信息，请参阅[了解 FIM 服务分区](https://social.technet.microsoft.com/wiki/contents/articles/2363.understanding-fim-service-partitions.aspx)。

## <a name="next-steps"></a>后续步骤
- [FIM 备份和还原指南](http://go.microsoft.com/fwlink/?LinkID=165864)
- [如何将用户从 Active Directory 同步到 FIM](http://go.microsoft.com/fwlink/?LinkID=188277) 
- [恢复模型概述](http://go.microsoft.com/fwlink/?LinkID=185370)。