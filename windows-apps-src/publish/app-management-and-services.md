---
Description: 在合作伙伴中心中管理和查看与每个应用相关的详细信息，并配置服务（如 A/B 测试和地图）。
title: 应用管理和服务
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 30610cdacbd9d2be10205958688376371f0387f6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260038"
---
# <a name="app-management-and-services"></a>应用管理和服务

你可以在[合作伙伴中心](https://partner.microsoft.com/dashboard)中管理和查看与每个应用相关的详细信息，并配置通知、A/B 测试和地图等服务。

使用合作伙伴中心的应用时，可以在 "**服务**和**应用管理**" 的左侧导航菜单中看到部分。 可以展开这些部分，访问如下所述的功能。

## <a name="services"></a>服务

**服务**部分让你可以管理多个不同的应用服务。

## <a name="xbox-live"></a>Xbox Live

如果你正在发布游戏，则可以在此页上启用[Xbox Live 创建者计划](https://www.xbox.com/developers/creators-program)。 这样，你就可以开始配置和测试 Xbox Live 功能，并最终发布 Xbox Live 创意者计划游戏。

有关详细信息，请参阅[Xbox Live 创建者计划入门](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators)和[创建新的 Xbox Live 创建者计划标题并发布到测试环境](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title)。

## <a name="experimentation"></a>实验

将**实验**页和 A/B 测试结合，用于创建并运行通用 Windows 平台 (UWP) 应用的实验。 在 A/B 测试中，你先衡量应用更改对某些客户的有效性，然后再为所有用户启用更改。

有关详细信息，请参阅[通过 A/B 测试运行应用实验](../monetize/run-app-experiments-with-a-b-testing.md)。

## <a name="maps"></a>地图

若要在面向 Windows 10 或 Windows 8.x 的应用中使用地图服务，请访问[必应地图开发人员中心](https://www.bingmapsportal.com/)。 有关如何从必应地图开发人员中心请求地图身份验证密钥并将其添加到应用的信息，请参阅[请求地图身份验证密钥](../maps-and-location/authentication-key.md)以获取详细信息。 

仅将 "**映射**" 页用于 Windows Phone 8.1 及更早版本的以前发布的应用。 若要在这些应用中使用地图服务，需要请求一个映射服务应用程序 ID 和一个令牌，将其包含在应用代码中。 单击 "**获取令牌**" 时，我们将为你的应用生成一个映射服务应用程序 ID （**ApplicationID**）和映射服务身份验证令牌（**AuthenticationToken**）。 打包并提交应用之前，请务必将这些值添加到代码。 有关详细信息，请参阅[如何将地图控件添加某一页面 (Windows Phone 8.1)](https://docs.microsoft.com/previous-versions/windows/apps/jj207033(v=vs.105)?redirectedfrom=MSDN)。

## <a name="product-collections-and-purchases"></a>产品收集和购买

若要使用 Microsoft Store 收集 API 和 Microsoft Store 购买 API 来访问应用和外接程序的所有权信息，需要在此处输入关联的 Azure AD 客户端 Id。 请注意，需要 16 个小时才能使这些更改生效。

有关详细信息，请参阅[管理来自服务的产品授权](../monetize/view-and-grant-products-from-a-service.md)。

## <a name="administrator-consent"></a>管理员许可

如果你的产品与 Azure AD 集成，并调用了要求管理员同意的[应用程序权限或委托权限](https://developer.microsoft.com/graph/docs/concepts/permissions_reference)的 api，请在此处输入你的 Azure AD 客户端 ID。 这样，为组织获取应用的管理员就会授权你的产品代表租户中的所有用户。

有关详细信息，请参阅[请求整个租户的同意](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant)。

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

具有 OEM 权限的开发人员还可以在[当前程序包](generate-preinstall-packages-for-oems.md)页面**生成预安装程序包**。

## <a name="wnsmpns"></a>WNS/MPNS

**WNS/MPNS**部分提供了一些选项，可帮助你创建通知并将通知发送到应用的客户。 

> [!TIP]
> 对于 UWP 应用，我们建议使用合作伙伴中心的**通知**功能。 此功能可让你将通知发送到你的应用的所有客户，或发送到你的 Windows 10 客户的目标子集，这些客户满足你在[客户段](create-customer-segments.md)中定义的条件。 有关详细信息，请参阅[将通知发送到应用客户](send-push-notifications-to-your-apps-customers.md)。

你还可以使用以下选项之一，具体取决于你的应用包类型及其特定要求： 

-   可使用 **Windows 推送通知服务 (WNS)** 从自己的云服务中发送 Toast、磁贴、锁屏提醒和原始更新。 有关详细信息，请参阅 [Windows 推送通知服务 (WNS) 概述](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)。

-   你可以使用 **Microsoft Azure 移动应用**发送推送通知、验证和管理应用用户，以及将应用数据存储在云中。 有关详细信息，请参阅[“移动应用”文档](https://docs.microsoft.com/azure/app-service-mobile/)。

-   **Microsoft 推送通知服务（MPNS）** 可以与以前发布的 .xap 包一起用于 Windows Phone。 你可以在此处发送有限数量的未经验证的通知而不进行任何配置，不过为了避免节流限制，我们建议使用经过验证的通知。 如果使用的是 MPNS，则需要将证书上传到**WNS/MPNS**页面上提供的字段。 有关详细信息，请参阅[设置经过验证的 Web 服务以发送 Windows Phone 8 的推送通知](https://docs.microsoft.com/previous-versions/windows/apps/ff941099(v=vs.105)?redirectedfrom=MSDN)。
 

 
