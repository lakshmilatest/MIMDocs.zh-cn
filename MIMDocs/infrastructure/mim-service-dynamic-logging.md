---
title: "MIM 服务动态日志记录 | Microsoft Docs"
description: "启用 MIM 服务动态日志记录，但无需重启管理服务"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 03/24/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
ms.openlocfilehash: 1e2fb9a9ae508ab601ebad1dec7acc21dc44d13e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# MIM SP1 (4.4.1436.0) 服务动态日志记录
<a id="mim-sp1-4414360--service-dynamic-logging" class="xliff"></a>
在 4.4.1436.0 中引入了新的日志记录功能。 这使得管理员和支持工程师无需重启管理服务即可打开日志记录功能。

安装后，将在 Microsoft.ResourceManagement.Service.exe.config 中看到以下新行

*   行 6：``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   行 8：``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   行 266：``</system.diagnostics> ``

![高亮部分显示新的动态日志记录条目](media/mim-service-dynamic-logging/screen01.png)

有关动态日志记录级别，可参见[此处](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3)

- 关键 = 默认级别服务将仅写入关键事件
- 行 8 (dynamicLogging mode="true" loggingLevel="Critical") 更新为首选日志记录值

动态日志记录配置位于行 266：Microsoft.ResourceManagement.Service.exe.config

![高亮部分显示具有各种可用日志记录区域的行](media/mim-service-dynamic-logging/screen02.png)

日志记录位置将默认位于 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**，且 FIM 服务帐户将需要此位置的写入权限才可生成动态日志。

![日志的文件夹位置](media/mim-service-dynamic-logging/screen03.png)

 >[!NOTE]
 如果出现意外错误（配置文件 Microsoft.ResourceManagement.Service.exe.config 中的语法错误或其他错误），相应错误消息将写入 %TMP%、%TEMP% 或 %USERPROFILE% 路径（第一个存在的）下的文件 Microsoft.ResourceManagement.Service.exe_Emergency.log。  
1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

若要查看跟踪，可使用[服务跟踪查看器工具](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![服务跟踪查看器屏幕截图](media/mim-service-dynamic-logging/screen04.png)
