---
title: 部署 PAM 步骤 4 – 安装 MIM | Microsoft Docs
description: 在 Privileged Access Management 服务器和工作站上安装和配置 MIM 服务和门户。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2b5340ef3f98ba94904e595c3526d09bdac3f95f
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518496"
---
# <a name="step-4--install-mim-components-on-pam-server-and-workstation"></a>步骤 4 - 在 PAM 服务器和工作站上安装 MIM 组件

> [!div class="step-by-step"]
> [« 步骤 3](step-3-prepare-pam-server.md)
> [步骤 5 »](step-5-establish-trust-between-priv-corp-forests.md)

在 PAMSRV 上，以 PRIV\Administrator 身份登录，以便能够安装 MIM 服务和门户以及示例门户 Web 应用程序。

  > [!NOTE]
  > 必须具有域管理员身份；如果你不是以域管理员身份运行以下命令，下一步中的信任验证检查将无法完成。

如果已下载了 MIM，请将 MIM 安装存档解压缩到新的文件夹。

## <a name="run-the-service-and-portal-install-program"></a>运行服务和门户安装程序。

按照安装程序的说明操作并完成安装。

1. 选择组件功能时请包含 MIM 服务（包括 Privileged Access Management，但不包括 MIM 报告）和 MIM 门户  

   ![自定义安装 - 屏幕截图](./media/PAM_GS_MIM_2015_Service_Portal.png)

2. 配置通用服务和 MIM 数据库连接时，请指定**创建一个新数据库**。

   > [!NOTE]
   > 如果为实现高可用性安装 MIM 服务多次，请为所有后续安装指定**使用现有数据库**。

3. 在配置邮件服务器连接时，请将邮件服务器设置为 CORP 环境的 Exchange 或 SMTP 服务器的主机名（如果没有邮件服务器，请使用 corpdc.contoso.local），并取消选中“使用 SSL”和“邮件服务器是 Exchange Server 2007 或 Exchange Server 2010”复选框。  

4. 选择以生成新的自签名证书。

5. 设置以下帐户凭据：
   - 服务帐户名称：MIMService   
   - 服务帐户密码：<em>Pass@word1</em>（或者步骤 2 中创建的密码）  
   - 服务帐户域：PRIV   
   - 服务电子邮件帐户：<em>MIMService@priv.contoso.local</em>  

6. 接受同步服务器主机名的默认值，并将 MIM 管理代理帐户指定为“PRIV\MIMMA”。  将显示 MIM 同步服务不存在的警告消息。 这没有关系，因为此场景中不使用 MIM 同步服务。

7. 将 *“PAMSRV”* 指定为 MIM 服务服务器地址。

8. 将 http://pamsrv.priv.contoso.local:82  设置为 SharePoint 网站集 URL。

9. 将注册门户 URL 留空。

10. 选中相应的复选框以打开防火墙中的端口 5725 和 5726，以及选中相应的复选框以授予所有经过身份验证的用户访问 MIM 门户站点的权限。

11. 将 PAM REST API 主机名留空，并将 8086 指定为端口号。 

    ![PAM REST API 的绑定信息 - 屏幕截图](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. 将 MIM PAM REST API 帐户配置为使用和 SharePoint 一样的帐户（因为 MIM 门户也位于该服务器上）：
    - 应用程序池帐户名称：*SharePoint*  
    - 应用程序池帐户密码：<em>Pass@word1</em>（或者步骤 2 中创建的密码）  
    - 应用程序池帐户域：PRIV   

    ![应用程序池帐户凭据 - 屏幕截图](./media/PAM_GS_Configure_Component_Service.png)

    可能会出现服务帐户在其当前配置中并不安全的警告。 这没有关系。

13. 配置 MIM PAM 组件服务：
    - 服务帐户名称：MIMComponent 
    - 服务帐户密码：<em>Pass@word1</em>（或者步骤 2 中创建的密码）  
    - 服务帐户域：PRIV 

    ![PAM 组件服务帐户凭据 - 屏幕截图](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. 配置 PAM 监视服务：
    - 服务帐户名称：MIMMonitor   
    - 服务帐户密码：<em>Pass@word1</em>（或者步骤 2 中创建的密码）  
    - 服务帐户域：PRIV   

    ![PAM 监视服务帐户凭据 - 屏幕截图](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. 在“输入 MIM 密码门户信息”页面上，将复选框留空并继续。 单击“下一步”  以继续安装。

安装完成后，将重新启动服务器，再验证 MIM 门户是否处于活动状态，并使用户能够查看其在 MIM 中的对象资源。

## <a name="set-up-mim-portal-management-policy-rules"></a>设置 MIM 门户管理策略规则

1. 重启 PAMSRV 后，以 PRIV\Administrator 身份登录。

2. 启动 Internet Explorer 并连接到 MIM 门户 http://pamsrv.priv.contoso.local:82/identitymanagement 。 首次打开此页面时可能会延迟片刻。

3. 如有必要，在 Internet Explorer 中以 PRIV\Administrator 身份登录。

4. 在 Internet Explorer 中，打开“Internet 选项”  ，转到“安全”  选项卡，并将该站点添加到“本地 Intranet”区域  （如果尚不存在）。 关闭“Internet 选项”对话框。

5. 使用 Internet Explorer 查看 MIM 门户，单击“管理策略规则”  。

6. 搜索管理策略规则“用户管理：  用户可以读取自己的属性”。

7. 选择此管理策略规则，取消选中“策略已禁用”  ，单击“确定”  ，然后单击“提交”  。

## <a name="verify-the-firewall-connections"></a>确认防火墙连接

防火墙应允许到 TCP 端口 5725、5726、8086 和 8090 的传入连接。

1.  启动“高级安全 Windows 防火墙”  （位于“管理工具”中）。  
2.  单击“入站规则”  。  
3.  确认列出了以下两个规则：  
    - Forefront Identity Manager 服务 (STS)
    - Forefront Identity Manager 服务 (Webservice)  
4.  单击“新规则” > “端口” > “TCP”，然后键入特定本地端口 8086 和 8090。      在向导中连续单击，接受默认设置，并为规则命名，然后单击“完成”  。  
5.  完成向导后，关闭“Windows 防火墙”应用程序。

6.  启动“控制面板”  。  
7.  在“网络和 Internet”下面选择“查看网络状态和任务”。   
8.  验证是否有活动网络（作为 priv.contoso.local 列出）和域网络。  
9. 关闭“控制面板”  。

## <a name="set-up-the-sample-web-application"></a>设置示例 Web 应用程序

在本节中你将为 MIM PAM REST API 安装和配置示例 Web 应用程序。

1. 从示例 web 应用程序存档中以 zip 文件形式下载[身份管理示例](https://github.com/Azure/identity-management-samples)。

2. 将 **identity-management-samples-master\Privileged-Access-Management-Portal\src** 文件夹中的内容解压缩到新文件夹 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal** 中。

3. 在 IIS 中创建一个新网站，站点名称为“MIM Privileged Access Management 示例门户”，物理路径为 C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal，端口为 8090。  此操作可使用以下 PowerShell 命令实现：

   ```PowerShell
   New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
   ```

4. 设置示例 Web 应用程序，以便将用户重定向至 MIM PAM REST API。 使用文本编辑器（如记事本）编辑文件 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management REST API\web.config**。在 **<system.webServer>** 部分中，添加以下行：

   ```XML
   <httpProtocol>
   <customHeaders>
     <add name="Access-Control-Allow-Credentials" value="true"  />
     <add name="Access-Control-Allow-Headers" value="content-type" />
     <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
   </customHeaders>
   </httpProtocol>
   ```

5. 配置示例 Web 应用程序。 使用文本编辑器（如记事本）编辑文件 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js**。 将  pamRespApiUrl 的值设置为  http://pamsrv.priv.contoso.local:8086/api/pamresources/ 。

6. 使用以下命令重启 IIS，以便使这些更改生效。

   ```cmd
   iisreset
   ```

7. （可选）验证用户是否可以对 REST API 进行身份验证。 在 PAMSRV 上，以管理员身份打开 Web 浏览器。  导航到网站 URL http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/ ，根据需要进行身份验证，并确保可进行下载。

## <a name="install-the-mim-pam-requestor-cmdlets"></a>安装 MIM PAM 请求程序 cmdlet

在步骤 1 中配置的工作站上安装 MIM PAM 请求程序 cmdlet。

1.  以管理员身份登录到 CORPWKSTN。

2.  将**加载项和扩展**下载到 CORPWKSTN 计算机（如果尚不存在）。

3.  将存档从文件夹 **加载项和扩展** 中解压缩至新文件夹。

4.  运行安装程序 **setup.exe**。

5.  在自定义安装中，指定要安装 **PAM 客户端**，而不是 **Outlook MIM 加载项**或 **MIM 密码和身份验证扩展**。

6.  在 PAM 服务器地址上，指定 pamsrv.priv.contoso.local  为 PRIV MIM 服务器主机名。

安装完成后，重新启动 CORPWKSTN 以完成注册新的 PowerShell 模块。

在下一步中你将在 PRIV 和 CORP 林之间建立信任关系。

> [!div class="step-by-step"]
> [« 步骤 3](step-3-prepare-pam-server.md)
> [步骤 5 »](step-5-establish-trust-between-priv-corp-forests.md)
