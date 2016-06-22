---
# required metadata

title: 创建软件证书 | Microsoft 标识管理器
description: 了解如何使用证书管理器来创建和续订带配置文件模板的软件证书。
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fed5ada9-d80f-4825-aad7-4172ac5d71d3

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# 使用证书管理器创建软件证书
注册和续订软件证书无需是管理员，而且也无需虚拟智能卡。 值得注意的是，在某一时刻会提示允许证书操作，这是正常现象。

## 在 MIM 2016 证书管理器中创建软件证书配置文件模板

1.  创建请求有关虚拟智能卡证书的模板。 打开 mmc。

2.  单击“文件”****，然后单击“添加/删除管理单元”****。

3.  在可用管理单元列表中单击“证书模板”****，然后单击“添加”****。

4.  “证书模板”**** 现在位于 MMC 的控制台根下。 双击以查看所有可用证书模板。

5.  右键单击“用户模板”****，然后单击“复制模板”****。

6.  在“兼容性”****选项卡上，在证书颁发机构下选择 Windows Server 2008，并在证书接收方下选择 Windows 8.1/Windows Server 2012 R2。

    1.  在“常规” **** 选项卡上，在显示名称字段中键入 **存档证书模板**。

    2.  b。  在“请求处理” **** 选项卡上

        1.  将“目的” **** 设置为签名和加密。

        2.  选中“包含使用者允许的对称算法” ****。

        3.  如果希望存档密钥，请选中“存档使用者的加密私钥” ****。

        4.  在“执行以下操作…”下 选择“在注册过程中提示用户” ****。

    3.  在“加密” **** 选项卡上

        1.  在提供程序类别下选择“密钥存储提供程序” ****

        2.  选择“请求可以使用使用者计算机上的任何可用提供程序” ****。

    4.  在“安全” **** 选项卡上添加你希望提供“注册” **** 访问权限的安全组。 例如，如果希望向所有用户提供访问权限，请选择“已经过身份验证” **** 用户组，然后为其选择“注册” **** 权限。

    5.  在“使用者名称” **** 选项卡上

        1.  取消选中“在使用者名称中包含电子邮件名称”****。

        2.  在“将此信息包含在另一个使用者名称中” ****下，取消选中“电子邮件名称” ****。

    6.  单击“确定” **** 以完成更改，并创建新的模板。 现在新模板应显示在证书模板列表中。

    7.  选择“文件”****，然后单击“添加/删除管理单元”****，向 MMC 控制台添加证书颁发机构管理单元。 当系统询问希望管理哪台计算机时，选择“本地计算机” ****。

    8.  在 MMC 左窗格中，展开“证书颁发机构（本地）” ****，然后再展开证书颁发机构列表内的 CA。

    9. 右键单击“证书模板”****、单击“新建”****，然后单击要颁发的“证书模板”****。

    10. 在列表中选择刚刚创建的新模板（“存档证书模板”****），然后单击“确定” ****。

## 创建配置文件模板

1.  作为拥有管理员权限的用户登录到 CM 门户。

2.  依次转到“管理”&gt;“管理配置文件模板” **** ，并确保已选中靠近“MIM CM 示例智能卡登录配置文件模板” **** 的复选框，然后单击“复制选定的配置文件模板” ****。

3.  键入配置文件模板名称，然后单击“确定” ****。

4.  在下一屏幕中，单击“添加新的证书模板” **** ，并确保选中 CA 名称旁边的复选框。

5.  选中存档软件证书旁边的复选框，并单击“添加” ****。

6.  通过选中其旁边的复选框，删除用户模板，然后单击“删除所选证书模板” **** ，再单击“确定” ****。

7.  单击“更改常规设置” ****。

8.  选中“在服务器上生成加密密钥” **** 左侧的复选框，并单击“确定” ****。 在左窗格中单击“恢复策略” ****。

9. 单击“更改常规设置” ****。

10. 如果你想要重新颁发已存档证书，请选中“重新颁发存档证书” **** 左侧的复选框，然后单击“确定” ****。

11. 如果你使用的是虚拟智能卡 CM，则必须禁用数据收集项，因为它不能与打开的数据收集一起使用。 单击左窗格中的策略，禁用每个策略的数据收集，然后取消选中“示例数据项” **** 旁边的复选框，再单击“删除数据集合项” ****。 然后单击“确定” ****。


<!--HONumber=Apr16_HO2-->

