---
title: "创建 PAM 请求 | Microsoft Docs"
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
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de28af5c49eb98c5a1ccfbd8ed9353cf202e9e66
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="create-pam-request"></a>创建 PAM 请求
特权帐户用以提升至 PAM 角色。

**注意**：本主题中所示 URL 相对于 API 部署期间选择的主机名，例如： `http://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
POST     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>查询参数
参数 | 说明
--------|-------------
Justification | 可选。 用户提供的提升请求原因。
RoleId| 必须的。 提升到的 PAM 角色唯一标识符 (GUID)。
RequestedTTL| 必需。 请求的过期时间（以秒为单位）。
请求edTime | 可选。 提升特权的时间。  
v | 可选。 API 版本。 如果未包括在内，将使用当前（最近发布）版本的 API。 有关详细信息，请参阅[“PAM REST API 服务详细信息”中的“版本控制”](privileged-access-management-rest-api-service-details.md#versioning)

**注意**：可以在请求正文中指定 *Justification*、 *RoleId*、 *RequestedTTL*和 *RequestedTime* 参数作为属性而非作为查询参数。 *V* 参数仅能指定为查询参数。

###<a name="request-headers"></a>请求标头
有关常见请求头，请参阅“PAM REST API 服务详细信息”中的 [HTTP 请求头和响应头](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="request-body"></a>请求正文
可选。 如上所述， *Justification*、 *RoleId*、 *RequestedTTL*和 *RequestedTime* 参数可指定为请求正文的属性，而非在 URL 查询字符串中指定。

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
成功的响应包含具有以下属性的 PAM 请求对象。

属性 | 说明
--------|-------------
请求ID | PAM 请求的唯一标识符 (GUID)。
CreatorID | 创建请求的帐户 MIM 服务中的唯一标识符 (GUID)。
Justification | 提升原因。
CreationTime | 创建请求的时间。
Creation方法 | 用于创建请求的方法。
ExpirationTime | 请求的过期时间。
RoleID| PAM 角色的唯一标识符 (GUID)。
请求edTTL | 请求的过期超时（以秒为单位）。
RequestedTime | 提升的请求时间。
RequestStatus | 请求的状态。 可取值为：“Processing”、“Active”、“Closed”、“Closing”、“Expired”、“PendingApproval”、“PendingMFA”和“Rejected”。

##<a name="example"></a>示例

###<a name="request-1"></a>请求 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###<a name="response-1"></a>响应 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###<a name="request-2"></a>请求 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###<a name="response-2"></a>响应 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
