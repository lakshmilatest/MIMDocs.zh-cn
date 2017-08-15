---
title: "Privileged Access Management REST API 参考 | Microsoft Docs"
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
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 02de09a7de843cb42080daa4ce43a5a0c74f2c18
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="privileged-access-management-rest-api-reference"></a>Privileged Access Management REST API 参考
Microsoft 标识管理器 (MIM) 2016 添加了新方案，称为特权访问管理 (PAM)。 PAM 使组织可以对高特权用户帐户（例如系统或服务管理员）对敏感资源的访问权限具有更多的控制权。 PAM 通过在需要访问权限时实时 (JIT) 提供限时访问权限来控制高特权帐户的访问。

用户可以使用两种方式要求 MIM 服务提供特许访问权限（提升）：

- 通过使用 PAM REST API
- 通过使用 PAM PowerShell New-PAMRequest cmdlet

本指南中的主题介绍 PAM REST API。 有关使用 PowerShell cmdlet 的详细信息，请参阅“测试实验室指南：使用 Microsoft 标识管理器演示特权访问管理，在 Connect 站点上提供。

##<a name="pam-rest-api-resources-and-operations"></a>PAM REST API 资源和操作
PAM REST API 对以下资源执行操作
- **PAM 角色**：PAM 角色用户的集合与访问权限的集合相关联。 访问权限由对安全组的引用来定义。  每个 PAM 角色都具有有权提升到 PAM 角色的用户帐户（称为候选人）的列表。 你可以对 PAM 角色执行以下操作：

    - [获取 PAM 角色](privileged-access-management-get-roles.md)

- **PAM 请求**：希望提升到 PAM 角色访问权限的用户必须提交 PAM 请求并获取该请求的批准才能进行提升。 PAM 请求对象将在 MIM 服务中跟踪此请求的生命周期。 你可以对 PAM 请求执行以下操作：

    - [创建 PAM 请求](privileged-access-management-create-request.md)
    - [获取 PAM 请求](privileged-access-management-get-requests.md)
    - [关闭 PAM 请求](privileged-access-management-close-request.md)

- **挂起的 PAM 请求**：用于批准或拒绝已由用户提交的 PAM 请求。 你可以对挂起的 PAM 请求执行以下操作：

    - [获取挂起的 PAM 请求](privileged-access-management-get-pending-requests.md)
    - [批准或拒绝挂起的 PAM 请求](privileged-access-management-approve-reject-pending-request.md)

- **PAM 会话**：在使用 PAM REST API 时，客户端（例如 Web 浏览器）具有带有 PAM REST API 终结点的会话。 在本会话中，将客户端身份验证到 REST API 终结点。 你可以对 PAM 会话执行以下操作：

     - [获取 PAM 会话信息](privileged-access-management-get-session-info.md)

有关该服务的详细信息，请参阅 [PAM REST API 服务详细信息](privileged-access-management-rest-api-service-details.md)

##<a name="pam-sample-portal-on-github"></a>GitHub 上的 PAM 示例门户
了解如何使用 PAM REST API 的一种方法是使用 PAM 示例门户，它是一种使用该 API 的示例 Web 应用程序。 可以在 [GitHub 上的 PAM 示例存储库](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409)中找到 PAM 示例门户的代码。 你可以在 PAM 测试实验室指南中了解如何部署示例门户。
