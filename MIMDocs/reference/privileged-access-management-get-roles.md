---
title: "获取 PAM 角色 |Microsoft Docs"
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
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4cb4bbc6c7696f354e5759a677ece88a4e606544
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-roles"></a>获取 PAM 角色
特权帐户用以列出其是候选身份的 PAM 角色。

**注意**：本主题中所示 URL 相对于 API 部署期间选择的主机名，例如： `http://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
GET     |/api/pamresources/pamroles

###<a name="query-parameters"></a>查询参数
参数 | 说明
----------|--------------
$filter | 可选。 在筛选器表达式中指定任意 PAM 角色属性，以返回已筛选的响应列表。 若要详细了解支持的运算符，请参阅[“PAM REST API 服务详细信息”中的“筛选”](privileged-access-management-rest-api-service-details.md#filtering)
v | 可选。 API 版本。 如果未包括在内，将使用当前（最近发布）版本的 API。 有关详细信息，请参阅[“PAM REST API 服务详细信息”中的“版本控制”](privileged-access-management-rest-api-service-details.md#versioning)
###<a name="request-headers"></a>请求标头
有关常见请求头，请参阅“PAM REST API 服务详细信息”中的 [HTTP 请求头和响应头](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="request-body"></a>请求正文
无

##<a name="response"></a>响应
###<a name="response-codes"></a>响应代码
代码  |说明  
---------|---------
200 | “确定”
401 | 未经授权
403 | 已禁止
408 | 请求超时   
500 | 内部服务器错误
503 | 服务不可用

###<a name="response-headers"></a>响应头
有关常见响应头，请参阅“PAM REST API 服务详细信息”中的 [HTTP 请求头和响应头](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="response-body"></a>响应正文
成功的响应包含一个或多个 PAM 角色集合，其中每个集合都具有以下属性。

属性 | 说明
--------|-------------
RoleID | PAM 角色的唯一标识符 (GUID)。
DisplayName | PAM 角色在 MIM 服务中的显示名称。
说明 | PAM 角色在 MIM 服务中的说明。
TTL | 角色的访问权限最大到期超时值（以秒为单位）。
AvailableFrom | 一天中激活请求的最早时间。
AvailableTo | 一天中激活请求的最晚时间。
MFAEnabled | 指示此角色的激活请求是否需要 MFA 质询的布尔值。
ApprovalEnabled | 指示此角色的激活请求是否需要由角色所有者批准的布尔值。
AvailibitlyWHTTP 请求头和响应头dowEnabled | 指示角色是否只能在指定时间间隔期间激活的布尔值。

##<a name="example"></a>示例

###<a name="request"></a>请求
```
GET /api/pamresources/pamroles HTTP/1.1
```
###<a name="response"></a>响应
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
