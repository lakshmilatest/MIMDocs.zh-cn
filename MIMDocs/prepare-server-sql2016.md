---
title: 配置 SQL Server for Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: 安装 SQL Server 2016，为 MIM 2016 的安装做准备。
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d32638bda8fd757233af0c697ea3d1ac9eb47eb9
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701375"
---
# <a name="set-up-an-identity-management-server-sql-server-2016"></a>设置身份管理服务器：SQL Server 2016

> [!div class="step-by-step"]
> [« Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
> 
> [!NOTE]
> 本演练使用名为 Contoso 的公司中的示例名和值。 将其替换为你自己的。 例如：
> - 域控制器名称 - corpdc 
> - 域名 - **contoso**
> - MIM 服务服务器名称 - corpservice 
> - MIM 同步服务器名称 - corpsync 
> - SQL Server 名称 - corpsql 
> - 密码 - <strong>Pass@word1</strong>

## <a name="install-sql-server-2016-standardenterprise-edition"></a>安装 SQL Server 2016 标准版/企业版 

1. 以域管理员身份启动 **PowerShell** 。

2. 更改为 SQL Server 安装程序所在的目录。

3. 键入下列命令。

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```
    
有关 SQL 部署帐户和服务的详细信息，可参阅[此处](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)
> [!NOTE]
> SSMS 不再包含在 SQL 2016 中。 可在[此处](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)查看下载详细信息
> 
> [!div class="step-by-step"]  
> [« Windows Server 2016](prepare-server-ws2016.md)
> [SharePoint »](prepare-server-sharepoint.md)
