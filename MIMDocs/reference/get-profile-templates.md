---
title: "获取配置文件模板 | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8736b328926445f6434bd037a1ecf2e5de9ee466
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-templates"></a>获取配置文件模板
获取指定用户可以注册的配置文件模板列表。 此方法将返回配置文件模板的有限视图。 返回的配置文件模板数据应足以使请求的用户可以决定他们需要注册哪些配置文件模板（如有）。 如果未指定任何工作流和权限，将返回用户可见的所有配置文件模板。

**注意**：本主题中所示的 URL 相对于 API 部署期间选择的主机名，例如： `https://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[targetuser\] 

###<a name="url-parameters"></a>URL 参数
参数| 说明
--------|-------------
targetuser| 可选。 指定返回配置文件模板的目标用户。 如果未指定，将使用当前用户的标识。 <br/>**注意**：目前仅支持当前用户。

###<a name="request-headers"></a>请求标头
请参阅针对公共请求头的服务主题
###<a name="request-body"></a>请求正文
无

##<a name="response"></a>响应
###<a name="response-codes"></a>响应代码
代码  |说明  
---------|---------
200     | 确定
204 | 无内容
500 | 内部错误

###<a name="response-headers"></a>响应标头
请参阅针对公共响应头的服务主题。
###<a name="response-body"></a>响应正文
在成功时将返回带有以下属性的 ProfileTemplateLimitedView 对象列表。

属性| 类型| 说明
--------|-----|--------
名称| 字符串| 配置文件模板的显示名称。
说明| 字符串| 配置文件模板说明。
Uuid| Guid| 配置文件模板的标识符。
IsSmartcardProfileTemplate| 布尔| 指示模板是否为智能卡配置文件模板。
IsVirtualSmartcardProfileTemplate| 布尔| 指示配置文件模板是否需要虚拟智能卡。

##<a name="example"></a>示例

###<a name="request"></a>请求
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###<a name="response"></a>响应
```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]

```       
