---
title: "将智能卡分配给请求 | Microsoft Docs"
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
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2886186ade7058b034565501e200ab3773b0af31
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="assign-smart-card-to-a-request"></a>将智能卡分配给请求
将指定的智能卡绑定到指定的请求。 绑定后，该请求只能使用此智能卡执行。

**注意**：本主题中所示的 URL 相对于 API 部署期间选择的主机名，例如： `https://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

###<a name="url-parameters"></a>URL 参数
无。
###<a name="request-headers"></a>请求标头
有关常见请求头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="request-body"></a>请求正文
请求正文包含下列属性。

属性 | 说明
---------|-----------
requestid | 智能卡应绑定到的请求的 ID。
cardID | 智能卡的 cardid。
atr | 智能卡复位应答 (ATR) 字符串。


##<a name="response"></a>响应
###<a name="response-codes"></a>响应代码
代码  |说明  
---------|---------
201     | 创建时间
204 | 无内容
403 | 已禁止
500 | 内部错误

###<a name="response-headers"></a>响应标头
有关常见响应头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="response-body"></a>响应正文
如果成功，将返回新建的智能卡对象的 URI。
##<a name="example"></a>示例

###<a name="request"></a>请求
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###<a name="response"></a>响应
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##<a name="see-also"></a>另请参阅

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard (String, String, Request) 方法](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
