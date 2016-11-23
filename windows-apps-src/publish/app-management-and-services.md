---
author: jnHs
Description: "在 Windows 开发人员中心仪表板中管理和查看与每一个应用相关的详细信息，并配置如推送通知、A/B 测试和地图等服务。"
title: "应用管理和服务"
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
translationtype: Human Translation
ms.sourcegitcommit: 9611b3cc87c29e29a19b87a5b5f37bef1a71dfbb
ms.openlocfilehash: d5244bda2cefa146e3c481270fe72fdb8f2d35cf

---

# 应用管理和服务

可在 Windows 开发人员中心仪表板中管理和查看与每一个应用相关的详细信息，并配置如推送通知、A/B 测试和地图等服务。

在仪表板中使用应用时，将在左侧导航菜单中看到“服务”和“应用管理”部分。 可以展开这些部分，访问如下所述的功能。

## 服务

**服务**部分让你可以管理多个不同的应用服务。

### 推送通知

**推送通知**部分支持创建目标推送通知，并将其发送到应用客户。 可将这些推送通知发送到所有应用客户，或者发送到满足[客户类别](create-customer-segments.md)中所定义条件的 Windows10 客户的子集。 有关详细信息，请参阅[将目标推送通知发送到应用客户](send-push-notifications-to-your-apps-customers.md)。

还可在左侧导航菜单中单击“WNS/MPNS”页，将以下选项的其中一项用于推送通知，具体取决于应用包类型及其特定要求： 

-   可使用 **Windows 推送通知服务 (WNS)** 从自己的云服务中发送 Toast、磁贴、锁屏提醒和原始更新。 有关详细信息，请参阅 [Windows 推送通知服务 (WNS) 概述](https://msdn.microsoft.com/library/windows/apps/mt187203)。

-   你可以使用 **Microsoft Azure 移动应用**发送推送通知、验证和管理应用用户，以及将应用数据存储在云中。 有关详细信息，请参阅[“移动应用”文档](http://go.microsoft.com/fwlink/p/?LinkId=221116)。

-   **Microsoft 推送通知服务 (MPNS)** 可以和 Windows Phone 的 .xap 程序包结合使用。 你可以在此处发送有限数量的未经验证的通知而不进行任何配置，不过为了避免节流限制，我们建议使用经过验证的通知。 如果你使用的是 MPNS，则需要将证书上传到**“推送通知”**页面上所提供的字段中。 有关详细信息，请参阅[设置经过验证的 Web 服务以发送 Windows Phone 8 的推送通知](http://go.microsoft.com/fwlink/p/?LinkId=528736)。

### 实验

将**实验**页和 A/B 测试结合，用于创建并运行通用 Windows 平台 (UWP) 应用的实验。 在 A/B 测试中，你先衡量应用更改对某些客户的有效性，然后再为所有用户启用更改。

有关详细信息，请参阅[通过 A/B 测试运行应用实验](../monetize/run-app-experiments-with-a-b-testing.md)。

### 地图

若要在适用于 Windows Phone 8.1 及更早版本的应用中使用地图服务，需要在应用代码中包含地图服务应用程序 ID 和令牌。 可在“服务”部分的“地图”页面上获取此令牌。

> **注意** 若要在面向 Windows10 或 Windows8.x 的应用中使用地图服务，请访问[必应地图开发人员中心](http://go.microsoft.com/fwlink/p/?LinkId=614880)。 有关详细信息，请参阅[请求地图身份验证密钥](https://msdn.microsoft.com/library/windows/apps/mt219694)。

有关详细信息，请参阅[使用地图服务](use-map-services.md)。

### 产品收集和购买

若要使用 Windows 应用商店收集 API 和 Windows 应用商店购买 API 来访问应用和加载项的所有权信息，需要在此处输入相关联的 Azure AD 客户端 ID。 请注意，需要 16 个小时才能使这些更改生效。

有关详细信息，请参阅[从服务查看和授予产品](https://msdn.microsoft.com/library/windows/apps/mt609002)。

## 应用管理

可以使用“应用管理”部分查看标识和程序包详细信息，并管理你的应用的名称。

### 应用标识

此页面向你显示了你的应用在应用商店中的唯一标识的相关详细信息，包括用于链接到应用一览的 URL。

有关详细信息，请参阅[查看应用标识详细信息](view-app-identity-details.md)。

### 管理应用名称

这是查看已为应用保留的所有名称的位置。 你可以从中保留额外名称，或删除将不再使用的名称。

有关详细信息，请参阅[管理应用名称](manage-app-names.md)。

### 当前程序包

可以使用此页面查看与所有已发布程序包相关的详细信息。

> **注意** 直到你的应用完成发布后，你才会在此处看到某些信息。

将显示每个程序包的名称、版本和体系结构。 单击“详细信息”可显示支持的语言、应用功能和文件大小等其他信息。 所见到的每个程序包的具体信息可能有所不同，具体取决于目标操作系统和其他因素。 

具有 OEM 权限的开发人员还可以在“当前程序包”页面[生成预安装程序包](generate-preinstall-packages-for-oems.md)。

 

 



<!--HONumber=Nov16_HO1-->


