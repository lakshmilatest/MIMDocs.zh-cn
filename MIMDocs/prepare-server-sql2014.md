---
title: 配置 SQL Server for Microsoft Identity Manager 2016 | Microsoft Docs
description: 安装 SQL Server 2014，为 MIM 2016 的安装做准备。
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8a33e09719b8c806de43531d12ea4b65b5cb443a
ms.sourcegitcommit: 637988684768c994398b5725eb142e16e4b03bb3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-an-identity-management-server-sql-server-2014"></a>设置标识管理服务器：SQL Server 2014

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> 本演练使用名为 Contoso 的公司中的示例名和值。 将其替换为你自己的。 例如：
> - 域控制器名称 - **mimservername**
> - 域名 - **contoso**
> - 密码 - **Pass@word1**

## <a name="install-sql-server-2014-standard-edition"></a>安装 **SQL Server 2014 标准版**。

1. 以域管理员身份启动 **PowerShell** 。

2. 更改为 SQL Server 安装程序所在的目录。

3. 键入下列命令。

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)
