---
title: "获取工作流策略 | Microsoft Docs"
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
ms.assetid: be636205-c1f0-457c-982e-e17478cf0889
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6bc88fee88bdfeec3ce4ead60bc17fe2ea169402
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="get-workflow-policy"></a>获取工作流策略
获取指定工作流的配置文件模板策略。 此数据在请求创建期间使用。 工作流策略指定客户端为创建请求所需的数据。 此类数据包括：各种数据收集项、请求注释和一个时间密码策略。

**注意**：本主题中所示的 URL 相对于 API 部署期间选择的主机名，例如： `https://api.contoso.com`。
##<a name="request"></a>请求


方法  |请求 URL  
---------|---------
GET     |/CertificateManagement/api/v1。0/profiletemplates/{ID}/policy/workflow/{类型}

###<a name="url-parameters"></a>URL 参数
参数| 说明
--------|-------------
id| 必须的。 对应于从中提取策略的配置文件模板的 GUID。
类型| 必须的。 所请求策略的类型。 可能的值为： *注册*、 *复制*、 *OfflineUnblock*、 *OnlineUpdate*、 *续订*、 *恢复*、 *RecoverOnBehalf*、 *恢复*、 *停用*、 *撤消*、 *TemporaryEnroll*、 *取消阻止*。

###<a name="request-headers"></a>请求标头
有关常见请求头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="request-body"></a>请求正文
无

##<a name="response"></a>响应
###<a name="response-codes"></a>响应代码
代码  |说明  
---------|---------
200     | “确定”
403 | 已禁止
204 | 无内容
500 | 内部错误

###<a name="response-headers"></a>响应标头
有关常见响应头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
###<a name="response-body"></a>响应正文
如果成功，将根据 [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) 对象返回策略对象。 策略对象至少将包含下表中的属性，但也可能包含其他属性，具体取决于请求的策略。 例如，注册策略请求将返回 [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy.aspx) 对象。 有关详细信息，请参阅与请求中的 {type} 参数关联的策略对象的文档。 有关不同类型的策略对象的文档，可以参阅 [Microsoft.Clm.Shared.ProfileTemplates 命名空间](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)文档下。

属性 | 说明
---------|------------
ApprovalsNeeded | 策略的 FIM CM 请求所需的批准数。
AuthorizedApprover | 针对有权批准策略的 FIM CM 请求的用户的安全描述符。
Authorized注册mentAgent | 针对充当策略注册代理的用户的安全描述符。
AuthorizedInitiator | 针对可以启动策略的 FIM CM 请求的用户的安全描述符。
CollectComments | 指示是否为策略的 FIM CM 请求启用注释集合的布尔值。
Collect请求Priority | 指示是否为策略的 FIM CM 请求启用请求优先级集合的布尔值。
Default请求Priority | 策略的 FIM CM 请求的默认优先级。
文档 | 为策略配置的策略文档。
启用 | 指示是否启用了策略的布尔值。
注册AgentRequired | 指示策略的 FIM CM 请求是否需要注册代理的布尔值。
OneTimePasswordPolicy | 获取有关如何分配策略的 FIM CM 请求的一次性密码的信息。
个性化设置 | 策略的智能卡个性化设置选项。
PolicyDataCollection | 与策略关联的数据集合项。
SelfService启用 | 指示是否为策略的 FIM CM 请求启用自助服务启动的布尔值。

##<a name="example"></a>示例

###<a name="request-1"></a>请求 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###<a name="response-1"></a>响应 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###<a name="request-2"></a>请求 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###<a name="response-2"></a>响应 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##<a name="see-also"></a>另请参阅

- [Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy 类](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Microsoft.Clm.Shared.ProfileTemplates 命名空间](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
