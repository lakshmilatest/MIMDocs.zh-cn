---
title: "资源控制显示配置 XML 参考 | Microsoft Docs"
description: 
keywords: 
author: fimguy
ms.author: fimguy
manager: mbaldwin
ms.date: 05/1/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c6ed843fb15b150fc934062945ab76ba32d72d33
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/13/2017
---
# <a name="resource-control-display-configuration-xml-reference"></a>资源控制显示配置 XML 参考

资源控制显示配置 (RCDC) 资源是用户定义的资源，可用于控制 Microsoft Identity Manager 2016 SP1 (MIM) 数据存储中的其他资源如何在用户界面 (UI) 中显示给最终用户。 每个 RCDC 资源都包含可以更改的 XML 配置文件，用于添加、修改或删除 UI 文本和 UI 控件。 虽然 MIM 2016 SP1 提供了多个默认 RCDC 资源，但也可以为自定义资源创建自定义 RCDC 资源。 若要详细了解如何在 FIM 门户中使用 RCDC UI，请参阅 FIM 文档中的[配置和自定义 FIM 门户简介](http://go.microsoft.com/fwlink/?LinkID=165848)。


## <a name="known-issues"></a>已知问题

许多资源控制显示配置控件中的默认值不受支持

在此版本中，除选项按钮控件之外，不支持在资源控制显示配置控件中设置默认值。 若要对下拉框解决此问题，可以指定与任何值都不相关的默认值，迫使用户更改选择。 若要对其他控件解决此问题，需要在提交请求期间，使用授权工作流提供默认值。

## <a name="basic-structure"></a>基本结构

RCDC 资源的 XML 数据由一个 XML 元素 ObjectControlConfiguration 组成。

>[!NOTE]
有关完整的 XSD 架构，请参阅本文档稍后的“附录 A：默认 XSD 架构”部分。

下面展示了 ObjectControlConfiguration 元素的 XSD 架构：

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```



ObjectControlConfiguration 元素包含以下元素：

1.  **ObjectDataSource**：此元素指定资源控制 (RC) 使用的数据源类的 TypeName。 有关说明和架构定义，请参阅本文档稍后的“数据源”部分。 ObjectControlConfiguration 元素最多可以包含 32 个节点的 ObjectDataSource 元素。

2.  **XmlDataSource**：这是简单数据源，最常用于指定摘要页设计。 有关说明和架构定义，请参阅本文档稍后的“数据源”部分。 ObjectControlConfiguration 元素最多可以包含 32 个节点的 XmlDataSource 元素。

3.  **Panel**：管理员可以修改 Panel 元素内的元素，从而自定义 RCDC 页的布局。 有关详细信息，请参阅本文档稍后的“Panel”部分。 ObjectControlConfiguration 元素只能有一个 Panel 元素。

4.  **Event**：管理员无法在后台提供自定义代码，此为受限功能。 这是面板或控件可以根据状态变化而触发的 Event。 有关详细信息，请参阅本文档稍后的“Event”部分。 ObjectControlConfiguration 元素可以视需要包含一个 Event 元素。 通常不支持使用自定义 Event，除非在更高版本的增强功能中专门开发。

## <a name="data-sources"></a>数据源

Microsoft Identity Manager 使用数据源将数据绑定到 UI 组件。 这有助于区分数据和表示层。 RCDC 资源配置数据的数据源有以下两种：ObjectDataSource 和 XmlDataSource。

-   **ObjectDataSource**：指定向 RC 提供数据的 Microsoft .NET 类。 管理员可以从一组固定的 ObjectDataSource 类型中进行选择，以用于创建 RCDC。

-   **XMLDataSource**：用于轻松构造基于 XML 的数据，管理员可用来提供自定义数据。 必须在 RCDC 中直接指定 XML 数据，除非使用内置的预定义 XML 结构。 内置的 XML 结构用于在 RC 中生成摘要页。

在 RCDC 中，可以将这些数据源绑定到 RCDC 中指定的 UI 控件属性，从而生成 UI。

### <a name="objectdatasource"></a>ObjectDataSource

Microsoft Identity Manager 提供下表中的常见数据源类型，适用于所有资源类型（除非另有说明）。

| TypeName                        | 说明     | 支持双向绑定 | 支持的绑定语法        |
|----------------|-----------|-------------------------|--------------|
| PrimaryResourceObjectDataSource | 这表示正在创建、编辑或查看的 FIM 2010 资源。 绑定字符串中的路径是属性名称。 请注意，资源类型是通过 RCDC 的 TargetObjectType 属性指定，而不是在 RCDC.ConfigurationData 属性中指定。 | 是                     | 名称给定的对象属性的 [AttributeName] 值。    |
| PrimaryResourceDeltaDataSource  | 此数据源生成增量 XML，用于比较 FIM 2010 资源的原始状态与当前状态。 RC 摘要控件使用生成的增量 XML，针对用户正在提交的请求来呈现 UI。                                    | 否                      | DeltaXml： </br> 与摘要控件配合使用，以显示增量。                                                 |
| PrimaryResourceRightsDataSource | 此数据源为 FIM 2010 资源的每个属性提供内联的权限。 这样，RC 可以在提交发生前确定用户对相应属性拥有哪些权限，再适当地针对相应属性呈现 UI。                     | 否                      | [AttributeName]                                                                                         |
| SchemaDataSource                | 此数据源可用于访问与架构相关的信息，如显示名称、说明、属性是否必需以及资源类型信息。                                                                                             | 否                      | [AttributeName].Required 布尔值，指明属性是否必须包含有效值。 <br/> [AttributeName].DisplayNameString 值，指明绑定的显示名称 <br/>[AttributeName].DescriptionString 值，指明绑定的说明 <br/>[AttributeName].StringRegexString 值，指明绑定的字符串正则表达式。 <br/>[AttributeName].DisplayName <br/> [AttributeName].Description <br/> [AttributeName]. [AttributeName].IntergerValueMinimum <br/>[AttributeName].IntergerValueMaximum <br/>[AttributeName].LocalizedAllowedValues|
| DomainDataSource                | 此数据源根据域配置资源枚举域。 请注意，此数据源只能用于组资源和用户资源的 RCDC。                                                                           | 是                     | Domain           |

下面的 RCDC 代码段示例可将三个数据源绑定到 UocTextBox 控件，以编辑组的 Description 属性：

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource"></a>XMLDataSource

使用 XMLDataSource，可以指定 RCDC 能对给定资源使用的自定义数据。 在这种情况下，必须在 RCDC 中指定 XML 数据。 也可以使用此数据源引用内置的 XML 数据结构，从而呈现摘要页的 UI。 在 RCDC 中定义时，可以控制要使用的 XMLDataSource 类型。


| TypeName                 | 说明   | | |
|--------------------------|------------|
| **XMLDataSource**            | 此数据源表示 XML 数据。 可以采用 XSL 或嵌入的 XSL 格式： <br/>**XSL 格式：** <br/> Microsoft.IdentityManagement.WebUI.Controls.dll<my:XmlDataSource my:Name=" <br/>summaryTransformXsl"my:Parameters=”Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl”> </my:XmlDataSource><br/> **嵌入的 XSL 格式：** <br/> <my:XmlDataSource my:Name="RequestStatusTransformXsl"> <br/> <xsl:stylesheet version="1.0" xmlns:xsl=http://www.w3.org/1999/XSL/Transform <br/> xmlns:msxsl="urn:schemas-microsoft-com:xslt"><br/></xsl:stylesheet></my:XmlDataSource>                       |否 | ```Xpath[;namespaces]``` <br/> 其中，Xpath 是有效的 XML xpath，用于选择相应注释，通常为“/”（根） <br/>命名空间是前缀为 URI 的字符串的可选分号分隔列表。如果 xpath 要处理 XML 命名空间，则为必需列表。 |
| **ReferenceDeltaDataSource** | 此数据源表示多值引用属性的增量。 仅适用于 Group 和 Set 的 RCDC。 <br/> 虽然数据源并不仅限于 Group 或 Set，但必须在 RCDC 主机中更改代码，才能提交此类增量。 目前，Group 和 Set 是识别此数据源的唯一主机。  | 是                      | [AttributeName].Add，其中 [AttributeName] 表示引用属性，返回数据为增量添加。 <br/> 示例：[ReferenceAttribute].Add <br/>示例： ```<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}" />``` <br/>[AttributeName].Remove，其中 [AttributeName] 表示引用属性，返回数据为增量删除。 <br/> DeltaXml |
|**RequestDetailsDataSource**| 此数据源表示 Request 对象的 RequestParameter 属性。 此参数规定了每个多值属性可以显示的属性值数量上限。仅适用于 Request 的 RCDC。 ```<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />```| 否 | DeltaXml |
|**RequestStatusDataSource**| 此数据源表示 Request 对象的 RequestStatusDetails 属性。 <br/>仅适用于 Request 的 RCDC。  | 否 | DeltaXml |

-   定义自定义 XML 数据源的具体方法：

 ```XML
   <my:XmlDataSource my:Name="MyCustomData" >
   %Insert custom, properly formatted XML data here%
   </my:XmlDataSource>
   ```

-   若要使用内置的摘要控件 XSL，请定义数据源，如下所示：

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

 若要创建自定义资源类型的 RCDC，可以使用此方法自动呈现相应自定义资源的摘要页。

下面的示例展示了如何通过内置的 XSL，将 PrimaryResourceDeltaDataSource 和 XMLDataSource 结合使用，从而在 RCDC 中创建摘要选项卡：

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

用户也可以使用以下格式替换之前指定的 XmlDataSource 元素，从而定义摘要页的自定义布局。 可以参考本文档稍后的“附录 B：默认摘要 XSL”中的默认 FIM 2010 摘要 XSL。

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>数据源架构
下面展示了两种数据源的 XSD 架构：

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="events"></a>事件
Event 定义了 Control 的状态变化。 此功能的扩展性受限，因为无法写入自定义函数（处理程序）来定义事件触发后的行为。 可以在 Panel 元素中使用同一 Event 元素。 有关详细信息，请参阅本文档稍后的“Panel”部分。 下面展示了 Event 元素的 XSD 架构：

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```


Event 为空元素，包含两个属性。

**属性：**

1.  **Name**：这是事件的唯一名称。 ObjectControlConfiguration 只支持 Load 事件。 此事件在页面首次加载时触发。

2.  **Handler**：这是处理程序的唯一名称。 事件触发时，通常会调用程序方法来处理控件的状态更改。 不支持执行以下操作：从现有控件中删除现有处理程序、新建处理程序，以及将处理程序附加到现有或新的控件。

示例：

下面的示例展示了 Event 元素。
```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

**Panel**


Panel 元素是 RCDC 布局中的核心元素。 下面展示了 Panel 元素的 XSD 架构：

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

此元素包含重复元素 Grouping。 有关详细信息，请参阅本文档的“Grouping”部分。

Panel 元素包含以下四个属性：

1.  **Name**：Panel 的名称。 此为必需的字符串类型属性。

2.  **DisplayAsWizard**：此属性目前已遭弃用。 RCDC 上的相应 VerbContext 属性用于控制资源布局处于向导模式，还是选项卡模式。 如果设置为 0（创建模式），也处于向导模式。 否则，处于选项卡模式。 有关详细信息，请参阅 FIM 文档中的的“配置和自定义 FIM 门户简介”。

3.  **Caption**：此属性目前已遭弃用。 用户可以添加仅包含标题信息的 Group，从而指定页面的描述文字。 有关详细信息，请参阅本文档的“Grouping”部分。

4.  **AutoValidate**：此为可选的布尔属性。 如果设置为 True，将对当前选项卡上的每个控件触发验证。 默认情况下，如果此属性缺失，则表示设置为 True。 可与 RegularExpression 属性结合使用。 有关详细信息，请参阅本文档稍后的“RegularExpression”部分。

## <a name="grouping"></a>Grouping

Grouping 元素定义了 Panel 的总体布局。 此元素用作容器，可将各个控件分入不同的部分和选项卡。 下面展示了 Grouping 元素的 XSD 架构：

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```


Grouping 元素有以下三种类型：

1.  **Header Grouping**：Header Grouping 为可选类型。 Panel 中只能有一个 Header Grouping。 Header Grouping 作为描述文字显示在面板顶部。
    只能在此类 Grouping 元素中使用一个 UocCaptionControl。 有关 Header Grouping 示例，请参阅“示例”部分。

2.  **Content Grouping**：至少必须有一个 Content Grouping。 Panel 中可以有多个 Content Grouping。 Content Grouping 显示为 RCDC 页的主要内容。 同一 Panel 中的每个 Content Grouping 均显示为选项卡，可以包含 1 到 256 个控件。 有关 Content Grouping 示例，请参阅下面的“示例”部分。

3.  **Summary Grouping**：Summary Grouping 为可选类型。 Panel 中只能有一个 Summary Grouping。 Summary Grouping 显示为 Panel 的最后一个选项卡。 只能在 Summary Grouping 中使用一个 UocHtmlSummary 控件，以显示用户在提交请求之前所做的更改。 有关 Summary Grouping 示例，请参阅下面的“示例”部分。

每种 Grouping 均包含下列元素：

1.  **Help**：此元素在选项卡中提供 Help 文本。 还可用于添加指向选项卡的 Help 文件的链接。

2.  **Control**：若要了解此元素，请参阅本文档的“Control”部分。 每个 Grouping 必须包含 1 到 256 个（含限值）控件，具体视 Grouping 类型而定。

3.  **Event**：若要了解此元素，请参阅本文档的“Event”部分。 每个 Grouping 可以视需要包含一个 Event。 Grouping 元素支持以下 Event：

    - **BeforeLeave**：当用户准备离开 Content Grouping 选项卡时，此 Event 触发。
    - **AfterEnter**：当用户准备进入 Content Grouping 选项卡时，此 Event 触发。

属性：

1.  **Name**：这是 Grouping 的必需名称。 在 Panel 中，Name 必须是唯一的。

2.  **Caption**：Caption 显示为 Header Grouping 中的描述文字。 还可显示为 Content Grouping 或 Summary Grouping 的选项卡描述文字。

3.  **Description**：可选的字符串属性。仅当用于 Content Grouping 时，Description 才能发挥作用。 此元素可用于在同一选项卡中为最终用户提供一些详细信息。

  >[!NOTE]
  如果此属性用于 Summary Grouping，则 XML 被视为无效。 如果此属性用于 Header Grouping，则 XML 被视为有效，但会遭到忽略。

4.  **Enabled**：此为可选的布尔属性。如果缺失，Enabled 设置为 True。 如果将 Enabled 设置为 False，最终用户看到的是 Disabled 选项卡。 仅当用于 Content Grouping 时，此属性才能发挥作用。

  >[!NOTE]
  如果此属性用于 Summary Grouping，则 XML 被视为无效。 如果此属性用于 Header Grouping，则 XML 被视为有效，但会遭到忽略。

5.  **Visible**：通过将此属性设置为 False，可以隐藏 RCDC 页的选项卡或其标题。 这个可选的布尔类型属性默认设置为 True。 仅当用于 Content Grouping 时，此属性才能发挥作用。

  >[!NOTE]
  如果 Panel 中只有一个 Content Grouping，此功能无法正常运行。 如果 Panel 中有多个 Content Grouping，此属性的行为如上文所述。

6.  **IsHeader**：此属性是可选的布尔属性，用于定义 Grouping 是否为 Header Grouping。 如果未指定此属性，则设置为 False。

7.  **IsSummary**：此属性是可选的布尔属性，用于定义 Grouping 是否为 Summary Grouping。 如果未指定此属性，则设置为 False。

![RCD 配置 XML](media/rcd-configuration-xml-reference/image005.jpg)

下面的 XML 示例代码生成上面的 Header Grouping。 Header Grouping 区域内包含文本“Sample Header Grouping”。

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

![RCD 配置 XML](media\rcd-configuration-xml-reference/image007.jpg)

下面的 XML 示例代码生成上面的 Content Grouping。 Content Grouping 是最左侧显示文本“Sample Content Grouping”的选项卡。

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

![RCD 配置 XML](media/rcd-configuration-xml-reference/image010.jpg)

下面的 XML 示例代码生成上面的 Summary Grouping。 Summary Grouping 是最右侧显示文本“Summary”的选项卡。

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```
### <a name="help"></a>帮助

Help 元素可以作为选项包含在 Grouping 或 Control 元素中。 如果用于 Grouping，必须是首个使用元素。 可用于为最终用户提供文本 Help，以帮助他们提供准确信息。 下面展示了 Help 元素的 XSD 架构：

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```
下面的示例展示了 Help 元素：
```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control"></a>控制

Grouping 元素包含一个或多个 Control 元素。 Control 是 RCDC 中的主要元素。 可通过定义所含的不同 Control 元素，自定义 Grouping 元素。 下面展示了 Control 元素的 XSD 架构：

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

Control 包含以下元素：

1.  **Help**：此元素会遭到忽略。 仅当用于 Grouping 时，此元素才能发挥作用。

2.  **CustomProperty**：不支持此元素。

3.  **Option**：此元素仅与 UocDropDownList 或UocRadioButtonList Control 配合使用。 无法与其他任何控件配合使用。 有关此元素的结构，请参阅本文档的“Option”部分。 请参阅“具体 Control”，了解如何在 Control 的上下文中使用它们。

4.  **Button**：此元素仅与 UocListView Control 配合使用。 无法与其他任何控件配合使用。 有关详细信息，请参阅本文档的“UocListView”部分。

5.  Property：此元素用于在所有 Control 中指定 Control 的其他行为。 若要了解此元素，请参阅本文档的“Property”部分。

6.  **Event**：有关此元素的结构，请参阅本文档前面的“Event”部分。 请参阅各个 Control 定义，以确定在此控件中使用哪个事件。

Control 包含以下属性：

1.  **Name**：这是 Control 的名称。 Control 的名称在每个面板内必须是唯一的。 此为必需的字符串类型属性。

2.  **TypeName**：此属性指定 Control 的类型。 此为必需的字符串类型属性。 有关每个控件的名称，请参阅本文档的“具体 Control”部分。

3.  **Caption**：此属性可用于添加控件的描述文字。
    描述文字通常是控件要显示或输入的数据的显示名称。 可以显式指定描述文字值，也可以将它与架构属性显示名称信息一起绑定。 描述文字显示在正常大小控件的最左侧。 如果控件横跨整个屏幕，描述文字显示在控件前方。 此为可选的字符串类型属性。 若要了解如何将数据源与属性或属性值一起绑定，请参阅“Property”部分。

   下面的示例展示了如何显式使用描述文字：

   ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
   ```
     下面的示例展示了如何将描述文字与数据源结合使用。 如果已将模板用于数据源（如本文档前面所述），那么数据源为架构。 建议将属性的 DisplayName 与 Caption 属性一起绑定。

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```
4.  Enabled：此为可选的布尔类型属性。 通过将此属性的值设置为 False，用户可以禁用 Control。 默认值设置为 True。

5.  Visible：此为可选的布尔类型属性。 此属性可用于隐藏整个控件。 默认值设置为 True。

6.  Description：此为可选的字符串类型属性，可用于添加说明，以帮助最终用户了解应在控件中输入什么或控件用途。 可以显式指定说明值，也可以将它与架构属性说明信息一起绑定。 <br/>Description 显示在正常大小控件最左侧的描述文字下方。 如果控件横跨整个屏幕，说明显示在控件顶部的描述文字下方。 若要了解如何将数据源与属性或属性值一起绑定，请参阅本文档的“Property”部分。

7.  下面的示例展示了如何显式使用 Description：
  ```XML
  <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
  ```
  下面的示例展示了如何将 Description 与数据源结合使用。 如果已将模板用于数据源（如本文档前面所述），那么数据源为架构。 建议将属性的 Description 与 Description 属性一起绑定。
  ```XML
  <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
  ````
8. ExpandArea：此属性指明控件是否横跨整个屏幕。 此为可选的布尔类型属性。 默认值设置为 False。

    >[!NOTE]
    如果此属性设置为 True，将禁用 Caption 和 Description 属性。 必须使用 UocLabel 控件，才能为展开的控件提供描述文字。
9. **Hint**：此为可选的字符串类型属性。 Hint 属性中的文本有助于最终用户决定控件的有效输入。 Hint 显示在控件下方。

10.  **AutoPostback**：此为可选的布尔类型属性。 默认值为 false。 如果设置为 False，页面刷新可能无法刷新控件。 若要了解 AutoPostback，请参阅同名的 Microsoft ASP.NET UI 控件属性。

11. **RightsLevel**：此为可选的字符串类型属性。 只能将此属性与具有数据源的内联权限一起绑定。 此控件根据用户权限动态启用或禁用。 若要了解如何将数据源与属性或属性值一起绑定，请参阅本文档的“Property”部分。

    下面的示例展示了如何将 RightsLevel 与数据源结合使用。 如果已将模板用于数据源（如本文档前面所述），那么数据源为权限。 将属性名称用作 Path。

### <a name="properties"></a>属性

可以使用 Property 进一步自定义每个控件的行为。 Property 为空元素。 下面展示了 Property 元素的 XSD 架构：
```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```



每个 Property 都有以下两个必需属性：

1.  **Name**：此字符串类型属性是 Property 的唯一名称。
    Property 因控件而异。 所有控件可使用一些通用属性。 若要详细了解哪些名称可用于给定控件，请参阅“通用属性”和“具体 Control”部分。

2.  **Value**：这是 Property 的值。 值的数据类型取决于所分配到的属性。 若要了解特定属性允许使用的值格式，请参阅以下部分。

一些 Property 可以与数据源中的信息一起绑定。 为此，需要采用以下字符串格式。 请参阅“具体 Control”部分中的各个属性，了解如何将它们与数据源一起绑定。

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}”

SourceExpression:= “Source=” + [ObjectDataSourceName]

PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

ModeExpression:= “Mode=” + [ModeChoice]

ModeChoice:= “OneWay”|”TwoWay”

ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

AttributeName:= valid schema attribute name from the data source.

AttributePropertyName:= valid property name of a schema attribute from the data source.

````
**示例：**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>

```



<a name="common-properties"></a>通用属性
-----------------

本文档中指定的所有 RCDC 控件都可以具有下列通用属性。 可以将这些 Property 与各个控件的其他专属 Property 结合使用。

1.  Required：此属性指明字段是必填字段，还是可选字段。 必填字段必须填充有值。 如果输入的是字符串，不支持空值。 可以将可选字段留空。 如果此字段是必填字段，但未填充任何值，那么输入控件上方会显示一条错误消息。 可以显式指定是必填字段，还是可选字段。 还可以将字段与属性和资源类型之间给定绑定的架构信息一起绑定。 默认情况下，如果此属性缺失，表示控件为可选的输入控件。

   下面的示例展示了如何对此属性使用确切值：

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```
   下面的示例展示了如何对此属性使用动态数据源。 如果已将模板用于数据源（如本文档前面所述），那么数据源为架构。 使用 \<attribute name\>.Required 作为 Path。

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```
2. **ReadOnly**：通过将此属性设置为 True，最终用户可以使用处于只读模式的控件。 此为可选的布尔类型属性。
    默认值设置为 False。 不过，有时，用户对控件绑定数据拥有的权限类型会覆盖此属性的行为。 例如，如果用户无权更新字段，且字段与内联权限一起绑定，那么即使此属性设置为 False，用户也仍会看到处于只读模式的数据。

3.  **RegularExpression**：此属性指定对控件中的值施加的限制。 此属性值采用 .NET StringRegex 标准支持的格式。 有关详细信息，请参阅 [.NET Framework正则表达式](http://go.microsoft.com/fwlink/?LinkId=165361)。 如果控件用于输入值，那么当用户试图离开当前页面时，将会根据此属性中指定的限制来检查值。
    如果输入无效，将在控件上方显示错误消息。 用户可以显式指定字符串正则表达式。 用户还可以将它与给定属性的架构信息一起绑定。 默认情况下，如果此属性缺失，表示控件不会检查输入字符串是否有任何限制。
    下面的示例展示了如何对此属性使用确切值：

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```
    下面的示例展示了如何对此属性使用动态数据源。 如果已将模板用于数据源（如本文档前面所述），那么数据源为架构。 使用 <attribute name>.StringRegex 作为 Path。
    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```
4.  Visible：此为可选的布尔类型属性。 此属性可用于隐藏整个控件。 默认值设置为 True。

### <a name="options"></a>选项

Option 元素包括一个或多个 Option 子节点。 Option 元素仅与 UocRadioButtonList 和 UocDropDownList 控件配合使用。 若要详细了解如何使用它们，请参阅“具体 Control”部分。 下面展示了 Option 元素的 XSD 架构：

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```


属性：

1.  Value：此为必需的字符串类型属性。 Value 属性在同一控件内必须是唯一的。 只能使用 A 到 Z 字符（不区分大小写）。

2.  Caption：此必需属性是每个 Option 的显示名称。

3.  Hint：此为可选属性。 此属性可用于向最终用户提供详细信息和提示。

### <a name="environment-variables"></a>环境变量

下表中的环境变量可用于任何 RCDC 配置。

| 变量 | description |
|--------|--------|
| `<LoginID>`       | 显示当前登录的用户的 ID。           |
| `<LoginDomain>`   | 显示当前登录的用户的域。       |
| `<Today>  `       | 显示当前日期和时间                                |
| `<FromToday_nnn>` | 显示当前日期，外加 nnn 和时间。 nnn 是一个整数。  |
| `<ObjectID> `     | RCDC 主要资源 ID。                                     |
| `<Attribute_xxx>` | 返回 RCDC 主要资源的指定属性 xxx。 |

### <a name="debugging-xml-configuration-files"></a>调试 XML 配置文件


开发或修改 RCDC 的 XML 配置文件时，可以使用 Microsoft Visual Studio® 等编辑器，根据 XSD 文件验证 XML，这样有助于减少错误。 有关详细信息，请参阅 [Visual Studio 2005 中的 XML 工具简介](http://go.microsoft.com/fwlink/?LinkID=74512)。

### <a name="customizing-a-help-file"></a>自定义 Help 文件

如果新建资源和属性，建议使用自定义资源的内容，更新 FIM 门户中的现有 Help 文件。 FIM 门户中的 Help 文件为 .htm 格式，可供手动编辑。

>[!IMPORTANT]
若要详细了解如何创建自定义属性，请参阅 FIM 2010 文档中的“自定义资源和属性管理简介”。

>[!IMPORTANT]
此部分不介绍有关如何格式化或编辑 HTML 的基础知识。 若要修改 Help 文件，应熟悉如何编辑 HTML


**Help 文件的位置**：Microsoft Identity Manager 2016 SP1 门户的所有 Help 文件都位于 MIM 服务服务器上的以下文件夹：

  `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html`

**如何查找相应的 Help 文件**：FIM 门户的所有 Help 文件都使用全局唯一标识符 (GUID) 进行命名。 若要正确找到自定义资源的文件，请执行以下操作：

1.  在 FIM 门户中，在门户页上打开要自定义的 Help 文件。

2.  右键单击 Help 文件，再单击“属性”。

3.  选中 `<GUID\>.htm` 文件，并将它复制到“URL 地址”字段。

4.  转到 Help 文件的存储文件夹，并搜索文件。

**添加新属性的内容**：若要在现有 Grouping 元素（选项卡）中添加新属性的描述性内容，请执行以下操作：

1.  标识并找到相应的 Help 文件。

2.  使用 HTML 编辑器打开文件。

3.  定位到要添加内容的位置。 通常是在附加段落中，例如：

`<p xmlns="">A new paragraph with customized information.</p>`

也可以将项插入现有列表，例如：
```
<li class="unordered"><b>First Name</b> – The first name of the User.<br>

<li class="unordered"><b>Last Name</b> - The last name of the User.<br>

<li class="unordered"><b>Added a new line</b><br>
```

**添加新 Grouping 元素的内容**：大部分 FIM 门户页都有多个 Grouping 元素（或选项卡），随附的 Help 文件将与每个 Grouping 元素相关的部分加入书签。 这些部分指定了 HTML 书签。 例如，下面是“工作信息”选项卡 HTML，来自 FIM 门户中“创建用户”页的 Help 文件：

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

由用户创建配置 RCDC 的配置数据 XML 文件中的 Grouping 元素 WorkInfo 进行引用。 请注意，my:Link 参数中指定了 `\<GUID\>.htm` 文件名和书签：

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

**简单控件示例**

下图的示例展示了不同模式下的各种简单文本框控件。

例如：

![](media/rcd-configuration-xml-reference/image016.gif)

下面的代码段创建第一个文本框控件，对所有属性使用确切文本：

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->

```


下面的代码段创建第二个文本框控件，使用动态绑定技术将控件与其他数据源相关联：

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```


下面的代码段创建第三个扩展标签和文本框控件：

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

下面的代码段创建第四个禁用文本框控件。
尽管此控件在外观上没有体现禁用状态与启用状态的明显差异，但用户无法再在文本框中输入数据。

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```
## <a name="individual-controls"></a>具体 Control

### <a name="uocbutton"></a>UocButton

**名称**：UocButton

**说明**：此为简单的按钮控件，可用于触发特定操作。 不过，由于无法指定自己的处理程序，此控件的用途受限。

**属性**：

1.  **所有通用属性**：若要了解此类属性，请参阅本文档的“通用属性”部分。

2.  **Text**：此属性指定按钮上显示的文本。 此为可选的字符串类型属性。 Text 需要使用确切的字符串值。

事件：

   • **OnButtonClicked**：当按钮获得单击时，触发此事件。

例如：

![](media/rcd-configuration-xml-reference/image017.png)


下面的 XML 片段生成此简单按钮：

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```

### <a name="uoccaptioncontrol"></a>UocCaptionControl

**名称**：UocCaptionControl

**说明**：此控件用于显示 RCDC 页的描述文字。 此控件只能用作 Header Grouping 中的唯一控件。
如果用于其他任何上下文，可能会引发呈现问题或门户错误。

**模式**：只读（单向）

**属性：**

1.  **所有通用属性**：有关详细信息，请参阅本文档的“通用属性”部分。

2.  **MaxHeight：**此属性指定描述文字部分中图标的最大高度。 此属性为可选项。 此属性需要使用以像素为单位的整数值。 默认值为 32 像素。

例如：

![](media/rcd-configuration-xml-reference/image020.jpg)

下面的代码段生成标题描述文字：
```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->

```


下面的代码段生成确切内容描述文字：

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

下面的代码段生成显示名称动态描述文字：
```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```

### <a name="uoccheckbox"></a>UocCheckBox

**名称**：UocCheckBox

说明：此为简单的复选框控件。 建议用户将此控件与布尔类型数据一起绑定。 此控件可用作只读控件或可更新控件，具体视绑定数据而定。

>[!NOTE]
在此版本中，若要在编辑模式下使用复选框控件显示布尔属性，且此属性之前未分配有值，那么资源控制显示配置会在“确定”在编辑模式下获得单击时，将“false”值添加到此属性。 解决方法是始终创建假定不存在等同于 false 的布尔属性，或对布尔属性使用其他控件（如单选按钮）。

**属性**：

1.  **所有通用属性**：有关详细信息，请参阅本文档的“通用属性”部分。

2.  **DefaultValue**：此为可选的布尔类型属性。 默认值设置为 False。 此字段指定复选框的默认行为。
    可以显式指定此属性。

3.  **Checked**：此为可选的布尔类型属性。 默认值设置为 False。 与 DefaultValue 并存时，此值将覆盖 DefaultValue 属性。 此字段指定复选框的行为。 与 DefaultValue 一样，可以显式指定此属性，也可以将它与服务器中的数据一起绑定。

4.  **Text**：此为可选的字符串类型属性。 文本显示在复选框的右侧。 此属性可用于指定向最终用户提供详细信息的文本。

**事件**：

   • CheckedChanged：当复选框状态发生变化时，触发此事件。

下面的示例在自定义资源类型与属性 IsConfigurationType 之间创建自定义绑定。 XML 用于自定义资源类型的 RCDC。

例如：

![](media/rcd-configuration-xml-reference/image022.png)

下面的代码段生成上图中所示的动态复选框。 此类绑定通常比显式复选框适用更多情形且更实用。 此属性必须属于当前资源类型。

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**说明**：这是支持特殊字符串格式的多行文本框控件。 多值条目中的每个值使用分号相互分隔；或在文本框中通过换行符分隔开来。 建议将此控件与多值、短字符串和整数类型数据一起绑定。 此控件支持只读模式和可更新模式。

**属性**：

1.  **所有通用属性**：有关详细信息，请参阅本文档的“通用属性”部分。

2.  **DataType**：此为必需的字符串类型属性。 可以将此属性显式指定为 String、Integer 或 DateTime 类型。 也可以将此属性与架构属性的 DataType 属性一起绑定。 Multivalued Reference 类型应由 UOCListView 或 UOCIdentityPicker 处理。 不支持 Multivalued Boolean 数据类型。

3.  **Row**：此为可选的整数类型属性。 可以字符数为单位定义框高度。 默认值设置为 1。

4.  **Cloumn**：此为可选的整数类型属性。 可以字符数为单位定义框宽度。 默认值设置为 20。
    20.

5.  **Value**：此为可选的字符串类型属性。 只能将此属性与数据源一起绑定。

活动：

   • **ValueListChanged**：当控件中的当前值发生变化时，触发此事件。

下面的示例创建 AMultiValueString 多值字符串属性，并将它绑定到自定义资源类型。 下面的示例仅在此绑定创建后才生效。

**示例：**

![](media/rcd-configuration-xml-reference/image024.jpg)

下面的代码段生成上图中的多值控件：

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```



### <a name="uocdatetimecontrol"></a>UocDateTimeControl


**名称**：UocDateTimeControl

**说明**：这类似于文本框控件，不同之处在于 Description 说明仅接受特定格式。 在只读模式下，看起来像是标签。 若要了解支持的输入字符串格式，请参阅此部分中的 DateTimeFormat 属性。

**属性**：

1.  **所有通用属性**：有关详细信息，请参阅本文档的“通用属性”部分。

2.  **DateTimeFormat**：此为可选的字符串类型属性。 支持的格式为 DateTime 或 DateOnly。 默认值设置为 DateTime 格式。
    a. DateTime 格式：此属性的格式设置为 mm/dd/yyyy hh:mm:ss AM。

      <[!NOTE]
      同时支持 DateTime 和 DateOnly 格式，无论用户是否指定区别。
3.  **Value**：此为可选的字符串类型属性。 将此属性与资源数据源一起绑定。 此属性的值必须采用正确的日期时间格式。

活动：

   • **DateTimeChanged**：当日期时间值发生变化时，触发此事件。

例如：

![](media/rcd-configuration-xml-reference/image027.jpg)

下面的代码段生成第一个 DateTime 控件。

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```


下面的代码段生成第二个 DateTime 控件。 如果已在“数据源”部分中使用示例代码，ExpirationTime 属性将绑定到所有资源类型。 因此，可以通过下列代码使用它：

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```



### <a name="uocdropdownlist"></a>UocDropDownList

**名称**：UocDropDownList

说明：此为简单的下拉框控件。 若要从一组定义的选项中进行选择，通常都会使用此控件。 字符串、整数、日期时间和布尔类型数据适合于此控件。

**属性**：

1.  **所有通用属性**：有关详细信息，请参阅本文档的“通用属性”部分。

2.  **ValuePath**：用于从 ItemSource 获取 Value 属性的属性。 当 ItemSource 指定为 Custom 时，值路径设置为 Value。 此属性与本文档稍后定义的 Option 元素中的 Value 字段一起绑定。

3.  **CaptionPath**：用于从 ItemSource 获取 Value 属性的属性。 当 ItemSource 指定为 Custom 时，值路径设置为 Caption。 此属性与本文档稍后定义的 Option 元素中的 Caption 字段一起绑定。

4.  **HintPath**：用于从 ItemSource 获取 Value 属性的属性。 当 ItemSource 指定为 Custom 时，值路径设置为 Hint。 此属性与本文档稍后定义的 Option 元素中的 Hint 字段一起绑定。

5.  **ItemSource**：定义列表选项的一组 ListControlItem。 用户可以将此属性显式设置为 Custom，并使用 Option 元素指定字符串值。

6.  **SelectedValue**：当前选择的值。 此为必需的字符串类型属性。 此属性与数据源中的字符串数据一起绑定。

活动：

  • SelectedIndexChanged：当下拉框中的选项发生变化时，触发此事件。

选项：

若要了解 Option 元素的结构，请参阅本文档的“Option”部分。

1.  **Value**：单个 Option 元素的 Value 可设置为任何字符串，此值是控件绑定到的数据源的有效输入。

2.  **Caption**：Caption 可以是任何字符串值。

3.  **Hint**：Hint 可以是任何字符串值。

例如：

![](media/rcd-configuration-xml-reference/image030.jpg)


![](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
若要让此示例生效，必须将现有字符串类型属性 Scope 与 RCDC 适用的自定义资源类型一起绑定。


下面的代码段生成下拉列表：

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

名称：UocFileDownload

说明：此控件包含超链接。 在超链接获得单击时，将显示“Windows 保存文件”页。 用户可以将文件保存到本地驱动器。
如果 Internet Explorer 可以呈现文件格式，还支持 Open 选项。 建议对此控件使用格式化字符串 (XML) 和二进制类型数据。

>[!NOTE]
在此版 Microsoft Identity Manager 2016 SP1 中，用户必须先关闭已打开文件的 Internet Explorer 窗口，再刷新页面。 在刷新 Internet Explorer 窗口后，用户便可以开始下载，以在原始窗口中再次保存或打开同一文件。


属性:

1.  **所有通用属性**：有关详细信息，请参阅本文档的“通用属性”部分。

2.  **Text**：此为可选的字符串类型属性，定义了超链接文本。 用户可以为此属性指定确切字符串。

3.  **Value**：此为必需属性。 它指定要下载其内容的服务器上的属性绑定。

4.  **PromptedFileName**：此为可选的字符串类型属性。 这是当用户保存下载文件时向其建议的文件名。

5.  **ContentType**：此为必需的字符串类型属性。 这是用于保存数据的文件类型。 两个支持的字符串选项是 Text 或 Binary。 如果是 Text，返回值为长字符串。
    否则，如果是 Binary，返回值为 byte[]。 如果选择的是 Text，用户可以根据需要添加后缀，以指定文本格式类型。 例如，text/xml 是有效类型。


>[!NOTE]
如果绑定到此控件的值为空，此控件就缺少用于触发下载操作的超链接。 这是因为没有内容可供下载。


**事件**：

没有针对此控件的事件。

**示例**：

![](media/rcd-configuration-xml-reference/image035.png)


>[!NOTE]
上传此示例文件之前，用户必须在自定义资源类型与现有 ConfigurationData 属性之间创建绑定。


下面的代码段生成上图中的文件下载控件：

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**名称**：UocFileUpload

**说明**：此控件包含显示要上传本地文件位置的文本框、文件浏览按钮和上传按钮。 当最终用户单击浏览按钮时，显示“Windows 打开文件”窗口。 最终用户可以从本地驱动器中选择一个文件进行上传。 选择文件后，文本框中将显示文件位置。 单击“上传”按钮后，文件将上传到客户端本地数据源。 文件内容尚未提交到服务器。 建议对此控件使用以下类型数据：格式化字符串 (XML) 或二进制类型。

>[!NOTE]
不显示上传进度或状态。 当文件上传到本地数据源后，将清除文本框。


属性:

1.  所有通用属性：若要了解此类属性，请参阅本文档的“通用属性”部分。

2.  Value：此为必需属性。 它指定要将数据上传到的服务器上的架构属性绑定。

3.  ContentType：此为可选的字符串类型属性。 这是服务器上的文件保存数据类型。 可以设置为 Text 或 Binary。 如果此属性缺失，默认值为 Binary。

4.  MaxFileSize：此为可选的字符串类型属性。 MaxFileSize 定义了可以上传的文件大小上限。 默认情况下，如果此属性缺失，大小上限为 1MB。

5.  PromptedForNoValue：此为可选的字符串类型属性。 它定义了文件未上传时向用户显示的文本。

活动：

   • FileUploaded：当文件成功上传时，触发此事件。

例如：

![](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
若要让下面的示例代码生效，必须新建名为 ABinaryAttribute 的二进制类型属性，再在自定义资源类型与此属性之间新建绑定。


下面的代码段生成上图中的上传控件：
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```

### <a name="uocfilterbuilder"></a>UocFilterBuilder

名称：UocFilterBuilder

说明：此为允许用户呈现 MIM 2016 XPath 表达式的复杂控件。 不支持一些 XPath 表达式。 若要了解如何使用筛选器生成器，请参阅筛选器生成器的 Help 文件。

属性:

1.  所有通用属性：有关详细信息，请参阅本文档的“通用属性”部分。

2.  PermittedObjectType：此属性定义了要在筛选器生成器的 select 语句中显示的资源类型列表。 若要了解如何使用筛选器生成器，请参阅筛选器生成器的 Help 文件。 字符串采用以下格式：ResourceTypeA, ResourceTypeB（其中，每个资源类型用逗号 (,) 隔开）。

3.  Value：这是筛选器生成器呈现的值。
    仅支持与包含 XPath 表达式的字符串类型数据进行绑定。 Filter 属性是建议用于绑定此控件的属性。

4.  PreviewButtonVisible：此为可选的布尔类型属性。 如果此属性设置为 False，用户将看不到“预览”按钮。 默认值设置为 True。 此按钮可以与列表视图控件结合使用，以预览 XPath 表达式的结果。

5.  ExcludeGroupMembership：此为布尔属性。 如果此属性设置为 True，将无法创建使用属于\<组对象\>的\<引用属性\>（例如，ResourceID）的筛选器。 也就是说，如果此属性设置为 True，将无法创建使用组成员身份目录的筛选器。

6.  PreviewButtonCaption：此为可选的字符串。 如果 PreviewButtonVisible 设置为 True，可以使用此属性提供按钮的自定义文本。 文本显示在“预览”按钮上。

活动：

   • OnFilterChanged：当筛选器生成器内容发生变化时，触发此事件。

例如：

![](media/rcd-configuration-xml-reference/image044.png)

>[!NOTE]
使用以下示例代码前，请先在现有 Filter 属性与自定义资源类型之间新建绑定。


下面的示例代码包含 UOCLabel 控件、具有 PermittedObjectType 的简单筛选器生成器和预览列表视图。 必须将列表视图 ListFilter 属性和筛选器生成器 Value 属性指向同一数据源属性，将两者关联起来。

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


<a name="uochtmlsummary"></a>UocHtmlSummary
--------------

名称：UocHtmlSummary

说明：此控件可用于在 RCDC 页中定义摘要页。
此摘要页在最终用户提交请求后显示。 此控件只能用作 Summary Grouping 中的唯一控件。 强烈建议使用所提供的示例代码。

>[!NOTE]
尚未对此控件进行广泛测试。


属性:

1.  所有通用属性：若要了解此类属性，请参阅本文档的“通用属性”部分。

2.  ModificationsXml：此属性的格式必须为 {Binding Source=delta, Path=DeltaXml}，其中增量在配置标头 ObjectDataSource 中进行定义。

3.  TransformXsl：此属性的格式通常为 {Binding Source=summaryTransformXsl, Path=/}，其中 summaryTransformXsl 在配置标头 XmlDataSource 中进行定义。

有关此控件的现有示例，请参阅本文档前面“Grouping”部分中的“Summary Grouping”。

### <a name="uochyperlink"></a>UocHyperLink

名称：UocHyperLink

说明：此为简单的超链接控件。 此控件可用于将信息显示为超链接。

属性:

1.  所有通用属性：若要了解此类属性，请参阅本文档的“通用属性”部分。

2.  ObjectReference：此为可选的引用类型属性。 如果是使用此属性中定义的 GUID 引用有效资源，那么最终用户可以通过超链接访问资源。 这与下文中的 NavigateUrl 属性互斥。

3.  Text：此为可选的字符串类型属性。 此属性可用于定义显示的超链接文本。

4.  NavigateUrl：此为可选的字符串类型属性。 此属性可用于定义超链接链接到的完整路径 URL。 这与上文中的 ObjectReference 属性互斥。

例如：

![](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
必须有资源的有效 GUID，才能将此超链接链接到它。 在此示例中，第二个超链接是通过有效的 GUID 生成。 第一个超链接可以是任意网站。


下面的代码段生成重定向超链接：

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->

```

下面的代码段生成引用资源的超链接。 可以将显式引用替换为表达式 {Binding Source=object, Path=Creator}，以将此控件与数据源一起绑定。 只有当资源管理器存在且值为引用类型时，此操作才有效。

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```

### <a name="uocidentitypicker"></a>UocIdentityPicker

名称：UocIdentityPicker

说明：此控件包含可选的解析框和浏览窗口。 可选的解析框包含用于输入标识的可选文本框、用于解析标识的解析按钮，以及用于弹出浏览窗口的浏览按钮。 在浏览窗口中，用户可以通过列表视图控件选择标识。 在浏览窗口中选择的标识将反映在解析框中。

属性:

1.  所有通用属性：若要了解此类属性，请参阅本文档的“通用属性”部分。

2.  UsageKeyword：此为可选的字符串属性。 可以提供 SearchScopeConfiguration 结构支持的使用情况关键字列表（其中，每个关键字用 ‘ 隔开），从而定义在资源选取器中使用的搜索范围列表。

3.  Filter：此为可选的字符串属性。 用户提供 XPath 表达式，将资源选取器限定为只显示定义范围内的项。 此属性与上文中的 UsageKeyword 属性互斥。 如果应用搜索范围，此属性无效。

4.  ResultObjectType：此为可选的字符串属性。 资源类型用于在弹出对话框列表中呈现资源。 此属性与 Filter 结合使用，有助于标识选取器确定 Filter 返回的资源类型，并呈现相应数据。 此属性与上文中的 UsageKeyword 属性互斥。 如果应用搜索范围，此属性无效。 此属性接受的字符串是任何一个有效的资源类型名称（例如，Person）。
    如果筛选器应返回多个资源类型，则使用 Resource。

5.  PreviewTitle：此为列表视图上使用的预览标题。 若要了解此属性，请参阅“UocListView”部分。

6.  ListViewTitle：此为可选的字符串属性。 此属性可用于定义列表视图顶部显示的标题文本。

7.  Value：此为可选的字符串属性。 建议将此属性与架构属性绑定，从而将值与数据源关联起来。

8.  Mode：此为可选的字符串属性。 此属性可用于定义标识选取器可以选择一个值，还是可以选择多个标识。 可取值为 SingleResult 和 MultipleResult。 默认情况下，此属性设置为 SingleResult。

9.  ObjectType：此为可选的字符串类型属性。 可用于定义资源类型列表，以便最终用户能够在标识选取器解析框中以此为依据解析条目。 此列表为资源类型名称的逗号 (,) 分隔列表。

10. AttributesToSearch：此为可选的字符串类型属性。 可以定义在标识选取器中解析项所用的属性列表，此列表为架构属性的逗号 (,) 分隔列表。 例如，如果 AttributesToSearch 设置为 DisplayName, Alias，表示用户可以使用 DisplayName=\<搜索值\> 或 Alias=\<搜索值\> 搜索项。 建议在此处输入对 Value 中绑定的数据源的目标资源类型有效的属性名称。 可以在 ObjectType 字段中找到目标资源类型。 所有属性都必须对 ObjectType 字段中引用的任何给定资源类型有效。

11. ColumnsToDisplay：此为可选的字符串类型属性。 用户提供架构属性名称的逗号 (,) 分隔列表。 此处定义的属性构成标识选取器中的列表视图列。

12. Row：此为可选的整数属性。 仅当 Mode 设置为 MultipleResult 时，此属性才适用。 此属性可用于将解析文本框的高度设置为以字符数为单位的给定大小。

13. MainSearchScreenText：此为可选的字符串类型属性。 这是在浏览窗口中运行搜索时出现的自定义文本。

活动：

 • SelectedObjectChanged：当用户更改选定资源时，触发此事件。

例如：

![](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
若要让此示例生效，必须在 Manager 属性与应用此 XML 的任何自定义资源类型之间新建绑定。


下面的代码段在 RCDC 中使用 Filter 和 ResultObjectType 属性，生成单一模式的标识选取器：

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```



例如：

![](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
若要让此示例生效，必须将 ExplicitMember 属性（多值引用属性）绑定到自定义资源类型。 此外，还必须创建搜索范围，同时将 UsageKeyword 属性设置为 Person 和 Group。


下面的代码段创建上图中的控件：

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

名称：UocLabel

说明：此为简单的只读文本标签控件。 建议将此控件用于显示只读数据。

属性:

1.  所有通用属性：若要了解此类属性，请参阅本文档的“通用属性”部分。

2.  Text：此为字符串类型属性。 可以定义此属性，具体方法为提供确切字符串值，或将它与数据源一起绑定。 分配此属性值的示例绑定为 {Binding Source=object, Path=\<有效属性名称\>}。

有关 UocLabel 控件示例，请参阅“简单控件示例”部分中的简单控件。

### <a name="uoclistview"></a>UocListView

名称：UocListView

说明：此为高级的列表视图控件。 它包含简单列表视图、可选简单搜索控件、可选高级搜索控件、可选选择预览框和操作按钮栏。 可选简单搜索控件包含搜索范围和简单搜索文本框。 高级搜索控件是筛选器生成器。 列表视图显示预呈现的资源列表。 它还可以显示此控件中搜索控件的搜索结果。 操作按钮栏定义了可以执行哪些操作，具体视列表视图中的选择而定。 选择预览框显示在列表视图中选择了哪些项。

>[!IMPORTANT]
UocListView 不支持单值引用属性。 只能与多值引用属性结合使用。 对于单值引用属性，请参阅本文档的“UocIdentityPicker”部分。


属性:

1.  所有通用属性：若要了解此类属性，请参阅本文档的“通用属性”部分。

2.  SelectedValue：此为可选的字符串类型属性，通常绑定到接受 GUID 格式字符串列表的多值引用属性。

3.  PageSize：此为可选的整数属性。 用户可以指定列表视图控件中的一页容纳多少条目。 默认值为 10 个条目。 任意正整数均有效。

4.  UsageKeyword：此为可选的字符串类型属性。 用户可以指定关键字列表，用于定义在列表视图搜索控件中使用的搜索范围。 FIM 2010 服务器中有搜索范围资源。 SearchScopeConfiguration 结构上的 UsageKeyword 属性用于对一组搜索范围进行分组。 列表视图使用此关键字列表。 每个关键字用逗号 (,) 隔开。
    这是对要在此列表视图中显示的相应搜索范围使用的 UsageKeyword。 仅当 ShowSearchControl 属性设置为 True 时，此属性才有效。

5.  SearchControlAutoPostback：此为可选的布尔属性。 将此属性的值设置为 True，即可在触发搜索时执行自动回发。 默认情况下，SearchControlAutoPostback 设置为 False。

6.  EmptyResultText：此为可选的字符串类型属性。 默认情况下，此属性设置为“No items”，但也可以设置为任意字符串值。 当搜索结果为空时，显示此文本。

7.  ButtonHeight：此为可选的整数类型属性。 将此属性的值设置为任意正整数值。 此属性以像素为单位定义操作栏中的按钮高度。 默认值为 32 像素。

8.  ButtonWidth：此为可选的整数类型属性。 将此属性的值设置为任意正整数值。 此属性以像素为单位定义操作栏中的按钮宽度。 默认值为 32 像素。

9.  CaptionImageMaxHeight：此为可选的整数类型属性。 将此属性的值设置为任意正整数。 此属性定义了可选描述文字的图标高度上限。 默认值为 32 像素。

10. CaptionImageMaxWidth：此为可选的整数类型属性。 将此属性的值设置为任意正整数。 此属性定义了可选描述文字的图标宽度上限。 默认值为 32 像素。

11. CaptionImageUrl：此为可选的字符串类型属性。 此属性定义了链接到描述文字图像的 URL。

12. PreviewTitle：此为可选的字符串类型属性。 此属性可用于定义选择预览框顶部显示的文本。

13. EnableSelection：此为可选的布尔类型属性。 此属性可用于定义列表视图是否处于选择模式。 如果列表视图处于选择模式，那么列表视图的最左侧会显示一列复选框，并且列表视图的底部会显示选择预览框。 此属性的默认值设置为 True。

14. SingleSelection：此为可选的布尔类型属性。 如果已为列表视图启用选择模式，那么将此值设置为 True 可以将最终用户限制为只能从列表中选择一项。 默认情况下，此属性的值设置为 False。 也就是说，默认情况下，最终用户可以从列表中选择多项。

15. RedirectUrl：此为可选的字符串类型属性。 此属性可用于指定在列表中的超链接项获得单击时重定向到的页。 此 URL 可以包含在运行时期间替换为实际值的占位符。 占位符如下所示：

     ◦ {0} - objectType

     ◦ {1} - objectID

     ◦ {2} - displayName

16.  ShowTitleBar：此为可选的布尔类型属性。 此属性可用于指定标题栏是否应可见。 此属性的默认值为 False。

17.  ShowActionBar：此为可选的布尔类型属性。 此属性可用于指定操作栏区域是否应可见。 此属性的默认值为 True。

18.  ShowPreview：此为可选的布尔类型属性。 此属性可用于指定预览区域是否应可见。 此属性的默认值为 True。

19.  ShowSearchControl：此为可选的布尔类型属性。 此属性可用于指定搜索控件是否应可见。 此属性的默认值为 True。

20.  ResultObjectType：此为可选的字符串类型属性。 此属性可用于指定搜索结果应为哪种对象类型。 此属性的默认值为 Resource。 如果搜索结果中包含多个资源类型，应将此值指定为 Resource。

21.  ColumnsToDisplay：此为可选属性。 此属性可用于指定要在列表视图中显示为列的属性。 此属性的默认值是 DisplayName, ResourceType。 每列均以属性的系统名称表示。 每列用逗号 (,) 隔开。 在选择模式下使用列表视图时，无需指定此属性的值。 在选择模式下，列设置源于当前选定搜索范围的 SearchScopeColumn 属性。

22.  ListFilter：此为可选的字符串类型属性。 这是用于呈现列表视图的 xpath，仅当 ShowSearchControl 属性设置为 False 时才有效。 指定此值后，列表视图将此属性值用于查询，且列表视图不处于选择模式。 筛选器可以绑定到资源的字符串属性，如下所示：<br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>` <br/> <br/>
     也可以是包含某预定义环境变量的字符串，如下所示： <br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

23.  TargetAttribute：此属性已过时。 它的值应为多值引用属性的系统名称。 建议不再使用此属性。 例如，在组管理中，不得使用以下代码：

  `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>` <br/>

  而应使用：`<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>` <br/><br/>
24.  ItemClickBehavior：此为可选的字符串类型属性。 此属性可用于指定要在列表视图项获得单击后触发服务器回发，还是显示项的详细视图。 支持以下两个选项值：ModelessDialog 和 Server。 默认值为 ModelessDialog。

25.  SearchOnLoad：此为可选的布尔类型属性，用于指定列表视图控件是否应在加载后进行查询。 仅当列表视图处于选择模式时，此属性才适用。 此属性的默认值为 True。 如果希望用户通常是通过在搜索控件中键入文本来获得有意义的结果，可以将此属性设置为 False。 在这种情况下，列表视图最初会显示一条消息，指导用户如何执行搜索。 可使用以下属性自定义文本：

26.  MainSearchScreenText：此为可选的字符串类型属性。仅当 SearchOnload 设置为 False 时，此属性才适用。 此属性可用于自定义在不自动搜索的列表视图中间显示的文本。 此属性的默认值是“Find the resources you want using the search above”。 可以指定值，让文本与方案更相关。

27.  SubSearchScreenText：此为可选的字符串类型属性，用于自定义在 MainSearchScreenText 下方显示的文本。 通常情况下，无需指定此属性的值，除非要添加其他一些有关如何使用列表视图的说明。

有关如何将列表视图作为预览列表与 UocFilterBuilder 控件结合使用的示例，请参阅本文档前面的“UocFilterBuilder”示例。 也可以在没有筛选器生成器的情况下使用 UocListView。

### <a name="uocnumericbox"></a>UocNumericBox

名称：UocNumericBox

说明：此为仅使用整数值的简单文本框。 此控件支持只读模式和可更新模式。

属性:

1.  所有通用属性：若要了解此类属性，请参阅本文档的“通用属性”部分。

2.  MaxValue：此为可选的整数类型属性。 此属性可用于定义控件的客户端验证。 最终用户输入的值不得超过此值。 可以输入确切整数，也可以使用 {Binding Source=schema Path=IntegerMaximum} 将此属性与数据源中的整数数据一起绑定。

3.  MinValue：此为可选的整数类型属性。 此属性可用于定义控件的客户端验证。 最终用户输入的值不得低于此值。 可以输入确切整数，也可以使用 {Binding Source=schema, Path=IntegerMinimum} 将此属性与数据源中的整数数据一起绑定。

4.  DefaultValue：此为可选的整数类型属性。 如果控件用于新建数据，此属性可用于定义控件的默认值。 只能将此值显式设置为静态整数。

5.  Value：此为可选的整数类型属性。 如果将此属性与数据源中的整数类型数据一起绑定，那么此属性值将在页面加载时显示，再在用户提交后保存到数据源。

处理程序：

   • TextChanged：当控件中的当前值发生变化时，触发此事件。

例如：

![](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
下面的示例代码生成第一个数字框。 此数字框不与数据源或任何架构信息相关联。

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->

```

下面的示例代码生成第二个数字框。

>[!NOTE]
若要让此示例生效，必须先新建名为 AnIntegerAttribute 的整数类型属性，再将它与自定义资源类型一起绑定。

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

名称：UocPictureBox

说明：此控件用于通过二进制类型数据呈现图片。 建议将此控件与二进制类型数据结合使用。 图片可通过提供的图像 URL、二进制类型数据或包含图片类型数据的属性源进行呈现。

属性:

1.  所有通用属性：若要了解此类属性，请参阅本文档的“通用属性”部分。

2.  ImageUrl：此为可选的字符串类型属性。 输入目标图片 URL。

3.  MaxHeight：此为可选的字符串类型属性。 它以像素为单位定义要呈现的图像高度上限。

4.  MaxWidth：此为可选的字符串类型属性。 它以像素为单位定义要呈现的图像宽度上限。

5.  ImageData：此为二进制类型属性。 此属性可用于将数据源与显示的图像一起绑定。 绑定的数据源必须是二进制类型。
    还可以提供 byte[] 格式数据，使用此字段显式设置图片。

6.  ImageResource：此为可选的二进制类型属性。

7.  AlternativeText：此为可选的字符串类型属性。 此属性显示为图片无法显示时的替换文字。

示例：

>[!NOTE]
若要使用此示例，必须将现有图像数据与控件一起绑定。


下面的代码段生成将数据源与控件一起绑定的图片框控件：

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

下面的代码段生成将 URL 图像与控件一起绑定的图片框控件：

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

### <a name="uocradiobuttonlist"></a>UocRadioButtonList

名称：UocRadioButtonList

说明：此为简单的选项按钮列表。 此列表中的选项互斥。 可供用户选择的选项不超过五个时，建议使用此控件。 否则，建议使用 UOCListView。

属性:

1.  所有通用属性：若要了解此类属性，请参阅本文档的“通用属性”部分。

2.  ValuePath：值路径设置为 Value。 此属性与本文档中定义的 Option 元素中的 Value 字段一起绑定。

3.  CaptionPath：值路径设置为 Caption。 此属性与本文档中定义的 Option 元素中的 Caption 字段一起绑定。

4.  HintPath：值路径设置为 Hint。 此属性与本文档中定义的 Option 元素中的 Hint 字段一起绑定。

5.  SelectedValue：当前选择的值。 此为必需的字符串类型属性。 此属性与数据源中的字符串数据一起绑定。

活动：

1.  SelectedIndexChanged

2.  CheckedChanged

选项：

此控件的 Option 只能有两个 Option 元素。

1.  Value：一个 Option 元素中的 Value 字段必须设置为 True 或 False。

2.  Caption：可以是任意字符串值。

3.  Hint：可以是任意字符串值。

例如：

![](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
若要让此示例生效，必须新建名为 ABooleanAttribute 的布尔属性，再将它与自定义资源类型一起绑定。

下面的代码段创建上图中的选项按钮列表：

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton


名称：UocSimpleRadioButton

说明：此为简单的选项按钮控件。 此控件的用法类似于简单的复选框。 两个有文本标签的选项按钮并排显示。 建议将此控件绑定到布尔类型数据。

属性:

1.  所有通用属性：若要了解此类属性，请参阅本文档的“通用属性”部分

2.  TrueText：此为可选的字符串类型属性。 这是选中选项按钮时显示的文本。

3.  FalseText：此为可选的字符串类型属性。 这是未选中选项按钮时显示的文本。

4.  SelectedItem：此为可选的布尔类型属性。 此值指明是否已选中选项按钮。 此属性可与数据源中的布尔类型数据一起绑定。 默认值设置为 False。

活动：

   • CheckedChanged：当选项按钮的状态从选中更改为未选中（反之亦然）时，触发此信号。

例如：

![](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
若要让此示例生效，必须新建名为 ABooleanAttribute 的布尔属性，再将它绑定到自定义资源类型。 RCDC 数据应用于同一自定义资源类型。

下面的代码段生成上图中的选项按钮：

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

名称：UocTextBox

说明：此为支持字符串类型输入的简单文本框。 建议使用此控件与字符串类型数据一起绑定。

属性:

1.  所有通用属性：若要了解此类属性，请参阅本文档的“通用属性”部分。

2.  MaxLength：此为可选的整数类型属性。 此属性指定字符串输入的长度上限。 此属性的默认值为 128 个字符。

3.  Text：此为可选的字符串类型属性。 这是文本框中显示的文本。 可以定义在此控件初始加载期间在文本框中显示的确切字符串，也可以将此属性绑定到字符串类型的架构属性。

4.  Row：此为可选的整数类型属性。 此属性以字符数为单位定义文本框高度。 默认值为 1 个字符。

5.  Column：此为可选的整数类型属性。 此属性以字符数为单位定义文本框宽度。 默认值为 20 个字符。

6.  Wrap：此为可选的布尔类型属性。 通过将此属性的值设置为 True，用户可以在文本框中启用自动换行功能。 此属性的默认值设置为 True。

7.  UniquenessValidationXPath：此为可选的字符串类型属性。 它需要使用有效的 FIM XPath 筛选表达式，并确保用户输入的值在属于筛选器范围的资源中是唯一的。
    例如，为了确保用户请求的显示名称在 FIM 服务 DB 中所有启用邮件的安全组中是唯一的，请使用 XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’`。 当用户离开页面时，将执行验证操作。 在 RCDC 中，仅支持使用此属性创建资源。

8.  UniquenessErrorMessage：此为可选的字符串类型属性。 此字符串用于在 UniquenessValidationXPath 验证失败时显示错误消息，可以是确切文本，也可以是字符串资源变量。 如果未指定此属性，那么验证失败时显示的默认错误消息为“%VALUE% 已存在。 请尝试其他值。”

活动：

   • TextChanged：当文本框中的文本发生变化时，触发此事件。

有关此控件的完整示例，请参阅“简单控件示例”部分。

## <a name="appendix-a-default-xsd-schema"></a>附录 A：默认 XSD 架构

下面展示了 Microsoft Identity Manager 2016 SP1 随附的所有默认 RCDC 的完整 XSD 架构：

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```



## <a name="appendix-b-default-summary-xsl"></a>附录 B：默认摘要 XSL

下面展示了 Microsoft Identity Manager 2016 SP1 随附的完整摘要 XSL：
```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```
