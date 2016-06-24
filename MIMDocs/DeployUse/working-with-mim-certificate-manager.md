---
# required metadata

title: 使用 MIM 证书管理器 | Microsoft 标识管理器
description: 了解如何部署证书管理器应用以使用户可以管理自己的访问权限。 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# 使用 MIM 证书管理器
在你启动并运行 MIM 2016 和证书管理器后，你可以部署 MIM 证书管理器 Windows 应用商店应用程序，以便你的用户可以轻松地管理其物理智能卡、虚拟智能卡和软件证书。 部署 MIM CM 应用的步骤如下所示：

1.  创建证书模板。

2.  创建配置文件模板。

3.  准备应用。

4.  通过 SCCM 或 Intune 部署应用。

## 创建证书模板
使用与平时相同的方法为 CM 应用创建证书模板，只不过你必须确保证书模板为版本 3 和更高版本。

1.  登录到运行 AD CS 的服务器（证书服务器）。

2.  打开 MMC。

3.  单击“文件 &gt; 添加/删除管理单元”；

4.  在可用管理单元列表中，单击“证书模板”，然后单击“添加”。

5.  现在你将在 MMC 中的“控制台根”  下看到“证书模板”  。 双击它以查看所有可用的证书模板。

6.  右键单击“智能卡登录”模板，然后单击“复制模板”。

7.  在“兼容性”选项卡上，在“证书颁发机构”下选择 Windows Server 2008，然后在“证书接收者”下选择 Windows 8.1/Windows Server 2012 R2。
    此步骤至关重要，因为它可确保你拥有版本 3（或更高版本）的证书模板，而且仅版本 3 适用于证书管理器应用。 由于版本在你第一次创建和保存证书模板时设置，因此如果你未使用此方法创建证书模板，则无法将其修改为正确的版本，应先创建一个新的证书模板才可继续。

8.  在“常规”  选项卡上，在“显示名称”  字段中，键入你希望在应用 UI 中显示的名称，例如 **虚拟智能卡登录**。

9. 在“请求处理”选项卡上，将“目的”设置为“签名和加密”，然后在“执行以下操作…”下 选择“在注册过程中提示用户” 。

10. 在“提供程序类别”  下的“加密” 选项卡上，选择“密钥存储提供程序和请求可使用在使用者的计算机上可用的任何提供程序” 。

    > [!NOTE]
    > 如果你的模板为版本 3，则你将仅看到密钥存储提供程序作为选项。 如果你未看到它，你可能未正确创建带有正确版本的证书模板。 从上面的步骤 5 重新开始。

11. 在“安全”  选项卡上，添加你希望为其提供“注册”  访问权限的安全组。 例如，如果你希望为所有用户提供访问权限，请选择“经身份验证的用户”  组，然后为他们选择“注册权限”  。

12. 单击“确定”  以完成更改并创建新模板。 你应该能够在证书模板列表中看到你的新模板。

13. 选择“文件”并单击“添加/删除管理单元”以将证书颁发机构管理单元添加到 MMC 控制台。 当系统询问希望管理哪台计算机时，选择“本地计算机” 。

14. 在 MMC 左窗格中，展开“证书颁发机构（本地）”  ，然后再展开证书颁发机构列表内的 CA。

15. 右键单击“证书模板”，单击“新建 &gt; 要颁发的证书模板”。

16. 从列表中选择你创建的新模板，然后单击“确定” 。

## 创建配置文件模板
请确保在创建配置文件模板时将其设置为创建/销毁 vSC 并删除数据收集。 CM 应用无法处理已收集的数据，因此禁用它很重要，如下所示。

1.  作为拥有管理员权限的用户登录到 CM 门户。

2.  依次转到“管理”&gt;“管理配置文件模板”，并确保已选中靠近“MIM CM 示例智能卡登录配置文件模板”的复选框，然后单击“复制选定的配置文件模板”。

3.  键入配置文件模板名称，然后单击“确定” 。

4.  在下一屏幕中，单击“添加新证书模板”  ，并确保选中 CA 名称旁边的框。

5.  选中配置文件模板“登录”  的名称旁边的框，然后单击“添加” 。

6.  删除 SmartCardLogon 模板，方法是选中它旁边的框，然后依次单击“删除所选的证书模板”  和“确定” 。

7.  向下滚动到底部并单击“更改设置” 。

8.  选中“创建/销毁虚拟智能卡”和“多元化管理密钥”旁边的框。

9. 在“用户 PIN 策略”  下选择“用户提供” 。

10. 在左窗格中，单击“续订策略&gt;更改常规设置” 。 选择“在续订时重复使用卡”  ，然后单击“确定” 。

11. 你必须通过单击左窗格中的策略，然后选中“示例数据项”  旁边的框来为每个策略禁用数据收集项，然后单击“删除数据收集项” 。 然后单击“确定” 。

## 使 CM 为部署做好准备

1.  在命令提示符处，运行以下命令来解包该应用，然后将内容提取到名为 appx 的新子文件夹并创建一个副本，这样你无需修改原始文件。

    ```
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2.  在 appx 文件夹中，将称为 CustomDataExample.xml 的文件的名称更改为 Custom.data

3.  打开 Custom.data 文件并根据需要修改参数。

    |||
    |-|-|
    |MIMCM URL|用于配置 CM 的门户的 FQDN。 例如，https://mimcmServerAddress/certificatemanagement|
    |ADFS URL|如果你将使用 AD FS，请插入你的 AD FS URL。 例如，https://adfsServerSame/adfs|
    |PrivacyUrl|你可以将 URL 包含到说明你将如何处理为证书注册收集的用户详细信息的网页。|
    |SupportMail|你可以为支持问题包含电子邮件地址。|
    |LobComplianceEnable|可将其设置为 true 或 false。 默认情况下它设置为 true。|
    |MinimumPinLength|默认情况下它设置为 6。|
    |NonAdmin|可将其设置为 true 或 false。 默认情况下它设置为 false。 仅当你希望在其计算机上不是管理员的用户能够注册并续订证书时修改它。|

4.  保存该文件并退出编辑器。

5.  对包进行签名时会创建一个签名文件，因此必须删除名为 AppxSignature.p7x 的原始签名文件。

6.  AppxManifest.xml 文件指定签名证书的使用者名称。 打开此文件以对其进行编辑。

7.  在开始此部分前，你需要获取一个签名证书。 如下所示，在 MIM 2016 证书管理器的步骤 1 中为非管理员启动智能卡续订。

8.  在&lt;标识&gt;元素中，将 Publisher 属性的值修改为与签名证书中列出的主题相同，例如“CN=SUBJECT”。

9. 保存该文件并退出编辑器。

10. 在命令提示符处，运行以下命令以重新打包 .appx 文件并对其签名。

    ```
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    应用包名称是你在创建副本时使用的相同名称。

    ```
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    这提供了新的 .appx 文件。 pfx 文件提供签名证书的位置和 .pfx 文件的密码。

11. 若要使用 AD FS 身份验证：

    -   打开虚拟智能卡应用程序。 这使你可以更轻松地找到下一步所需的值。

    -   若要将应用程序作为客户端添加到 AD FS 服务器并在服务器上配置 CM，请在 AD FS 服务器上，打开 Windows PowerShell 并运行命令 `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`。

        下面是 ConfigureMimCMClientAndRelyingParty.ps1 脚本：

        ```
        # HELP

        <#
        .SYNOPSIS
                        Configure ADFS for CM client app and server.
        .DESCRIPTION
           What the Script does:
                                        a. Registers the MIM CM client app on the ADFS server.
                                        b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                        For parameter information, see 'get-help -detailed'
        .PARAMETER redirectUri
                        The redirectUri for CM client app. Will be added to ADFS server.
                        It can be found as follows:
                        1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                        2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
        .PARAMETER serverFqdn
                        Your deployed MIM CM server’s FQDN.
        .EXAMPLE
           .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
        #>

        # Parameter declaration
        [CmdletBinding()]
        Param(
          [Parameter(Mandatory=$True)]
           [string]$redirectUri,

           [Parameter(Mandatory=$True)]
           [string]$serverFqdn
        )

        Write-Host "Configuring ADFS Objects for OAuth.."

        #Configure SSO to get persistent sign on cookie
        Set-ADFSProperties -SsoLifetime 2880

        #Configure Authentication Policy
        #Intranet to use Kerberos
        #Extranet to use U/P

        #Create Client Objects

        Write-Host "Creating Client Objects..."

        $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

        if ($existingClient -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Modern App, removing"
            Remove-ADFSClient -TargetName "MIM CM Modern App"
            Write-Host "Client object removed"
        }

        Write-Host "Adding Client Object for MIM CM Modern App client"
        Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
        Write-Host "Client Object for MIM CM Modern App client Created"

        #Create Relying Parties
        Write-Host "Creating Relying Party Objects"

        $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
        if ($existingRp -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Service RP, removing"
            Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
            Write-Host "RP object Removed"
        }

        $authorizationRules =
        "@RuleTemplate = `"AllowAllAuthzRule`"
        => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

        $issuanceRules =
        "@RuleTemplate = `"LdapClaims`"
        @RuleName = `"Emit UPN and common name`"
        c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
        => issue(store = `"Active Directory`", types =
        (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
        `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
        `";userPrincipalName,cn;{0}`", param = c.Value);

        @RuleTemplate = `"PassThroughClaims`"
        @RuleName = `"Pass through Windows Account Name`"
        c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

        Write-Host "Creating RP Trust for MIM CM Service"
        Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
        Write-Host "RP Trust for MIM CM Service has been created"
        ```

    -   更新 redirectUri 和 serverFQDN 的值。

    -   若要查找 redirectUri，在虚拟机智能卡应用程序中打开应用程序设置面板，单击“设置” ，重定向 URI 应在 AD FS 服务器地址栏下方列出。 仅在配置了 ADFS 服务器地址时，该 URI 才会显示。

    -   ServerFQDN 仅限 MIMCM 服务器完整计算机名称。

    -   有关 **ConfigureMIimCMClientAndRelyingParty.ps1** 脚本的帮助，请运行 `get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1`

## 部署应用
设置 CM 应用后，在下载中心中下载文件 MIMDMModernApp_&lt;version&gt;_AnyCPU_Test.zip 并提取其所有内容。 .appx 文件是安装程序。 你可以使用通常部署 Windows 应用商店应用时使用的任何方式来部署它，使用 [System Center Configuration Manager](https://technet.microsoft.com/library/dn613840.aspx)，或使用 [Intune](https://technet.microsoft.com/library/dn613839.aspx) 旁加载应用，以使用户必须通过公司门户访问它或直接向其计算机推送。


<!--HONumber=Apr16_HO2-->


