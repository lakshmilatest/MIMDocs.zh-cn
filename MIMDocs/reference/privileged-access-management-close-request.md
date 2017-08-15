---
title: "关闭 PAM 请求 | Microsoft Docs"
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
ms.assetid: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 181bf1a2953e461925d1b7efc5da4a2fa0c86914
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="close-pam-request"></a>关闭 PAM 请求
特权帐户用以关闭由其启动提升为 PAM 角色的请求。

**注意**：本主题中所示 URL 相对于 API 部署期间选择的主机名，例如： `http://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
POST     |/api/pamresources/pamrequests({requestId)/Close

###<a name="url-parameters"></a>URL 参数
参数 | 说明
----------|-----------
requestId | 表示要关闭的 PAM 请求的标识符 (GUID)，指定方式如下： `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###<a name="query-parameters"></a>查询参数
参数 | 说明
----------|--------------
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
无
##<a name="example"></a>示例

###<a name="request"></a>请求
```
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1


```
###<a name="response"></a>响应
```
HTTP/1.1 200 OK

```       
