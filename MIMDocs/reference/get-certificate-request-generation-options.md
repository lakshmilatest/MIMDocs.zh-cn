---
title: "获取证书请求生成选项 | Microsoft Docs"
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5822da1b9cf8558becccff815fd0208923fcaaa0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="get-certificate-request-generation-options"></a>获取证书请求生成选项

获取客户端证书请求生成的参数。

**注意**：本主题中所示 URL 相对于 API 部署期间选择的主机名，例如：`https://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
GET     |/CertificateManagement/api/v1。0/requests/{requestid}/certificaterequestgenerationoptions

###<a name="url-parameters"></a>URL 参数
参数 | 说明
--------|--------------
requestid| 必须的。 要为其检索证书请求生成参数的 MIM CM 请求的 GUID 标识符。

###<a name="request-headers"></a>请求标头
有关常见请求头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="request-body"></a>请求正文
无。


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
成功时，返回 CertificateRequestGenerationOptions 对象的列表。 每个 CertificateRequestGenerationOptions 对象对应于客户端必须生成的单个证书请求，并具有以下属性：

属性| 说明
--------|-----------
Exportable | 指定为请求创建的私钥是否可导出的值。
FriendlyName | 已注册证书的显示名称。
HashAlgorithmName | 在创建证书请求签名时使用的哈希算法。
KeyAlgorithmName | 公钥算法。
KeyProtectionLevel | 强密钥保护级别。
KeySize | 要生成的私钥大小，以位为单位。
KeyStorageProviderNames | 可用于生成私钥的可接受密钥存储提供程序 (KSP) 的列表。 在第一个 KSP 无法用于生成证书请求的情况下，任何指定的 KSP 均可使用，直到有一个成功。
KeyUsages | 为此证书请求创建的私钥可执行的操作。 默认值为 Signing。
使用者 | 使用者名称。

**注意**：虽然 [Windows.Security.Cryptography.Certificates.CertificateRequestProperties 类](https://msdn.microsoft.com/library/windows/apps/br212079.aspx)说明中详细介绍了这些属性，但请注意，此类和 CertificateRequestGenerationOptions 对象之间不存在一对一的对应关系。

##<a name="example"></a>示例

###<a name="request"></a>请求
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###<a name="response"></a>响应
```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```       
