---
title: 配置 SQL Server for Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: 安装 SQL Server 2016，为 MIM 2016 的安装做准备。
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e2006ca2a74f8c974f6019004aeaefdc73069fa6
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-an-identity-management-server-sql-server-2016"></a>设置标识管理服务器：SQL Server 2016

>[!div class="step-by-step"]
[« Windows Server 2016](prepare-server-ws2016.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> 本演练使用名为 Contoso 的公司中的示例名和值。 将其替换为你自己的。 例如：
> - 域控制器名称 - corpdc
> - 域名 - **contoso**
> - MIM 服务服务器名称 - corpservice
> - MIM 同步服务器名称 - corpsync
> - SQL Server 名称 - corpsql
> - 密码 - **Pass@word1**

## <a name="install-sql-server-2016-standardenterprise-edition"></a>安装 SQL Server 2016 标准版/企业版

1. 以域管理员身份启动 **PowerShell** 。

2. 更改为 SQL Server 安装程序所在的目录。

3. 键入下列命令。

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"

More info SQL deployment accounts and services can be found [here](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)
> [!NOTE]
> SSMS is no longer included in SQL 2016 downlaod details can be found [here](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)    ```

>[!div class="step-by-step"]  
[« Windows Server 2016](prepare-server-ws2016.md)
[SharePoint »](prepare-server-sharepoint.md)