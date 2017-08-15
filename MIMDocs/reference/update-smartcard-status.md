---
title: "更新智能卡状态 | Microsoft Docs"
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
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cb7b04539b8568a8b2ae83172b2aa8cddbf6174d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="update-smartcard-status"></a>更新智能卡状态
更新智能卡的状态。

**注意**：本主题中所示的 URL 相对于 API 部署期间选择的主机名，例如： `https://api.contoso.com`。
## <a name="request"></a>请求


方法  |请求 URL  
---------|---------
GET     |/CertificateManagement/api/v1。0/requests/{reqid}/smartcards/{scid}

### <a name="url-parameters"></a>URL 参数
参数 | 说明
---------|------------
reqid | 必须的。 请求标识符（特定于 MIM CM）
scid | 必须的。 智能卡标识符（特定于 MIM CM）。 这是 [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) 对象中的“uuid”属性。

### <a name="request-headers"></a>请求标头
有关常见请求头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
### <a name="request-body"></a>请求正文
请求正文包含下列属性。

属性 | 说明
---------|-----------
状态 | 要设置的请求状态。 例如，“已停用”。


## <a name="response"></a>响应
### <a name="response-codes"></a>响应代码
代码  |说明  
---------|---------
200     | 确定
204 | 无内容
403 | 已禁止
500 | 内部错误

### <a name="response-headers"></a>响应标头
有关常见响应头，请参阅“CM REST API 服务详细信息”中的 [HTTP 请求头和响应头](certificate-management-rest-api-service-details.md#http-request-and-response-headers)。
### <a name="response-body"></a>响应正文
成功时，将返回一个 JSON 序列化的 [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) 对象，且具有以下属性：

Name | 说明
-----|-----------
AssignedUserUuid | 智能卡分配给用户的标识符。
Atr | 当前正在初始化卡的智能卡的应答到重置 (ATR) 字符串。
注释 | 描述智能卡的备注。
Flags | 描述智能卡的标志。
中间件 | 智能卡的中间件。
ParentSmartcardUuid | 由该智能卡替换的旧智能卡的标识符。
PermanentSmartcardUuid | 与智能卡相关联的永久智能卡的标识符。
PrimarySmartcardUuid | 主智能卡的标识符。
ProfileTemplateUuid | 包含管理智能卡的策略和设置的配置文件模板的标识符。
ProfileTemplateVersion | 创建智能卡配置文件时的配置文件模板的版本。
SerialNumber | 智能卡的序列号。
状态 | 智能卡的状态。
Uuid | 智能卡配置文件的标识符。

## <a name="example"></a>示例

### <a name="request"></a>请求
```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1

```
### <a name="response"></a>响应
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":6,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
## <a name="see-also"></a>另请参阅

- [Microsoft.Clm.Shared.Smartcards.Smartcard Class](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx))
