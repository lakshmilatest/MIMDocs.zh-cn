---
title: "BHOLD FIM/MIM 集成安装 | Microsoft Docs"
description: "BHOLD 集成模块会向 MIM 和 FIM 添加自助服务角色管理"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: ef68de19bd0eabd6d9203469ecc991d496f05846
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-fimmim-integration-installation"></a>BHOLD FIM/MIM 集成安装

BHOLD FIM 集成模块会向 Microsoft Identity Manager 添加自助服务角色管理，使用户可以请求其他角色，以及强制要求可以扮演这些角色的人员。 BHOLD FIM 集成模块扩展了 FIM 门户，因此在整个 FIM 管理过程中可以轻松地管理用户角色。 本主题介绍必须如何配置网络基础结构才能安装和使用 BHOLD FIM 集成模块。 它还介绍如何安装 BHOLD FIM 集成模块，以及安装 BHOLD FIM 集成模块后所需的配置。

## <a name="bhold-fim-integration-installation-requirements"></a>BHOLD FIM 集成安装要求

BHOLD FIM 集成模块扩展了 FIM 门户和 FIM 服务，因此用户可以在 FIM 门户中管理他们的角色。 为此，必须先安装和配置 BHOLD 核心模块以及必要的 FIM 功能，然后才能安装 BHOLD FIM 集成模块。
在能够安装 BHOLD FIM 集成模块之前，计算机必须具有以下软件组件：

- Microsoft Identity Manager 2016 门户和服务
- Microsoft Silverlight 3 或更高版本
- Internet information Services 和 ASP.NET
- Microsoft Silverlight 工具

此外，BHOLD 核心和访问管理连接器模块必须已部署到该环境的某个服务器上，FIM 必须配置有一个或多个 BHOLD 管理代理。 有关如何安装和配置 BHOLD 核心模块的信息，请参阅 [BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)（BHOLD 核心安装）。 有关如何安装和使用访问管理连接器模块的信息，请参阅 [Access Management Connector Installation](https://technet.microsoft.com/en-us/library/jj874042(v=ws.10).aspx)（访问管理连接器安装）和 [Test Lab Guide: BHOLD Access Management Connector](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx)（测试实验室指南：BHOLD 访问管理连接器）。

>[!IMPORTANT]
FIM 服务数据库的名称必须是 FIMService。 如果未使用默认 FIM 服务数据库名称安装 FIM，则 BHOLD FIM 集成安装将失败。

## <a name="before-you-begin"></a>在开始之前

在开始安装 BHOLD FIM 集成模块之前，必须在 C: 磁盘驱动器 (C:\BHOLD) 的根目录中创建 BHOLD 目录。

此外，需要准备提供 BHOLD FIM 集成安装向导完成安装所需的信息。 以下工作表可以帮助记录这些信息，以便在需要时可以随时提供。

### <a name="bholdfim-account-settings"></a>BHOLDFim 帐户设置

| **项目**                            | **描述**                                                                                                                                                                                                               | **值**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **对域使用安全提供程序** | 选中后，会指定 Active Directory 域服务安全控制对 BHOLD 核心的访问。                                                                                                                    | 选定复选框。 重要说明：如果未选中此复选框，则安装将失败。                                                                                                                                                                                                                   |
| **域**                          | 指定包含在安装 BHOLD 核心时创建的服务帐户的域。 有关详细信息，请参阅 [BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)（BHOLD 核心安装）。 | 域名由向导自动提供。 仅当不正确时，更改名称。 重要说明：指定域名时，请使用 NetBIOS（短）名称，不要使用完全限定的域名 (FQDN)。 例如，如果域的 FQDN 是 fabrikam.com，将域名指定为 FABRIKAM。 |
| **用户名**                        | 指定 BHOLD 核心服务用户帐户的登录名。                                                                                                                                                              | 在此处填写用户帐户名称：                                                                                                                                                                                                                                                                                    |
| **密码**                        | 指定服务用户帐户的密码。                                                                                                                                                                           | 在此处填写密码：重要说明：确保将此密码保存在一个隐藏的安全位置。                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>FIM 服务设置

| **项目**            | **描述**                                                                                                                                                                                                                               | **值**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **User**            | 使用 FIM 的管理员特权指定帐户的登录名。 Microsoft 强烈建议不使用与 BHOLD 核心中的根用户关联的帐户（默认情况下，该帐户用于安装 BHOLD 核心）。 | 在此处填写用户帐户名称：                                                                   |
| **密码**        | 指定 FIM 管理员用户帐户的密码。                                                                                                                                                                                 | 在此处填写密码：重要说明：确保将此密码保存在一个隐藏的安全位置。 |
| **FIM 数据库**    | 指定 FIM 服务数据库的名称。                                                                                                                                                                                               | FIMService                                                                                          |
| **网站 IP/端口** | 指定 FIM 门户服务器和网站端口的名称或 IP 地址。                                                                                                                                                               | 在此处填写服务器名称或地址和端口：                                                     |

### <a name="bhold-core-connection"></a>BHOLD 核心连接

| **项目**               | **描述**                                                                                                                                                                                                                                                                                                                                                                               | **值**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **域**             | 指定在以下“用户”中指定的帐户的域名。 以 NetBIOS（短）格式指定域。                                                                                                                                                                                                                                                                   | 在此处填写用户帐户域名：                                                            |
| **User**               | 指定 BHOLD 用户的帐户登录名，该用户负责管理所有用户和角色，并且有权链接和取消链接用户角色。 Microsoft 强烈建议不使用与 BHOLD 核心中的根用户关联的帐户（默认情况下，该帐户用于安装 BHOLD 核心）。 此帐户可以是用于连接到 FIM 的同一个帐户 | 在此处填写用户帐户名称：                                                                   |
| **密码**           | 指定在“用户”中指定的用户帐户的密码。                                                                                                                                                                                                                                                                                                                             | 在此处填写密码：重要说明：确保将此密码保存在一个隐藏的安全位置。 |
| **IP/计算机地址** | 指定 BHOLD 核心网站服务器的 IP 地址。 请勿使用服务器名称。                                                                                                                                                                                                                                                                                                        | 在此处填写 IP 地址：                                                                          |
| **端口号**        | 指定 BHOLD 核心网站的端口号。                                                                                                                                                                                                                                                                                                                                          | 在此处填写端口号：                                                                         |

## <a name="bhold-fim-integration-setup"></a>BHOLD FIM 集成安装

要安装 BHOLD FIM 集成模块，请以域管理员组成员的身份登录，下载以下文件，并在要安装 BHOLD FIM 集成模块的服务器上以管理员身份运行该文件：

- BholdFIMIntegration\<版本\>\_Release.msi

将 \<版本\> 替换为要安装的 BHOLD FIM 集成版本的版本号。

若要以管理员身份运行程序文件，右键单击该文件，然后单击“以管理员身份运行”。

![运行 msi](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>安装后的任务

安装 BHOLD FIM 集成后，必须将 Microsoft SharePoint 配置为向 BHOLD 服务帐户授予站点所有者权限。 此外，如果 FIM 门户配置为使用安全套接字层 (SSL) 安全，则必须修改其中包含对添加到 FIM 门户的 BHOLD 页地址的引用的文件。

### <a name="configuring-sharepoint"></a>配置 SharePoint

为了正常工作，BHOLD FIM 集成要求 BHOLD 服务帐户在 Microsoft SharePoint 中具有站点成员权限。

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>向 BHOLD 服务帐户授予站点成员权限

1.  使用管理员特权登录运行 BHOLD FIM 集成的服务器。

2.  单击“开始”，然后单击“Internet Explorer”。

3.  如果 SharePoint 配置为使用 SSL 安全，则在地址栏中键入 <https://localhost>，否则键入 <http://localhost>。

4.  在“团队网站”页左侧，单击“人员和组”。

5.  在“组”下单击“团队网站成员”，在中心窗格工具栏中单击“新建”，然后单击“添加用户”。

6.  在“添加用户: 团队网站”页上，在“用户/组”中键入 BHOLDApplicationGroup，然后单击“用户/组”框下的“检查名称”按钮。 组名解析后才能包括域名。

7.  单击“直接向用户授予权限”，选择“完全控制 – 具有完全控制权限”，然后单击“确定”。

8.  验证 BHOLDApplicationGroup 在“权限: 团队网站”中列出，然后关闭 Internet Explorer。

![运行 msi](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>将 BHOLD 配置为支持 SSL

如果 FIM 门户配置为使用 SSL 安全，则必须修改 FIM 服务器上的文件，这样才能打开指向 BHOLD 页面的链接。 文件位于以下文件夹中：```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```。

下表列出了这些文件，以及要编辑的字符串的原始版本和更改版本。

| 文件                  | 原始字符串                                                                                                                   | 更改字符串                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Selfservice.aspx          | RoleExchangePoint=http://\<*FIM_Server*\>: \<*FIM_Port*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport | RoleExchangePoint=https://\<*FIM_Server_FQDN*\>: \<*FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport |

其中：

-   \<BHOLD_Server\> 指定在原始版本文件中找到的 BHOLD 服务器名称

-   \<MIM_Server\> 指定在原始版本文件中找到的 FIM 服务器名称

-   \<BHOLD_Server_FQDN\> 指定 BHOLD 服务器的完全限定的域名 (FQDN)

-   \<MIM_Port\> 指定在原始版本文件中找到的 FIM 服务器的端口号

-   \<MIM_Server_FQDN\> 指定 FIM 服务器的 FQDN

-   \<MIM_SSL_Port\> 指定用于 FIM 服务器上的 SSL 的其他端口

### <a name="enable-approval-workflows-in-bhold-core"></a>在 BHOLD 核心中启用审批工作流

如果集成 FIM 和 BHOLD 以实现自助服务，则会在 FIM 服务中运行审批的工作流。 这类似于在 FIM 门户中发出的请求的工作流模型，例如用户提交加入通讯组列表的请求时。 但是，BHOLD 角色工作流和承载在 FIM 服务中的其他工作流之间存在一些重要的差异。 对于 BHOLD，用于指定哪些用户必须批准角色请求的工作流参数源自 BHOLD，而不是存储在 FIM 服务数据库的工作流定义中。 发出第一个请求时，BHOLD 将这些参数提供给 FIM 服务，并且操作工作流会将结果传递回 BHOLD 核心。

BHOLD 通过以下三种方式之一选择自助服务请求的审批者：

-   职能经理作为审批者：针对组织单位 (OrgUnit) 基于角色的选择 如果角色具有名为 roletype 的属性，该属性设置为“审批者”或“呈报者”，并且该角色链接到 OrgUnit 上下文中的一个或多个用户，则该 OrgUnit 中的用户发出的请求必须由其中一个链接到“审批者”或“呈报者”roletype 的用户审批。

-   职能经理作为审批者：针对 OrgUnit 基于属性的选择 每个 OrgUnit 可以具有一个或多个属性，用于指定用户的别名，该用户可以审批 OrgUnit 中其他用户的角色分配。 这些属性命名为 approver1、approver2 等等。 OrgUnit 中的用户请求角色分配时，BHOLD 会（通过 FIM）将请求路由到由 OrgUnit 审批者属性指定的用户。 如果 OrgUnit 不具有以上属性集中的任一项，则 BHOLD 会检查各父级 OrgUnit，直到根 OrgUnit。

-   角色管理者作为审批者：针对角色基于属性的选择 一个角色可以具有一个或多个属性（也可命名为 approver1 等等），用于指定可以审批角色分配的用户的别名。 用户请求系统向其分配具有这些审批者属性集的角色时，BHOLD 会将请求路由到这些属性所指定的用户。

如果自助服务角色请求的审批者不是由以上方法指定的，则默认情况下，BHOLD 会自动分配角色，而无需批准。 为此，安装 BHOLD FIM 集成后，应该立即使用审批者的别名配置根 OrgUnit，例如根帐户。 这样可以防止在实现更全面的审核策略前，无意地向用户授予角色。

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>配置根 OrgUnit 的审批者

1.  以管理员身份登录 BHOLD 核心服务器。

2.  单击“开始”，然后单击“Internet Explorer”。

3.  在 Internet Explorer 地址栏中，键入 <http://localhost:5151/bhold/core>，然后按 Enter 键。

4.  在 BHOLD 核心主页上，在“属性 def”下单击“属性类型”。

5.  在“属性类型”页上单击“添加”。

6.  在“添加属性类型页”中，在“标识”中键入 approver1，在“数据类型”列表中单击“字母数字”，在“最大长度”中键入 255，在“值列表”中单击“否”，在“英语”中键入 Approver 1，单击“确定”，然后单击“完成”。

7.  在左窗格中，在“属性 def”下单击“属性类型集”。

8.  在“属性类型集”页上单击“添加”。

9.  在“添加属性类型集”页的“说明”中键入 OrgUnit 属性，然后单击“确定”。

10. 在“OrgUnit 属性”页上展开“属性类型”，然后单击“修改”。

11. 在“属性类型”列表中单击“approver1”，单击“添加”，然后单击“完成”。

12. 在左窗格中，单击“对象类型”。

13. 在“对象类型”页上单击“OrgUnit”。

14. 在“对象类型/OrgUnit”页上展开“属性类型集”，然后单击“修改”。

15. 在“链接属性类型集/OrgUnit”页的“顺序”中键入 10，在“属性类型集”列表中单击“OrgUnit 属性”，单击“添加”，然后单击“完成”。

16. 在左窗格的“模型”下单击“组织单位”。

17. 在“组织单位”页上单击“根”。

18. 在“组织单位/根”页上单击“修改”。

19. 在“修改组织单位属性/跟”页的“审批者”中，键入批准角色分配请求的用户的域和用户名，键入的格式为：\<域\>\\\<用户\>，其中\<域\>是 NetBIOS（短）域名，\<用户\>是用户的登录名。
20. 单击" **确定**"。

>[!IMPORTANT]
域和用户名必须匹配 BHOLD 核心数据库中用户的默认别名。

指定组织单位审批者的一种替代方法是在 BHOLD 核心数据库中指定建议角色的审批者。 为此，请创建 approver1 属性，将其添加到与角色对象类型关联的属性类型集，然后修改每个建议的角色，从而指定审批者。

为提供更佳的工作流安全性，除审批者外，还应通过为 OrgUnit 和角色创建和填充以下属性，指定其他审批模式和用户：

- escalator*\<n\>*

- owner*\<n\>*

- securityOfficer*\<n\>*

- notification*\<n\>*

其中，\<n\> 指示可选的数字后缀，以提供相同类型的多个属性。

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>验证在 FIM 服务中配置的审批工作流

BHOLD FIM 集成安装会在 FIM 服务中创建集、工作流定义和管理策略规则 (MPR)。 如果已自定义 FIM 部署以更改可发出请求的管理员集或用户集，应确保 MPR 引用正确的用户集。

>[!NOTE]
用户帐户必须先从 FIM 同步服务同步到 BHOLD 数据库，FIM 门户的用户才能使用 BHOLD 提供的自助服务功能。 具体而言，每个用户如果可以发出自助服务请求，或者指定为自助服务请求的审批者或呈报者，则在 BHOLD 核心数据库和 FIM 服务数据库中必须存在用户记录。

## <a name="next-steps"></a>后续步骤

- 有关如何安装 FIM 门户和其他 FIM 功能的信息，请参阅 Microsoft Forefront 技术资源库中的 [Planning and Architecture](https://technet.microsoft.com/library/ee808044.aspx)（规划和体系结构）。
- [BHOLD 安装指南](bhold-installation-guide.md)
- [BHOLD 开发人员参考](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD 版本历史记录](../reference/version-bhold-history.md)
