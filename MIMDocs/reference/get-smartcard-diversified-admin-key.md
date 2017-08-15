---
title: "获取智能卡多样化管理密钥 | Microsoft Docs"
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
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 57a8481b2a4f976717b07061e96ccb041164a5a6
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-diversified-admin-key"></a>获取智能卡多样化管理密钥
获取指定智能卡的多样化管理密钥。

**注意**：管理密钥仅在配置文件模板策略指示其应多样化时多样化。

**注意**：本主题中所示的 URL 相对于 API 部署期间选择的主机名，例如： `https://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
GET     |/CertificateManagement/api/v1。0/requests/{reqid}/smartcards/{scid}/diversifiedkey

###<a name="url-parameters"></a>URL 参数
参数 | 说明
---------|------------
reqid | 必需。 请求标识符（特定于 MIM CM）。
scid | 必须的。 智能卡标识符（特定于 MIM CM）。 获取自 [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) 对象。
###<a name="query-parameters"></a>查询参数
参数 | 说明
---------|------------
atr | 可选。 智能卡应答到重置 (ATR) 字符串。
cardID | 必需。 卡 ID。

###<a name="request-headers"></a>请求头
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
成功时，返回一个表示多样化管理密钥的字节 BLOB。

##<a name="example"></a>示例

###<a name="request"></a>请求
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```
###<a name="response"></a>响应
```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       
##<a name="see-also"></a>另请参阅

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard(String, String, Request) 方法](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
