---
author: jnHs
Description: Manage and view details related to each of your apps in Partner Center, and configure services such as A/B testing and maps.
title: 应用管理和服务
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7ffac7fa77191bbe56e7aa3870c71c3c02254d72
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6270649"
---
# <a name="app-management-and-services"></a>应用管理和服务

你可以管理和查看与每一个应用的相关的详细信息 [合作伙伴中心中，并配置服务，例如通知、 A / B 测试和地图。

使用合作伙伴中心中的应用时，你将看到在左侧的导航菜单中的部分**服务**和**应用管理**。 可以展开这些部分，访问如下所述的功能。

## <a name="services"></a>服务

**服务**部分让你可以管理多个不同的应用服务。

## <a name="xbox-live"></a>Xbox Live

如果你要发布游戏，你可以启用此页面上的[Xbox Live 创意者计划](http://xbox.com/developers/creators-program)。 这允许你启动配置和测试 Xbox Live 功能，并最终发布你的 Xbox Live 创意者计划游戏。

有关详细信息，请参阅[Xbox Live 创意者计划入门](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)和[创建新的 Xbox Live 创意者计划主题作品并发布到测试环境](../xbox-live/get-started-with-creators/create-and-test-a-new-creators-title.md)。

## <a name="experimentation"></a>实验

将**实验**页和 A/B 测试结合，用于创建并运行通用 Windows 平台 (UWP) 应用的实验。 在 A/B 测试中，你先衡量应用更改对某些客户的有效性，然后再为所有用户启用更改。

有关详细信息，请参阅[通过 A/B 测试运行应用实验](../monetize/run-app-experiments-with-a-b-testing.md)。

## <a name="maps"></a>地图

若要在面向 Windows 10 或 Windows 8.x 的应用中使用地图服务，请访问[必应地图开发人员中心](http://go.microsoft.com/fwlink/p/?LinkId=614880)。 有关如何从必应地图开发人员中心中请求地图验证密钥并将其添加到你的应用的信息，请参阅有关详细信息的[请求地图验证密钥](../maps-and-location/authentication-key.md)。 

仅对以前发布的应用适用于 Windows Phone 8.1 及更早版本中使用**地图**页面。 若要在这些应用中使用地图服务，你将需要请求地图服务应用程序 ID 和令牌要包含在你的应用代码中。 当你单击**获取令牌**时，我们将生成地图服务应用程序 ID (**ApplicationID**) 和地图服务身份验证令牌 (**AuthenticationToken**) 为你的应用。 请务必将这些值添加到你的代码之前你的程序包并提交你的应用。 有关详细信息，请参阅[如何将地图控件添加某一页面 (Windows Phone 8.1)](http://go.microsoft.com/fwlink/p/?LinkId=614882)。

## <a name="product-collections-and-purchases"></a>产品收集和购买

若要使用 Microsoft Store 收集 API 和 Microsoft Store 购买 API 访问应用和加载项的所有权信息，你需要输入关联 Azure AD 客户端 Id 下面。 请注意，需要 16 个小时才能使这些更改生效。

有关详细信息，请参阅[管理来自服务的产品授权](../monetize/view-and-grant-products-from-a-service.md)。

## <a name="administrator-consent"></a>管理员同意

f 你的产品与 Azure AD 集成，并调用的 Api，请求[应用程序权限或委派的权限](https://developer.microsoft.com/graph/docs/concepts/permissions_reference)需要管理员同意，输入你的 Azure AD 客户端 ID。 这将允许管理员为你的产品租户中的所有用户的名义执行其组织授权同意获取该应用。

有关详细信息，请参阅[请求同意整个租户](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant)。

## <a name="app-management"></a>应用管理

可以使用**应用管理**部分查看标识和软件包详细信息，并管理你的应用的名称。

## <a name="app-identity"></a>应用标识

此页面向你显示了你的应用在应用商店中的唯一标识的相关详细信息，包括用于链接到应用一览的 URL。

有关详细信息，请参阅[查看应用标识详细信息](view-app-identity-details.md)。

## <a name="manage-app-names"></a>管理应用名称

这是查看已为应用保留的所有名称的位置。 你可以从中保留额外名称，或删除将不再使用的名称。

有关详细信息，请参阅[管理应用名称](manage-app-names.md)。

## <a name="current-packages"></a>当前程序包

可以使用此页面查看与所有已发布程序包相关的详细信息。

> [!NOTE]
> 直到你的应用完成发布后，你才会在此处看到信息。

将显示每个程序包的名称、版本和体系结构。 单击**详细信息**可显示支持的语言、应用功能和文件大小等其他信息。 所见到的每个程序包的具体信息可能有所不同，具体取决于目标操作系统和其他因素。 

具有 OEM 权限的开发人员还可以在**当前程序包**页面[生成预安装程序包](generate-preinstall-packages-for-oems.md)。

## <a name="wnsmpns"></a>WNS/MPNS

**WNS/MPNS**部分提供了可帮助你创建并向你的应用客户发送通知的选项。 

> [!TIP]
> 对于 UWP 应用，我们建议你在合作伙伴中心中使用**通知**功能。 此功能，可以将通知发送到所有应用的客户，或者你符合条件的 Windows 10 客户子集定向到已定义[的客户类别](create-customer-segments.md)中。 有关详细信息，请参阅[将通知发送到应用客户](send-push-notifications-to-your-apps-customers.md)。

具体取决于你的应用包类型及其特定要求，你还可以使用以下选项之一： 

-   可使用 **Windows 推送通知服务 (WNS)** 从自己的云服务中发送 Toast、磁贴、锁屏提醒和原始更新。 有关详细信息，请参阅 [Windows 推送通知服务 (WNS) 概述](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)。

-   你可以使用 **Microsoft Azure 移动应用**发送推送通知、验证和管理应用用户，以及将应用数据存储在云中。 有关详细信息，请参阅[“移动应用”文档](http://go.microsoft.com/fwlink/p/?LinkId=221116)。

-   **Microsoft 推送通知服务 (MPNS)** 可以使用 Windows Phone 以前发布的.xap 程序包。 你可以在此处发送有限数量的未经验证的通知而不进行任何配置，不过为了避免节流限制，我们建议使用经过验证的通知。 如果你使用的 MPNS，你将需要证书上传到提供的**WNS/MPNS**页面上的字段。 有关详细信息，请参阅[设置经过验证的 Web 服务以发送 Windows Phone 8 的推送通知](http://go.microsoft.com/fwlink/p/?LinkId=528736)。
 

 
