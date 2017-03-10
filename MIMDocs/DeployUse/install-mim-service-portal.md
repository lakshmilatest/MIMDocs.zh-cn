---
title: "安装 Microsoft Identity Manager 服务和门户 | Microsoft Docs"
description: "获取为 Microsoft 标识管理器 2016 配置和安装 MIM 服务和门户的步骤"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 54d03fbd03f6c44298139324ea2dc7d945f008bc
ms.openlocfilehash: eb572bc3f062763265a9f24910a82be0cac77a5e


---

# <a name="install-mim-2016-mim-service-and-portal"></a>安装 MIM 2016：MIM 服务和门户

>[!div class="step-by-step"]
[« MIM 同步服务](install-mim-sync.md)
[同步数据库 »](install-mim-sync-ad-service.md)

> [!NOTE]
> 本演练使用名为 Contoso 的公司中的示例名和值。 将其替换为你自己的。 例如：
> - 域控制器名称 - **mimservername**
> - 域名 - **contoso**
> - 密码 - **Pass@word1**
> - 服务帐户名称 - **MIMService**

如果你在最后一步中没有设置 MIM 安装包，请先返回并安装 Microsoft Identity Manager 2016 组件，才继续操作。


## <a name="configure-mim-service-and-portal-for-installation"></a>配置 MIM 服务和门户以便安装

1. 从解压缩的“服务和门户”子文件夹运行“MIM 服务和门户安装程序”。

2. 在欢迎屏幕中，单击“下一步” 。

3. 阅读最终用户许可协议；如果接受许可条款，请单击**下一步**。

4. 在“MIM 客户体验改善计划”屏幕中，单击“下一步”。

5. 对于此部署，在选择组件功能时，请确保包括 MIM 服务（MIM 报告除外）和 MIM 门户功能。 还可以选择 MIM 密码重置门户和 MIM 密码更改通知服务。

6. 在**配置 MIM 数据库连接**页上，选择**创建新数据库**。

    ![配置 MIM 数据库连接图像](media/MIM-Install10.png)

7. 在“配置邮件服务器连接”上，输入作为“邮件服务器”的 Exchange 服务器的名称。 如果未配置邮件服务器，则将**localhost**用作邮件服务器名称，并取消选中顶部的两个复选框。 单击“下一步” 。

    ![配置邮件服务器连接图像](media/MIM-Install11.png)

8. 指定你想要生成新的自签名证书，或选择相关的证书。

9. 指定要使用的服务帐户名称（例如 *MIMService*）、服务帐户密码（例如 *Pass@word1*）、服务帐户域（例如 *contoso*）以及服务电子邮件帐户（例如 *contoso*）。

    ![配置 MIM 服务帐户图像](media/MIM-Install12.png)

10. 请注意，可能会出现如下警告：服务帐户在其当前配置中并不安全。

11. 接受同步服务器位置的默认设置，并将 MIM 管理代理帐户指定为 *contoso\MIMsync*。

    ![配置 MIM 服务和门户图像](media/MIM-Install13.png)

12. 将 *CORPIDM*（此计算机的名称）指定为 MIM 门户的 MIM 服务服务器地址。

13. 将 *http://CorpIDM.contoso.local:82* 指定为 SharePoint 站点集 URL。

14. 将 *http://CorpIDM.contoso.local:8080* 指定为密码注册 URL。

15. 将 *http://CorpIDM.contoso.local:8088* 指定为密码重置 URL。

16. 选中打开防火墙中的端口 5725 和 5726 的复选框，以及授予所有经过身份验证的用户访问 MIM 门户权限的复选框。

## <a name="configure-mim-password-registration-portal"></a>配置 MIM 密码注册门户

1.  将 SSPR 注册的服务帐户名设为 *contoso\MIMSSPR*，并将其密码设为 *Pass@word1*。

2.  将 *CORPIDM* 指定为 MIM 密码注册的主机名称，并将该端口设置为 **8080**。 启用“打开防火墙中的端口”选项。

    ![输入 IIS 映像使用的配置信息](media/MIM-Install14.png)

3.  将显示警告 – 阅读该警告，然后单击“下一步” 。

4. 在下一个 MIM 密码注册门户配置屏幕中，将 *http://CorpIDM.contoso.local* 指定为密码注册门户的 MIM 服务服务器地址。

## <a name="configure-mim-password-reset-portal"></a>配置 FIM 密码重置门户

1.  将 SSPR 注册的服务帐户名设为 *Contoso\MIMSSPRService*，并将其密码设为 *Pass@word1*。

2.  将 *CORPIDM* 指定为 MIM 密码重置门户的主机名称，并将该端口设置为 **8088**。 启用“打开防火墙中的端口”选项。

    ![输入 IIS 映像使用的配置信息](media/MIM-Install15.png)

3.  将显示警告 – 阅读该警告，然后单击“下一步” 。

4. 在下一个 MIM 密码注册门户配置屏幕中，将 *CorpIDname  http://CorpIDname.domain.local* 指定为密码重置门户的 MIM 服务服务器地址。

## <a name="install-mim-service-and-portal"></a>安装 MIM 服务和门户

所有预安装定义准备就绪后，单击“安装”以开始安装所选的“服务和门户”组件。

安装完成后，验证 MIM 门户是否处于活动状态。

1. 启动 Internet Explorer 并连接到位于 *http://corpidm.contoso.local:82/identitymanagement* 上的 MIM 门户。 请注意，第一次访问此页可能有一个短暂的延迟。

    - 如有必要，以 *contoso\Administrator* 身份向 Internet Explorer 进行身份验证。

2. 在 Internet Explorer 中，打开“Internet 选项” ，更改为“安全”  选项卡，并将该站点添加到“本地 Intranet”  区域（如果尚不存在）。  关闭“Internet 选项”  对话框。

3. 使用户能够在 MIM 中查看他们自己的条目。

    1.  使用 Internet Explorer，在“MIM 门户” 中，单击“管理策略规则” 。

    2.  搜索管理策略规则，**用户管理：用户可以读取自己的属性**。

    3.  选择此管理策略规则，取消选中“策略已禁用”。

    4.  单击“确定”  ，然后单击“提交” 。

4.  验证防火墙是否允许传入连接到 TCP 端口 5725 和 5726。

    1.  通过“高级安全”启动“管理工具” »“Windows 防火墙”。

    2.  单击“入站规则” 。

    3.  验证是否出现以下两个规则：

        -   Forefront Identity Manager 服务 (STS)。

        -   Forefront Identity Manager 服务 (Webservice)。

    4.  完成向导并关闭“Windows 防火墙”  应用程序。

    5.  在“控制面板”的“网络和 Internet”中启动“查看网络状态和任务”。

    6.  验证是否有活动的网络将 contoso.local 列为域网络。

    7.  关闭“控制面板” 。

> [!NOTE]
> 可选：此时，你可以安装 MIM 外接程序和扩展。

>[!div class="step-by-step"]  
[« MIM 同步服务](install-mim-sync.md)
[同步数据库 »](install-mim-sync-ad-service.md)



<!--HONumber=Jan17_HO4-->


