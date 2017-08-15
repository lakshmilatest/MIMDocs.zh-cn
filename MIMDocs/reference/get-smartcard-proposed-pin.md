---
title: "获取智能卡建议的 PIN | Microsoft Docs"
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
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 08d28819402dd56f996d39aa03b8ac829bef0838
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-proposed-pin"></a>获取智能卡建议的 PIN
获取服务器生成的用户 PIN。

**注意**：仅当配置文件模板策略指示应执行此操作时，服务器才会设置 PIN。 否则，用户应提供 PIN。

**注意**：本主题中所示的 URL 相对于 API 部署期间选择的主机名，例如： `https://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
GET     |/CertificateManagement/api/v1。0/smartcards/{ID}/serverproposedpHTTP 请求头和响应头

###<a name="url-parameters"></a>URL 参数
参数 | 说明
---------|------------
ID | 智能卡标识符（特定于 MIM CM）。 从 Microsft.Clm.Shared.Smartcard 对象中获取。
###<a name="query-parameters"></a>查询参数
参数 | 说明
---------|------------
atr | 卡属性。
cardID | 卡 ID。
challenge | 表示智能卡发出的质询的 Base 64 编码字符串。

###<a name="request-headers"></a>请求标头
有关常见请求头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="request-body"></a>请求正文
无

##<a name="response"></a>响应
###<a name="response-codes"></a>响应代码
代码  |说明  
---------|---------
200     | 确定
204 | 无内容
403 | 已禁止
500 | 内部错误

###<a name="response-headers"></a>响应标头
有关常见响应头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="response-body"></a>响应正文
成功时将返回表示由服务器提出的 PIN 的字符串。

##<a name="example"></a>示例

###<a name="request"></a>请求
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###<a name="response"></a>响应
```
HTTP/1.1 200 OK

... body coming soon
```       
