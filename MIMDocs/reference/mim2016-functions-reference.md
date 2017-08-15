---
title: "Microsoft Identity Manager 2016 函数参考 | Microsoft Docs"
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
ms.openlocfilehash: 8f36cf981971db0d6c55fc17cce874a8faf0ecaf
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2017
---
# <a name="functions-reference-for-microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016 函数参考


在 Microsoft Identity Manager (MIM) 2016 中，可以先使用函数修改属性值，然后再将属性值流式传输到函数活动或声明性预配中的目标。 本文档旨在概览可用函数，并介绍如何使用这些函数。

配置属性流映射是配置同步规则时的基本任务。 属性流映射的最简单形式是直接映射。 顾名思义，直接映射需要使用源属性的值，并将它应用于已配置的目标属性。 有时，需要先修改现有属性值或计算新属性值，然后系统才能将属性值应用于目标。

函数是一种内置方法，用于定义在为目标生成属性值时需要同步引擎应用的修改类型。

在 MIM 中，现有函数可以分为以下几类：

-   **数据操作函数**： 用于对字符串执行各种操作的函数。

-   **数据检索函数**： 用于从属性值中提取数据的函数。

-   **数据生成函数**： 用于生成值的函数。

-   **逻辑函数**。 用于根据条件执行操作的函数。

下面各部分详细介绍了每种函数。

## <a name="data-manipulation-functions"></a>数据操作函数

数据操作函数用于对字符串执行各种操作。

| Concatenate        |   |
|--------------------|-------------------------|
| 说明        | Concatenate 函数用于连接两个或多个字符串。                                                                                                       |
| 函数签名 | string1 + string2...                                                                                                                                                     |
| 输入             | 两个或多个字符串                                                                                                                                                        |
| 操作         | 将所有输入字符串参数相互连接。                                                                                                              |
| 输出             | 一个字符串        |


| UpperCase         |         |
|-------------------|---------|
| 说明        | UpperCase 函数将字符串中的所有字符都转换为大写。         |
| 函数签名 | String UpperCase(string)                                                                                                                                                   |
| 输入             | 一个字符串                                                                                                                                                                 |
| 操作         | 将输入参数的所有小写字符都转换为大写字符。 示例：UpperCase(“test”) 生成“TEST”。                                     |
| 输出             | 一个字符串                                                              |


| LowerCase          |                                 |
|--------------------|---------------------------------|
| 说明        | LowerCase 函数将字符串中的所有字符都转换为小写。                                                                                                  |
| 函数签名 | String LowerCase(string)                                                                                                                                                   |
| 输入             | 一个字符串                                                                                                                                                                 |
| 操作         | 将输入参数的所有大写字符都转换为小写字符。 示例：LowerCase(“TeSt”) 生成“test”。                                     |
| 输出             | 一个字符串               |


| ProperCase        |                                                          |
|-------------------|----------------------------------------------------------|
| 说明        | ProperCase 函数将字符串中以空格分隔的每个字词的首字符转换为大写，并将其他所有字符转换为小写。           |
| 函数签名 | String ProperCase(string)                                                                                                                                                  |
| 输入             | 一个字符串                                                                                                                                                                 |
| 操作         | 将输入参数中以空格分隔的每个字词的首字符转换为大写，并将其他所有大写字符转换为小写。 如果输入参数中的字词以非字母字符开头，那么此字词的首字符不会转换为大写。 <br/> 例如： <br/> - ProperCase(“TEsT”) 生成“Test”。 <br/> - ProperCase(“britta simon”) 生成“Britta Simon”。 <br/>- ProperCase(“ TEsT”) 生成“ Test”。 <br/> - ProperCase(“\$TEsT”) 生成“\$Test”。|
| 输出             | 一个字符串      |


| LTrim              |      |
|--------------------|------|
| 说明        | LTrim 函数删除字符串中的前导空格。                                                                                                             |
| 函数签名 | String LTrim(string)                                                                                                                                                       |
| 输入             | 一个字符串                                                                                                                                                                 |
| 操作         | 删除输入参数中的前导空格字符。 <br/><br/>示例：LTrim(“ Test ”) 生成“Test ”。                                              |
| 输出             | 一个字符串      |



| RTrim              |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| 说明        | RTrim 函数从字符串中删除尾随空格。                                                                 |
| 函数签名 | String RTrim(string)                                                                                                            |
| 输入             | 一个字符串                                                                                                                      |
| 操作         | 删除输入参数中的尾随空格字符。 示例：RTrim(“ Test ”) 生成“ Test”。  |
| 输出             | 一个字符串                                                                                                                      |


| Trim               |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| 说明        | Trim 函数删除字符串中的前导空格和尾随空格。                                                      |
| 函数签名 | String Trim(string)                                                                                                             |
| 输入             | 一个字符串                                                                                                                      |
| 操作         | 删除字符串中的前导和尾随空格字符。 示例：Trim(“ Test ”) 生成“Test”。 |
| 输出             | 一个字符串                                                                                                                      |




| RightPad           |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| 说明        | RightPad 函数使用提供的填充字符，在字符串右侧将字符串填充到指定长度。                          |
| 函数签名 | String RightPad(string, length, padCharacter)                                                                                   |
| 操作         | 如果字符串的长度小于指定长度，那么会向字符串末尾重复追加 padCharacter，直到字符串的长度等于指定长度。 <br/> 例如： <br/> - RightPad(“User”, 10, “0”) 生成“User000000”。 <br/> - RightPad(RandomNum(1,10), 5, “0”) 生成“9000”。   |
| 输出                                                                                                                                                          | 如果字符串的长度大于或等于 length，则返回与 string 相同的字符串。 如果字符串的长度小于 length，则返回具有所需长度的新字符串，其中包含用 padCharacter 填充的字符串。 如果字符串为 null，该函数则返回空字符串。 |   |   |
>[!NOTE]
padCharacter 可以是空格字符，但不能为 null 值。 如果字符串的长度不小于指定长度，那么返回的字符串没有变化。


| LeftPad      |     |
|----|-------|
| 说明  | LeftPad 函数使用提供的填充字符，在字符串左侧将字符串填充到指定长度。    |
| 函数签名      | String LeftPad(string, length, padCharacter)     |
| 输入 |  - **String：** 要填充的字符串。 <br/> - **length：** 一个表示所需的字符串长度的整数。 <br/> - **padCharacter：** 包含一个填充字符的字符串。 |
| 操作  | 如果字符串的长度小于指定长度，那么会向字符串开头重复追加 padCharacter，直到字符串的长度等于指定长度。 <br/> 例如： <br/> - LeftPad(“User”, 10, “0”) 生成“000000User”。 <br/> LeftPad(RandomNum(1,10), 5, “0”) 生成“0009”。 |  
|输出 | 如果字符串的长度大于或等于 length，则返回与 string 相同的字符串。 <br/> 如果字符串的长度小于 length，则返回具有所需长度的新字符串，其中包含用 padCharacter 填充的字符串。 <br/>  如果字符串为 null，此函数返回空字符串。                                                   |

<[!NOTE]
padCharacter 可以是空格字符，但不能为 null 值。 如果字符串的长度不小于指定长度，那么返回的字符串没有变化。

| BitOr    |  |
|----- |------|
| 说明  | BitOr 函数将标志上的指定位设置为 1。     |
| 函数签名  | Int BitOr(mask, flag)       |  
| 输入     | 1. **mask：** 指定要在标志上设置的位的十六进制值。 <br/> 2. **flag：** 要修改特定位的十六进制值。    |   
| 操作         | 此函数将两个参数转换为二进制表示形式，并进行比较： <br/> - 如果 mask 和 flag 中的两个相应位至少有一个为 1，将位设置为 1；如果两个相应位均为 0，将位设置为 0。 <br/> - 除非两个参数的相应位均为 0，否则返回 1。 <br/> - 生成的位模式为两个操作数之一的“set”（1 或 true）位。 如果 mask 中多个位的值为 1，可以设置多个标志位。  |
| 输出             | 将 mask 中指定的位设置为 1 的新版标志。                   |


| BitAnd             |                                                                                    |
|--------------------|------------------------------------------------------------------------------------|
| 说明        | BitAnd 函数将标志上的指定位设置为 0。                           |
| 函数签名 | Int BitOr(mask, flag)                                                              |
| 输入             | 1. **mask：** 指定要在标志上修改的位的十六进制值。 <br/> 2. **flag：** 要修改特定位的十六进制值   |
| 操作         | 此函数将两个参数转换为二进制表示形式，并进行比较： <br/> - 如果 mask 和 flag 中的两个相应位至少有一个为 0，将位设置为 0；如果两个相应位均为 1，将位设置为 1。 <br/> - 除非两个参数的相应位均为 1，否则返回 0。 如果 mask 中多个位的值为 0，可以将多个标志位设置为 0。 |
| 输出             | 将 mask 中指定的位设置为 0 的新版标志。                    |

| DateTimeFormat                                 |    |
|---------------------------------------|------------|
| 说明       | DateTimeFormat 函数用于将字符串形式的日期时间设置为指定格式。     |
| 函数签名   | String DateTimeFormat(dateTime, format)      |
| 输入   | 1. dateTime： 表示要设置格式的日期时间的字符串。  <br/> 2. **format：** 表示转换后格式的字符串。  |   
| 操作           | 将 format 中指定的格式字符串应用于 dateTime 字符串中的日期时间。 <br/> format 中指定的字符串必须是有效的日期时间格式。 否则，将会返回错误，指明 format 指定的日期时间格式无效。 <br/> 示例：DateTime(“12/25/2007”, “yyyy-MM-dd”) 生成“2007-12-25”。|   
| 输出     | 将 format 应用于 dateTime 生成的字符串。   |

>[!Note]                                                                                                                                                                             
若要了解允许用于创建用户定义格式的字符，请参阅[用户定义的日期/时间格式](http://go.microsoft.com/fwlink/?LinkId=195182)


| ConvertSidToString       |    |   
|--------------------------|----|
| 说明       | ConvertSidToString 将包含安全标识符的字节数组转换为字符串。         |
| 函数签名      | String ConvertSidToString(ObjectSID)    |
| 输入  | **ObjectSID：** 包含安全标识符 (SID) 的字节数组。   |
| 操作    | 将指定的二进制 SID 转换为字符串。    |
| 输出              | 字符串表示形式的 SID。   |  

| ConvertStringToGuid |        |
|---------------------|--------|
| 说明         | ConvertStringToGuid 函数将字符串表示形式的 GUID 转换为二进制表示形式的 GUID。      |
| 函数            | Byte[] ConvertStringToGuid(stringGuid)  |  
| 输入              | **Guid：** 采用 xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx 格式模式的字符串，其中 GUID 值表示为一系列用连字符隔开的十六进制数，每个分组的十六进制数为 8 个、4 个、4 个、4 个和 12 个。 例如，返回值为“382c74c3-721d-4f34-80e557657b6cbc27”。  |
| 操作          | 将参数 1 中指定的字符串 Guid 转换为二进制表示形式。 <br/> 如果字符串未采用有效的 Guid 表示形式，此函数会拒绝参数，并显示以下错误： <br/> ConvertStringToGuid 函数的参数必须是表示有效 Guid 的字符串。  |
| 输出              | 二进制表示形式的 Guid。           |                                                                           


| ReplaceString       |     |
|--------------------|-------|
| 说明         | ReplaceString 函数在一个字符串出现的所有地方将它全部替换为另一个字符串。  |   
| 函数            | String ReplaceString(string, OldValue, NewValue)    |                                                                          
| 输入              | 1.**String：** 要替换其值的字符串。 <br/> 2.**OldValue：** 要搜索并替换的字符串。 <br/> 3.NewValue： 要替换成的字符串。 |
| 操作          | 将字符串中出现的所有 OldValue 全部替换为 NewValue。 此函数必须能够处理以下特殊字符： <br/> - **\n：** 换行符。 <br/> - **\r：** 回车。 <br/> - **\t：** 制表符。 <br/> 示例：ReplaceString(“One\n\rMicrosoft\n\r\Way”,”\n\r”,” “) 返回“One Microsoft Way”。 |   
| 输出              | 出现的所有 OldValue 全部替换为 NewValue 的字符串。      |

## <a name="data-retrieval-functions"></a>数据检索函数

数据检索函数用于执行操作，以便从字符串中检索相应字符。

| Word       |        |
|--------------------|---------------|
| 说明        | Word 函数根据描述要使用的分隔符和要返回的单词编号的参数，返回字符串中包含的一个单词。                                                                |
| 函数签名 | String Word(string, number, delimiters)                                                                                                                                                                        |
| 输入             | 1. **string：** 要返回其字词的字符串。 <br/> 2. **number：** 指明应返回第几个字词的数字。 <br/> 3. **delimeters：** 表示应用来识别字词的分隔符的字符串。 |
| 操作         | 将字符串中以分隔符之一分隔的每个字符串识别为一个字词。 返回在参数 2 (number) 指定的位置找到的字词： <br/> - 如果 number < 1，返回空字符串。 <br/> - 如果字符串为 null，返回空字符串。 <br/><br/> 例如： <br/> 1.Word(“Test;of%function;”, 3, “;$&%”) 返回“function”。 <br/> 2.Word(“Test;;Function” , 2 , “;”) 返回“”（空字符串）。 3. Word(“Test;of%function;”, 0, “;$&%”) 返回“”（空字符串）。
| 输出             | 包含用户指定位置处的字词的字符串。 如果字符串的字词数少于应返回的字词数，或字符串不包含通过分隔符识别的任何字词，返回空字符串。 |  


| 左               |   |
|-------|-------|
| 说明        | Left 函数从字符串的左侧返回指定数目的字符。       |
| 函数签名 | String Left(string, numChars)     |
| 输入             | 1. **string：** 要返回其字符的字符串。 2. **numChars：** 指明要返回字符串开头前几个字符的数字。         |
| 操作         | 返回字符串开头前 numChars 个字符。 <br/> 示例：Left(“Britta Simon”, 3) 返回“Bri”。   |
| 输出             | 包含字符串中前 numChars 个字符的字符串。  <br/> - 如果 numChars = 0，返回空字符串。 <br/> - 如果 numChars < 0，返回输入字符串。 <br/> - 如果字符串为 null，返回空字符串。 |




| 右       |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| 说明 | Right 函数从字符串的右侧（末尾）开始返回指定数目的字符。                                 |
| 函数签名   | String Right(string, numChars)   |
| 输入      | 1.**String：** 要返回其字符的字符串。 <br/> 2. **numChars：** 指明要返回字符串末尾后几个字符的数字。  |
| 操作  | 返回字符串末尾后 numChars 个 字符。 <br/> 示例：Right(“Britta Simon”, 3) 返回“mon”。                  |
| 输出      | 包含字符串中最后 numChar 个字符的字符串。 如果 numChars = 0，返回空字符串。 <br/> - 如果 numChars < 0，返回空字符串。 <br/> - 如果字符串为 null，返回空字符串。 <br/> - 如果字符串包含的字符数小于 numChars 指定的数字，返回完全相同的字符串。 |




| Mid         |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| 说明 | Mid 函数从字符串中的指定位置返回指定数目的字符。                              |
| 函数签名    | String Mid(string, pos, numChars)                                                                                             |
| 输入      | 1. **string：** 要返回其字符的字符串。   <br/> 2. **pos：** 指明字符串中用于返回字符的起始位置的数字。 <br/> 3. **numChars：** 指明要从字符串中的位置起开始返回几个字符的数字。  |
| 操作  | 从字符串中的位置 pos 起开始返回 numChars 个字符。 <br/>示例：Mid(“Britta Simon”, 3, 5) 返回“itta ”。 |
| 输出      | 包含从字符串中的位置 pos 起开始返回的 numChars 个字符的字符串： <br/> - 如果 numChars = 0，返回空字符串。 <br/> - 如果 numChars < 0，返回空字符串。 <br/> - 如果 pos 大于字符串的长度，返回输入字符串。 <br/> - 如果 pos ≤ 0，返回空字符串。 <br/> - 如果字符串为 null，返回空字符串。 <br/> 如果从字符串中的位置 pos 起没有 numChar 个字符剩余，有多少字符就返回多少。

## <a name="data-generation-functions"></a>数据生成函数

数据生成函数用于生成特定数据类型的值。

| CRLF               |                                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| 说明        | CRLF 生成回车/换行。 此函数可用于添加新行。 |
| 函数签名 | String CRLF                                                                              |
| 输入             | 无参数                                                                            |
| 操作         | 返回 CRLF。                                                                      |
|                    | 示例：AddressLine1 + CRLF() + AddressLine2 生成： <br/> - AddressLine1 <br/> - AddressLine2 |
| 输出             | 输出是 CRLF。                                                                                             |

| RandomNum          |                                                                                                                   |  
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| 说明        | RandomNum 函数返回指定间隔内的随机数。                                       |   
| 函数签名 | Int RandomNum(start, end)                                                                                         |   
| 输入             | - **start**： 指明要生成的随机值下限的数字。   <br/> - end。 指明要生成的随机值上限的数字。  |
| 操作         | 生成不小于 start 且不大于 end 的随机数。 <br/>  示例：Random(0,999) 返回 100。                      |
| 输出             | 由 start 和 end 界定的范围内的随机数。                                                      |  

| EscapeDNComponent  |                                                                                                                   |   
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| 说明        | EscapeDNComponent 方法根据在使用的管理代理类型处理输入字符串。 |
| 函数签名 | String EscapeDNComponent(string)                                                                                  |
| 输入     | **string**： 用于处理可分辨名称的字符串。 此字符串不得包含转义字符。 |
| 操作 | MIISUtils 提供的 EscapeDNComponent 方法用于执行此操作。 此方法根据在使用的管理代理类型处理输入字符串。 <br/> 因为不同的管理代理要求的可分辨名称格式也不同，所以此方法根据管理代理类型来处理输入字符串。 类型包括轻型目录访问协议 (LDAP) 可分辨名称，如 Active Directory® 域服务、Sun Directory Server（前身为 iPlanet Directory Server）、Microsoft Exchange Server；分层的非 LDAP，如 Microsoft Lotus Notes；以及外部类型，如不含 LDAP 可分辨名称的数据库和 XML。 <br/> **LDAP 可分辨名称：** <br/> - 对给定部分的值部分中的任何无效 XML 字符进行十六进制编码。 <br/>- 给定部分的名称部分中的任何非法字符（包括无效的 XML 字符）都会生成错误。 <br/> - 转义下列字符： <br/> &nbsp;&nbsp;&nbsp; - 逗号（“,”） <br/> &nbsp;&nbsp;&nbsp; - 等于号（“=”） <br/> &nbsp;&nbsp;&nbsp; - 加号（“+”） <br/> &nbsp;&nbsp;&nbsp; - 小于号（“<”） <br/> &nbsp;&nbsp;&nbsp; - 大于号（“>”） <br/> &nbsp;&nbsp;&nbsp; - 数字符号（“#”） <br/> &nbsp;&nbsp;&nbsp; - 分号（“;”） <br/> &nbsp;&nbsp;&nbsp; - 反斜杠（“\'） <br/> &nbsp;&nbsp;&nbsp; - 引号（“"”） <br/> - 如果字符串中的最后一个字符是空格，转义此空格。 <br/> - 删除部分名称周围任何无关的前导或尾随空格。 <br/> - 对于 XML 管理代理，如果有多个部分，按字母顺序排列这些部分。 <br/> - 如果指定了多个部分，复合可分辨名称字符串是将各个字符串串联，并用加号隔开。 <br/> - 如果输入字符串不是格式标准的 LDAP 可分辨名称字符串，则会生成错误。 <br/><br/> **分层的非 LDAP** <br/> - 此类管理代理不支持多个部分。 如果将多个字符串传递给 EscapeDNComponent，则会引发 ArgumentException。 <br/> - 如果输入字符串中的所有字符都是无效的 XML 字符，则会引发 ArgumentException。 <br/> - 转义输入字符串中的所有逗号和反斜杠。 <br/> - 如果字符串中的最后一个字符是空格，转义此空格。 <br/><br/> **外部：** <br/> 1.如果任何部分是二进制类型或包含无效的 XML 字符，将此部分存储为原始数据的十六进制编码版本，并在字符串前面加上“#”字符作为前缀。 例如，如果某部分是“AxC”（其中 x 表示非法的 XML 字符，如“0x10”），将此部分编码为“#410010004300”。 <br/> 2.在其他情况下，转义以下字符的所有实例： <br/> &nbsp;&nbsp;&nbsp; - 反斜杠（“\'） <br/> &nbsp;&nbsp;&nbsp; - 逗号（“,”） <br/> &nbsp;&nbsp;&nbsp; - 加号（“+”） <br/> &nbsp;&nbsp;&nbsp; - 数字符号（“#”） <br/> 3.如果给定部分字符串中的最后一个字符是空格，转义此空格。 <br/> 4.如果指定了多个部分，复合可分辨名称字符串是将所有字符串串联，并用加号隔开。
| 输出      | 包含有效域名的字符串。                                                                                                                  |   

>[!NOTE]
可分辨名称的验证不如 LDAP 规范中定义的语法严格。 EscapeDNComponent(String[]) 允许部分名称包含以下一个或多个字符的组合：“a”-“z”、“A”-“Z”、“0”-“9”、“-”和“.”。 <br/>
使用此方法，无法指定二进制部分。 不过，如果可分辨名称是通过定位属性构成，并且其中一个定位属性是二进制类型，那么就可以在 CommitNewConnector 中指定二进制部分。


| Null        |                                                                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 说明 | Null 函数用于定义此 MA 没有可归因属性，属性优先级应继续传递给下一个 MA。 |   
| 函数签名    | String Null    |
| 输入      | 无参数                                                                                                                                             |   
| 操作  | 返回 Null。 <br/> 示例：IIF(Eq(domain), “unknown”, Null())                                                                                           |   
| 输出      | 输出是 Null。                                                                                                                                         |   |   |


## <a name="logic-functions"></a>逻辑函数
逻辑函数用于根据系统求出的条件执行操作。

| IIF        |  |
|-------------|---|
| 说明 | IIF 函数根据指定的条件返回一组可能值中的一个。    |
| 函数签名   | Object IIF(condition, valueIfTrue, valueIfFalse)   |                                                 |
| 输入      | 1.**Condition**： 计算结果为 true 或 false 的任何值或表达式。 2. **valueIfTrue**：条件计算结果为 true 时返回的值。 <br/> 3. **valueIfFalse**：条件计算结果为 false 时返回的值。 <br/><br/> 下面列出了可用作 IIF 函数中的 condition 表达式的函数： <br/> **Eq：** 此函数比较两个参数是否相等。 <br/> **NotEquals：** 此函数比较两个参数是否不相等。如果不相等，返回 true；如果相等，返回 false。<br/> 示例：NotEquals(EmployeeType, "Contractor")<br/> **LessThan：** 此函数比较两个数字的大小。如果第一个数字小于第二个数字，返回 true；否则，返回 false。<br/>示例：LessThan(Salary, 100000) <br/>**GreaterThan：** 此函数比较两个数字的大小。如果第一个数字大于第二个数字，返回 true；否则，返回 false。<br/> 示例：GreaterThan(Salary, 100000) <br/> **LessThanOrEquals：** 此函数比较两个数字的大小。如果第一个数字不大于第二个数字，返回 true；否则，返回 false。<br/>示例：LessThanOrEquals(Salary, 100000) <br/> **GreaterThanOrEquals：* 此函数比较两个数字的大小。如果第一个数字不小于第二个数字，返回 true；否则，返回 false。 <br/>示例：GreaterThanOrEquals(Salary, 100000)<br/> IsPresent： 此函数将 ILM 架构中的属性用作输入。如果属性不为 null，返回 true；如果属性为 null，返回 false。|
| 操作  | 如果 condition 计算结果为 true，返回 valueIfTrue。 否则，返回 valueIfFalse。 <br/>示例：如果用户是实习生，IIF(Eq(EmployeeType,”Intern”),”t-“ + Alias, Alias) 返回开头添加有“t-”的用户别名。 否则，按原样返回用户别名。 |
| 输出      | 如果条件为 true，输出为 valueIfTrue；如果条件为 false，输出为 valueIfFalse。 |      
