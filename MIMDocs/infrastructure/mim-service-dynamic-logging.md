---
title: MIM 服务动态日志记录 | Microsoft Docs
description: 启用 MIM 服务动态日志记录，但无需重启管理服务
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 06/25/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ''
ms.openlocfilehash: ff82b2fce31abe417509347ce7b477dd1b4056f2
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332331"
---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>MIM SP1 (4.4.1436.0) 服务动态日志记录
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

日志记录位置将默认位于 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service，且 FIM 服务帐户将需要此位置的写入权限才可生成动态日志。

![日志的文件夹位置](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  如果出现意外错误（配置文件 Microsoft.ResourceManagement.Service.exe.config 中的语法错误或其他错误），相应错误消息将写入 %TMP%、%TEMP% 或 %USERPROFILE% 路径（第一个存在的）下的文件 Microsoft.ResourceManagement.Service.exe_Emergency.log。  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

若要查看跟踪，可使用[服务跟踪查看器工具](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![服务跟踪查看器屏幕截图](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>更新：内部版本 4.5.x.x 或更高版本

在内部版本 4.5.x.x 中，我们修订了日志记录功能，从而将默认日志记录级别指定为“警告”。 该服务将消息写入到两个文件中（在扩展名之前添加“00”和“01”索引）。 文件位于“C:\Program Files\Microsoft Forefront Identity Manager\2010\Service”目录内。 当文件超过最大限制时，服务开始写入另一个文件。 如果存在另一个文件，它将被覆盖。 默认文件最大为 1 GB。 要更改默认最大大小，必须将带有最大文件大小 (KB) 值的“maxOutputFileSizeKB”参数添加到侦听器中（请参阅下面的示例）并重启 MIM 服务。 服务启动后，它会将日志附加到更新的文件中（如果超出空间限制，则会覆盖最旧的文件）。 

> [!NOTE] 服务会在写入消息之前检查文件大小，因此文件大小可能大于某个消息大小的最大限制。 默认情况下，日志的大小约为 6 GB（3 个带有两个 1 GB 大小的文件的侦听器）。

> [!NOTE] 服务帐户应具有写入“C:\Program Files\Microsoft Forefront Identity Manager\2010\Service”目录的权限。 如果服务帐户没有此类权限，则不会创建文件。

有关如何将 svclog 文件的最大文件大小设置为 200 MB (200 * 1024 KB)，将 txt 文件的最大文件大小设置为 100 MB *(100 * 1024 KB) 的示例

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
