---
title: Microsoft Identity Manager 2016 密码管理 | Microsoft Docs
description: ''
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: def1be943b4f2f919a079e3fc4aa544af10463aa
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333377"
---
# <a name="microsoft-identity-manager-2016-password-management"></a>Microsoft Identity Manager 2016 密码管理

管理具有多个数据源的企业环境的复杂问题之一就是管理多个用户帐户的密码。 Microsoft Identity Manager 2016 (MIM) 提供了 2 个密码管理解决方案：

-   密码同步 - 利用密码更改通知服务 (PCNS) 捕获 Active Directory 中的密码更改并将它们传播到其他连接的数据源。

-   基于用户的密码更改管理 - 通过基于 Web 的技术支持和自助服务密码重置来利用 Windows Management Instrumentation (WMI)。

通过使用密码同步和基于用户的密码更改管理，你可以：

-   减少用户需要记住的不同密码数量。

-   同时将用户的多个帐户密码设置或更改为相同密码。

-   允许用户在 Active Directory 中更改自己的密码并将密码更改推送到其他系统。

-   消除构建其他密码或凭据存储的风险。

-   通过将 Active Directory 用作权威源，可在多个数据源之间同步密码。

-   独立于 MIM 操作，实时执行密码管理操作。

## <a name="password-extensions"></a>密码扩展

默认情况下，目录服务器的管理代理支持密码更改和设置操作。 对于基于文件的、数据库形式的和可扩展的连接管理代理（默认情况下，不支持密码更改和设置操作），可创建 .NET 密码扩展动态链接库 (DLL)。
每当为任一这些管理代理调用密码更改或设置调用时，都会调用 .NET 密码扩展 DLL。 将在 Synchronization Service Manager 中为这些管理代理配置密码扩展设置。 若要深入了解如何配置密码扩展，请参阅“FIM 开发人员参考”。

| 默认情况下，下列管理代理支持密码管理： | 通过使用密码扩展，下列管理代理也支持密码管理： |
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Active Directory                                                          | 属性值对文本文件                                                                    |
| Active Directory 轻型目录服务 (ADLDS)                   | 带分隔符的文本文件                                                                               |
| IBM Directory Server                                                      | 目录服务标记语言 (DSML)                                                          |
| Lotus Notes                                                               | Extensible Connectivity                                                                            |
| Novell eDirectory                                                         | 定宽文本文件                                                                             |
| Sun 和 Netscape 目录服务器                                        | IBM DB2 Universal Database                                                                         |
|                                                                           | LDAP 数据交换格式 (LDIF)                                                                |
|                                                                           | Microsoft SQL Server                                                                               |
|                                                                           | Oracle 数据库                                                                                    |

## <a name="password-synchronization"></a>密码同步


密码同步在 Active Directory 域中与密码更改通知服务 (PCNS) 配合使用，并且允许将来自 Active Directory 的密码更改自动传播到其他连接的数据源。 MIM 通过作为远程过程调用 (RPC) 服务器运行来侦听来自 Active Directory 域控制器的密码更改通知，从而完成此操作。 接收到密码更改请求并对其进行身份验证后，MIM 将对其进行处理，然后再传播到合适的管理代理。

> [!IMPORTANT]
> MIM 不支持双向密码同步。 配置双向密码同步会创建一个循环，这将占用服务器资源并且可能会对 Active Directory 和 MIM 产生负面影响。

PCNS 会在每个 Active Directory 域控制器上运行。 接收密码通知的系统被称为目标。 必须将 MIM 服务器配置为 Active Directory 中的 PCNS 目标，然后才能发送密码通知。 PCNS 配置必须定义包含组并选择性地定义排除组。 这些组用于限制来自域的敏感密码流。 例如，若要发送所有用户密码而不发送管理员密码，可选择将“域用户”用作包含组并将“域管理员”用作排除组。 若要深入了解如何配置密码更改通知服务，请参阅[使用密码同步](https://technet.microsoft.com/library/jj590288(v=ws.10).aspx)

密码同步过程所需的组件包括：

-   密码更改通知服务 (Pcnssvc.exe) - 密码更改通知服务将在域控制器上运行，负责接收来自本地密码筛选器的密码更改通知、对其进行排队以供运行 MIM 的目标服务器使用，以及使用 RPC 传送通知。 该服务将加密密码并确保密码的安全性，直到将其成功传送到运行 MIM 的目标服务器。

-   服务主体名称 (SPN) - SPN 是 Active Directory 中帐户对象上的属性， Kerberos 协议用它来手动对 PCNS 和目标进行身份验证。 SPN 将确保 PCNS 向运行 MIM 的正确服务器进行身份验证，并且没有其他服务可接收密码更改通知。 使用 setspn.exe 工具可创建并分配 SPN。 若要深入了解如何配置 SPN，请参阅“使用密码同步”。

-   密码更改通知筛选器 (Pcnsflt.dll) - 密码筛选器用于获取来自 Active Directory 的纯文本密码。 此筛选器通过本地安全机构 (LSA) 加载到每个 Windows Server 域控制器，这些域控制器会将密码分发到运行 MIM 的目标服务器。 安装筛选器并重启域控制器后，筛选器将开始接收来自域控制器的密码更改通知。 密码通知筛选器将与在域控制器上运行的其他筛选器同时运行。

-   密码更改通知服务配置实用程序 (Pcnscfg.exe) - pcnscfg.exe 实用程序用于管理和维护存储在 Active Directory 中的密码更改通知服务配置参数。 对运行 MIM 的目标服务器进行身份验证以及向其发送密码通知时，将使用这些配置参数，如定义目标服务器、密码队列重试间隔和启用或禁用目标服务器。
    服务配置存储在 Active Directory 中，因此只需更新一台域控制器上的配置。 Active Directory 会将更改复制到所有其他域控制器。

-   运行 MIM 的服务器上的远程过程调用 (RPC) 服务器 - 启用密码同步后，将启动运行 MIM 的服务器上的 RPC 服务器，使其能够接收来自密码更改通知服务的通知。 RPC 将动态选择要使用的端口范围。 如果要求 MIM 通过防火墙与 Active Directory 林进行通信，则必须打开端口范围。

-   密码扩展 DLL - 通过密码扩展 DLL，可借助任何数据库、扩展连接或基于文件的管理代理的规则扩展来完成密码设置或更改操作。
    通过创建名为“export_password”的仅导出加密属性可完成此操作，此属性实际不存在于已连接的目录中，但可在设置规则扩展中访问和设置，或可在导出属性流期间使用。 若要深入了解如何配置密码扩展，请参阅 [FIM 开发人员参考](https://msdn.microsoft.com/library/windows/desktop/ee652263(v=vs.100).aspx)。

## <a name="preparing-for-password-synchronization"></a>准备密码同步

为 MIM 和 Active Directory 环境设置密码同步前，请验证以下内容：

-   是否根据安装说明安装 MIM。

-   是否创建了用于管理密码同步的连接数据源管理代理，以及是否成功联接和同步了对象。

设置密码同步：

-   扩展 Active Directory 架构，添加安装和运行密码更改通知服务 (PCNS) 所需的类和属性。

-   在每个域控制器上安装 PCNS。

-   在 Active Directory 中为 MIM 服务帐户配置服务主体名称 (SPN)。

-   配置 PCNS，使其与目标 MIM 服务通信。

-   配置用于管理密码同步的连接数据源管理代理。

-   在 MIM 上启用密码同步。

若要深入了解如何设置密码同步，请参阅“使用密码同步”。

## <a name="password-synchronization-process"></a>密码同步过程

将密码更改请求从 Active Directory 域控制器同步到其他连接数据源的过程如下图所示：

1.  用户通过按 Ctrl+Alt+Del 可启动密码更改请求。密码更改请求（包括新密码）将发送到最近的域控制器。

2.  域控制器将记录密码更改请求并通知密码更改通知筛选器 (Pcnsflt.dll)。

3.  密码更改通知筛选器会将请求传递到密码更改通知服务 (PCNS)。

4.  PCNS 将验证密码更改请求，然后通过使用 Kerberos 对服务主体名称 (SPN) 进行身份验证并将加密 RPC 中的密码更改请求转发到 MIM 目标服务器。

5.  MIM 将验证源域控制器，然后使用域名查找支持该域的管理代理并使用密码更改请求中的用户帐户信息查找连接器空间中的相应对象。

6.  通过使用联接表信息，MIM 可确定接收密码更改的管理代理并将密码更改推送到这些代理。

## <a name="password-synchronization-security"></a>密码同步安全

已解决以下密码同步安全问题：

-   通过密码源进行身份验证 - 收到密码更改通知后，Kerberos 身份验证将由 MIM 和源域控制器完成，从而确保收件人和发件人都有效。 收到密码更改通知时，MIM 将确保调用方在所属域的域控制器容器中具有帐户。

-   由于非安全连接，密码同步到目标数据源的操作失败 - 如果管理代理配置为需要安全连接，但未检测到安全连接，则同步将失败。
    如果管理代理配置为允许非安全连接，则仍会进行同步。 仅当检查并了解所涉及的风险后，才可启用允许非安全连接。

-   密码的安全存储 - MIM 仅暂时存储加密的密码。 MIM 在密码更改通知操作期间收到的所有密码进入 MIM 过程之后都将立即进行加密。
    成功将密码发送到目标连接数据源时，密码将立即解密，而存储密码的内存将被直接清除。 如果该操作未能写入到目标连接数据源，则将存储加密的密码，直到尝试过所有重试方法后再将加密密码从内存中清除。

-   安全密码队列 - PCNS 密码队列中存储的密码已加密，直到发送时才解密。

## <a name="password-synchronization-error-recovery-scenarios"></a>密码同步错误恢复方案

理想情况下，每当用户更改密码时，此更改都将正确无误地进行同步。 下列方案介绍了 MIM 如何从常见的同步错误中恢复：

-   Active Directory 中的密码通知未能发送到 MIM - 如果网络故障或运行 MIM 的服务器不可用，则可能发生此错误。 密码更改通知仍然由 PCNS 在本地域控制器上进行排列。 PCNS 将根据通知的重试间隔配置重新发送通知。

-   未能将密码同步到目标数据源 - 如果网络故障或目标数据源不可用，则可能发生此错误。
    密码更改通知将根据管理代理的重试尝试和重试间隔配置进行排队和重试。 所有密码将在存储以供重试时进行加密，而在操作成功或达到重试限制时删除。

-   失败后激活运行 MIM 的热备用服务器 - 如果运行 MIM 的主服务器失败，则可配置温备用服务器来同步密码并在不会造成密码更改丢失的情况下激活它。 有关详细信息，请参阅 [MIISactivate: Server Activation Tool](https://technet.microsoft.com/library/jj590194(v=ws.10).aspx)（MIISactivate：服务器激活工具）

有些错误十分严重，导致再多的重试都无法促使操作成功。 在这些情况下，将记录错误事件并停止进程。 以下事件不会重试：

| 事件 | 严重性    | 描述                                                                                                                                                            |
|-------|-------------|-----------|
| 6919  | 信息 | 时间戳已过时，因此未执行密码同步设置操作。                                                                      |
| 6921  | 错误       | 目标管理代理上未启用密码管理，因此未执行密码同步设置操作。                                |
| 6922  | 错误       | 目标管理代理上未配置密码管理，因此未执行密码同步设置操作。                             |
| 6923  | 警告     | 连接目录中找不到目标连接器空间对象，因此未处理密码同步设置操作。                  |
| 6927  | 错误       | 密码不符合目标系统的密码策略，因此密码同步设置操作失败。                                      |
| 6928  | 错误       | 未将目标管理代理的密码扩展配置为支持密码设置操作，因此密码同步设置操作失败。 |

## <a name="user-based-password-change-management"></a>基于用户的密码更改管理

MIM 提供了 2 个 Web 应用程序，这两个应用程序都使用 Windows Management Instrumentation (WMI) 进行密码重置。 与密码同步一样，在“管理代理设计器”中配置管理代理时，可激活密码管理。 若要深入了解密码管理和 WMI，请参阅“MIM 开发人员参考”。

MIM 在安装期间创建了 2 个安全组，专门支持密码管理操作：

-   FIMSyncBrowse - 此组的成员有权在使用 WMI 查询执行搜索操作时收集用户帐户的相关信息。

-   FIMSyncPasswordSet - 此组的成员有权将密码管理界面用于 WMI，执行帐户搜索、密码设置和密码更改操作。
