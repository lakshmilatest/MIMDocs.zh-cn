---
title: "配置 SharePoint for Microsoft Identity Manager 2016 | Microsoft Docs"
description: "安装和配置 SharePoint Foundation，以便它能承载 MIM 门户页。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1114be2ce13ca012582676803eb1dc29cadae596
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# 设置标识管理服务器：SharePoint
<a id="set-up-an-identity-management-server-sharepoint" class="xliff"></a>

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> 本演练使用名为 Contoso 的公司中的示例名和值。 将其替换为你自己的。 例如：
> - 域控制器名称 - **mimservername**
> - 域名 - **contoso**
> - 密码 - **Pass@word1**


## 安装**附带 SP1 的 SharePoint Foundation 2013**。
<a id="install-sharepoint-foundation-2013-with-sp1" class="xliff"></a>

> [!NOTE]
> 安装程序需要 Internet 连接才能下载其必备组件。 如果计算机位于不提供 Internet 连接的虚拟网络上，将其他网络接口添加到该计算机以便提供 Internet 连接。 安装完成后可将其禁用。

按照以下步骤安装 SharePoint Foundation 2013 SP1。 安装结束时，将重启服务器。

1.  以域管理员身份启动 **PowerShell** 。

    -   更改为 SharePoint 解压缩所在的目录。

    -   键入下列命令。

        ```
        .\prerequisiteinstaller.exe
        ```

2.  安装了 **SharePoint** 必备组件后，通过键入以下命令安装 **附带 SP1 的 SharePoint Foundation 2013** ：

    ```
    .\setup.exe
    ```

3.  选择完整的服务器类型。

4.  安装完成后，运行该向导。

## 运行向导以配置 SharePoint
<a id="run-the-wizard-to-configure-sharepoint" class="xliff"></a>

按照 **SharePoint 产品配置向导**中列出的步骤配置 SharePoint 以使用 MIM。

1. 在 **连接到服务器场** 选项卡上，更改为创建新的服务器场。

2. 将此服务器指定为用于配置数据库的数据库服务器，并将 *Contoso\SharePoint* 指定为要使用的 SharePoint 的数据库访问帐户。

3. 创建一个密码，用作场安全密码。

4. 配置向导完成配置任务 10（共 10 项），单击“完成”，然后将打开 Web 浏览器。

5. 在 Internet Explorer 弹出窗口中，以 *Contoso\Administrator* 身份（或等效的域管理员帐户）进行身份验证才能继续。

6. 启动向导（在 Web 应用内）配置 SharePoint 场。

7. 选择选项以使用现有托管帐户 (*Contoso\SharePoint*)，然后单击“下一步”。

8. 在“创建站点集合”  窗口中，单击“跳过” 。  然后单击 **“完成”**。

## 准备 SharePoint 以承载 MIM 门户
<a id="prepare-sharepoint-to-host-the-mim-portal" class="xliff"></a>

> [!NOTE]
> 最初，将不配置 SSL。 在启用对此门户的访问之前，请务必配置 SSL 或同等协议层。

1. 启动 **SharePoint 2013 命令行管理程序**并运行以下 PowerShell 脚本，以创建**SharePoint Foundation 2013 Web 应用程序**。

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
    ```

    > [!NOTE]
    > 将显示一条警告消息，指示 Windows 经典身份验证方法正在使用中，最后的命令可能需要几分钟才能返回。 完成后，输出将显示新门户的 URL。 保持 **SharePoint 2013 命令行管理程序** 窗口处于打开状态，以供稍后参考。

2. 启动 SharePoint 2013 命令行管理程序并运行以下 PowerShell 脚本，以创建与 Web 应用程序关联的 **Sharepoint 站点集合**。

  ```
  $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
  $w = Get-SPWebApplication http://corpidm.contoso.local:82
  New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
  -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
  $s = SpSite($w.Url)
  $s.AllowSelfServiceUpgrade = $false
  $s.CompatibilityLevel
  ```

  > [!NOTE]
  > 验证 *CompatibilityLevel* 变量的结果是否是“14”。 如果结果为“15”，则站点集合不是针对 2010 体验版本创建的；删除该站点集合并重新创建它。

3. 通过在 **SharePoint 2013 Management Shell** 中运行以下 PowerShell 命令来禁用 **SharePoint 服务器端师徒状态**和 SharePoint 任务“运行状况分析作业（每小时、Microsoft SharePoint Foundation 计时器、所有服务器）”：

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. 在身份管理服务器上，打开一个新的 Web 浏览器选项卡，导航到 http://localhost:82 / 并以 *contoso\Administrator* 身份登录。  将显示一个名为 *MIM 门户* 的空 SharePoint 站点。

    ![MIM 门户网站：http://localhost:82/ image](media/MIM-DeploySP1.png)

5. 复制该 URL，然后在 Internet Explorer 中打开“Internet 选项”，更改为“安全”选项卡，选择“本地 Intranet”，然后单击“网站”。

    ![Internet 选项图像](media/MIM-DeploySP2.png)

6. 在“本地 intranet”窗口中，单击“高级”并将复制的 URL粘贴到“将此网站添加到该区域”文本框。 单击“添加”，然后关闭窗口。

7. 打开“管理工具”程序，导航到“服务”，找到 SharePoint 管理服务，并启动它（如果未运行）。

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)
