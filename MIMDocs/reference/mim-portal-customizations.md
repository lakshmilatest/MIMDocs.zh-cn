---
title: "Microsoft Identity Manager 2016 门户自定义 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: bebb299ece077a6ab2e0d75f3b30ad1776678303
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Microsoft Identity Manager 2016 门户自定义


>[!Warning]
在进行任何 CSS 自定义时，请务必清除浏览器缓存。

在 Microsoft Identity Manager 2016 (MIM) 中，可以自定义密码门户中的选定元素，包括横幅徽标、字符串资源和级联样式表。

为此，需要使用一些项，具体视自定义级别而定。 下面列出了自定义 MIM 2016 密码注册和重置门户时涉及的项。

-   Customizations 文件夹 - 这是 MIM 2016 在使用默认值前检查的文件夹。 每个要自定义的门户都需要有一个 Customizations 文件夹。 只能在此文件夹中进行自定义，因为安装程序不会在升级、安装更改模式或修复模式时覆盖它。

-   Strings.resources - 使用这个 XML 文件，可以修改门户中的字符串。 此文件必须驻留在 Customizations 文件夹中。

-   Style.css - 这是门户用于自定义的级联样式表。 必须创建和修改此样式表，才能更改徽标。也可以将此样式表完全替换为自己的自定义内容。

若要逐步详细了解如何自定义密码注册和重置门户，请参阅“测试实验室指南：演示 MIM 2016 密码注册和重置门户自定义”。

>[!警告] 为了让 MIM 能够识别任何自定义更改，必须通过运行 iisreset 来重启 IIS。


## <a name="creating-the-customizations-folder"></a>创建 Customizations 文件夹

启动后，MIM 先在 Customizations 文件夹中查找 Strings.resources 文件，然后再使用默认值。 必须在要自定义的门户（即密码注册门户或密码重置门户）的目录下创建 Customizations 文件夹。 若要同时自定义这两个门户，必须在以下目录下分别创建 Customizations 文件夹：

-   C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Password Registration Portal

-   C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Password Reset Portal

### <a name="to-create-the-customization-folder"></a>创建 Customizations 文件夹的具体步骤

1.  转到 C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Password Registration Portal 文件夹。

2.  创建名为 Customizations 的文件夹。

3.  返回一级回到 Password Reset Portal 文件夹，并创建名为 Customizations 的文件夹。

## <a name="customizing-strings"></a>自定义字符串

可以创建 Strings.resources 文件，并将此文件添加到 Customizations 文件夹，从而自定义门户 UI 中的许多字符串。 必须为要自定义的每个门户分别创建 Strings.resources 文件。

### <a name="to-customize-strings"></a>自定义字符串的具体步骤

1.  将下面的代码复制到记事本中，并将记事本作为 Strings.resources 保存到 Customizations 文件夹

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```
2.  在 `<!-- Customizations begin here -->` 部分下，将数据名称更改为与要自定义的字符串一致，并在 <value></value> 标记之间输入此字符串的值。 请参阅下面的列表，了解可自定义的字符串及其默认值。

>[!NOTE]
Strings.resources 文件为中性语言。 若要创建语言专属自定义字符串，必须已安装相应语言包，并将文件保存为格式 Strings. `<language>-<culture>.resources`（例如，Strings.en us.resources）。

下表列出了可以自定义的门户字符串。

<a name="portal-strings"></a>门户字符串
--------------

| Name                                                     | 默认值                                                                                                                                                                                                                                                                                                                                         | 注释                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | 关于         |       |
| ButtonCancel                                             | 取消                                                                                 |     |
| ButtonFinish                                             | 完成    |    |
| ButtonNext                                               | 下一步    |    |
| ButtonOk                                                 | 确定     |   |
| CancelFinishedMessage                                    | 会话不再处于活动状态。 可以关闭窗口，也可以通过单击下面的链接进行重启。         |      |
| CancelFinishedTitle                                      | 会话已结束                                                              |      |
| ErrorDescription_3000                                    | 已出现错误。 请重试。如果此问题一直存在，请联系支持人员或系统管理员。 (错误 3000)       |          |
| ErrorDescription_3001                                    | 请务必正确输入用户名。如果仍无法重置密码，请联系      |           |
|                                                          | 支持人员以寻求帮助。 (错误 3001)   |                   |
| ErrorDescription_3002                                    | 会话已结束。 请返回到主页，以重新开始。 (错误 3002)                                     |                |
| ErrorDescription_3003                                    | Forefront Identity Manager 无法识别当前用户帐户。请联系支持人员或系统管理员。 (错误 3003)           |               |
| ErrorDescription_3004                                    | 无权注册密码重置。请联系支持人员或系统管理员。 (错误 3004)     |              |
| ErrorDescription_3005                                    | 提供的一个或多个答案与密码注册期间提供的答案不一致。现在提供的答案必须与注册时提供的答案一致，才能重置密码。可以返回到主页重新开始，也可以联系支持人员或系统管理员。 (错误 3005) |           |
| ErrorDescription_3006                                    | 输入的密码不正确。必须输入正确的密码，才能注册密码重置。 (错误 3006)            |              |
| ErrorDescription_3007                                    | 暂时禁止重置密码。请稍后重试，或联系支持人员或系统管理员以寻求帮助。 (错误 3007)     |         |
| ErrorDescription_3008                                    | 已出现错误。 请重试。如果此问题一直存在，请联系支持人员或系统管理员。 (错误 3008)        |          |
| ErrorDescription_3009                                    | 输入文本采用被禁格式。 请重新尝试输入其他格式的文本，或联系支持人员或系统管理员。 (错误 3009)     |             |
| ErrorDescription_3010_Registration                       | 浏览器未启用脚本。 请启用脚本并返回到“密码注册”主页，或联系支持人员以寻求帮助。      |               |
| ErrorDescription_3010_Reset                              | 浏览器未启用脚本。 请启用脚本并返回到“密码重置”主页，或联系支持人员以寻求帮助。     |            |
| ErrorDescription_3011                                    | 此网站使用 Cookie。 请浏览器配置为接受 Cookie 并重试，或联系支持人员以寻求帮助。          |                                           |
| ErrorDescription_3012                                    | 输入的数据与发送给你的安全码不一致。 可以尝试再次重置密码，也可以联系支持人员以寻求帮助。      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | 无法发送安全码。 请联系支持人员以寻求帮助。                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | 请输入正确格式的用户名。                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | 输入用户名即可继续。                                              |                         |
| ErrorMessagePasswordRequired                             | 输入密码。              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | 请确保这两个密码一致。                                |           |
| ErrorPageDefaultHeading                                  | 应用程序错误                                            |               |
| ErrorPageServerTime                                      | 服务器时间: {0:T}                     | {0} 是异常捕捉时间。 “T”让传入时间采用“长时间”格式。 最终显示小时、分钟和秒，可能还会显示 AM/PM 标识（具体视当前区域性而定）。 |
| ErrorPageTitle                                           | Forefront Identity Management - 密码错误                     |   |
| ErrorTitle_3000                                          | 错误                                  |  |
| ErrorTitle_3001                                          | 访问被拒绝。                          |  |
| ErrorTitle_3002                                          | 会话已结束                          |  |
| ErrorTitle_3003                                          | 无法识别用户                      |  |
| ErrorTitle_3004                                          | 用户未经授权                      |  |
| ErrorTitle_3005                                          | 答案不一致                    |  |
| ErrorTitle_3006                                          | 密码不正确                     |  |  
| ErrorTitle_3007                                          | 访问暂时遭拒              |  |
| ErrorTitle_3008                                          | 通信错误                    |  |
| ErrorTitle_3009                                          | 禁止输入                       |  |
| ErrorTitle_3010                                          | 浏览器配置错误            |  |
| ErrorTitle_3011                                          | 浏览器配置错误            |  |
| ErrorTitle_3012                                          | 验证失败                    |  |
| ErrorTitle_3013                                          | 无法发送安全码   |  |
| FinalizeRegistrationHeading1                             | 如果需要重置密码:        |   |
| FinalizeRegistrationSubHeading1                          | 转到密码重置门户   |   |
| FinalizeRegistrationSubHeading2                          | 验证身份   |   |
| FinalizeRegistrationSubHeading3                          | 选择新密码    |     |
| FinishingDescription                                     | 选择新密码        |    |
| FinishingTitle                                           | 密码重置:      |     |
| GotoPortalPrefix                                         | 转到     |        |
| GotoPortalSuffix                                         | 主页     |      |
| LabelTroubleshootingLinkText                             | 查看详细信息           |    |
| LoadingText                                              | 正在加载...     |  |
| NoScriptTagErrorMessage                                  | 浏览器未启用脚本。 请启用脚本并返回到主页，或联系支持人员以寻求帮助。      |   |
| PasswordResetOperationGeneralErrorMessage                | 尝试重置密码时出错了。       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | 密码不符合组织的密码策略。             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | 重置密码时出错了，用户无法更改密码。    |   |
| PrivacyStatement                                         | 隐私声明                                                      |    |
| RegistrationDescription                                  | 自助式密码注册     |    |
| RegistrationMission                                      | 如果忘记了密码，可以自行重置，而无需联系支持人员。   |      |
| RegistrationPageTitle                                    | Forefront Identity Management - 密码注册                                          |   |
| RegistrationSteps                                        | 若要开始注册过程，请单击“下一步”。   |      |
| RegistrationSuccessDescription                           | 现已注册        |   |
| RegistrationSuccessTitle                                 | 已完成:   |    |
| RegistrationWelcomeTitle                                 | 密码注册:    | |
| ResetDescription                                         | 自助服务密码重置  |    |
| ResetEnterNamePrompt                                     | 请在下面输入用户名     |     |
| ResetEnterPassword                                       | 输入新密码:                                                  |   |
| ResetExample1                                            | contoso\\mmeyers                                                                            |      |
| ResetExample2                                            | mmeyers\@contoso.com     |      |
| ResetExamples                                            | 例如：  |    |
| ResetPageTitle                                           | Forefront Identity Management - 密码重置       |     |
| ResetReenterPassword                                     | 重新输入密码:    | |
| ResetSuccessDescription                                  | 你的密码已重置    |    |
| ResetSuccessTitle                                        | 成功：                                |    |
| ResetUseNewPassword                                      | 现在可以使用新密码进行登录。     |      |
| ResetUsernameTextFormat                                  | (重置 {0} 的密码)       | {0} 是用户登录名             |
| ResetWelcomeTitle                                        | 密码重置:     |      |
| TroubleshootingEmailSubject                              | FIM 请求处理错误详细信息     |       |
| TroubleshootingLabelAttributes                           | 属性：    |    |
| TroubleshootingLabelCloseButton                          | 关闭       |    |
| TroubleshootingLabelCopyToClipboard                      | 复制到剪贴板     |     |
| TroubleshootingLabelCorrelationId                        | 相关 ID:      |          |
| TroubleshootingLabelDetails                              | 详细信息:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | 此信息已复制到剪贴板。           |    |
| TroubleshootingLabelRequestId                            | 请求 ID:                  |    |
| TroubleshootingLabelSendEmail                            | 通过电子邮件的方式发送信息                            |    |
| TroubleshootingLabelSource                               | 原因：                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | 查看请求详细信息        |              |                                                    
| TroubleshootingLinkText                                  | 疑难解答信息          |                  | |



下表列出了可以自定义的身份验证入口字符串。

<a name="authentication-gate-strings"></a>身份验证入口字符串
---------------------------

| Name                                          | 默认值                                                                                                                                                                                                                | 备注 |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | 电子邮件地址：             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | 电子邮件地址字段不得为空。     |          |
| OTPEmailRegistrationFooterReadOnly            | 若要更新电子邮件地址，请按照组织定义的流程操作，或联系支持人员。                   |          |
| OTPEmailRegistrationFooterReadWrite           | 组织在 Forefront Identity Manager 中存储电子邮件地址。                       |          |
| OTPEmailRegistrationGateTitle                 | 电子邮件地址验证   |          |
| OTPEmailRegistrationHeaderReadOnly            | 如果需要重置密码，你的电子邮件地址将收到验证安全码。 如果下面的电子邮件地址不正确，必须更新此地址，才能使用自助式密码重置。 |          |
| OTPEmailRegistrationHeaderReadWrite           | 请在下面输入电子邮件地址。 如果需要重置密码，你的电子邮件地址将收到验证码。                 |          |
| OTPEmailResetGateTitle                        | 验证身份:电子邮件地址         |          |
| OTPEmailResetHeader                           | 请在下面输入安全码。 安全码已发送到向此组织注册的电子邮件地址。                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | 指定的值与预期格式不一致。                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | 安全码字段不得为空。        |          |
| OTPResetVerificationLabel                     | 安全码:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | 若要更新移动电话号码，请按照组织定义的流程操作，或联系支持人员。   ||
| OTPSmsRegistrationFooterReadWrite                     | 组织在 Forefront Identity Manager 中存储移动电话号码。                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | 移动电话号码验证                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | 如果需要重置密码，你的移动电话将收到验证安全码。 如果下面的移动电话号码不正确，必须更新此号码，才能使用自助式密码重置。 |   |
| OTPSmsRegistrationHeaderReadWrite                     | 请在下面输入移动电话号码。 如果需要重置密码，你的移动电话将收到验证码。                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | 移动电话号码字段不得为空。      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | 移动电话号码:                    |   |
| OTPSmsResetGateTitle                                  | 验证身份:移动电话号码         |   |
| OTPSmsResetHeader                                     | 请在下面输入安全码。 安全码已发送到向此组织注册的移动电话号码。                                                                                                                           |   |
| PasswordGateDescriptionText                           | 请在下面输入最新密码，再单击“下一步”。        |   |
| PasswordGateErrorMessagePasswordRequired              | 请输入最新密码。                  |   |
| PasswordGateGateTitle                                 | 当前密码               |   |
| PasswordGatePasswordLabelText                         | 密码:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(登录身份: `<b>{0}</b>)</i>`                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | 至少必须回答 {0} 道问题。        |   |
| QAGateIncorrectAnswer                                 | 答案不正确。       |   |
| QAGatePrivacyNotice                                   | 组织在 Forefront Identity Manager 中存储你提供的答案。                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | 至少必须回答 {0} 道问题才能注册。     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | 一个或多个答案不符合策略。      |   |
| QAGateRegistrationThisAnswerValidationFailed          | 此答案不符合策略。      |   |
| QAGateRegistrationTitle                               | 记录答案         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | 必须回答以下 {0} 道(共 {1} 道)问题。   |   |
| QAGateResetTitle                                      | 验证身份:提交答案                                  |   |



## <a name="customizing-the-logo-banner"></a>自定义徽标横幅

可以为组织自定义门户页上的默认横幅。
自定义徽标横幅的具体步骤：

1.  创建自定义横幅，并将横幅保存为 .png 文件。 文件应符合以下建议要求：

 - 大小：490X50 像素。

 - 位深度：32

2.  将文件复制到要自定义的每个门户中的 \\Customizations 文件夹。

3.  在每个文件夹中，创建 Style.css 文件。 让其指向 Customizations 文件夹和新徽标。 如果适用，可以更改徽标名称（即 /Customizations/contosologo.png）。 代码应如下所示：

   **示例 1：**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  如果使用的是 Internet Explorer 6.0，必须提供备用的非透明徽标，并将下面的代码添加到 Style.css 中：

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **示例 2：**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

如果使用的是 Internet Explorer 6.0，必须提供备用的非透明徽标，并将下面的代码添加到 Style.css 中：

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## <a name="customizing-image-for-smartphones"></a>自定义智能手机图像

可以使用下列代码，自定义智能手机图像。 自定义智能手机图像的具体步骤：

1.  创建图像，并将图像保存为 .png 文件。 文件应符合以下建议要求：

    - 大小：190X50 像素。
    - 位深度：32

2.  将文件复制到要自定义的每个门户中的 \\Customizations 文件夹。

2.  在每个文件夹中，创建 Style.css 文件。 让其指向 Customizations 文件夹和新徽标。 如果适用，可以更改徽标名称（即 /Customizations/contosologo.png）。 代码应如下所示：

  **示例 1：**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="customizing-style-sheets"></a>自定义样式表

使用自定义的级联样式表 (CSS)，可以修改密码门户的布局和样式。 使用自定义 CSS 的具体步骤：

1.  创建自定义 CSS 文件，并将文件保存为 Style.css。

2.  将文件复制到要自定义的每个门户中的 \\Customizations 文件夹。

下面展示了 Style.css 文件的基本示例：

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .
title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```


>[!IMPORTANT]
为了让 MIM 能够识别任何自定义更改，必须通过运行 iisreset 来重启 IIS。                                                                                                                                                                                       

下面展示了 Style.css 文件的更高级示例。 此文件提供了智能手机和 ipad 专属信息，以供在这些设备上显示门户。

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/
#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>常见自定义问题

下表列出了升级 FIM 服务和门户时可能会遇到的已知常见问题。 此表还介绍了这些问题的解决方法

|问题 |解决方法                                                                    |
|------|------------------------------|
| 我自定义了字符串，但 UI 中未予以反映         | strings.resources 中的字符串自定义始终需要运行 iisreset         |
| 更改 strings.resources 后，我再也看不到所做的任何字符串更改     | Strings.resources 可能因格式不正确而被门户忽略。 请检查“Windows 日志 - 应用程序和服务日志 - Forefront Identity Manager”下的事件日志                        |
| 首次添加 Style.css 时，我在门户中看不到所做的样式更改            | 首次引入 Style.css 文件时，必须运行 iisreset     |
| 在 Style.css 中添加/修改了新样式，但在浏览器看不到这些更改      | 请清除浏览器缓存并刷新页面 <br/> 请检查 CSS 语法    |
| 我直接在 path_to_sspr_portal\\css\\\*.css 中更改了 CSS 文件夹的内容，或直接在 path_to_sspr_portal\\images\\fimlogo.png 中更改了横幅徽标，但在升级时丢失了这些更改 | 不得从更改这些文件入手。 只能使用 Customization 文件，在 Style.css 中提供横幅徽标和 CSS 样式自定义。 主要升级不会故意覆盖 Customizations 文件夹 <br/><br/>请勿使用 ILSpy 和 Reflector 等工具更改门户程序集中的字符串。 请使用 Strings.resources 替代默认字符串。 升级时，这些程序集会被替换  |
| 门户未显示横幅徽标/仍能看到 FIM 徽标     | Style.css 中的图像名称/路径无效，或未清除浏览器缓存          |
| 横幅徽标在 IE6 中看起来不美观       | 必须为 IE6 提供非透明图像，并在style.css 中随附特殊样式        |
