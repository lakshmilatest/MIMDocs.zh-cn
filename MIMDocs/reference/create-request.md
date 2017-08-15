---
title: "创建请求 | Microsoft Docs"
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
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d5af8767583cb6999de399de58b321147846291a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="create-request"></a>创建请求
创建 MIM CM 请求。

**注意**：本主题中所示的 URL 相对于 API 部署期间选择的主机名，例如： `https://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
POST     |/CertificateManagement/api/v1。0/requests

###<a name="url-parameters"></a>URL 参数
无

###<a name="request-headers"></a>请求标头
有关常见请求头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="request-body"></a>请求正文
请求正文包含下列属性。

属性 | 说明
---------|-----------
profiletemplateuuid | 必须的。 用户为其创建请求的配置文件模板的 GUID。
datacollection | 必须的。 名称-值对的集合，表示由登记者提供的数据。 必须提供的必要数据的集合可以从配置文件模板的工作流策略检索。 可以指定空集。
target | 可选。 要为其创建请求的目标用户的 GUID。 如果未指定，则默认为当前用户。
类型 | 必须的。 正在创建的请求的类型。 可用的请求类型包括：“Enroll”、“Duplicate”、“OfflineUnblock”、“OnlineUpdate”、“Renew”、“Recover”、“RecoverOnBehalf”、“Reinstate”、“Retire”、“Revoke”、“TemporaryCards”和“Unblock”。<br/>**注意**：所有请求类型并非在所有配置文件模板上都受支持。 例如，不能在软件配置文件模板上指定解除阻止操作。
注释 | 必须的。 可能由用户输入的任何注释。 工作流策略将定义这是否是必需的。 可以指定一个空字符串。
priority | 可选。 请求的优先级。 如果未指定，将使用由配置文件模板设置确定的默认请求优先级。


##<a name="response"></a>响应
###<a name="response-codes"></a>响应代码
代码  |说明  
---------|---------
201     | 已创建
403 | 已禁止
500 | 内部错误

###<a name="response-headers"></a>响应标头
有关常见响应头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="response-body"></a>响应正文
成功时，返回新创建的请求的 URI。
##<a name="example"></a>示例

###<a name="request-1"></a>请求 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###<a name="response-1"></a>响应 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###<a name="request-2"></a>请求 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###<a name="response-2"></a>响应 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###<a name="request-3"></a>请求 3
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```
##<a name="see-also"></a>另请参阅

- [Microsoft.Clm.Provision.RequestOperations.InitiateEnroll 方法](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock 方法](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRecover 方法](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRetire 方法](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateUnblock 方法](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
