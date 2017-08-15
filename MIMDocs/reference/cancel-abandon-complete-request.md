---
title: "取消、放弃或完成请求 | Microsoft Docs"
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
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4f81c9d86006c477993b2ac8cc2e845de2c1d2f3
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="cancel-abandon-or-complete-a-request"></a>取消、放弃或完成请求
将 MIM CM 请求标记为已完成、已取消或已放弃。

**注意**：本主题中所示的 URL 相对于 API 部署期间选择的主机名，例如： `https://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
PUT     |/CertificateManagement/api/v1。0/requests/{ID}

###<a name="url-parameters"></a>URL 参数
属性| 说明
---------|--------
id| 必须的。 完成请求的 GUID。


###<a name="request-headers"></a>请求标头
有关常见请求头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="request-body"></a>请求正文
请求正文包含下列属性。

属性 | 说明
---------|-----------
状态 | 要将请求设置为的状态。 可能的值为：“已完成”、“已取消”或“已放弃”。


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
如果成功，将返回 [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) 对象，所含属性如下，用于描述标记为已完成的 MIM CM 请求：

Name | 说明
-----|------------
注释 | 与 MIM CM 请求关联的注释。
完成 | 完成 MIM CM 请求的时间。
DataCollection | 与 MIM CM 请求关联的数据收集项。
DataCollectionFlags | MIM CM 请求的数据收集选项。
标志 | 与 MIM CM 请求关联的选项。
IsDataCollectionComplete | 指示 MIM CM 请求的数据收集是否已完成的布尔值。
IsEnrollmentAgent | 指示运行 MIM CM 请求是否需要注册代理的布尔值。
IsSmartcard | 指示 MIM CM 请求是智能卡请求还是软件配置文件请求的布尔值。
NewProfileUuID | 由 MIM CM 请求生成的新软件配置文件标识。
NewSmartcardUuID | 由 MIM CM 请求分配的新智能卡标识。
OldProfileUuID | 为其创建 MIM CM 请求的软件配置文件标识。
OldSmartcardUuID | 为其创建 MIM CM 请求的智能卡标识。
OrigHTTP 请求头和响应头atorUserUuID | 发起 MIM CM 请求的用户标识
优先级 | MIM CM 请求的优先级。
ProfileTemplateUuID | 为其创建 MIM CM 请求的配置文件模板标识。
请求Type | MIM CM 请求类型。
SecurityDescriptor | MIM CM 请求的安全描述符。
状态 | MIM CM 请求的状态。
Submitted | MIM CM 请求提交的时间。
TargetUserUuID | MIM CM 请求的目标用户标识。
UuID | MIM CM 请求的标识符。

##<a name="example"></a>示例

###<a name="request"></a>请求
```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    “status”:”Completed”
}
```
###<a name="response"></a>响应
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```       
##<a name="see-also"></a>另请参阅

- [Microsoft.Clm.Provision.ExecuteOperations.Complete 方法](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
