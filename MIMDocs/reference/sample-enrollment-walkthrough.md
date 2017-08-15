---
title: "示例注册演练 | Microsoft Docs"
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
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 574a4d6fc2d5d4a51483a371cf96c2d48c582a8b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="sample-enrollment-walkthrough"></a>示例注册演练
本主题介绍了执行虚拟智能卡自助注册时需要执行的步骤。 它介绍了使用用户设置的 PIN 自动批准的请求。
1.  客户端对用户进行身份验证，然后请求经过身份验证的用户可以注册的配置文件模板的列表：

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    <br/>
2.  客户端向用户显示生成的列表。 用户选择具有名称“虚拟智能卡 VPN”和 UUID *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*的 vSC（虚拟智能卡）配置文件模板。

3.  客户端通过使用上一步中返回的 UUID 检索所选配置文件模板的注册策略：

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```
 <br/>   
4.  客户端将分析返回的策略中的“DataCollection”字段，请注意，将出现标题为“请求原因”的单个数据收集项目。 客户端还指出要将“collectComments”标志设置为 *false*，因此它不会提示用户输入任何内容。

5.  用户输入要求证书的原因后，客户端将创建一个请求：

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.  服务器成功创建请求，并将请求的 URI 返回给客户端：/CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5。

7.  客户端通过调用返回的 URI 检索请求对象：

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/>
8.  客户端将验证该请求中的“state”属性是否设置为“approved”。 请求执行可能会开始。

9.  客户端检查请求，以便通过分析“newsmartcarduuid”参数的内容，确定智能卡是否已与请求相关联。

10. 由于仅包含空 GUID，因此客户端必须使用 MIM CM 尚未使用的现有卡，或在配置了虚拟智能卡的配置文件模板的情况下创建一个智能卡。

11. 因为后一种情况已通过可注册配置文件模板的初始查询（第 1 步）向客户端指明，所以客户端现在必须创建虚拟智能卡设备。

12. 客户端从配置文件模板（使用第 3 步中所选模板的 UUID）检索智能卡策略：

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```
13. 智能卡策略在“DefaultAdminKeyHex”属性中包含智能卡的默认管理密钥。 创建智能卡时，客户端必须将智能卡的初始管理密钥设置为此密钥。  

14. 创建智能卡设备后，客户端必须将它分配给请求：

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```
<br/>
15. 服务器将在对客户端的响应中返回新建的智能卡对象的 URI：api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644。

16. 客户端检索智能卡对象：

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17. 通过检查在第 12 步中获取的智能卡策略中的“diversifyadminkey”标志值，客户端确定必须实现管理密钥多样化。

18. 客户端检索建议的管理密钥：

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```
<br/>
19. 客户端必须将该卡验证为管理员，才能设置管理密钥。 为此，客户端请求从智能卡中获取身份验证质询，并将它提交到服务器：

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    服务器在 HTTP 响应正文中发送质询响应。 找到十六进制质询字符串，将它转换为 base64，再将它作为 URL 参数进行传递。 URL 将返回另一个响应，必须将它转换回十六进制格式。
<br/>
20. 服务器发送 HTTP 响应正文中的质询响应，并且客户端使用它来使管理密钥多样化。

21. 客户端指出，用户必须检查智能卡策略中的“UserPinOption”字段，从而提供相应的 PIN。

22. 用户将他们所需的 pin 输入到对话框后，客户端将按步骤 19 中所述执行质询响应身份验证，唯一区别在于应立即将多样化的标志设置为“true”：

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```
<br/>
23. 客户端使用从服务器收到的响应来设置所需的用户 pin。

24. 客户端现在就可以生成证书请求。 它将查询配置文件模板证书生成参数，以确定应如何生成密钥/请求。

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25. 服务器通过单个 JSON 序列化 CertificateRequestGenerationOptions 对象（详细说明密钥的输出、证书友好名称、哈希算法、密钥算法、密钥大小等）做出响应。客户端使用这些参数来生成证书请求。

26. 将单个证书请求提交到服务器。 在证书模板指定存档 CA 上的证书的情况下（即 CA 生成密钥对，然后将其发送到客户端），客户端可以另外指定应用于解密任何 PFX blob 的 PFX 密码。 客户端还可以选择添加一些注释。

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27. 几秒后，服务器通过单一、JSON 序列化的 Microsoft.Clm.Shared.Certificate 对象做出响应。 通过检查“isPkcs7”标志，客户端了解到此响应不是 PFX blob。 客户端通过 base64 解码“pkcs7”字符串提取 blob 并安装它。

28. 客户端将请求标记为已完成。

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
