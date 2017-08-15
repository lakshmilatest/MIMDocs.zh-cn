---
title: "获取配置文件数据 | Microsoft Docs"
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
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 068497509b07b879fcadde6b60a9530d3d8db093
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-data"></a>获取配置文件数据
获取用户的软件证书配置文件列表，以及当前用户可执行的可能操作的列表。 然后可以为任何指定操作启动某项请求。

**注意**：服务器仅在配置文件模板策略指示应完成此操作时设置 PIN。 否则，用户应提供 PIN。

**注意**：本主题中所示的 URL 相对于 API 部署期间选择的主机名，例如： `https://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/api/v1.0/requests/{requestid}/profiles

###<a name="url-parameters"></a>URL 参数
参数 | 说明
---------|------------
id | 要返回的配置文件的标识符 (GUID)。
requestId | 要为其返回配置文件的请求的标识符。

###<a name="query-parameters"></a>查询参数
参数 | 说明
---------|------------
状态 | 可选。 指示要为其检索数据的配置文件的状态。 可能的状态类型包括：“活动”、“已批准”、“已取消”、“已完成”、“已拒绝”、“正在执行”、“已失败”、“无”和“挂起”。 <br/>如果未指定任何状态，将返回所有配置文件，无论状态是什么。
###<a name="request-headers"></a>请求标头
有关常见请求头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="request-body"></a>请求正文
无

##<a name="response"></a>响应
###<a name="response-codes"></a>响应代码
代码  |说明  
---------|---------
200 | 确定
204 | 无内容
403 | 已禁止
500 | 内部错误

###<a name="response-headers"></a>响应标头
有关常见响应头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="response-body"></a>响应正文
如果成功，将返回 JSON 序列化 [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) 对象的列表，所含属性如下：

属性 | 说明
---------|------------
AssignedUserUuid | 向其分配配置文件的用户的标识符。
注释 | 描述配置文件的说明。
标志 | 描述配置文件的标志。
ParentProfileUuid | 配置文件已替换的旧配置文件的标识符。
PrimaryProfileUuid | 主配置文件的标识符。
ProfileOperations | 可由配置文件上的当前用户执行的可能操作的列表。
ProfileTemplateUuid | 包含管理配置文件的策略和设置的配置文件模板的标识符。
ProfileTemplateVersion | 创建配置文件时的配置文件模板的版本。
状态 | 配置文件的状态。
UuID | 配置文件的标识符。


##<a name="example"></a>示例

###<a name="request"></a>请求
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###<a name="response"></a>响应
```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```       
##<a name="see-also"></a>另请参阅

- [Microsoft.Clm.Shared.Profiles.Profile 类](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
