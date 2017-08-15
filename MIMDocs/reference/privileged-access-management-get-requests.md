---
title: "获取 PAM 请求 | Microsoft Docs"
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
ms.assetid: 620eebd6-e4c3-473b-b824-ee6cfe83e509
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00467b6443d3e6e0511ee1817a945ffe569b99b0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-requests"></a>获取 PAM 请求
由特权帐户用于返回以前发布的 PAM 请求的历史记录。

**注意**：本主题中所示 URL 相对于 API 部署期间选择的主机名，例如： `http://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
GET     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>查询参数
参数 | 说明
----------|--------------
$filter | 可选。 在筛选器表达式中指定任何 PAM 请求属性以返回经筛选的响应列表。 若要详细了解支持的运算符，请参阅[“PAM REST API 服务详细信息”中的“筛选”](privileged-access-management-rest-api-service-details.md#filtering)
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
成功的响应包含带有以下属性的 PAM 请求对象列表。

属性 | 说明
--------|-------------
请求ID | PAM 请求的唯一标识符 (GUID)。
CreatorID | 创建了 PAM 请求的 Active Directory 帐户的唯一标识符 (GUID)。
Justification | 提升原因。
DisplayName | 在 MIM 中显示的 PAM 请求显示名。
CreationTime | 创建请求的时间。
Creation方法 | 用于创建请求的方法。
ExpirationTime | 请求的过期时间。
RoleID| PAM 角色的唯一标识符 (GUID)。
请求edTTL | 请求的过期超时（以秒为单位）。
RequestedTime | 提升的请求时间。
请求edStatus | 请求的状态。 可能的值为：“活动”、“已关闭”、“正在关闭”、“已过期”、“等待批准”和“已拒绝”。

##<a name="example"></a>示例

###<a name="request"></a>请求
```
GET /api/pamresources/pamrequests?$filter=CreationTime%20gt%20datetime'2015-06-12T04:49:32.431Z'%20and%20CreationTime%20lt%20datetime'2015-07-13T04:49:32.432Z' HTTP/1.1
```

###<a name="response"></a>响应
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests",
    "value":[
        {
            "RequestId":"b22e1343-9a2b-4e33-a70a-1bb7b2d405b9",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-06-23T11:34:38.58Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-06-23T12:34:38.847Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-23T11:34:36.417Z",
            "RequestStatus":"Expired"
        },
        {
            "RequestId":"3a98d1c7-524d-4b72-9da7-bd9f907eab55",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Reason for Request",
            "CreationTime":"2015-07-12T04:35:14.433Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T04:43:51.95Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:35:00Z",
            "RequestStatus":"Closed"
        },
        {
            "RequestId":"f5e80be1-e9a3-42c4-81f8-4be5a4a429f4",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-07-12T04:48:17.46Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T05:48:17.853Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-12T04:48:14.057Z",
            "RequestStatus":"Active"
        },
        {
            "RequestId":"b0f0ddc0-c809-4770-9d39-0d706f97a2de",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"",
            "CreationTime":"2015-06-30T07:01:13.147Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-30T07:01:13.119Z",
            "RequestStatus":"Rejected"
        },
        {
            "RequestId":"5ec10e61-cdd1-404e-a18e-740467d87dbf",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Example Reason",
            "CreationTime":"2015-07-12T04:49:09.963Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:50:00Z",
            "RequestStatus":"PendingApproval"
        }
    ]
}
```       
