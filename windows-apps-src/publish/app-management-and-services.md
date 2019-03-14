---
Description: 管理和查看与每个合作伙伴中心，您的应用程序相关的详细信息和配置服务，如 A / B 测试和映射。
title: 应用管理和服务
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d6261a7cce86c82b4865d7ca1d68c082cba9ccca
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610002"
---
# <a name="app-management-and-services"></a>应用管理和服务

可以管理和查看到您的应用程序的每个相关的详细信息[合作伙伴中心](https://partner.microsoft.com/dashboard/)，并配置服务，例如通知、 A / B 测试和映射。

使用合作伙伴中心中的应用，你将看到的左侧的导航菜单中的部分**Services**并**应用程序管理**。 可以展开这些部分，访问如下所述的功能。

## <a name="services"></a>服务

**服务**部分让你可以管理多个不同的应用服务。

## <a name="xbox-live"></a>Xbox Live

如果您要发布一个游戏，则可以启用[Xbox Live Creators 计划](https://xbox.com/developers/creators-program)此页上。 这样可以启动配置和测试 Xbox Live 功能，并最终发布您的 Xbox Live Creators 计划游戏。

有关详细信息，请参阅[开始使用 Xbox Live Creators 计划](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)并[创建一个新的 Xbox Live Creators 计划标题，并将发布到测试环境](../xbox-live/get-started-with-creators/create-and-test-a-new-creators-title.md)。

## <a name="experimentation"></a>实验

将**实验**页和 A/B 测试结合，用于创建并运行通用 Windows 平台 (UWP) 应用的实验。 在 A/B 测试中，你先衡量应用更改对某些客户的有效性，然后再为所有用户启用更改。

有关详细信息，请参阅[通过 A/B 测试运行应用实验](../monetize/run-app-experiments-with-a-b-testing.md)。

## <a name="maps"></a>地图

若要在面向 Windows 10 或 Windows 8.x 的应用中使用地图服务，请访问[必应地图开发人员中心](https://go.microsoft.com/fwlink/p/?LinkId=614880)。 有关如何从必应地图开发人员中心请求映射身份验证密钥并将其添加到您的应用程序的信息，请参阅[请求一个地图身份验证密钥](../maps-and-location/authentication-key.md)的详细信息。 

使用**Maps**仅对以前发布的应用适用于 Windows Phone 8.1 及更早版本的页面。 若要在这些应用程序中使用地图服务，将需要请求要包含在您的应用程序代码中的地图服务应用程序 ID 和令牌。 当您单击**获取令牌**，我们将生成映射服务应用程序 ID (**ApplicationID**)，并映射服务身份验证令牌 (**AuthenticationToken**) 为你的应用。 请确保将这些值添加到你的代码之前您可以打包和提交您的应用程序。 有关详细信息，请参阅[如何将地图控件添加某一页面 (Windows Phone 8.1)](https://go.microsoft.com/fwlink/p/?LinkId=614882)。

## <a name="product-collections-and-purchases"></a>产品收集和购买

若要使用 Microsoft Store 集合 API 和 Microsoft Store 购买 API 来访问应用程序和外接程序的所有权信息，您需要输入关联 Azure AD 客户端 Id 此处。 请注意，需要 16 个小时才能使这些更改生效。

有关详细信息，请参阅[管理来自服务的产品授权](../monetize/view-and-grant-products-from-a-service.md)。

## <a name="administrator-consent"></a>管理员同意

如果您的产品与 Azure AD 集成，并调用 Api 请求任一[应用程序权限或委派的权限](https://developer.microsoft.com/graph/docs/concepts/permissions_reference)需要管理员同意，请输入你的 Azure AD 客户端 ID。 这样，获取适用于您的产品可以代表租户中的所有用户执行操作其组织授予许可的应用的管理员。

有关详细信息，请参阅[请求整个租户的许可](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant)。

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

将显示每个程序包的名称、版本和体系结构。 单击“详细信息”可显示支持的语言、应用功能和文件大小等其他信息。 所见到的每个程序包的具体信息可能有所不同，具体取决于目标操作系统和其他因素。 

具有 OEM 权限的开发人员还可以在“当前程序包”页面[生成预安装程序包](generate-preinstall-packages-for-oems.md)。

## <a name="wnsmpns"></a>WNS/MPNS

**WNS/MPNS**部分提供选项来帮助你创建并将通知发送到应用程序的客户。 

> [!TIP]
> 对于 UWP 应用，我们建议使用**通知**合作伙伴中心中的新功能。 此功能允许你将通知发送到的所有应用程序的客户或中已定义到 Windows 10 客户符合条件的目标子集[客户群](create-customer-segments.md)。 有关详细信息，请参阅[将通知发送到应用客户](send-push-notifications-to-your-apps-customers.md)。

根据您的应用程序的包类型和其独特的要求，您还可以使用以下选项之一： 

-   可使用 **Windows 推送通知服务 (WNS)** 从自己的云服务中发送 Toast、磁贴、锁屏提醒和原始更新。 有关详细信息，请参阅 [Windows 推送通知服务 (WNS) 概述](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)。

-   你可以使用 **Microsoft Azure 移动应用**发送推送通知、验证和管理应用用户，以及将应用数据存储在云中。 有关详细信息，请参阅[“移动应用”文档](https://go.microsoft.com/fwlink/p/?LinkId=221116)。

-   **Microsoft 推送通知服务 (MPNS)** 可用于 Windows Phone 以前发布的.xap 程序包。 你可以在此处发送有限数量的未经验证的通知而不进行任何配置，不过为了避免节流限制，我们建议使用经过验证的通知。 如果使用 MPNS 时，您需要将证书上传到上提供的字段**WNS/MPNS**页。 有关详细信息，请参阅[设置经过验证的 Web 服务以发送 Windows Phone 8 的推送通知](https://go.microsoft.com/fwlink/p/?LinkId=528736)。
 

 
