---
title: Microsoft Identity Manager 2016 SP1 支持的语言 | Microsoft Docs
description: Microsoft Identity Manager 2016 SP1 支持的语言的列表。
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 05/23/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.openlocfilehash: bb0287b894786d13398819b04bdb089f0f36b33e
ms.sourcegitcommit: acb2c61831cb634278acc439d6d9496ff51a6a54
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2018
ms.locfileid: "43694633"
---
# <a name="supported-languages"></a>支持的语言

本文概述了支持的语言以及 Microsoft Identity Manager 2016 SP1 版本 4.5.x 或更高版本的更新映射。

## <a name="mim-service-and-portal-and-add-ins-and-extensions-language-pack"></a>MIM 服务和门户及加载项和扩展语言包 

Microsoft MIM 服务和门户语言包支持以下 33 种语言。  

> [!NOTE]
> 在 [4.4.1642.0](https://support.microsoft.com/en-us/help/4021562/hotfix-rollup-package-build-4-4-1642-0-is-available-for-microsoft) 中，已将名为“OverrideDefaultUILocale”的注册表项添加到 MIM 加载项，扩展语言包会尝试将所有类似语言映射到受支持的语言。 例如，如果 Windows 显示语言为 ES-CL（智利西班牙语），或任何 ES-\*，它将尝试将此项映射到 ES-ES（西班牙的西班牙语）。

> [!IMPORTANT]
> SSPR 加载项和门户中的文本将得到本地化，但问题还需要额外的工作进行处理。 将需要创建 AuthN 工作流（以及随附集和 MPR 来对它们进行定位）以将每种语言的问题定位到目标位置。

|       语言        | FIM(4.3.x.x)/MIM(4.4.xx) | MIM(4.5.x.x) |
|-----------------------|--------------------------|--------------|
|       保加利亚语       |          bg-BG           |      bg      |
| 中文（简体）  |          简体中文           |   zh-hans    |
|   中文（台湾）    |          zh-TW           |   zh-hant    |
|       克罗地亚语        |          hr-HR           |      hr      |
|         捷克语         |          cs-CZ           |      cs      |
|        丹麦语         |          da-DK           |      da      |
|         荷兰语         |          nl-NL           |      nl      |
|       爱沙尼亚语        |          et-EE           |      et      |
|        法语         |          fr-FR           |      fr      |
|        芬兰语        |          fi-FI           |      fi      |
|        德语         |          de-DE           |      de      |
|         希腊语         |          el-GR           |      el      |
|         印地语         |          hi-IN           |      hi      |
|       匈牙利语       |          hu-HU           |      hu      |
|        意大利语        |          it-IT           |      it      |
|       日语        |          ja-JP           |      ja      |
|        朝鲜语         |          ko-KR           |      ko      |
|      立陶宛语       |          lt-LT           |      lt      |
|        拉脱维亚语        |          lv-LV           |      lv      |
|       挪威语       |          nb-NO           |    nb-NO     |
|        波兰语         |          pl-PL           |      pl      |
| 葡萄牙语（葡萄牙） |          pt-PT           |      pt      |
|  葡萄牙语（巴西）  |          pt-BR           |    pt-BR     |
|        俄语        |          ru-RU           |      ru      |
|       罗马尼亚语        |          ro-RO           |      ro      |
|        西班牙语        |          es-ES           |      es      |
|        斯洛伐克语         |          sk-SK           |      sk      |
|        瑞典语        |          sv-SE           |      sv      |
|       斯洛文尼亚语       |          sl-SI           |      sl      |
|   塞尔维亚语 - 塞尔维亚    |  sr-latn-CS(弃用)  |  sr-Latn-RS  |
|         泰语          |          th-TH           |      th      |
|        土耳其语        |          tr-TR           |      tr      |
|       乌克兰语       |          uk-UA           |      uk      |

## <a name="certificate-management"></a>证书管理 
Microsoft 证书管理支持以下 9 种语言。 

|语言|FIM(4.3.x.x)/MIM(4.4.xx)|新的 MIM(4.5.x.x)
|-----|-----|-----|-----|
|中文（简体）|简体中文|zh-hans|
|中文（台湾）|zh-TW|zh-hant|
|荷兰语|nl-NL|nl|
|法语|fr-FR|fr|
|德语|de-DE|de|
|意大利语|it-IT|it|
|日语|ja-JP|ja|
|葡萄牙语（葡萄牙）|pt-PT|pt-PT|
|西班牙语|es-ES|es|

## <a name="certificate-management-modern-application"></a>证书管理现代应用程序  
Microsoft 证书管理现代应用程序支持以下 33 种语言。 

|语言 | [1.0.225.104](https://www.microsoft.com/en-us/download/details.aspx?id=54954) | |
|-----|-----|-----|-----|
|荷兰语|nl-NL|nl|
|中文（简体）|简体中文|zh-hans|
|中文（台湾）|zh-TW|zh-hant|
|捷克语|cs-CZ|cs|
|丹麦语|da-DK|da|
|法语|fr-FR|fr|
|芬兰语|fi-FI|fi|
|德语|de-DE|de|
|希腊语|el-GR|el|
|匈牙利语|hu-HU|hu|
|意大利语|it-IT|it|
|日语|ja-JP|ja|
|朝鲜语|ko-KR|ko|
|挪威语|nb-NO|nb-NO|
|波兰语|pl-PL|pl|
|葡萄牙语（葡萄牙）|pt-PT|pt|
|葡萄牙语（巴西）|pt-BR|pt-BR|
|俄语|ru-RU|ru|
|罗马尼亚语|ro-RO|ro|
|西班牙语|es-ES|es|
|瑞典语|sv-SE|sv|
|土耳其语|tr-TR|tr|

## <a name="next-steps"></a>后续步骤

- [首次部署](microsoft-identity-manager-deploy.md)
- [版本历史记录](/reference/version-history.md)
