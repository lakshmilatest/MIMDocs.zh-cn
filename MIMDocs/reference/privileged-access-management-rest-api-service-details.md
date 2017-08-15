---
title: "PAM REST API 服务详细信息 | Microsoft Docs"
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
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e248ba4a65e75b1e914c61de70072c7e094123a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="pam-rest-api-service-details"></a>PAM REST API 服务详细信息
以下各节讨论 Microsoft 标识管理器 (MIM) 特权访问管理 (PAM) REST API 的详细信息。

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

## <a name="versioning"></a>版本控制 
该 API 的当前版本为 1。 可以通过请求 URL 中的查询参数指定 API 版本，如以下示例所示：`http://localhost:8086/api/pamresources/pamrequests?v=1`。如果请求中未指定版本，那么请求会针对发布的最新版 API 执行。 

## <a name="security"></a>安全 
访问 API 需要集成的 Windows 身份验证 (IWA)。 这应在 Microsoft 标识管理器 (MIM) 安装前在 IIS 中手动配置。

支持 HTTPS (TLS)，但应在 IIS 中手动配置。 有关详细信息，请参阅“安装 FIM 2010 R2 测试实验室指南”的[第 9 步：执行 FIM 2010 R2 安装后任务](https://technet.microsoft.com/library/hh322875(v=ws.10%29.aspx)中的“实现 FIM 门户的安全套接字层 (SSL)”。 

你可以通过在 Visual Studio 命令提示符处运行以下命令来生成新的 SSL 服务器证书：
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
此命令创建可用于测试 Web 应用程序的自签名证书，该应用程序在其 URL 为 test.cwap.com 的服务器上使用 SSL。 由 -eku 选项定义的 OID 将证书标识为 SSL 服务器证书。 此证书存储在我的存储中，并且在计算机级别上可用，因此你可以在 mmc.exe 中将其从证书管理单元中导出

## <a name="cross-domain-access-cors"></a>跨域访问 (CORS) 
支持 CORS，但应在 IIS 中手动配置。 将以下元素添加到 API web.config 文件以将 API 配置为允许跨域调用： 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```
<br/>

## <a name="error-handling"></a>错误处理 
该 API 返回 HTTP 错误响应以指示错误条件。 错误符合 OData。 下表显示可能返回到客户端的错误代码。

HTTP 状态代码 | 说明
-----------------|------------
401 | 未经授权 
403 | 已禁止 
408 | 请求超时   
500 | 内部服务器错误 
503 | 服务不可用 
<br/>

## <a name="filtering"></a>筛选 
PAM REST API 请求可以包括筛选器来指定应包含在响应中的属性。 筛选器语法基于 OData 表达式。

筛选器可以指定 PAM 请求、PAM 角色 或挂起的 PAM 请求的任何属性。 例如：PAM 请求、PAM 角色或挂起的请求的 *ExpirationTime*、 *DisplayName*或其他任何有效属性。

该 API 在筛选器表达式中支持以下运算符： *And*、 *Equal*、 *NotEqual*、 *GreaterThan*、 *LessThan*、 *GreaterThenOrEqueal*和 *LessThanOrEqual*。 

以下示例请求包括筛选器：

- 此请求返回特定日期之间的所有 PAM 请求： `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- 此请求返回带有显示名称“SQL 文件访问”的 Pam 角色： `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `
