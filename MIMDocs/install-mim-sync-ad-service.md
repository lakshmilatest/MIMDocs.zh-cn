---
title: 使用 Microsoft Identity Manager 与 AD 同步 | Microsoft Docs
description: 使用管理代理和 MIM 同步服务来同步 Active Directory 和 MIM 数据库。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/12/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 69698721b0fbabc78cf5bb4c1032ab8fc2613772
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701199"
---
# <a name="install-mim-2016-synchronize-active-directory-and-mim-service"></a>安装 MIM 2016：同步 Active Directory 和 MIM 服务

> [!div class="step-by-step"]
> [« MIM 服务和门户](install-mim-service-portal.md)
> 
> [!NOTE]
> 本演练使用名为 Contoso 的公司中的示例名和值。 将其替换为你自己的。 例如：
> - 域控制器名称 - **mimservername**
> - 域名 - **contoso**
> - 密码 - <strong>Pass@word1</strong>

默认情况下，MIM 同步服务 (Sync) 未配置任何连接器。  典型的第一步是通过 MIM 同步来使用现有的 Active Directory 帐户填充 MIM 服务数据库。 为此，你将使用 MIM 同步服务应用程序。

## <a name="create-the-mim-management-agent"></a>创建 MIM 管理代理
MIM 管理代理 (MA) 是 MIM 同步到 MIM 服务的连接器。 若要创建此连接器，请使用“创建管理代理向导”。

在配置 MIM 管理代理时，需要指定一个用户帐户。 本文档使用 **MIMMA** 作为此帐户的名称。

> [!NOTE]
> 用于 MIM 管理代理的帐户必须与 MIM 服务安装过程中指定的帐户相同。

### <a name="to-create-the-mim-ma"></a>创建 MIM MA

1.  打开“同步服务管理器”。

2.  若要打开“创建管理代理”向导，请转到“管理代理”  页面，然后在“操作”  菜单上单击“创建”  。

3.  在“创建管理代理”  页上，提供以下设置，然后单击“下一步”  。

    -   下列产品的管理代理：FIM 服务管理代理

    -   名称：MIMMA

4.  在“连接到数据库”  页上，提供以下设置，然后单击“下一步” 

    -   服务器：localhost

    -   数据库：FIMService

    -   MIM 服务基址： http://localhost:5725

    -   身份验证模式：Windows 集成的身份验证

    -   用户名：mimma

    -   密码：Pass@word

    -   域：contoso

5.  在“所选对象类型”  页上，验证是否选择了下面列出的对象类型，然后单击“下一步” 

    -   DetectedRuleEntry

    -   ExpectedRuleEntry

    -   Group

    -   Person

    -   SynchronizationRule

6.  在“所选属性”  页面上，勾选“全部显示”  并验证是否选择了列出的所有属性，然后单击“下一步”  。

7.  在“配置连接器筛选”  页上，单击“下一步”  。

8.  在“配置对象类型映射”  页上，添加以下映射，然后单击“下一步” 

    - 在**数据源对象类型**列表中，选择**Person**。
    - 单击**添加映射**，以打开“映射”对话框。
    - 在**Metaverse 对象类型**列表中，选择**person**。
    - 单击**确定**，以关闭“映射”对话框。
    - 在“数据源对象类型”  列表中选择“Group”  。
    - 单击**添加映射**，以打开“映射”对话框。
    - 在“Metaverse 对象类型”  列表中选择“group”  。
    - 单击**确定**，以关闭“映射”对话框。

9.  在“配置属性流”  页面上，如下所示创建属性流映射，然后单击“下一步” 

    -   选择“Person”  作为数据源和 Metaverse 对象类型。

    -   选择**Direct**作为映射类型。

    -   对于下表中的每一行，完成这些步骤：

        -   选择为表中该行显示的**流方向**。

        -   选择为表中该行显示的**数据源属性**。

        -   选择为表中该行显示的**Metaverse 属性**。

        -   若要应用数据流映射，请单击**新建**。

    | **数据源属性** | **流方向** | **Metaverse 属性** |
    |-|-|-|
    | AccountName | 导出 | accountName |
    | DisplayName | 导出 | displayName |
    | Domain | 导出 | domain |
    | Email | 导出 | mail |
    | EmployeeID | 导出 | employeeID |
    | Employee类型 | 导出 | employeeType |
    | 名字 | 导出 | firstName |
    | 姓氏 | 导出 | lastName |
    | ObjectSID | 导出 | objectSid |

    -   选择“Group”  作为数据源类型和 Metaverse 对象类型。

    -   选择**Direct**作为映射类型。

    -   对于下表中的每一行，完成这些步骤：

        -   选择为表中该行显示的**流方向**。

        -   选择为表中该行显示的**数据源属性**。

        -   选择为表中该行显示的**Metaverse 属性**。

        -   若要应用数据流映射，请单击**新建**。

    | **数据源属性** | **流方向** | **Metaverse 属性** |
    |-|-|-|
    | AccountName | 导出 | accountName |
    | DisplayName | 导出 | displayName |
    | Domain | 导出 | domain |
    | Email | 导出 | mail |
    | MailNickName | 导出 | mailNickName |
    | 成员 | 导出 | 成员 |
    | ObjectSID | 导出 | objectSid |
    | 作用域 | 导出 | scope |
    | 类型 | 导出 | 类型 |
    | 成员shipAddWorkflow | 导出 | membershipAddWorkflow |
    | 成员shipLocked | 导出 | membershipLocked |
    | AccountName | 导入 | accountName |
    | DisplayedOwner | 导入 | displayedOwner |
    | DisplayName | 导入 | displayName |
    | MailNickName | 导入 | mailNickName |
    | 成员 | 导入 | 成员 |
    | 范围 | 导入 | scope |
    | 类型 | 导入 | 类型 |

10.  在“取消配置设置”  页上，单击“下一步” 

11.  若要创建管理代理，请单击“配置扩展”  页上的“完成”  。

## <a name="create-the-ad-management-agent"></a>创建 AD 管理代理
Active Directory 管理代理是 AD 域服务的连接器。 若要创建此连接器，请使用“创建管理代理向导”。

1. 若要打开“创建管理代理向导”，请单击“操作”  菜单上的“创建”  。

2. 在“创建管理代理”页  上，提供以下设置，然后单击“下一步”  ：

    - 下列产品的管理代理：Active Directory 域服务
    - 名称：ADMA

3. 在“连接到 Active Directory 林”  页上，提供以下设置，然后单击“下一步”  ：

    - 林名称：contoso.local
    - 用户名：administrator
    - 密码：&lt;帐户的密码&gt;
    - 域：contoso

4. 在“配置目录分区”  页上，提供以下设置，然后单击“下一步”  ：

    - 在“选择目录分区”  列表中，选择“DC=CONTOSO,DC=local”  。

    - 若要打开“选择容器”对话框，请单击“容器”  。

    - 如果想要将容器更改为只在特定容器中包含 MIM 管理对象，则单击“DC=CONTOSO,DC=local”  节点，然后单击感目标容器的节点。

    - 若要关闭“选择容器”对话框，请单击“确定”  。

5. 在“配置设置层次结构”  页上，单击“下一步”  。

6. 在“选择对象类型”  页上，提供以下设置，然后单击“下一步”  ：

    - 在“对象类型”  列表中，选择“用户”  和“组”  。

7. 在“选择属性”  页面上，勾选“全部显示”  并选择以下属性，然后单击“下一步”  ：

    -   company
    -   displayName
    -   employeeID
    -   employeeType
    -   givenName
    -   group类型
    -   managedBy
    -   manager
    -   成员
    -   objectSid
    -   sAMAccountName
    -   sAMAccount类型
    -   sn
    -   unicodePwd
    -   userAccountControl

8. 在“配置连接器筛选”  页上，单击“下一步”  。

9. 在“配置联接和投影规则”  页上，单击“下一步”  。

10. 在“配置属性流”  页上，单击“下一步”  。

11. 在“取消配置设置”  页上，单击“下一步”  。

12. 在“配置扩展”  页上，单击“完成”  。


## <a name="create-run-profiles"></a>创建运行配置文件

创建 ADMA 和 MIMMA 连接器的运行配置文件。

### <a name="create-run-profiles-for-the-adma-connector"></a>创建 ADMA 连接器的运行配置文件

下表展示需要创建的用于 ADMA 连接器的 5 个运行配置文件

| 名称 | 类型 |
| ---- | ---- |
| Profile1 | 完全导入（仅阶段） |
| Profile2 | 完全同步 |
| Profile3 | 增量导入（仅阶段） |
| Profile4 | Delta 同步 |
| Profile5 | 导出 |

若要创建 ADMA 连接器的运行配置文件：

1. 打开同步服务管理器，在“工具”  菜单上，单击“管理代理”  。

2. 在“管理代理”  列表中，选择“ADMA”  。

3. 若要打开“配置运行配置文件”对话框，请单击“操作”  菜单上的“配置运行配置文件”  。

4. 对表中的每一个运行配置文件，完成以下步骤：

    - 若要打开“配置运行配置文件向导”，请单击“新建配置文件”  。

    - 在“名称”  框中，键入表中的配置文件名称，然后单击“下一步”  。

    - 在“类型”  列表中，选择表中的步骤类型，然后单击“下一步”  。

    - 单击“完成”  以创建该运行配置文件。

5. 若要关闭“配置运行配置文件”对话框，请单击“确定”  。

### <a name="create-run-profiles-for-the-mimma-connector"></a>创建 MIMMA 连接器的运行配置文件

下表显示与 MIMMA 连接器匹配的五个运行配置文件：

| 名称 | 类型 |
| -------- | -------- |
| Profile1 | 完全导入（仅阶段） |
| Profile2 | 完全同步 |
| Profile3 | 增量导入（仅阶段） |
| Profile4 | Delta 同步 |
| Profile5 | 导出 |

若要创建 MIMMA 连接器的运行配置文件：

1. 打开同步服务管理器，在“工具”  菜单上，单击“管理代理”  。

2. 在“管理代理”  列表中，选择“MIMMA”  。

3. 若要打开“配置运行配置文件”对话框，请单击“操作”  菜单上的“配置运行配置文件”  。

4. 对表中的每一个运行配置文件，完成以下步骤：

    - 若要打开“配置运行配置文件向导”，请单击“新建配置文件”  。

    - 在“名称”  框中，键入表中的配置文件名称，然后单击“下一步”  。

    - 在“类型”  列表中，选择表中的步骤类型，然后单击“下一步”  。

    - 单击“完成”  以创建该运行配置文件。

5. 若要关闭“配置运行配置文件”对话框，请单击“确定”  。

## <a name="configure-the-mim-service"></a>配置 MIM 服务

使用 MIM 门户，你将为 MIM 服务创建 AD 用户入站同步规则。

创建 AD 用户入站同步规则：

1. 在“MIM 门户主页”上，单击导航栏上的“管理”  。

2. 若要打开“同步规则页”，请单击“同步规则”  。

3. 若要打开“创建同步规则向导”，请单击工具栏中的“新建”  。

4. 在“常规”  选项卡上，提供以下信息，然后单击“下一步”  ：

    -   显示名称：AD 用户入站同步规则
    -   数据流方向：入站

5. 在“作用域”  选项卡上，提供以下信息，然后单击“下一步”  ：

    -   Metaverse 资源类型：person
    -   外部系统：ADMA
    -   外部系统资源类型：user

6. 在“关系”  选项卡上，提供以下信息，然后单击“下一步”  ：

    -   若要配置关系条件，从 MetaverseObject:person（属性）列表和ConnectedSystemObject:person（属性）列表中选择“ObjectSID”  。

    -   选择“在 FIM 中创建资源”  。

7. 在“入站属性流”  页上，提供以下信息，然后单击“下一步”  ：

    | 流规则 | 源 | 目标 |
    |-|-|-|
    |规则 1|samAccountName|accountName|
    |规则 2|displayName|displayName|
    |规则 3|Employee类型|employeeType|
    |规则 4|givenName|firstName|
    |规则 5|sn|lastName|
    |规则 6|Manager|manager|
    |规则 7|objectSID|ObjectSID|
    |规则 8|“Contoso”|domain|

    对于此表中的每一行，执行以下步骤：

    - 若要打开“流定义”对话框，请单击“新建属性流”  。

    - 在“源”  选项卡上，选择为表中该行显示的属性。

    - 在“目标”  选项卡上，选择为表中该行显示的属性。

    - 若要应用属性流配置，请单击“确定”  。

8. 在“摘要”  选项卡上，单击“提交”  。

## <a name="initialize-the-testing-environment"></a>初始化测试环境
需要执行 4 个步骤，才可使用 AD 数据测试 MIM 配置：

### <a name="enable-provisioning"></a>启用设置

1. 打开“同步服务管理器”。

2. 若要打开“选项”对话框，请单击“工具”  菜单上的“选项” 

3. 选择“启用同步规则设置”  。

4. 若要关闭“选项”对话框，请单击“确定”  。

### <a name="initialize-the-mimma"></a>初始化 MIMMA

在此连接器上运行完整同步周期。 完整周期包括以下运行配置文件：

- 完全导入
- 完全同步
- 导出
- 增量导入

请按照下列步骤来分别运行这 4 个运行配置文件。

1. 打开“同步服务管理器”，并单击“工具”  菜单上的“管理代理”  。

2. 在“管理代理”  列表中，选择“MIMMA”  。

3. 若要打开“运行管理代理”对话框，请单击“操作”  菜单上的“运行”  。

4. 对上面列出的每一个运行配置文件，完成以下步骤：

    - 若要打开“运行管理代理”对话框，请单击“操作”  菜单上的“运行”  。

    - 在“运行配置文件”  列表中，选择想要运行的运行配置文件。

    - 若要开始运行配置文件，请单击“确定”  。

#### <a name="configure-attribute-flow-precedence"></a>配置属性流优先顺序

在初始化 MIM 连接器期间，已对 metaverse 应用了配置的同步规则。

调整由此连接器提供的属性的属性流优先顺序，以确保已在 AD 中的属性可以流入 metaverse 并且稍后还可以流入 MIM 服务数据库。

### <a name="initialize-the-adma"></a>初始化 ADMA

若要初始化 Active Directory 连接器，需要在该连接器上运行完全导入和完全同步。 完全导入会将现有对象从 AD 带入连接器空间。 完全同步将更新同步规则，以匹配这些 MIM 连接器。

1. 打开“同步服务管理器”，并单击“工具”  菜单中的“管理代理”  。

2. 在“管理代理”  列表中，选择“ADMA”  。

3. 若要打开“运行管理代理”对话框，请单击“操作”  菜单上的“运行”  。

4. 对上面列出的每一个运行配置文件，完成以下步骤：

    - 若要打开“运行管理代理”对话框，请单击“操作”  菜单上的“运行”  。

    - 在“运行配置文件”  列表中，选择想要运行的运行配置文件。

    - 若要开始运行配置文件，请单击“确定”  。

### <a name="populate-the-mim-service-database"></a>填充 MIM 服务数据库

若要使用对象填充 MIM 服务数据库，需要在 MIMMA 连接器上运行同步周期。 周期包括：

- 导出
- 完全导入
- 完全同步

请按照下列步骤来分别运行这 3 个运行配置文件。

1. 打开 Synchronization Service Manager，并单击**工具**菜单上的**管理代理**。

2. 在**管理代理**列表中，选择**MIMMA**。

3. 单击**操作**菜单上的**运行**，以打开“运行管理代理”对话框。

4. 对上面列出的每一个运行配置文件，完成以下步骤：

    - 单击**操作**菜单上的**运行**，以打开“运行管理代理”对话框。
    - 在**运行配置文件**列表中，选择想要运行的运行配置文件。
    - 单击**确定**以启动运行配置文件。

> [!div class="step-by-step"]
> [« MIM 服务和门户](install-mim-service-portal.md)
