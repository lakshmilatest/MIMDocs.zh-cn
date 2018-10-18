---
title: BHOLD 核心安装 | Microsoft Docs
description: BHOLD 套件安装核心文档
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 48ff4a06233ba95288432c4cfe48e37b4d1449ab
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358731"
---
# <a name="bhold-core-installation"></a>BHOLD 核心安装

BHOLD 核心模块在你的环境中提供 BHOLD 套件的主要功能。 必须先安装 BHOLD 核心模块，并在本地局域网的服务器上进行配置，然后才能安装其他 BHOLD 套件模块。

## <a name="bhold-core-installation-requirements"></a>BHOLD 核心安装要求

BHOLD 核心模块构成了 Microsoft BHOLD 套件的基础。 必须先安装 BHOLD 核心模块，然后才能安装其他 Microsoft BHOLD 套件模块。

### <a name="bhold-core-hardware-requirements"></a>BHOLD 核心硬件要求

BHOLD 核心模块构成了 Microsoft BHOLD 套件的基础。 必须先安装 BHOLD 核心模块，然后才能安装其他 Microsoft BHOLD 套件模块。

|          |        |          |
|----------|--------|----------|
|**组件** |最低 | 推荐 |
|处理器 | 64 位处理器 | 多核 64 位处理器 |
| 内存 |3 GB | 6 GB 或更多 |
|存储| 30 GB 可用空间 |取决于部署大小 |
|网络适配器| 到 SQL 和 Forefront Identity Manager (FIM) 服务器的 100Mbps 连接 | 到 SQL 和 FIM 服务器的 1Gbps 连接|

这些建议基于典型实现，未考虑在服务器上运行的其他应用程序。 可能需要使用更高性能的组件，具体取决于特定环境。

### <a name="bhold-core-software-requirements"></a>BHOLD 核心软件要求

BHOLD 核心模块必须安装在满足以下要求的计算机上：

- 服务器必须运行 Windows Server 2012（64 位）或 Windows Server 2016 
- 服务器必须是 Active Directory 域服务 (AD DS) 域的成员。 在测试环境中，服务器可能是 AD DS 域控制器。
- BHOLD 核心必须由使用服务器所在域中的帐户登录的用户安装，并且该用户属于域中的域管理员组和服务器上的管理员组。
- 带 ASP.NET 的 Microsoft Internet Information Services (IIS) 必须安装在服务器上。 IIS 必须在启用 Windows 身份验证时进行配置。 BHOLD 核心安装在 Windows Server 2012/2016 上时，必须安装 IIS 6 管理兼容性。 BHOLD 核心安装在 Windows Server 2012 上时，必须安装 IIS 6 脚本工具
- 必须安装 .NET 3.5 框架。
  - 因为 BHOLD 核心需要 .NET 3.5，所以不能安装在服务器核心上。
- 其他 BHOLD 模块需要 Silverlight 4，因此建议在安装 BHOLD 核心之前进行安装。
- Microsoft SQL Server 2014 或 Microsoft SQL Server 2016 必须安装在 BHOLD 核心服务器或 LAN 上的另一个服务器上。 

Windows 客户端计算机必须运行 Microsoft Internet Explorer 版本 6 或更高版本和 Microsoft Silverlight 版本 4 或更高版本。

## <a name="before-you-begin"></a>在开始之前

BHOLD 核心模块需要用于向服务器和其他网络实体验证和授权 BHOLD 核心服务的用户帐户。 本部分介绍如何创建和配置该用户帐户，并提供预安装的工作表，用于帮助收集完成 BHOLD 核心安装所需的信息。

>{!IMPORTANT} 安装 BHOLD 核心时，可将现有 SQL Server 数据库用作 BHOLD 核心数据库，或者可以让 BHOLD 核心安装向导为你创建 BHOLD 核心数据库。 如果选择使用现有数据库，则必须确保安装 BHOLD 核心时，任何其他用户都不可以访问数据库。 安装 BHOLD 核心前，请验证 SQL Server 数据库的访问控制不允许任何人访问，安装 BHOLD 核心的用户除外。

## <a name="required-user-and-group"></a>所需用户和组

BHOLD 核心模块必须能够使用登录专用的用户帐户登录域，并且该用户账户是两个特定安全组的成员，其中一个组是专门为 BHOLD 核心模块创建的。 执行此过程需要具有域管理员组中的成员身份。
**创建和配置 BHOLD 核心用户和安全组**

1.  在域控制器上，单击“开始”，指向“管理工具”，然后单击“Active Directory 用户和计算机”。

2.  在控制台树中，展开要在其中创建帐户的域，右键单击“用户”，指向“新建”，然后单击“组”。

3.  在“新建对象 – 组”对话框中，在“组名称”下键入组的名称（BHOLD 默认：BHOLDApplicationGroup），然后单击“确定”。

4.  右键单击 **“Users”**，指向 **“新建”**，然后单击 **“用户”**。

5.  在“全名”中键入帮助标识帐户的名称，例如，BHOLD 核心服务帐户。

6.  在“用户登录名”中键入 BHOLD 核心服务帐户的用户名（BHOLD 默认：b1user），然后单击“下一步”。

7.  在“密码”和“确认密码”框中，键入服务帐户的密码。

8.  清除“用户下次登录时必须更改密码”，选择“用户不能更改密码”和“密码永不过期”，单击“下一步”，然后单击“完成”。

9.  在控制台结果窗格中，右键单击用户帐户，然后单击“添加到组”。

10. 在“选择组”对话框中，键入之前创建的组的显示名称，键入分号 (;)，然后键入 IIS_IUSRS。

11. 单击“检查名称”，然后单击“确定”。  

必须在要安装 BHOLD 核心模块的计算机上执行以下过程。 必须以域管理员组的成员身份登录，才能执行此过程。

## <a name="bhold-core-installation-worksheet"></a>BHOLD 核心安装工作表

开始安装 BHOLD 核心模块前，需要准备提供 BHOLD 核心安装向导完成安装所需的信息。 以下工作表可以帮助记录这些信息，以便在需要时可以随时提供。 每个部分对应于 BHOLD 核心安装向导中的一页。

### <a name="account-settings"></a>帐户设置

| **项目**                                    | **描述**                                                                                                                                                                                                                                                                                             | **值**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 对域/计算机使用安全提供程序 | 选中后，会指定 Active Directory 域服务安全控制对 BHOLD 核心的访问。                                                                                                                                                                                                  | 选定复选框。 重要说明：如果未选中此复选框，则安装将失败。                                                                 |
| **域**                                  | 指定包含 BHOLD 服务器、服务帐户和应用程序组的域。 重要说明：指定域名时，请使用 NetBIOS（短）名称，不要使用完全限定的域名 (FQDN)。 例如，如果域的 FQDN 是 fabrikam.com，将域名指定为 CONTOSO。 | 在此处填写域名：                                                                                                                                        |
| 应用程序组                       | 指定之前在 [Required user and group](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug)（所需用户和组）中创建的安全组的名称。                                                                                                                                  | 在此处填写组名称：                                                                                                                                         |
| 服务用户                            | 指定之前在 [Required user and group](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug)（所需用户和组）中创建的服务用户帐户的登录名。                                                                                                                      | 在此处填写用户帐户名称：                                                                                                                                  |
| **密码**                                | 指定 BHOLD 核心服务用户帐户的密码。                                                                                                                                                                                                                                              | 在此处填写密码：重要说明：确保将此密码保存在一个隐藏的安全位置。                                                                |
| **网站 IP/端口**                         | 指定要在 Intranet 服务器上创建的网站的 IP 地址和端口号。 仅当不使用同一个 IP 地址作为默认网站时，更改默认值 (\*)。 仅当默认端口 (5151) 已在使用时，将端口号更改为可用端口。             | 如果默认网站使用非默认 IP 地址，则在此处填写：如果已在使用默认端口号，则在此处填写 BHOLD 网站端口号： |

### <a name="database-settings"></a>数据库设置

| **项目**                                       | **描述**                                                                                                                                                                                                                                                           | **值**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 使用集成安全性                    | 指定 Windows 身份验证用于访问数据库。                                                                                                                                                                                                     | 如果 Windows 身份验证用于连接到 SQL Server，则选中该复选框。 如果使用 SQL Server 身份验证，则清除该复选框。 如果使用 SQL Server 身份验证，则运行 BHOLD 核心安装程序前必须创建数据库。 注意：如果使用 Windows 身份验证，则必须使用在数据库服务器中具有 sysadmin 服务器角色的帐户登录。 |
| 数据库用户和数据库密码 | 指定在数据库服务器中具有 sysadmin 服务器角色的用户的用户名和密码。 仅当使用 SQL Server 身份验证时提供这些值。                                                                                               | 在此处填写 SQL Server 用户名：在此处填写 SQL Server 用户密码：注意：确保将此密码保存在一个隐藏的安全位置。                                                                                                                                                                                                                                                  |
| 数据库服务器和数据库名称   | 指定数据库服务器的 NetBIOS 名称，以及 BHOLD 核心安装程序将创建的数据库的名称（默认：b1）。 如果不使用默认数据库服务器实例，请指定数据库服务器实例，采用的格式为：\<服务器\>\\\<实例\>。 | 在此处填写服务器（或服务器和实例）的名称：在此处填写数据库名称：                                                                                                                                                                                                                                                                                                                   |
| 对数据库用户进行限制    | 已过时。                                                                                                                                                                                                                                                                 | 请勿更改默认值                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>BHOLD 核心安装

要安装 BHOLD 核心模块，请以域管理员组成员的身份登录，下载以下文件，并在要安装 BHOLD 核心模块的服务器上以管理员身份运行该文件： 

- BholdCore *\<版本\>*\_Release.msi

将 \<版本\> 替换为要安装的 BHOLD 核心版本的版本号。

若要以管理员身份运行程序文件，右键单击该文件，然后单击“以管理员身份运行”。

### <a name="postinstallation-settings"></a>安装后的设置

BHOLD 核心安装完成后，必须配置 Windows 防火墙，并在 Internet Information Services 的 BHOLD 核心应用程序池中更改高级设置，完成 BHOLD 核心配置。 如有必要，还应更改 BHOLD 系统属性，以满足要求。

#### <a name="configuring-windows-firewall"></a>配置 Windows 防火墙

如果用户通过在远程计算机上使用 Web 浏览器访问 BHOLD，则必须在 BHOLD 核心服务器上配置 Windows 防火墙，允许在安装 BHOLD 核心时指定的指向网站端口的传入连接。

若要执行此过程，您必须是本地计算机 Administrators 组的成员。

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>允许指向 BHOLD 网站的传入连接

1.  单击“启动”，指向“管理工具”，右键单击“具有高级设置的 Windows 防火墙”，然后单击“以管理员身份运行”。

2.  在左窗格中单击“入站规则”，然后在右窗格中单击“新建规则”。

3.  在“新建入站规则”向导中，单击“端口”，然后单击“下一步”。

4.  确保选中“TCP”，在“特定本地端口”中键入默认 BHOLD 核心端口号 (5151) 或安装 BHOLD 核心时指定的端口号，然后单击“下一步”。

5.  确保选中“允许连接”，然后单击“下一步”。

6.  在“配置文件”页上，对于不想从中访问 BHOLD 网站的位置，清除其复选框，然后单击“下一步”。

7.  在“名称”页上键入规则的名称（如允许指向 BHOLD 核心的传入连接），然后单击“完成”。  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>对 BHOLD 核心应用程序池启用 32 位应用程序

若要使 IIS 与 BHOLD 核心模块正常工作，必须将 BHOLD 核心应用程序池配置为支持 32 位应用程序。 必须使用用于安装 BHOLD 核心模块的帐户登录，才能执行此过程。

对 BHOLD 核心应用程序池启用 32 位应用程序支持

1.  要打开 Internet Information Services 管理器，请单击“开始” ，指向“管理工具” ，然后单击“Internet Information Services (IIS) 管理器” 。

2.  在控制台树中，展开服务器名称，然后单击“应用程序池”。

3.  在“应用程序池”列表中，右键单击“CoreAppPool”，然后单击“高级设置”。

4.  在“高级设置”对话框中，在“启用 32 位应用程序”列表中选择“True”，然后单击“确定”。

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>确定 BHOLD 网站的服务主体名称

如果用于联系 BHOLD 网站的网络名称与服务器主机名不同，则必须确定 HTTP 的服务主体名称 (SPN)。 例如，如果使用 DNS 中的 CNAME 资源记录指定服务器别名，或者如果使用网络负载均衡，则必须在 Active Directory 中注册这些额外的网络地址。 如果未能这样操作，则 Internet Explorer 联系 BHOLD 网站时无法使用 Kerberos 协议。

> [!IMPORTANT]
> 如果 BHOLD 核心模块安装在与 FIM 门户相同的计算机上，则必须使用不同的主机名，为运行 BHOLD 核心的服务器和运行 FIM 门户的服务器创建 DNS 资源记录（CNAME 或 A）。 仅可为特定的服务类型/服务器别名对确定一个 SPN，由于 BHOLD 核心和 FIM 门户通常在不同的帐户下运行，因此需要单独的 SPN。 如果已在其他帐户下确定了 SPN，则 setspn 命令会报告错误。

若要完成该过程，必须至少具有域管理员的成员资格或同等权限。

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>确定 BHOLD 网站的 SPN

1.  在 Active Directory 域服务域控制器上，依次单击“开始”->“所有程序”->“附件”，右键单击“命令提示符”，然后单击“以管理员身份运行”。

2.  在命令提示符下，键入以下命令，然后按 Enter：setspn –S HTTP/ *\<networkalias\> \<domain\>* \\ *\<accountname\>* 其中：

    -   \<networkalias\> 是客户端用于联系 BHOLD 网站的地址

    -   \<domain\>\\\<accountname\> 是安装 BHOLD 核心时所创建的 BHOLD 核心服务帐户的域和用户名。

3.  对于客户端用于联系 BHOLD 网站的所有其他名称重复上述步骤，例如 CNAME 别名、包含完全限定的域名的名称，或包含 NetBIOS（短）域名的名称。

#### <a name="setting-bhold-system-attributes"></a>设置 BHOLD 系统属性

为验证 BHOLD 核心模块的安装是否成功，请打开 BHOLD 核心门户并查看系统属性。 此外，为确保 BHOLD 核心模块在你的环境中运行正常，可以根据需要修改以下 BHOLD 系统属性：

| 属性                | **描述**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| NoHistory                | 如果 BHOLD 网站在群集的 Web 服务上运行，则设置为 Y，确保最近显示的项目正常运行。 如果 BHOLD 网站在独立的 IIS 服务器上运行，则设置为 N。                                                                                                                      |
| MoveorgunitToSameorgtype | 设置为 Y，可确保组织单位 (orgunit) 只能移动到组织类型与父级 orgunit 相同的 orgunit。 例如，这样可以防止项目 orgunit 移入部门 orgunit。 设置为 N，可允许将一个 orgunit 放入其他类型的 orgunit。 |
| 运行 ABA 的间隔天数     | 设置为两位整数，指定两次运行基于属性的授权 (ABA) 之间的间隔时间（以天为单位）。 例如，要将运行 ABA 的间隔时间指定为两天，则键入 02。                                                                                                                     |
| 运行 ABA 的开始时间    | 设置为两位整数，指定在当天开始运行基于属性的授权的具体时间（具体到小时）。 例如，若要指定晚上 11:00 (23:00) 开始运行 ABA ，则键入 23。                                                                                                             |
| 系统基数       | 如果不想要系统基数签入 BHOLD，则设置为 N。 默认值为 Y。                                                                                                                                                                                                                             |
| 日志记录                  | 如果不想要记录更改，则设置为 N。 默认值为 Y。                                                                                                                                                                                                                                            |
| 系统队列处理   | 如果不想要进行系统队列处理，则设置为 N。 请勿更改此值，除非产品支持部门指示这样操作。                                                                                                                                                                                           |

必须以域管理员组的成员身份登录，才能执行此过程。

设置 BHOLD 系统属性

1.  依次单击“开始”->“所有程序”->“Internet Explorer”。

2.  在地址框中键入 ，其中 \<server\> 是 BHOLD 网站服务器的名称，\<port\> 是绑定到网站的端口号。

3.  依次单击“主页”->“值”->“修改”。

4.  找到要更改属性的名称，在属性名称旁边的框中键入新值，然后单击“确定”。

## <a name="next-steps"></a>后续步骤

安装 BHOLD 核心并确定安装成功后，可以安装其他模块。 此时，BHOLD 数据库实际上将为空，仅包含一个用户帐户、根帐户，以及一个组织单位 (orgunit)，即根 orgunit。 要向 BHOLD 数据库添加更多用户，可以根据需要安装访问管理连接器模块，或 BHOLD 模型生成器模块。 可以使用访问管理连接器模块从 FIM 同步服务中导入用户数据，也可以使用 BHOLD 模型生成器从一组结构化文件中导入用户数据。 有关使用访问管理连接器模块的详细信息，请参阅 [Test Lab Guide: BHOLD Access Management Connector](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx)（测试实验室指南：BHOLD 访问管理连接器）。

有关使用 BHOLD 模型生成器模块的详细信息，请参阅：

- [Microsoft BHOLD Suite Concepts Guide](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)（Microsoft BHOLD 套件概念指南）
- [Microsoft BHOLD Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)（Microsoft BHOLD 套件技术参考）。
