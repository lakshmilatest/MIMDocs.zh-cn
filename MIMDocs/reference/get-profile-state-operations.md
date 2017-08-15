---
title: "获取配置文件状态操作 | Microsoft Docs"
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
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8e8428984404b2aebda8f53e4f7841b699a5d23a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-state-operations"></a>获取配置文件状态操作
获取指定配置文件上的当前用户可执行的可能操作列表。 然后，可以为任何指定的操作启动请求。

**注意**：本主题中所示 URL 相对于 API 部署期间选择的主机名，例如： `https://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/operations <br/>/CertificateManagement/api/v1.0/smartcards/{id}/operations

###<a name="url-parameters"></a>URL 参数
参数 | 说明
---------|------------
ID | 配置文件或智能卡的标识符 (GUID)。

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
在成功时返回智能卡上的用户可执行的可能操作列表。 此列表可能包含以下任意内容： *OnlineUpdate*、 *续订*、 *恢复*、 *RecoverOnBehalf*、 *停用*、 *撤销*和 *取消阻止*。

##<a name="example"></a>示例

###<a name="request"></a>请求
```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```
###<a name="response"></a>响应
```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
