---
title: "部署密码更改通知服务 | Microsoft Docs"
description: "获取在域控制器上安装和配置 MIM 密码更改通知服务的步骤。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d7f054d8d82dcc0ac71a94f6e44407b0c41a75af
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="deploy-the-mim-password-change-notification-service-on-a-domain-controller"></a>在域控制器上部署 MIM 密码更改通知服务

## <a name="install-the-password-change-notification-service"></a>安装密码更改通知服务
密码更改通知服务 (PCNS) 是安装在域控制器上的一项服务，它允许 MIM 将密码同步到其他系统（例如另一个供应商的目录服务器）。 若需要密码同步，请在每一台域控制器服务器上安装 PCNS。

1.  以域管理员身份登录到运行在附带 Active Directory 域服务角色的 Windows Server 上的服务器。

2.  将密码更改通知服务安装程序文件夹复制到计算机。

3.  找到 *Password Change Notification Service.msi* 文件，右键单击它，并创建一个快捷方式。

4.  找到该快捷方式文件，右键单击并打开其“属性” 。

5.  在目标字段中，将该前导码 *msiexec.exe /i* 添加到 msi 文件路径之前，并将后缀 *SCHEMAONLY=TRUE* 添加到 msi 路径之后。 例如，如果安装程序文件夹是 *C:\PCNS*，则要运行的命令将如下所示：（都在同一行）。

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  将更改保存到快捷方式文件中。

7.  运行快捷方式文件以便在架构扩展模式下启动 PCNS 安装。 出现以下屏幕时，单击“下一步” 。

8.  你将收到通知，安装程序马上要开始更新密码更改通知服务的 Active Directory 架构。 单击“确定”  继续进行架构更新。

9. 当架构扩展过程完成后，将出现以下屏幕，单击“完成” 。

10. 再次运行 *Password Change Notification Service.msi* 文件 – 这一次直接运行（不需要运行字符串）。  出现以下屏幕时，单击“下一步” 。

11. 接受许可协议并单击“下一步” 。

12. 单击此项可开始安装。

13. 出现安装成功完成屏幕时，单击“完成” 。

14. 重新启动计算机才会使对 MIM 密码更改通知服务所做的配置更改生效。 可以通过单击弹出窗口中显示的**是**来重启，或稍后重启。

## <a name="configuring-the-password-change-notification-service"></a>配置密码更改通知服务
以域管理员身份重新连接到 DC 服务器后，请转到 *C:\Program Files\Microsoft Password Change Notification。* 运行 *pcnscfg.exe*。
