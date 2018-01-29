---
title: "部署 PAM 步骤 7 - 用户访问权限 | Microsoft 文档"
description: "作为最后一步，授予特权用户临时访问权限，以演示 Privileged Access Management 部署已成功。"
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/17/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.openlocfilehash: 2d92be315547da1c352c0e60732f9aeecf0c2d90
ms.sourcegitcommit: 3d8a2493eae1218bfdb75a399ffa4adc8c2a8fdf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2018
---
# <a name="step-7--elevate-a-users-access"></a>步骤 7 - 提升用户的访问权限

>[!div class="step-by-step"]
[« 步骤 6](step-6-transition-group-to-pam.md)


此步骤演示了用户可以通过 MIM 请求对某个角色的访问权限。

## <a name="verify-that-jen-cannot-access-the-privileged-resource"></a>验证 Jen 是否无法访问特权资源

如果不提升权限，则 Jen 无法访问 CORP 林中的特权资源。

1. 注销 CORPWKSTN 以删除任何缓存的开放连接。
2. 以 CONTOSO\Jen 身份登录到 CORPWKSTN，并切换到“桌面”视图。
3. 打开 DOS 命令提示符。
4. 键入命令 `dir \\corpwkstn\corpfs`。 应显示错误消息“访问被拒”。
5. 使命令提示符窗口保持打开状态。

## <a name="request-privileged-access-from-mim"></a>从 MIM 中请求特许访问权限。

> [!NOTE]
> 建议工作站是特权工作站 (PAW)。  有关详细信息，请参阅 [PAW](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)。

1. 在 PRIVWKSTN 上，以 PRIV\priv.jen 身份登录。
2. 依次单击“开始”和“运行”，并输入 PowerShell.exe。
3. 键入下列命令。

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. 出现提示时，键入 PRIV.Jen 帐户的密码。 将出现新的命令提示符窗口。
3. 出现 PowerShell 窗口时，键入以下命令。

    > [!NOTE]
    > 运行这些命令后，所有以下步骤都具有时效性。

    ```PowerShell
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. 完成该操作后，关闭 PowerShell 窗口。
5. 在 DOS 命令窗口中，键入以下命令

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. 键入 PRIV.Jen 帐户的密码。 将出现新的命令提示符窗口。

## <a name="validate-the-elevated-access"></a>验证提升的访问权限。
在新打开的窗口中，键入以下命令。

```cmd
whoami /groups
dir \\corpwkstn\corpfs
```

如果 dir 命令失败并显示错误消息“访问被拒”，请重新检查该信任关系。

## <a name="activate-the-privileged-role"></a>激活特权角色

通过 PAM 示例门户请求特权访问进行激活。

1. 在 CORPWKSTN 上，确保以 CORP\Jen 身份登录。
2. 在 DOS 命令窗口中，键入以下命令。

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. 出现提示时，键入 PRIV.Jen 帐户的密码。 将出现新的 Web 浏览器窗口。
4. 导航到 http://pamsrv.priv.contoso.local:8090 ，并确保示例门户中的网页可见。
5. 在 Internet Explorer 中，依次选择“工具” > “Internet 选项”，然后单击“安全”选项卡。
6. 依次单击“本地 Intranet 区域” > “站点” > “高级”，然后将该网站添加到该区域。
7. 关闭“Internet 选项”  对话框。
8. 在左侧选项卡上，单击“激活” 。 选择“PAM 角色”，然后单击“激活”。

> [!Note]
> 在此环境中，还可了解如何开发使用 PAM REST API 的哟哟林程序，如 [Privileged Access Management REST API 参考](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference)中所述。

## <a name="summary"></a>“摘要”

完成本演练中的步骤后，你已演示了一个 Privileged Access Management 方案，其中在有限时间内对用户权限进行了提升，从而允许用户使用独立的特权帐户访问受保护的资源。 当提升会话过期时，特权帐户将无法再访问受保护的资源。 哪些安全组表示特权角色的决策由 PAM 管理员来协调。 访问权限迁移到 Privileged Access Management 系统后，以前可使用原始用户帐户实现的访问现在只能通过登录特殊特权帐户来实现，并且仅在请求时提供。 因此，高特权组的组成员资源只在有限时间内有效。

>[!div class="step-by-step"]
[« 步骤 6](step-6-transition-group-to-pam.md)
