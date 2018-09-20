---
title: 配置 SharePoint for Microsoft Identity Manager 2016 | Microsoft Docs
description: 安装和配置 SharePoint Foundation，以便它能承载 MIM 门户页。
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a64ee79897ce73242d0f8510842fdcb6758fab2c
ms.sourcegitcommit: 28834821cbddd6384613d8ba45424c35f4c39ce6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538568"
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>设置标识管理服务器：SharePoint

> [!div class="step-by-step"]
> [« SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server »](prepare-server-exchange.md)
> 
> [!NOTE]
> 本演练使用名为 Contoso 的公司中的示例名和值。 将其替换为你自己的。 例如：
> - 域控制器名称 - corpdc
> - 域名 - **contoso**
> - MIM 服务服务器名称 - corpservice
> - MIM 同步服务器名称 - corpsync
> - SQL Server 名称 - corpsql
> - 密码 - <strong>Pass@word1</strong>


## <a name="install-sharepoint-2016"></a>安装 SharePoint 2016

> [!NOTE]
> 安装程序需要 Internet 连接才能下载其必备组件。 如果计算机位于不提供 Internet 连接的虚拟网络上，将其他网络接口添加到该计算机以便提供 Internet 连接。 安装完成后可将其禁用。

按照以下步骤安装 SharePoint 2016。 安装结束时，将重启服务器。

1.  在 SQL 数据库服务器的 corpservice 和 sysadmin 上使用本地管理员的域帐户来启动 PowerShell，我们将使用 contoso\miminstall。

    -   更改为 SharePoint 解压缩所在的目录。

    -   键入下列命令。

        ```
        .\prerequisiteinstaller.exe
        ```

2.  安装 **SharePoint** 必备组件后，通过键入以下命令安装 **SharePoint 2016**：

    ```
    .\setup.exe
    ```

3.  选择完整的服务器类型。

4.  安装完成后，运行该向导。

## <a name="run-the-wizard-to-configure-sharepoint"></a>运行向导以配置 SharePoint

按照 **SharePoint 产品配置向导**中列出的步骤配置 SharePoint 以使用 MIM。

1. 在 **连接到服务器场** 选项卡上，更改为创建新的服务器场。

2. 将此服务器指定为用于配置数据库的数据库服务器（如 corpsql），并将 Contoso\SharePoint 指定为要使用的 SharePoint 的数据库访问帐户。
3. 创建一个密码，用作场安全密码。

4. 建议在配置向导中选择 [MinRole](https://docs.microsoft.com/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server-2016) 类型的“前端”

5. 当配置向导完成配置任务 10（共 10 项）后，单击“完成”，将打开 Web 浏览器。

6. 如果出现 Internet Explorer 弹出窗口，以 Contoso\miminstall 身份（或等效的管理员帐户）进行身份验证才能继续。

7. 在 Web 向导中（在 Web 应用中），单击“取消/跳过”。


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>准备 SharePoint 以承载 MIM 门户

> [!NOTE]
> 最初，将不配置 SSL。 在启用对此门户的访问之前，请务必配置 SSL 或同等协议层。

1. 启动 SharePoint 2016 命令行管理程序并运行以下 PowerShell 脚本，以创建 SharePoint 2016 Web 应用程序。

    ```
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > 将显示一条警告消息，指示 Windows 经典身份验证方法正在使用中，最后的命令可能需要几分钟才能返回。 完成后，输出将显示新门户的 URL。 保持“SharePoint 2016 命令行管理程序”窗口处于打开状态，以供稍后参考。

2. 启动 SharePoint 2016 命令行管理程序并运行以下 PowerShell 脚本，以创建与 Web 应用程序关联的 Sharepoint 站点集合。

   ```
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
   ```

   > [!NOTE]
   > 验证 CompatibilityLevel 变量的结果是否为“15”。 如果结果不是“15”，则站点集合不是为正确体验版本创建的；删除该站点集合并重新创建。

3. 通过在“SharePoint 2016 命令行管理程序”中运行以下 PowerShell 命令来禁用“SharePoint 服务器端视图状态”和 SharePoint 任务“运行状况分析作业（每小时、Microsoft SharePoint Foundation 计时器、所有服务器）”：

   ```
   $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   ```

4. 在身份管理服务器上，打开一个新的 Web 浏览器选项卡，导航到 http://mim.contoso.com/ 并以 contoso\miminstall 身份登录。  将显示一个名为 *MIM 门户* 的空 SharePoint 站点。

    ![MIM 门户 (http://mim.contoso.com/) 的图像](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. 复制该 URL，然后在 Internet Explorer 中打开“Internet 选项”，更改为“安全”选项卡，选择“本地 Intranet”，然后单击“网站”。

    ![Internet 选项图像](media/MIM-DeploySP2.png)

6. 在“本地 intranet”窗口中，单击“高级”并将复制的 URL粘贴到“将此网站添加到该区域”文本框。 单击“添加”，然后关闭窗口。

7. 打开“管理工具”程序，导航到“服务”，找到 SharePoint 管理服务，并启动它（如果未运行）。

> [!div class="step-by-step"]  
> [« SQL Server 2016](prepare-server-sql2016.md)
> [Exchange Server »](prepare-server-exchange.md)
