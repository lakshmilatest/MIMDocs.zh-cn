---
title: 安装 Microsoft Identity Manager 同步服务 | Microsoft Docs
description: 通过安装和配置同步服务立刻开始使用 MIM 2016 组件。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/01/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: fba7eb3caea1f00c37f00f3fd2bf67dfe3f12871
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701273"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>安装 MIM 2016：MIM 同步服务

> [!div class="step-by-step"]
> [« Exchange Server](prepare-server-exchange.md)
> [MIM 服务和门户 »](install-mim-service-portal.md)
> 
> [!NOTE]
> 本演练使用名为 Contoso 的公司中的示例名和值。 将其替换为你自己的。 例如：
> - 域控制器名称 - corpdc 
> - 域名 - **contoso**
> - MIM 服务服务器名称 - corpservice 
> - MIM 同步服务器名称 - corpsync 
> - SQL Server 名称 - corpsql 
> - 密码 - <strong>Pass@word1</strong>

要安装 Microsoft Identity Manager 2016 组件，请先设置安装包。

1. 以 contoso\miminstall  身份登录到用于标识管理同步服务器 corpsync  的服务器。

2. 解压缩 MIM 安装包或装载 MIM 映像 DVD。  如果没有此 DVD，请参阅 [Microsoft Identity Manager 授权和下载](microsoft-identity-manager-licensing.md)。

## <a name="install-mim-2016-sp1-synchronization-service"></a>安装 MIM 2016 SP1 同步服务

1. 在已解压缩的 MIM 安装文件夹中，导航到“同步服务”  文件夹。

2. 运行“MIM 同步服务安装程序”  。 按照安装程序的说明操作并完成安装。

3. 在欢迎屏幕上 – 单击“下一步”  。

    ![MIM 安装程序向导欢迎图像](media/install-mim-sync/MIM_Install1.png)

4. 查看许可条款，并单击**下一步**来接受它们。

5. 在“自定义安装”  屏幕上，单击“下一步”  。

    ![自定义安装图像](media/install-mim-sync/MIM_Install2.png)

6. 在同步服务数据库配置屏幕中，选择：

   1.  SQL Server 位置：名为 corpsql.contoso.com  的远程计算机  。

   2.  SQL Server 实例：**默认实例**

   ![数据库连接图像](media/install-mim-sync/MIM_Install3.png)

7. 根据之前创建的帐户配置同步服务帐户：

   1. 服务帐户：*MIMSync*

   2. 密码：<em>Pass@word1</em>

   3. 服务帐户域或本地计算机名称： *contoso*

   ![服务帐户图像](media/install-mim-sync/MIM_Install4.png)

8. 提供具有相关安全组的 MIM 同步服务安装程序：

   1. Administrator = *contoso\MIMSyncAdmins*

   2. Operator= *contoso\MIMSyncOperators*

   3. Joiner = *contoso\MIMSyncJoiners*

   4. Connector Browse = *contoso\MIMSyncBrowse*

   5. WMI Password Management= *contoso\MIMSyncPasswordReset*

   ![安全组图像](media/install-mim-sync/MIM_Install5.png)

9. 在安全设置屏幕中，选中“为入站 RPC 通信启用防火墙规则”  ，然后单击“下一步”  。

10. 单击“安装”  以开始安装 MIM 同步服务。

    1. 可能会出现有关 MIM 同步服务帐户的警告 – 单击“确定”  。

    2. 将安装 MIM 同步服务。

    3. 会出现为加密密钥创建备份的通知 – 单击“确定”  ，然后选择要存储加密密钥备份的文件夹。

        ![MIM 同步备份加密密钥通知图像](media/MIM-Install7.png)

    4. 当安装程序成功完成安装时，请单击“完成”  。

    5. 你需要注销，然后登录，才能使组成员身份更改生效。 单击“是”  以注销。

> [!div class="step-by-step"]  
> [« Exchange Server](prepare-server-exchange.md)
> [MIM 服务和门户 »](install-mim-service-portal.md)
