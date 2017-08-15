---
title: "管理代理运行错误代码 | Microsoft Docs"
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
ms.openlocfilehash: 511bd9ae4e303a2142c3f101c3880dc8cb46be3d
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2017
---
# <a name="management-agent-run-error-codes"></a>管理代理运行错误代码

下表列出了 Microsoft Identity Manager (MIM) 2016 的 Synchronization Service Manager 用户界面中可能显示的错误代码，以及各个错误的说明。

## <a name="connection-errors"></a>连接错误

| 错误                  | 说明                                    |
|--------------------------|--------------------------------------------|
| failed=connection         | 出于非身份验证原因，无法连接到已连接目录。 例如，网络中断或目标服务器处于脱机状态。                                                                                                                                                                                                                                                                                                                      |
| dropped-connection        | 管理代理与已连接目录的连接已不存在。 在大部分情况下，管理代理会尝试重新连接到已连接目录。                                                                                                                                                                                                                                                                                                         |
| failed-authentication     | 无法使用提供的凭据进行身份验证。                                                                                                                                                                                                                                                                                                                                                                                                                          |
| failed-permission         | 权限不足，无法访问已连接目录中的容器。 系统只会对搜索不同已连接目录容器的轻型目录访问协议 (LDAP) 管理代理显示此错误。                                                                                                                                                                                                                                                              |
| failed-search            | 因为意外错误而无法搜索容器或表。                                                                                                                                                                                                                                                                                                                                                                                                                            |
| warning-no-watermark      | 管理代理无法在运行完全导入时读取水印。 如果初始管理代理配置完成且已连接目录启用了更改日志，系统只会对 Sun ONE 目录服务器 5.1（前身为 iPlanet 目录服务器）管理代理显示此错误。 稍后，如果已连接目录禁用了更改日志，且管理代理配置没有更新，那么此警告会在完全导入完成时显示。 |
| no-start-partition-delete | 最初为此管理代理配置的 LDAP 分区已不存在。 当分区遭删除，或分区遭删除且重新创建了同名分区时，系统会返回此错误。 在后一种情况中，即使同名，错误分区 GUID 也会发生变化。                                                                                                                                                                           |

## <a name="discovery-errors"></a>发现错误

| 错误                   | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| missing-change-type      | 如果更改类型列值（add、modify、delete）不存在，那么基于文件的管理代理、数据库管理代理、Sun 和 Netscape 目录服务器管理代理会在增量导入运行期间返回此错误。                                                                                                                                                                                                                                          |
| invalid-change-type      | 如果更改类型列值与有效更改类型列表不一致，那么基于文件的管理代理、数据库管理代理、Sun 和 Netscape 目录服务器管理代理会在增量导入运行期间返回此错误。 此外，如果更改类型字段存在但值不是 add，也会在运行 LDAP 数据交换格式 (LDIF) 完全导入时返回此错误。                                                                                  |
| multi-valued-change-type | 如果更改类型值有多个，那么基于文件的管理代理、Sun 和 Netscape 目录服务器管理代理会在增量导入运行期间返回此错误。                                                                                                                                                                                                                                                                                                        |
| need-full-object        | 在基于文件的管理代理运行增量导入期间，或从基于文件的管理代理恢复时，系统会返回此错误。 此错误表示，管理代理已提交对象修改，但在连接器空间内找不到此对象。 同步引擎在请求获取此对象所有属性的当前值。 由于这是从文件导入，因此无法获取相应信息。 完全导入应该可以解决此问题。 |
| missing-dn              | 如果没有域名值，系统会对基于文件的管理代理（即 LDIF 管理代理、DSML 管理代理或已配置域名属性的平面文件管理代理）返回此错误。 此外，如果 Sun ONE 目录服务器更改日志损坏，即缺少域名属性，系统也会返回此错误。 此错误表示，管理代理可以读取并分析元素，但没有对象的域名值。                           |
| dn-not-ldap-conformant   | 当 LDAP 管理代理、LDIF 管理代理、DSML 管理代理或已配置域名属性的平面文件管理代理报告域名值不符合 LDAP 规范时，系统会返回此错误。                                                                                                                                                                                                                                                                                |
| invalid-dn              | 当管理代理报告域名不符合 FIM 约束时，系统会返回此错误。不符合的地方包括：   <br/>&#176; 使用 FIM 禁用的一个或多个字符 <br/>&#176; 相关辨别名称 (RDN) 为空 <br/>&#176; 相关辨别名称超过 FIM 上限 <br/>&#176; 域名的层次结构级别数超过 FIM 上限                                                                                                                                                                                                                                                                                                                                                            |
| missing-anchor-component | 如果因为一个或多个定位标记构造规则属性没有值而无法构造定位标记，那么基于文件的管理代理、数据库管理代理、Sun 和 Netscape 目录服务器管理代理会返回此错误。   |
| multi-valued-anchor-component | 如果因为定位标记构造规则属性有多个值而无法构造定位标记，那么 Sun 和 Netscape 目录服务器管理代理会返回此错误。 |   
|anchor-too-long | 如果定位标记构造生成的定位标记超过 FIM 大小上限，那么基于文件的管理代理、数据库管理代理、Sun 和 Netscape 目录服务器管理代理会返回此错误。|
|duplicate-object | 如果在完全导入运行期间已向同步引擎报告包含相同定位标记的对象，那么基于文件的管理代理和数据库管理代理会在运行完全导入时返回此错误。|

>[!Note]
只有在当前运行步骤成功完成、完成但出现同步故障、完成但出现警告或瞬间完成时，连接器空间对象才会过时。

|错误| 说明 |
|----|----|  
|missing-object-class | 如果更改日志损坏，那么基于文件的管理代理（即 DSML 管理代理、LDIF 管理代理或已配置对象类属性的平面文件管理代理）或 Sun 和 Netscape 目录服务器管理代理会返回此错误。 此错误表示，管理代理无法读取对象类属性值。 |
|missing-object-type | 当继续从损坏的删除文件导入时，系统会返回此错误。 在正常操作期间，应该不会看到此错误。        |
|unmappable-object-type  | 如果基于文件的管理代理读取的对象包含一组无法与任何前缀映射匹配的对象类值，基于文件的管理代理会返回此错误。 |
|parse-error | 增量模式下的 Sun 和 Netscape 目录服务器管理代理会返回此错误，基于文件的管理代理在无法分析条目时也会返回此错误。 为了方便定位此错误，系统会显示 `<entry-number>` 元素（大多数情况下为 `<line-number>` 和 `<column-number>`）。 可能会显示 `<attribute-name>` 元素。 如果遇到此错误，Sun 和 Netscape 目录服务器管理代理会终止运行。 基于文件的管理代理会记录发现错误，并继续运行。 |   
|read-error | 如果在读取特定对象时遇到一般性错误，基于调用的管理代理会返回此错误。 这通常都会导致运行终止。 系统会显示已连接数据源错误元素，可用于排查此问题。    |
|staging-error | 大多数管理代理都会返回此错误。 此错误表示，同步引擎无法在连接器空间中暂存增量。 服务器会创建包含此问题相关信息的事件日志，可用于故障排除。 在系统记录此错误后，大多数管理代理都会继续运行导入，但 Sun 和 Netscape 管理代理会停止运行增量导入，因为如果更改日志处理出现缺口，可能会导致连接器空间中的状态不一致。 在正常操作期间，应该不会看到此错误。  |
|invalid-modification-type | 如果对象级修改类型不是标准的 LDIF 修改类型之一，或 objectclass 的修改类型（如 add: objectclass 或 delete: objectclass）不可替换，LDIF 管理代理会在增量导入运行期间返回此错误。    |
|conflicting-modification-types        | LDIF 管理代理返回此错误，以指明同一记录中有不同的属性级修改类型（在这种情况下，会报告生成冲突类型的属性名称），或同一文件中显示多个替代 LDIF 增量，例如：    <br/> [LDIF 文件示例](./media/maerrorcodes/conflict-modification-types.jpg) |
|multi-single-mismatch | 如果基于文件的管理代理报告在 FIM 中定义为单值属性的属性有多个值 add 或 delete，那么基于文件的管理代理会返回此错误。 此错误可能表示，与 FIM 一同存储的已连接数据源架构（基于文件的管理代理）被错误指定，或与当前架构相比已过时。 此错误消息包含 `<attribute-name>` 元素，用于提供错误上下文。    |
|invalid-attribute-value | 当读取的属性值与架构中声明的属性类型不一致时，基于调用的管理代理会返回此错误。 此错误消息包含 `<attribute-name>` 元素，用于提供错误上下文。  |
|invalid-base64-value| 当遇到无效的 base64 字符串时，LDIF 管理代理、DSML 管理代理、Sun 和 Netscape 目录服务器管理代理会返回此错误。     |
|invalid-numeric-value | 当无法分析数字值时，基于文件的管理代理和 LDAP 管理代理会返回此错误。 此错误消息包含 `<attribute-name>` 元素，用于提供错误上下文。 |
|invalid-boolean-value | 当无法分析布尔值时，基于文件的管理代理和 LDAP 管理代理会返回此错误。 此错误消息包含 `<attribute-name>` 元素，用于提供错误上下文。  |
|reference-value-not-ldap-conformant | 当域名值不符合 LDAP 规范时，LDAP 管理代理、LDIF 管理代理、DSML 管理代理或平面文件（已配置域名属性）管理代理会返回此错误。 此错误消息包含 `<attribute-name>` 元素，用于提供错误上下文。|
|invalid-reference-value | 当域名不符合 FIM 约束时，管理代理会返回此错误。不符合的地方包括：  <br/>&#176; 使用 FIM 禁用的一个或多个字符   <br/>&#176; 相关辨别名称 (RDN) 为空  <br/>&#176; 相关辨别名称超过 FIM 上限  <br/>&#176; 域名的层次结构级别数超过 FIM 上限                |
|unsupported-value-type | 当文件中提供的值类型与属性类型不兼容时，DSML 或 LDIF 管理代理会返回此错误。不兼容的地方包括：    <br/>&#176; 为非字符串属性或任何保留的关键字（如 dn、objectclass 或 changetype）提供 URI 或 URL 值。  <br/>&#176; 为 changetype 属性提供 base64 值。  <br/>&#176; 为二进制属性提供包含非 ASCII 字符的字符串值。 |

## <a name="synchronization-errors"></a>同步错误

| 错误               |说明      |
|---------|-------------|
| extension-dll-exception  | 当规则扩展引发异常时，系统会返回此错误。 如果看到此错误，请查看 \<exception-error-info\> 元素，检查异常的调用堆栈。 在某些情况下，系统会显示 `<rule-error-info>`，并提供附加信息，方便用户了解在此错误发生时正在处理哪个规则。        |
| extension-dll-crash | 当执行规则扩展的进程意外终止时，系统会返回此错误。 只有当规则扩展在进程外执行时，系统才会返回此错误。 此错误值的可能原因是，规则扩展调用的代码导致访问冲突。 |
| extension-dll-timeout                          | 如果客户配置了扩展超时，并且对单客户扩展代码入口点的调用超出了所配置的超时，那么系统会返回此错误。 `<exception-error-info>` 可提供超时调用的入口点的上下文信息。 在某些情况下，系统会显示 `<rule-error-info>，并提供附加信息，方便用户了解在此错误发生时正在处理哪个规则。 请注意，若要调试正在执行扩展的进程，系统不会强制实施超时。  |              
| extension-projection-object-type-not-set        | 如果规则扩展中的 IMASynchronization.ShouldProjectToMV 方法实现代码没有指定 metaverse 对象类型，那么系统会返回此错误。                    |
| extension-projection-invalid-object-type        | 如果规则扩展中的 IMASynchronization.ShouldProjectToMV 方法实现代码设置的出站 metaverse 对象类型值在 Synchronization Service Manager 的 Metaverse 设计器中没有列出，那么系统会返回此错误。 请检查此方法是否使用指定的对象类型值之一。   |
| extension-join-resolution-invalid-object-type   | 如果规则扩展中的 IMASynchronization.ResolveJoinSearch 方法实现代码设置的出站 metaverse 对象类型值在 Synchronization Service Manager 的 Metaverse 设计器中没有列出，那么系统会返回此错误。 请检查此方法是否将出站 metaverse 对象类型值设置为列出的对象类型值之一。   |
| extension-join-resolution-index-out-of-bounds   | 如果规则扩展中的 IMASynchronization.ResolveJoinSearch 方法实现代码将索引值设置为负值或不小于 metaverse 对象数，那么系统会返回此错误。                     |
| extension-provisioning-call-limit-reached       | 如果 IMASynchronization.Provision 方法在单对象同步过程中被调用超过 10 次，那么系统会返回此错误。 如果 Provision 方法中的客户逻辑撤销对象，并且属性重新调用导致 metaverse 对象发生变化，进而导致新调用 Provision，那么此方法可以被多次调用。 之所以设置 Provision 方法的 10 次调用限制，是为了停止可能无限的预配说明。                                                                                                                                               |
| extension-deprovisioning-invalid-result         | 如果 IMASynchronization.Deprovision 方法实现代码返回的 DeprovisionAction 枚举值无效，那么系统会返回此错误。 请确认此方法返回的值是否有效。        |
| extension-entry-point-not-implemented           | 当规则扩展引发 EntryPointNotImplementedException 异常时，系统会返回此错误。 |
| extension-unexpected-attribute-value            | 当规则扩展引发 UnexpectedDataException 异常时，系统会返回此错误。            |
| flow-multi-values-to-single-value              | 如果 Synchronization Service Manager 中配置的导入或导出属性流规则试图将包含多个值的属性流式传输到单值属性，那么系统会返回此错误。 系统只会对 Synchronization Service Manager 中配置的直接流规则返回此错误。 如果流规则使用的规则扩展将多个值流式传输到单值属性，则会引发 TooManyValuesException 异常。          |
| cs-attribute-type-mismatch                     | 如果已导入属性的类型与管理代理架构中指定的属性类型不一致，那么系统会返回此错误。 导致此错误发生的一个原因可能是，与已连接数据源的实际架构相比，存储的已连接数据源架构已过时。 若要确保存储的已连接数据源架构为最新，请使用 Synchronization Service Manager 刷新架构。|
| join-object-id-must-be-single-valued            | 如果通过 Synchronization Service Manager 管理代理属性中指定的联接规则来联接 metaverse 对象时，使用的数据源属性值包含多个值，那么系统会返回此错误。 联接规则中使用的数据源属性值只能包含一个值。     |
| dn-index-out-of-bounds       | 如果 Synchronization Service Manager 管理代理属性中配置的导入属性流使用的可分辨名称组件索引值大于源对象的可分辨名称组件数，那么系统会返回此错误。      |
| connector-filter-rule-violation                | 如果执行 add 或 rename 预配操作或导出属性流，且连接器对象因连接器筛选配置而成为筛选后的连接器对象，那么系统会返回此错误。 显式连接器对象上不会出现此值。    |
| unsupported-container-delete   | 管理代理试图在撤销期间删除容器对象。 FIM 管理代理无法删除含子对象的容器对象。            |
| ambiguous-import-flow-from-multiple-connectors   | 如果在与 metaverse 对象连接的源管理代理中有多个连接器，且定义了声明性导入属性流规则，那么系统会返回此错误。 若要通过具有多个连接器的管理代理将属性导入 metaverse 对象，可以使用规则扩展来定义流规则，而不是在管理代理的属性中配置直接规则。                                                                                                                                                                                                          |
| ambiguous-export-flow-to-single-valued-attribute | 如果在 Synchronization Service Manager 管理代理属性中配置的导出流规则试图将多个值从 metaverse 对象流式传输到单值属性，那么系统会返回此错误。       |
| cannot-parse-object-id | 在 Synchronization Service Manager 管理代理属性中指定的联接规则中，用于搜索 metaverse 对象的字符串值的全局惟一标识符 (GUID) 格式不正确。 GUID 格式为 {nnnnnnnn-nnnn-nnnnnnnn-nnnnnnnnnnnn}，其中 n 是十六进制数。 |
| unexported-container-rename  | IMVSynchronization.Provision 或 IMASynchronization.Deprovision 方法实现代码试图重命名包含一个或多个未导出子对象的容器对象。   |
| mv-constraint-violation           | 如果直接导入属性流发生，但连接器空间中的属性值超过 metaverse 属性的长度上限，那么系统会返回此错误。 |
| locking-error-needs-retry   | 多个管理代理试图同步同一连接器空间对象。 请重新运行管理代理。     |
| unique-index-violation              | 用户在为 metaverse 表中的属性手动设置惟一索引。 请勿手动配置 metaverse 表。    |
| encryption-key-lost                            | 运行 FIM 的服务器上缺少加密密钥集。  |
| unexpected-error                              | 当同步引擎试图将更改应用到 metaverse（包括预配和导出属性流）时，系统会返回此错误。 此错误只能在将更改应用到 metaverse 期间发生。 有关详细信息，请查看事件日志。  |
| exported-change-not-reimported                 | 如果在此导入管理代理运行期间没有重新确认导出到管理代理的更改，那么系统会返回此错误。 FIM 以外的用户或系统进程操作更改了已连接数据源中的数据，导致以下配置问题发生：导出属性流规则试图将值流式传输到已连接数据源对象，但已连接数据源自动将值重置为其他值，而未向管理代理报告错误。 \<change-not-reimported\> 元素表明未重新确认哪些更改。 |
| cannot-parse-dn-component                      | 如果从连接器空间到 metaverse 的同步失败，那么已配置 LDAP 样式可分辨名称 (DN) 的任何管理代理都会返回此错误。 dn 组件映射无法分析可分辨名称组件，因为它采用的格式对目标属性类型并不正确。|
| missing-partition-for-run-step                 | 此错误表示，找不到运行配置文件中指定的分区。 请确认分区是否尚未被删除或重命名。  |

## <a name="export-errors"></a>导出错误

| 错误                    |说明     |
|--------------------------|------------|
| cd-missing-object         | 如果已将对象修改导出到已连接数据源，但却在已连接数据源中找不到对象，那么系统会返回此错误。 系统只会对基于调用的管理代理返回此错误。 导致此错误发生的原因是，个人或外部进程已从 FIM 外的已连接数据源中删除了对象。  |
| cd-existing-object        | 如果已将 add 导出到已连接数据源，但已连接数据源中已有对象，那么系统会返回此错误。 系统只会对基于调用的管理代理和关系数据库管理代理返回此错误。 |
| duplicate-anchor          | 如果新预配的对象的定位标记并不唯一，那么系统会返回此错误。 系统只会对基于调用的管理代理、数据库管理代理、Sun 和 Netscape 目录服务器管理代理返回此错误。 如果看到此错误，请检查定位标记构造规则，以确保为每个对象定义了唯一的定位标记值。      |
| ambiguous-update          | 如果管理代理因定位标记不唯一而无法应用 update 或 delete 增量，那么系统会返回此错误。 系统只会对 Microsoft SQL Server 管理代理和 Oracle 数据库管理代理返回此错误。 如果看到此错误，请检查定位标记构造规则，以确保为每个对象定义了唯一的定位标记值。 |
| password-policy-violation | 如果密码属性的设置或更改后值不符合已连接数据源的管理员定义密码策略，那么 Active Directory 管理代理和 Active Directory 全局地址列表 (GAL) 管理代理会返回此错误。 |
| password-set-disallowed   | 如果密码加密被设置为不加密或 128 位安全套接字层 (SSL)，但管理员并未明确将设置替代为在此方案中设置的密码集，那么 Active Directory 应用程序模式 (ADAM) 管理代理会返回此错误。      |
| kerberos-time-skew        | 如果正在设置或更改密码属性，但 FIM 服务器计算机时间与域控制器时间相差超过 5 分钟，那么 Active Directory 管理代理和 Active Directory 全局地址列表 (GAL) 管理代理会返回此错误。  |
| kerberos-no-logon-server  | 如果尝试设置或更改密码属性，但无法为服务器解析登录凭据的域部分，那么 Active Directory 管理代理和 Active Directory 全局地址列表 (GAL) 管理代理会返回此错误。 这可能是由不正确的 NetBIOS 或 DNS 配置所致。  |
| encryption-not-enabled    | 如果已设置或正在更改密码属性，但管理代理用于与已连接数据源进行通信的连接尚未配置适当的加密机制（128 位 SSL 或 TLS），那么 Active Directory 应用程序模式 (ADAM) 管理代理会返回此错误。 ADAM 必须使用 128 位 SSL 或 TLS 配置来设置密码。   |
| invalid-dn               | 如果导出新预配的对象或重命名现有对象，但可分辨名称不符合已连接数据源命名要求，那么 LDAP 管理代理和 Windows NT 4.0 管理代理会返回此错误。|
| schema-violation          | 如果导出对象修改并添加已连接数据源架构中没有的属性，或从架构需要的对象中删除属性，那么 LDAP 管理代理会返回此错误。 FIM 不允许这些操作发生，因为它的规则会检查已连接数据源架构的存储副本。 不过，如果与已连接数据源架构相比，FIM 架构已过时，可能会出现此问题。 如果遇到此问题，请使用用户界面刷新管理代理架构。             |
| constraint-violation      | 如果 add、modify 或 delete 的导出违反了已连接数据源的强制约束，那么 LDAP 管理代理和数据库管理代理会返回此错误。 导致 LDAP 管理代理返回此错误的典型原因包括：为单值属性设置多个值、超过字符串和二进制属性的字段宽度限制，或违反数字属性的范围约束。 导致数据库管理代理返回此错误的可能原因有许多，具体包括：可能为数据库定义的引用完整性、规则和约束。 |
| syntax-violation                   | 当属性值违反某些值约束时，LDAP 管理代理和 Windows NT 4.0 管理代理会返回此错误。 例如，当导出的值包含无效字符时。 |
| modify-naming-attribute             | 如果命名属性（如许多对象类型的 CN）被设置的值与相关辨别名称 (RDN) 值冲突，那么 LDAP 管理代理会返回此错误。 这可能是因为，未正确定义导出属性流规则，或脚本代码出错，为新预配的对象设置初始值。         |
| insufficient-field-width            | 如果将 add 或 modify 导出到对象中，但属性值超过列宽度，那么定宽文本文件管理代理会返回此错误。   |
| insufficient-columns                | 如果将 add 或 modify 导出到对象中，但多值属性的值数量超过为此属性的多个值配置的列数量，那么定宽文本文件管理代理和带分隔符的文本文件管理代理会返回此错误。 |
| permission-issue                    | 如果因管理代理没有足够权限对已连接数据源执行操作，而无法导出 add、modify 或 delete，那么 LDAP 管理代理和 Windows NT 4.0 管理代理会返回此错误。     |
| dn-attributes-failure               | 如果导出 add 或 modify 所设置的引用值没有对应的已连接数据源对象，那么 Active Directory 管理代理、Active Directory 全局地址列表 (GAL) 管理代理、Active Directory 应用程序模式 (ADAM) 管理代理会返回此错误。 如果看到此错误，请使用连接器空间对象查看器，确定未成功导出哪些引用属性更改。 |
| non-existent-parent                 | 如果因已连接数据源中没有父对象而无法导出 add 或 rename，那么 LDAP 管理代理会返回此错误。       |
| code-page-conversion                | 如果因转换错误而无法将 Unicode（位于运行 FIM 的服务器中）存储的属性值转换为导出文件的代码页，那么基于文件的管理代码会返回此错误。    |
| no-export-to-this-object-type       | 如果尝试对计算机对象执行预配操作或导出属性流，那么 Windows NT 4.0 管理代理会返回此错误。 不允许对此类对象执行导出操作，但可以对此类对象执行导入操作。  |
| missing-provisioning-attribute       | 若要导出新预配的对象，但规则扩展尚未设置预配新对象所需的特定属性，那么 Lotus Notes 管理代理会返回此错误。 |
| invalid-provisioning-attribute-value | 若要导出新预配的对象，但规则扩展设置的特定预配属性无效（例如，不在特定值范围内），那么系统会返回此错误。       |
|provision-to-secondary-nab                 | 当试图将个人或认证者对象预配到辅助 Lotus Notes 通讯簿时，Lotus Notes 管理代理会返回此错误。 Lotus Notes 只允许将联系人预配到辅助通讯簿。  |                                                           
| missing-anchor-component                   | 若要导出新预配的对象，但由于构造定位标记所需的值不可用而无法生成定位标记，那么系统会返回此错误。 可能的原因是，在预配期间未设置属性（即在 Sun 或 Netscape 目录服务器管理代理、数据库管理代理和基于文件的管理代理中），或通过自动递增列构造定位标记时，无法从已连接数据源读取数据（即在 Active Directory 管理代理、Sun 和 Netscape 目录服务器管理代理、数据库管理代理中）。 |
| multi-valued-anchor-component               | 如果由于构造定位标记时使用的一个属性有多个值，而无法为新预配的对象构造定位标记，那么 Sun 和 Netscape 目录服务器管理代理会生成此错误。 虽然可以将构造定位标记时使用的属性定义为已连接数据源架构中的多值，但在 FIM 中的实际对象上只能包含一个值。                                                                                                                                                                           |
| anchor-too-long                           | 如果定位标记构造生成的定位标记超过 FIM 大小上限，那么基于文件的管理代理、数据库管理代理、Sun 和 Netscape 目录服务器管理代理会返回此错误。 在连接器空间中，一个属性的定位标记值长度不得超过 398 个字符。 如果定位标记通过多个属性构造而成，那么每增加一个属性，上限就减去 2 个字符。 例如，如果定位标记通过 3 个属性 (sn+location+telephoneNumber) 构造而成，长度上限为 392 个字符。                                    |
| invalid-attribute-value                    | 如果试图输出的属性值包含对已连接数据源无效的字符，那么系统会返回此错误。 例如，导出到定宽文本文件管理代理、带分隔符的文本文件管理代理和属性值对文本文件管理代理的属性值不能包含 CR、LF 或 EOF 字符。       |
| encryption-key-lost          | 在正常操作期间，应该不会看到此错误。 此错误表示，在加载对象时，FIM 无法解密在连接器空间中存储的加密属性的值。 也可能表示，计算机中缺少 FIM 使用的加密密钥集。 包含密码属性的任何管理代理（如 Active Directory 管理代理、Active Directory 全局地址列表 (GAL) 管理代理、Sun 和 Netscape 目录服务器管理代理、Lotus Notes 管理代理、Windows NT 4.0 管理代理）都可能会生成此错误。    |
| locking-error-needs-retry                  | 只有当多个管理代理同时尝试同步同一连接器空间对象时，系统才会返回此错误。 如果看到此错误，请尝试重新运行导出操作。     |
| cd-error                                  | 当已连接数据源包含指定的错误类型时，系统会返回此错误。 此错误随附 \<cd-error\> 元素，其中所含信息应该有助于进行故障排除。|
| unexpected-error                           | 如果试图导出更改，但操作导致故障发生，那么系统会返回此错误。 如果看到此错误，请在事件日志中查找有助于排查问题的详细信息。 |
| no-export-to-this-object-type              | 如果尝试对计算机对象执行预配操作或导出属性流，那么 Windows NT 4.0 管理代理会返回此错误。 Windows NT 4.0 管理代理不支持对此类对象执行导出操作。|  
| certifier-ou-not-configured                | 如果尝试预配新用户或容器，但为 \_MMS_Certifier 属性指定的认证者名称并不是正确配置的认证者容器的名称，那么 Lotus Notes 管理代理会返回此错误。 必须先使用 Synchronization Service Manager 配置每个认证者容器，然后才能在预配中使用它。 |
| temporary- certifier-file-creation-failure | 如果已预配新用户或容器，但出于某种原因（例如，磁盘空间不足、权限等）而无法创建认证者文件，那么 Lotus Notes 管理代理会返回此错误。 用于创建认证者文件的 FIM 进程为，提取认证者容器的认证者信息（由 \_MMS_Certifier 属性指定），并在 Lotus Notes 管理代理的 MAData 文件夹中临时创建认证者文件，以供 Notes API 使用。                                                                               |
| unexpected-provisioning-attribute           | 若要导出新预配的对象，但由于与其他预配属性值不兼容，客户扩展设置的特定预配属性不得包括在内，那么 Lotus Notes 管理代理会返回此错误。 例如，可能会在以下情况中看到此错误：  <br/>&#176; 创建联系人 (_MMS_IDRegType=0)，并提供以下属性之一：\_MMS_Certifier、\_MMS_OU、\_MMS_Password、\_MMS_IDStoreType、\_MMS_IDPath 或 MailFile  <br/>&#176; 创建美国用户或国际用户，但未指定创建 ID 文件 (_MMS_IDStoreType=0)，而是提供 \_MMS_IDPath 或 MailFile 属性。&#176; 创建 OU（认证者），并提供 \_MMS_OU 属性  <br/>&#176; 创建 OU（认证者），并提供 \_MMS_Certifier 属性
