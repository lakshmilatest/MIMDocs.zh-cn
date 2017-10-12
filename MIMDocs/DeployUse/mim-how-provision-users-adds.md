---
title: Microsoft Identity Manager 2016 | Microsoft Docs
description: "了解使用 Microsoft Identity Manager 2016 在 ADDS 中创建用户的过程"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 0e039c0b05cc9088a6934c07ead09fb5250f8a81
ms.sourcegitcommit: 81ad3ede52c3a4308b806c0c8a3fd42b5f5965b0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="how-do-i-provision-users-to-ad-ds"></a>如何将用户预配到 AD DS

适用于：Microsoft Identity Manager 2016 SP1 (MIM)

标识管理系统的一个基本要求就是具备将资源预配到外部系统的能力。

本指南包含以下内容：指导你完成将用户从 Microsoft® Identity Manager (MIM) 2016 预配到 Active Directory® 域服务 (AD DS) 的过程中所涉及的主要构建基块；概述如何验证方案是否按预期工作；针对使用 MIM 2016 管理 Active Directory 提供相关建议；列出其他信息来源。

## <a name="before-you-begin"></a>准备工作


本部分将介绍此文档范围内的相关信息。 “如何操作”指南一般面向已基本熟悉通过 MIM 同步对象的流程（相关[入门指南](http://go.microsoft.com/FWLink/p/?LinkId=190486)中有介绍）的用户。

### <a name="audience"></a>受众


本指南面向满足以下要求的信息技术 (IT) 专业人员：已基本了解 MIM 同步过程的工作原理；希望获得特定方案的实践经验和更多相关概念信息。

### <a name="prerequisite-knowledge"></a>必备知识


本文档假定你有权访问正在运行的 MIM 实例并具有以下文档中所述的配置简单同步方案的经验：

-   [Introduction to Inbound Synchronization](http://go.microsoft.com/FWLink/p/?LinkId=189652)（入站同步简介）

-   [Introduction to Outbound Synchronization](http://go.microsoft.com/FWLink/p/?LinkId=189653)（出站同步简介）

本文档内容的范围限于充当这些介绍性文档的扩展。

### <a name="scope"></a>范围


本文档中概述的方案已进行简化，以满足基本实验室环境的要求。 重点在于帮助你了解所讨论的概念和技术。

本文档有助于你开发一种解决方案，即使用 MIM 管理 AD DS 中的组。

### <a name="time-requirements"></a>时间要求


本文档中的过程需要花费 90 到 120 分钟才能完成。

此预估时间假定已配置测试环境且不包含设置测试环境所需的时间。

### <a name="getting-support"></a>获取支持


如果存在与本文档内容相关的问题，或者需要提供想要讨论的一般性反馈，请在 [Forefront Identity Manager 2010 论坛](http://go.microsoft.com/FWLink/p/?LinkId=189654)中发布消息。

## <a name="scenario-description"></a>方案说明


虚构公司 Fabrikam 计划使用 MIM 管理公司 AD DS 中的用户帐户。 在此过程中，Fabrikam 需要将用户预配到 AD DS。 为了启动初始测试，Fabrikam 安装了包括 MIM 和 AD DS 的基本实验室环境。
在该实验室环境中，Fabrikam 将测试包含 MIM 门户中手动创建的用户的方案。 此方案旨在将用户作为已启用预定义密码的用户预配到 AD DS。

## <a name="scenario-design"></a>方案设计


若要使用本指南，必须具备三个体系结构组件：

-   Active Directory 域控制器

-   运行 FIM 同步服务的计算机
-   运行 FIM 门户的计算机

下图概述了必需的环境。

![必需的环境](media/how-provision-users-adds/image001.png)


可在一台计算机上运行所有组件。

>[!NOTE]
有关设置 MIM 的详细信息，请参阅 [FIM 安装指南](http://go.microsoft.com/FWLink/p/?LinkId=165845)。

## <a name="scenario-components-list"></a>方案组件列表


下表列出了本指南中方案所包含的组件。

| ![组织单位](media/how-provision-users-adds/image005.jpg)   | 组织单位                | MIM 对象 - 用作预配用户目标的组织单位 (OU)。                                                       |
|----------------------------------------|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| ![用户帐户](media/how-provision-users-adds/image006.jpg)   | 用户帐户                      | · ADMA - 具有足够权限可连接到 AD DS 的 Active Directory 用户帐户。<br/> · FIMMA - 具有足够权限可连接到 MIM 的 Active Directory 用户帐户。
                                                                 |
| ![管理代理和运行配置文件](media/how-provision-users-adds/image007.jpg)  | 管理代理和运行配置文件 | · Fabrikam ADMA - 与 AD DS 交换数据的管理代理。 <br/> · Fabrikam FIMMA - 与 MIM 交换数据的管理代理。                                                                                 |
| ![同步规则](media/how-provision-users-adds/image008.jpg)  | 同步规则              | Fabrikam Group 出站同步规则 - 将用户预配到 AD DS 的出站同步规则。                                     |
| ![设置数](media/how-provision-users-adds/image009.jpg)   | 设置数                               | 所有合同工 - 一个集，其中所有对象的动态成员资格都具有合同工的 EmployeeType 属性。                                |
| ![工作流](media/how-provision-users-adds/image010.jpg)  | 工作流                          | AD 预配工作流 - 将 MIM 用户纳入 AD 出站同步规则的作用域的工作流。                                |
| ![管理策略规则](media/how-provision-users-adds/image011.jpg)   | 管理策略规则            | AD 预配管理策略规则 - 当资源成为“所有合同工”集成员时触发的管理策略规则 (MPR)。 |
| ![MIM 用户](media/how-provision-users-adds/image012.jpg) | MIM 用户                          | Britta Simon - 预配到 AD DS 的 MIM 用户。                                                                                             |



## <a name="scenario-steps"></a>方案步骤


本指南中概述的方案包括下图中所示的构建基块。

![方案步骤](media/how-provision-users-adds/image013.png)


## <a name="configuring-the-external-systems"></a>配置外部系统


本部分将说明需要在 MIM 环境外部创建的资源。

### <a name="step-1-create-the-ou"></a>步骤 1：创建 OU


需要 OU 作为预配示例用户的容器。 有关创建 OU 的详细信息，请参阅[新建组织单位](http://go.microsoft.com/FWLink/p/?LinkId=189655)。

在 AD DS 中创建名为 MIMObjects 的 OU。

### <a name="step-2-create-the-active-directory-user-accounts"></a>步骤 2：创建 Active Directory 用户帐户

本指南中的方案需要 2 个 Active Directory 用户帐户：

- ADMA - 被 Active Directory 管理代理使用。

- FIMMA - 被 FIM 服务管理代理使用。

这两种情况都只需创建普通用户帐户。 本文档后面部分将介绍这两个帐户特定要求的详细信息。 有关创建用户的详细信息，请参阅[新建用户帐户](http://go.microsoft.com/FWLink/p/?LinkId=189656)。


## <a name="configuring-the-fim-synchronization-service"></a>配置 FIM 同步服务


本部分中的配置步骤需要启动 FIM Synchronization Service Manager。

### <a name="creating-the-management-agents"></a>创建管理代理

本指南中的方案需要创建 2 个管理代理：

-   Fabrikam ADMA - 用于 AD DS 的管理代理。

-   Fabrikam FIMMA - 用于 FIM 服务管理代理的管理代理。

### <a name="step-3-create-the-fabrikam-adma-management-agent"></a>步骤 3：创建 Fabrikam ADMA 管理代理

配置用于 AD DS 的管理代理时，请指定与 AD DS 的数据交换中管理代理使用的帐户。 应使用普通用户帐户。 但若要从 AD DS 导入数据，该帐户必须有权轮询 DirSync 控件中的更改。 如果希望管理代理将数据导出到 AD DS，请授予帐户对目标 OU 的足够权限。 有关该主题的详细信息，请参阅[配置 ADMA 帐户](http://go.microsoft.com/FWLink/p/?LinkId=189657)。

若要在 AD DS 中创建用户，则必须流出对象的 DN。 除此之外，最好流出名字、姓氏和显示名称，确保对象可发现。

在 AD DS 中，用户使用 sAMAccountName 属性登录目录服务仍是常见操作。 如果不指定此属性的值，目录服务会为其生成随机值。 但是，这些随机值并非用户友好型，这也是此属性的用户友好版本通常包含在导出到 AD DS 过程的原因。 若要使用户能够登录到 AD DS，还需要包括导出逻辑中使用 unicodePwd 属性创建的密码。

>[!Note]                                
请确保指定为 unicodePwd 的值符合目标 AD DS 的密码策略。

为 AD DS 帐户设置密码时，还需要创建一个帐户作为已启用的帐户。 可通过设置 userAccountControl 属性来实现此目的。 有关 userAccountControl 属性的详细信息，请参阅[使用 FIM 启用或禁用 Active Directory 中的帐户](http://go.microsoft.com/FWLink/p/?LinkId=189658)。

下表列出了需要配置的特定于方案的最重要设置。

| 管理代理设计器页面                          | 配置                                                  |
|---------------------------------------------------------|----------------------------------------------------------------|
| 创建管理代理                                 | 1.管理代理适用对象：AD DS  <br/> 2.名称：Fabrikam ADMA |
| 连接到 Active Directory 林                      | 1.选择目录分区：“DC=Fabrikam,DC=com”   <br/>   2.单击“容器”打开“选择容器”对话框，确保“MIMObjects”是唯一选中的 OU。        |
| 选择对象类型                                     | 除了已选择的对象类型，请选择“用户”。 |
| 选择属性                                       | 1.单击“全部显示”。 <br/>   2.选择以下属性： <br/> &nbsp;&nbsp;&nbsp;° displayName <br/> &nbsp;&nbsp;&nbsp;° givenName <br/> &nbsp;&nbsp;&nbsp;° sn <br/> &nbsp;&nbsp;&nbsp;° SamAccountName <br/> &nbsp;&nbsp;&nbsp;° unicodePwd <br/> &nbsp;&nbsp;&nbsp;° userAccountControl     

有关详细信息，请参阅帮助中的下列主题：
- 创建管理代理
- 连接到 Active Directory 林
- 使用适用于 Active Directory 的管理代理
- 配置目录分区

>[!Note]
确保导入为 ExpectedRulesList 属性配置的属性流规则。

### <a name="step-4-create-the-fabrikam-fimma-management-agent"></a>步骤 4：创建 Fabrikam FIMMA 管理代理

配置 FIM 服务管理代理时，请指定与 FIM 服务的数据交换中管理代理使用的帐户。

应使用普通用户帐户。 该帐户必须与 MIM 安装过程中指定的帐户相同。 对于可用于确定安装期间指定的 FIMMA 帐户名称以及测试此帐户是否仍有效的脚本，请使用 Windows PowerShell 完成 [FIM MA 帐户配置快速测试](http://go.microsoft.com/FWLink/p/?LinkId=189659)。

下表列出了需要配置的特定于方案的最重要设置。 根据下表中提供的信息创建管理代理。  

| 管理代理设计器页面 | 配置 |
|------------|------------------------------------|
| 创建管理代理 | 1.管理代理适用对象：FIM 服务管理代理 <br/> 2.名称：Fabrikam FIMMA |
| 连接到数据库     | 使用以下设置： <br/> · 服务器：localhost <br/> · 数据库：FIMService <br/> · FIM 服务基址：http://localhost:5725 <br/> <br/> 提供为此管理代理创建的帐户相关信息 |
| 选择对象类型                                     | 除了已选择的对象类型，请选择“人员”。   |
| 配置对象类型映射                          | 除了已存在的对象类型映射，请将“数据源对象类型”人员的映射添加到“Metaverse”对象类型人员。 |
| 配置属性流                                | 除了已存在的属性流映射，请添加以下属性流映射： <br/><br/> ![属性流](media/how-provision-users-adds/image018.jpg) |




有关详细信息，请参阅帮助中的下列主题：
-   创建管理代理

-   连接到 Active Directory 数据库

-   使用适用于 Active Directory 的管理代理

-   配置目录分区

>[!NOTE]
 确保导入为 ExpectedRulesList 属性配置的属性流规则。

### <a name="step-5-create-the-run-profiles"></a>步骤 5：创建运行配置文件

下表列出了需要为本指南中的方案创建的运行配置文件。

| 管理代理  | 运行配置文件     |
|-------------------|--------------------------------------|
| Fabrikam ADMA     | 1.完全导入  <br/> 2.完全同步 <br/> 3.增量导入 <br/> 4.增量同步 <br/> 5.导出                                                                    |
| Fabrikam FIMMA   | 1.完全导入 <br> 2.完全同步 <br/> 3.增量导入 <br/> 4.增量同步 <br/> 5.导出|                                                                                                                                                                                   

根据上表为每个管理代理创建运行配置文件。


>[!Note]
有关详细信息，请参阅 MIM 帮助中的创建管理代理运行配置文件。                                                                                                                  


>[!Important]
 验证环境中是否启用了预配。 可通过运行脚本实现此目的，请参阅使用 Windows PowerShell 启用预配 (http://go.microsoft.com/FWLink/p/?LinkId=189660)。


## <a name="configuring-the-fim-service"></a>配置 FIM 服务


本指南中的方案需要配置预配策略，如下图所示。

![设置策略](media/how-provision-users-adds/image019.png)

此预配策略的目的是将组纳入 AD 用户出站同步规则的作用域。 通过将资源纳入同步规则的作用域，同步引擎能够根据你的配置将资源预配到 AD DS。

若要配置 FIM 服务，可在 Windows Internet Explorer® 中导航到 http://localhost/identitymanagement。 在 MIM 门户页上，若要创建预配策略，请从“管理”部分转到相关页面。 若要验证配置，应运行[使用 Windows PowerShell 记录预配策略配置](http://go.microsoft.com/FWLink/p/?LinkId=189661)中的脚本。

### <a name="step-6-create-the-synchronization-rule"></a>步骤 6：创建同步规则

下表显示了所需的 Fabrikam 预配同步规则的配置。 根据下表中的数据创建同步规则。

| 同步规则配置                                                                         |                                                                             |                                                           
|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|
| Name                                                                                                       | Active Directory 用户出站同步规则                         |                                                          
| 说明                                                                                               |                                                                             |                                                           
| 优先级                                                                                                | 2                                                                           |                                                           
| 数据流方向   | 出站             |       
| 依赖关系       |         |                                         


| 作用域 |                                                                             |                                                           
|--------|-------|
| Metaverse 资源类型 | 个人 |                                                         
| 外部系统                   |Fabrikam ADMA                                                               |                                                       
| 外部系统资源类型                                                                              | user      



| 关系 ||
|------------|---------|
| 在外部系统中创建资源                                                                         | True                                                                        |                                                           
| 启用取消预配                                                                                      | False                                                                       |                                                           

| 关系条件                                                                                      | |
|------------|----------|
| ILM 属性     | 数据源属性                                                       |
| 数据源属性         | sAMAccountName    |

| 初始出站属性流        | |                                                             |
|-------------------|---------------------- |---------------|
| 允许 Null                 | 目标                                                                 | 源                                                    |
| false                       | dn                                                                          | \+("CN=",displayName,",OU=MIMObjects,DC=fabrikam,DC=com") |
| false                       | userAccountControl                                                          | 常量：512                                         |
| false                                                                     | unicodePwd                    | 常量：P\@\$\$W0rd                                    |

| 永久性出站属性流  |                                                                     |                                                           |
|--------------------------------------|---------------------------------------------------------------------|-----------------------------------------------------------|
| 允许 Null                                                                                                | 目标                                                                 | 源                                                    |
| false                                                                                                      | sAMAccountName                                                              | accountName                                               |
| false                                                                                                      | displayName                                                                 | displayName                                               |
| false                                                                                                      | givenName                                                                   | firstName                                                 |
| false                                                                                                      | sn                                                                          | lastName                                                  |



 >[!NOTE]
确认是否已为以 DN 为目标的属性流选择了“仅初始流”。                                                                          

### <a name="step-7-create-the-workflow"></a>步骤 7：创建工作流

AD 预配工作流的目的是将 Fabrikam 预配同步规则添加到资源。 下表显示了此配置。  根据下表中的数据创建工作流。

| 工作流配置               |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Name                                 | Active Directory 用户预配工作流                     |
| 说明                          |                                                                 |
| 工作流类型                        | 操作                                                          |
| 策略更新时运行                 | False                                                           |

| 同步规则                 |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Name                                 | Active Directory 用户出站同步规则             |
| 操作                               | 添加                                                             |




### <a name="step-8-create-the-mpr"></a>步骤 8：创建 MPR

所需的 MPR 的类型为“集转换”并在资源成为“所有合同工”集的成员时被触发。 下表显示了此配置。  根据下表中的数据创建 MPR。

| MPR 配置                    |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Name                                 | AD 用户预配管理策略规则                 |
| 说明                          |                                                             |
| 类型                                 | 集转换                                              |
| 授予权限                   | False                                                       |
| 已禁用                             | False                                                       |

| 转换定义                |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| 转换类型                      | 转换目标                                               |
| 转换集                       | 所有合同工                                             |

| 策略工作流                     |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| 类型                                 | 操作                                                      |
| 显示名称                         | Active Directory 用户预配工作流                 |




## <a name="initializing-your-environment"></a>初始化环境


初始化阶段的目标如下所示：

-   将同步规则引入 Metaverse。

-   将 Active Directory 结构引入 Active Directory 连接器空间。

### <a name="step-9-run-the-run-profiles"></a>步骤 9：运行运行配置文件

下表列出了初始化阶段过程中的运行配置文件。  根据下表运行运行配置文件。

| “运行”                                                                                                           | 管理代理                                      | 运行配置文件          |
|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|----------------------|
| 1                                                                                                             | Fabrikam FIMMA                                        | 完全导入          |
| 2                                                                                                             |                                                       | 完全同步 |
| 3                                                                                                             |                                                       | 导出               |
| 4                                                                                                             |                                                       | 增量导入         |
|                                                                                                               |                                                       |                      |
| 5                                                                                                             | Fabrikam ADMA                                         | 完全导入          |
| 6                                                                                                             |                                                       | 完全同步 |



>[!NOTE]
应验证出站同步规则是否已成功投影到 Metaverse。

## <a name="testing-the-configuration"></a>测试配置


本部分旨在测试你的实际配置。 若要测试配置，请执行以下操作：

1.  在 FIM 门户中创建示例用户。

2.  验证示例用户的预配必备条件。

3.  将示例用户预配到 AD DS。

4.  验证此用户是否在 AD DS 中存在。

### <a name="step-10-create-a-sample-user-in-mim"></a>步骤 10：在 MIM 中创建示例用户


下表列出了示例用户的属性。 根据下表中的数据创建示例用户。

| 属性                              | 值                                                          |
|----------------------------------------|----------------------------------------------------------------|
| 名字                             | Britta                                                         |
| 姓氏                              | Simon                                                          |
| 显示名称                           | Britta Simon                                                   |
| 帐户名                           | BSimon                                                         |
| Domain                                 | Fabrikam                                                       |
| 员工类型                          | 合同工                                                     |



### <a name="verify-the-provisioning-requisites-of-the-sample-user"></a>验证示例用户的预配必备项


若要将示例用户预配到 AD DS，必须满足两个前提条件：

1.  用户必须是“所有合同工”集的成员。

2.  集用户必须位于出站同步规则的作用域中。

### <a name="step-11-verify-the-user-is-a-member-of-all-contractors"></a>步骤 11：验证用户是否是“所有合同工”的成员

若要验证用户是否是“所有合同工”集的成员，可打开集，然后单击“查看成员”。

![验证成员是否是“所有合同工”的成员](media/how-provision-users-adds/image022.jpg)


### <a name="step-12-verify-the-user-is-in-the-scope-of-the-outbound-synchronization-rule"></a>步骤 12：验证用户是否位于出站同步规则的作用域中

若要验证用户是否位于出站同步规则的作用域中，可打开用户的属性页，然后在“预配”选项卡中查看“预期规则列表”特性。“预期规则列表”特性应列出 AD 用户

出站同步规则。 下面的屏幕截图显示了一个“预期规则列表”特性的示例。

![同步规则状态](media/how-provision-users-adds/image023.jpg)

此时进程中的同步规则状态为“挂起”。 这意味着，同步规则尚未应用于用户。



### <a name="step-13-synchronize-the-sample-group"></a>步骤 13：同步示例组


在开始第一个测试对象同步周期之前，应在测试计划中运行的每个运行配置文件之后跟踪对象的预期状态。 测试计划应包括对象的一般状态（已创建、已更新或已删除）以及所需属性值。
使用测试计划验证测试计划预期结果。 如果某个步骤未返回预期结果，必须先解决预期结果和实际结果之间的差异，然后再继续下一步。

若要验证预期，可使用同步统计信息作为第一个指示器。 例如，如果期望将新对象暂存在连接器空间，但导入统计信息未返回“Adds”，则显然你的环境中存在未按预期工作的内容。

![同步统计信息](media/how-provision-users-adds/image024.jpg)

尽管同步统计信息可提供方案是否按预期工作的第一个指示，但还应使用 Synchronization Service Manager 的“搜索连接器空间”和“Metaverse 搜索”功能验证预期属性值。

将用户同步至 AD DS：

1.  将用户导入到 FIM MA 连接器空间。

2.  将用户投影到 Metaverse。

3.  将用户预配到 Active Directory 连接器空间。

4.  将状态信息导出到 FIM。

5.  将用户导出到 AD DS。

6.  确认创建用户。

若要完成这些任务，请运行以下运行配置文件。

| 管理代理 | 运行配置文件  |
|------------------|--------------|
| Fabrikam FIMMA   | 1.增量导入 <br/> 2.Delta 同步 <br/> 3.导出 <br/> 4.增量导入 |
| Fabrikam FIMMA   | 1.导出 <br/> 2.增量导入       |


从 FIM 服务数据库导入后，Britta Simon 和将 Britta 链接到 AD 用户出站同步规则的 ExpectedRuleEntry 对象

将暂存在 Fabrikam FIMMA 连接器空间。 当你在

连接器空间查看 Britta 的属性时，在 FIM 门户中配置的特性值旁还可找到对 Expected Rule Entry 对象的有效引用。 以下屏幕截图演示了此示例。

![连接器空间对象属性](media/how-provision-users-adds/image025.jpg)

Fabrikam FIMMA 上运行增量同步旨在执行以下几种操作：

-   投影 - 新用户对象和相关 Expected Rule Entry 对象将被投影到 Metaverse 中。

-   设置 - 新投影的 Britta Simon 对象将被预配到 Fabrikam ADMA 的连接器空间中。

-   导出属性流 - 导出两个管理代理上发生的属性流。 在 Fabrikam ADMA 上，最新预配的 Britta Simon 对象包含新属性值。 在 Fabrikam FIMMA 上，现有的 Britta Simon 对象和相关 ExpectedRuleEntry 对象更新为投影结果的属性值。

![同步统计信息](media/how-provision-users-adds/image026.jpg)

根据同步统计信息的指示，预配活动已在 Fabrikam ADMA 的连接器空间中发生。 查看 Britta Simon 的 Metaverse 对象属性时，将发现此活动是包含有效引用的 ExpectedRulesList 特性的结果。

![Metaverse 对象属性](media/how-provision-users-adds/image027.jpg)

在 Fabrikam FIMMA 上进行以下导出时，Britta Simon 的同步规则状态将从“挂起”升级为“已应用”，这表明你的出站同步规则现已在 Metaverse 中的对象上处于活动状态。

![已应用的同步规则](media/how-provision-users-adds/image028.jpg)

由于新对象已预配到 ADMA 连接器空间，因此该管理代理上应有一个 Add 挂起导出。 通过使用用于实现此目的的脚本，可查看针对 Fabrikam ADMA 报告的 Add 挂起导出。 若要使用脚本，请参阅[使用 Windows PowerShell 显示管理代理的导出状态](http://go.microsoft.com/FWLink/p/?LinkId=189664)。

![管理代理的挂起导出](media/how-provision-users-adds/image029.jpg)

在 FIM 中，每个导出运行都需要以下增量导入来完成导出操作。 在前一导出运行后运行的增量导入被称为确认导入。 确认导入是启用 FIM 同步服务所必需的，以便在连续同步运行期间提出适当的更新要求。


根据本部分中的说明运行运行配置文件。

>[!IMPORTANT]
每个运行配置文件都必须成功运行。

### <a name="step-14-verify-the-provisioned-user-in-ad-ds"></a>步骤 14：验证 AD DS 中预配的用户

若要验证示例用户是否已预配到 AD DS，请打开 FIMObjects OU。 Britta Simon 应位于 FIMObjects OU 中。

![验证用户是否位于 FIMObjects OU 中](media/how-provision-users-adds/image033.jpg)

<a name="summary"></a>“摘要”
=======

本文档旨在介绍将 MIM 中的用户与 AD DS 进行同步的主要构建基块。 在初始测试中，应先从完成任务所需的最少属性开始，然后在常规步骤按预期工作后再向方案添加更多属性。 保持最低级别的复杂性可简化故障排除的过程。

测试配置时，很有可能会删除和重新创建测试对象。 对于包含 

ExpectedRulesList 属性的对象，这可能会生成孤立的 ERE 对象。
有关如何从测试环境中移除这些对象的说明，请参阅[从环境中移除孤立的 ExpectedRuleEntry 对象的方法](http://go.microsoft.com/FWLink/p/?LinkId=189667)。

在以 AD DS 为同步目标的典型同步方案中，MIM 并未对对象的所有属性授权。 例如，使用 FIM 管理 AD DS 中的用户对象时，AD DS 管理代理至少需要提供域和 objectSID 属性。
必须提供帐户名称、域和 objectSID 属性，用户才能登录到 FIM 门户。 若要从 AD DS 填充这些属性，则 AD DS 连接器空间必须提供一个附加入站同步规则。 在管理具有多来源属性值的对象时，请确保已正确配置属性流优先顺序。 如果未正确配置属性流优先顺序，则同步引擎将阻止填充属性值。 有关属性流优先顺序的详细信息，请参阅 [About Attribute Flow Precedence](http://go.microsoft.com/FWLink/p/?LinkId=189675)（属性流优先顺序介绍）一文。

<a name="see-also"></a>另請參閱
=========

<a name="other-resources"></a>其他資源
---------------

[使用 FIM 启用或禁用 Active Directory 中的帐户](http://go.microsoft.com/FWLink/p/?LinkId=189670)

[关于引用属性](http://go.microsoft.com/FWLink/p/?LinkId=189671)

[如何管理我的 FIM MA 帐户](http://go.microsoft.com/FWLink/p/?LinkId=189672)

[Detecting Nonauthoritative Accounts – Part 1: Envisioning](http://go.microsoft.com/FWLink/p/?LinkId=189673)（检测未经授权帐户 - 第 1 部分：构想）

[廉价版连接器检测机制](http://go.microsoft.com/FWLink/p/?LinkId=189674)

[配置 ADMA 帐户](http://go.microsoft.com/FWLink/p/?LinkId=189657)

[从环境中移除孤立的 ExpectedRuleEntry 对象的方法](http://go.microsoft.com/FWLink/p/?LinkId=189667)

[About Attribute Flow Precedence](http://go.microsoft.com/FWLink/p/?LinkId=189675)（属性流优先顺序介绍）

[关于导出](http://go.microsoft.com/FWLink/p/?LinkId=189676)
