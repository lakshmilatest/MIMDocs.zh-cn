---
title: "自助服务智能卡续订 | Microsoft Identity Manager"
description: "对于不具有自己计算机的管理员访问权限的用户，了解如何为其注册智能卡，以便他们使用证书管理器。"
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: 2fddede481b5ba677d0d463be4b14cda4b463865


---

# 为非管理员注册智能卡
如果用户不是计算机的本地管理员，则默认情况下，他们将无法在自己的计算机上注册智能卡。 以下过程可以让你解决此限制。

## 在 MIM 2016 证书管理器中启用非管理员智能卡续订

1.  **解压缩 appx 文件**

    获取签名证书。 请遵循相应步骤[使用内部 PKI 登录 Windows 8 应用程序](http://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx)。 在转到“签署应用程序”时停止。 命名导出的 pfx 文件。 也将其导出到 .cer 文件，并使用新签名证书的 cer 文件将其导入客户端。

    运行以下内容以解压缩 appx 文件：

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **修改配置文件**

    重命名名称为 `CustomDataExample.xml custom.data`的文件。 CM 应用将查找此文件名。

    编辑 custom.data 文件并修改以下内容：

    1.  在 &lt;NonAdmin&gt; 元素中，将 Value 属性的值更改为“True”

    2.  保存文件并退出编辑器

    3.  删除名为 AppxSignature.p7x 的文件

    4.  编辑名为 AppxManifest.xml 的文件

    5.  在 &lt;Identity&gt; 元素中，将 Publisher 属性的值修改为签名证书的主题，例如：“CN=ABCD”

        此处的主题应与用于签署应用的签名证书中的主题相同。

    6.  保存文件并退出编辑器。

3.  **重新打包并签署应用程序包（appx 文件）**

    运行以下命令以打包和签署 appx 文件：

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    s`igntool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  复制配置文件模板并添加初始管理员密钥，以配置 MIM 服务器：

    1.  作为拥有管理员权限的用户登录到 CM 门户。

    2.  转到“管理”&gt;“管理配置文件模板”并确保已选中所创建的配置文件模板旁的复选框，然后单击“复制选定的配置文件模板”。

    3.  键入配置文件模板的名称、添加“nonAdmin”，然后单击“确定” 。

    4.  在显示配置文件模板常规设置时，一直向下滚动，然后在**智能卡配置**下单击**更改设置**。

    5.  在“管理员密钥初始值（十六进制）”  下输入默认管理员密钥：“010203040506070801020304050607080102030405060708”

    6.  向下滚动，然后单击“确定” 。

5.  **在客户端计算机上创建非管理员帐户**

    非管理员用户无法在 TPM 上创建虚拟智能卡，因此你必须为他们创建一个虚拟智能卡。

6.  **使用 TpmVscMgr 创建虚拟智能卡**

    在计算机上（仍作为管理员）执行以下操作，创建空的虚拟智能卡。 这可以通过 Intune、SCCM 或组策略来完成。

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **在非管理员帐户中安装 CM 应用**

8.  **启动 CM 应用并注册虚拟智能卡**



<!--HONumber=Jul16_HO3-->


