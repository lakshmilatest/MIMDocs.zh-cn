---
# required metadata

title: 设置标识管理服务器 &#58; SharePoint | Microsoft 标识管理器
description: 安装和配置 SharePoint Foundation，以便它能承载 MIM 门户页。 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# 设置标识管理服务器：SharePoint

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> 在下面的所有示例中，**mimservername** 表示域控制器的名称，**contoso** 表示你的域名，**Pass@word1** 表示示例密码。


## 安装**附带 SP1 的 SharePoint Foundation 2013**。

> [!NOTE]
> 安装程序需要 Internet 连接以便下载其必备组件。

安装结束时将重新启动服务器。

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

按照 **SharePoint 产品配置向导**中列出的步骤配置 SharePoint 以使用 MIM。

1. 在 **连接到服务器场** 选项卡上，更改为创建新的服务器场。

2. 将此服务器指定为用于配置数据库的数据库服务器，并将 *Contoso\SharePoint* 指定为要使用的 SharePoint 的数据库访问帐户。

3. 将一个密码指定为场安全密码（此实验环境以后将不会使用它）。

4. 配置向导完成配置任务 10（共 10 项），单击“完成”，然后将打开 Web 浏览器。

5. 在 Internet Explorer 弹出窗口中，以 *Contoso\Administrator* 身份（或等效的域管理员帐户）进行身份验证才能继续。

6. 启动向导（在 Web 应用内）配置 SharePoint 场。

7. 选择选项以使用现有托管帐户 (*Contoso\SharePoint*)，然后单击“下一步”****。

8. 在“创建站点集合” **** 窗口中，单击“跳过” ****。  然后单击 **“完成”**。

## 准备 SharePoint 以承载 MIM 门户

1. 创建 **SharePoint Foundation 2013 Web 应用程序**。

    > [!NOTE]
    > 最初，将不配置 SSL。 在启用对此门户的访问之前，请务必配置 SSL 或同等协议层。

    1. 启动  **SharePoint 2013 Management Shell** 并运行以下 PowerShell 脚本：

        ```
        $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
        New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"
        -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
        ```

        2. 请注意，将出现一条警告消息“正在使用 Windows 经典身份验证方法”，最后一条命令返回可能需要几分钟时间。  完成后，输出将显示新门户的 URL。  保持 **SharePoint 2013 Management Shell** 窗口的打开状态，因为后续任务需要使用此窗口。

2. 创建与该 Web 应用程序关联的 **SharePoint 站点集合**。

    1. 启动 SharePoint 2013 Management Shell 并运行以下 PowerShell 脚本：

        ```
        $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
        $w = Get-SPWebApplication http://corpidm.contoso.local:82
        New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
        -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
        $s = SpSite($w.Url)
        $s.AllowSelfServiceUpgrade = $false
        $s.CompatibilityLevel
        ```

        2. 验证 *CompatibilityLevel* 变量的结果是否是“14”。  （[请参阅“在 SharePoint Foundation 2013 上安装 FIM 2010 R2”](http://technet.microsoft.com/library/jj863242.aspx)了解详细信息）。 如果结果为“15”，则站点集合不是针对 2010 体验版本创建的；删除该站点集合并重新创建它。

3. 通过在 **SharePoint 2013 Management Shell** 中运行以下 PowerShell 命令来禁用 **SharePoint 服务器端师徒状态**和 SharePoint 任务“运行状况分析作业（每小时、Microsoft SharePoint Foundation 计时器、所有服务器）”：

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

4. 在身份管理服务器上，打开一个新的 Web 浏览器选项卡，导航到 http://localhost:82 / 并以 *contoso\Administrator* 身份登录。  将显示一个名为 *MIM 门户* 的空 SharePoint 站点。

    ![MIM 门户网站：http://localhost:82/ image](media/MIM-DeploySP1.png)

5. 复制该 URL，然后在 Internet Explorer 中打开“Internet 选项”****，更改为“安全”****选项卡，选择“本地 Intranet”****，然后单击“网站”****。

    ![Internet 选项图像](media/MIM-DeploySP2.png)

6. 在“本地 intranet”****窗口中，单击“高级”****并将复制的 URL粘贴到“将此网站添加到该区域”****文本框。 单击“添加”****，然后关闭窗口。

7. 打开“管理工具”****程序，导航到“服务”****，找到 SharePoint 管理服务，并启动它（如果未运行）。

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)


<!--HONumber=Apr16_HO2-->


