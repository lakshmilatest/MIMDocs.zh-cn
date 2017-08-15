---
title: "获取请求 | Microsoft Docs"
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
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 56bb25f7282879943dc624f3c24b836c7ae4a58c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="get-request"></a>获取请求
获取一个或多个指定的 MIM CM 请求。

**注意**：本主题中所示的 URL 相对于 API 部署期间选择的主机名，例如： `https://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{id}

###<a name="url-parameters"></a>URL 参数
属性| 说明
---------|--------
ID| 可选。 要检索的请求的 GUID。
###<a name="query-parameters"></a>查询参数
属性| 说明
---------|--------
targetuser| 可选。 请求的目标用户。 如果未指定目标，将返回所有请求，而不考虑目标用户。 <br/> **注意**：当前仅支持“current”。
状态| 可选。 指示要检索的请求的状态。 可能的状态类型包括： *批准*、 *取消*、 *完成*、 *拒绝*、 *执行*、 *失败*、 *无*和 *挂起*。 <br/>如果未指定任何状态，将返回所有请求，无论状态是什么。

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
如果成功，将返回一个或多个包含以下属性的 [Request](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx) 对象：

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

###<a name="request-1"></a>请求 1
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1

```
###<a name="response-1"></a>响应 1
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":3,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"00000000-0000-0000-0000-000000000000",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}```       
###Request 2
```
获取 /certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1
```
###Response 2
```
HTTP/1.1 200 正常

{ "Uuid":"0c96d73f-967b-420e-854a-43ad2a1504bc", "RequestType":5, "Status":3, "Flags":2, "DataCollection":[ { "Name":"Sample Data Item", "Value":null } ], "DataCollectionFlags":32, "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780", "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780", "Submitted":"2015-07-07T23:40:33.133Z", "Completed":"0001-01-01T00:00:00", "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc", "OldProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc", "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9", "OldSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9", "Priority":0, "Comment":"", "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3", "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000", "IsSmartcard":true, "IsEnrollmentAgent":false, "IsDataCollectionComplete":false }
```       

##See Also

- [Microsoft.Clm.Provision.FindOperations.FindRequest Method](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests.aspx)
- [Microsoft.Clm.Shared.RequestPermission Enumeration](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requestpermission.aspx)
- [Microsoft.Clm.Shared.Requests.Request Class](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)
