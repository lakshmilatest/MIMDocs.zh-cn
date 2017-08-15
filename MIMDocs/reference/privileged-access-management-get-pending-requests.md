---
title: "获取挂起的 PAM 请求 | Microsoft Docs"
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
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aa1e8cd48b1bcca6e856bd553b6b92a062a08ff4
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="get-pending-pam-requests"></a>获取挂起的 PAM 请求
由特权帐户用户用于返回需要批准的挂起请求的列表。

**注意**：本主题中所示 URL 相对于 API 部署期间选择的主机名，例如： `http://api.contoso.com`。
##<a name="request"></a>请求

方法  |请求 URL  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

###<a name="query-parameters"></a>查询参数
参数 | 说明
----------|--------------
$filter | 可选。 指定筛选器表达式中的任何挂起的 PAM 请求属性返回经筛选的响应列表。 若要详细了解支持的运算符，请参阅[“PAM REST API 服务详细信息”中的“筛选”](privileged-access-management-rest-api-service-details.md#filtering)
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
成功的响应包含带有以下属性的 PAM 请求批准对象列表。

属性 | 说明
---------|-------------
RoleName | 需要批准的角色的显示名称。
请求or | 要批准的请求者的用户名。
Justification | 用户提供的理由。
RequestedTTL | 请求的过期时间（以秒为单位）。
请求edTime | 请求的提升时间。
CreationTime | 请求的创建时间。
FIM请求ID | 包含单个元素“Value”，并附带 PAM 请求的唯一标识符 (GUID)。
请求orID | 包含单个元素“Value”，并附带创建了 PAM 请求的 Active Directory 帐户的唯一标识符 (GUID)。
ApprovalObjectID | 包含单个元素“Value”，并附带批准对象的唯一标识符 (GUID)。

##<a name="example"></a>示例

###<a name="request"></a>请求
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###<a name="response"></a>响应
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
