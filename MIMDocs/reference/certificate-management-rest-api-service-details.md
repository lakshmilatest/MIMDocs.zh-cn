---
title: "CM REST API 服务详细信息 | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3d724dea8b1536f0155f9fc287d0cd74f5f16b3c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="cm-rest-api-service-details"></a>CM REST API 服务详细信息
以下部分讨论 Microsoft 标识管理器 (MIM) 证书管理 (CM) REST API 的详细信息。

## <a name="architecture"></a>体系结构 
MIM CM REST API 调用由控制器处理。 下表显示控制器的完整列表以及可在其中使用它们的上下文的示例。

控制器| 示例路由
----------|-------------
CertificateDataController| /api/v1.0/requests/{requestid}/certificatedata /
CertificateRequestGenerationOptionsController| /api/v1.0/requests/{requestid}/certificaterequestgenerationoptions
CertificatesController| /api/v1.0/smartcards/{smartcardid}/certificates <br/> /api/v1.0/profiles/{profileid}/certificates
OperationsController| /api/v1.0/smartcards/{smartcardid}/operations <br/> /api/v1.0/profiles/{profileid}/operations
PoliciesController| /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
ProfilesController| / api/v1.0/profiles/{id} <br/> /api/v1.0/Profiles <br/> / api/v1.0/requests/{requestid}/profiles/{id}
ProfileTemplatesController| /api/v1.0/profiletemplates/{id} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
RequestsController| / api/v1.0/requests/{id} <br/> /api/v1.0/requests
SmartcardsController| /api/v1.0/requests/{requestid}/smartcards/{id}/diversifiedkey <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/serverproposedpin <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/authenticationresponse <br/> /api/v1.0/requests/{requestid}/smartcards/{id} <br/> /api/v1.0/smartcards/{id} <br/> /api/v1.0/smartcards
SmartcardsConfigurationController| /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards
<br/>

## <a name="http-request-and-response-headers"></a>HTTP 请求头和响应头

发送到 API 的 HTTP 请求应包含以下头（此列表并不详尽）：

标头 | 说明
-------|------------
授权 | 必须的。 内容将取决于身份验证方法，后者可以配置，并且可基于 WIA（Windows 集成身份验证）或 ADFS。
Content-Type | 如果请求具有正文则为必需。 必须为“application/json”。
内容长度 | 如果请求具有正文则为必需。 
Cookie | 会话 Cookie。 根据身份验证方法，可能为必需。
<br/>
HTTP 响应将包括以下头（此列表并不详尽)：

标头 | 说明
-------|------------
内容类型 | 该 API 始终返回“application/json”。
内容长度 | 请求正文的长度（如果存在），以字节为单位。


## <a name="api-versioning"></a>API 版本控制 
CM REST API 的当前版本为 1.0。 在紧接在 URI 中的 `/api` 段后的段中指定版本： `/api/v1.0`。 在将来，如果 API 接口发生重要更改，则版本号将更改。


## <a name="enabling-the-api"></a>启用 API 
Web.config 文件的 `<ClmConfiguration>` 部分已由新键扩展：

```
<add key="Clm.WebApi.Enabled" value="false" />
```
<br/>

此键决定是否向客户端公开 CM REST API。 如果此键设置为“false”，则不在应用程序启动时执行 API 的路由映射。 这意味着对 API 终结点的任何后续请求都将返回 HTTP 404 错误代码。 此键默认设置为“已禁用”。
将此值更改为“true”后，请记得在服务器上运行 **iisreset** 。

## <a name="enabling-tracing-and-logging"></a>启用跟踪和日志记录 
MIM CM REST API 为发送到它的每个 HTTP 请求发射跟踪数据。 你可以通过设置以下配置值来设置所发射的跟踪信息的详细级别：

```
<add name="Microsoft.Clm.Web.API" value="0" />
```
<br/>

## <a name="error-handling-and-troubleshooting"></a>错误处理和故障排除 
如果在处理请求时发生异常，MIM CM REST API 会将 HTTP 状态代码返回到 Web 客户端。 对于常见错误，API 将返回相应的 HTTP 状态代码和错误代码。 

未处理的异常转换为带有 HTTP 状态代码 500 的（“内部错误”） `HttpResponseException` ，并且在事件日志和 MIM CM 跟踪文件中跟踪该异常。 每个未处理的异常都将写入事件日志，并带有相应的相关 ID。 相关 ID 也会在错误消息中发送给 API 的使用者。 若要解决此错误，管理员可以搜索事件日志以查找相应的关联 ID 和错误详细信息。

出于安全性考虑，对应于由使用 MIM CM REST API 而生成的错误的堆栈跟踪不会发送回客户端。
