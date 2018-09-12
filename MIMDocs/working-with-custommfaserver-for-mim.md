---
title: 通过 API 使用备用多重身份验证提供程序激活 PAM 或 SSPR 方案 | Microsoft Docs
description: 设置自定义 MFA API，使其在用户激活 Privileged Access Management 中的角色和使用自助密码重置时作为第二安全层。
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.openlocfilehash: 85ab84231c2ebfede125ffaf5fb39964e8a8450c
ms.sourcegitcommit: acb2c61831cb634278acc439d6d9496ff51a6a54
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2018
ms.locfileid: "43694773"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>通过 API 使用自定义多重身份验证提供程序激活 PAM 角色或 SSPR

Azure AD Premium 或 Azure MFA 的客户可以在两个 MIM 方案中集成 Azure MFA： Privileged Access Management (PAM) 角色激活和自助密码重置 (SSPR)。

MIM 客户有两个其他选项：

 - 使用自定义一次性密码提供程序，这只适用于 MIM SSPR 方案和有案可稽的指南[使用 OTP SMS 入口配置自助密码重置](https://docs.microsoft.com/en-us/previous-versions/mim/hh824692(v=ws.10))
 - 使用自定义多重身份验证电话服务提供程序。 这同时适用于 MIM SSPR 和 PAM 方案，如本文中所述

本文概述如何通过客户开发的 API 和集成 SDK，结合使用自定义多重身份验证提供程序和 MIM。  

## <a name="prerequisites"></a>先决条件

要结合使用自定义多重身份验证提供程序 API 和 MIM，需要：

- 适用于所有候选用户的电话号码
- MIM 修补程序 [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) 或更高版本 - 请查阅[版本历史记录](/reference/version-history.md)，了解相关公告
- 针对 SSPR 或 PAM 配置的 MIM 服务

## <a name="approach-using-custom-multi-factor-authentication-code"></a>使用自定义多重身份验证代码的方法

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>步骤 1：确保 MIM 服务为 4.5.202.0 版本或更高版本

下载并安装 MIM 修补程序 [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) 或更高版本。

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>步骤 2：创建可实现 IPhoneServiceProvider 接口的 DLL

DLL 必须包括一个可实现以下三种方法的类：

- `InitiateCall`：MIM 服务会调用此方法。 该服务将以参数的形式传递手机号和请求 ID。  该方法必须返回为 `Pending`、`Success` 或 `Failed` 的 `PhoneCallStatus` 值。
- `GetCallStatus`：如果之前对 `initiateCall` 的调用返回 `Pending`，MIM 服务将调用此方法。 此方法也会返回为 `Pending`、`Success` 或 `Failed` 的 `PhoneCallStatus` 值。
- `GetFailureMessage`：如果之前对 `InitiateCall` 或 `GetCallStatus` 的调用返回 `Failed`，MIM 服务将调用此方法。 此方法将返回一条诊断消息。

这些方法的实现必须处于线程安全状态，因此，不得假设 `GetCallStatus` 和 `GetFailureMessage` 的实现将被之前调用 `InitiateCall` 的相同线程调用。

将 DLL 存储在 `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\` 目录中。

可使用 Visual Studio 2010 或更高版本编译的代码示例。

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>步骤 3：备份位于“C:\Program Files\Microsoft Forefront Identity Manager\2010\Service”中的 MfaSettings.xml

### <a name="step-4-edit-the-mfasettingsxml-file"></a>步骤 4：编辑 MfaSettings.xml 文件

更新或清除以下行：

- 删除/清除所有配置条目行 

- 更新或将以下各行添加到自定义电话服务提供程序的 MfaSettings.xml 中 <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>步骤 5：重启 MIM 服务

重启该服务后，使用 SSPR 和/或 PAM 验证自定义标识提供程序的功能。

> [!NOTE] 
> 若要还原设置，请将 MfaSettings.xml 替换为步骤 3 中的备份文件


## <a name="next-steps"></a>后续步骤

- [Azure 多重身份验证服务器入门](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [什么是 Azure 多重身份验证](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [MIM 版本发布历史记录](./reference/version-history.md)
